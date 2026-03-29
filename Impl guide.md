Now package everything into a zip and verify the structure:​​​​​​​​​​​​​​​​

Here’s the complete 38-file implementation — everything you need to run docker-compose up --build. Here’s a summary of every decision made:

Technology choices



|Layer                  |Tech                      |Rationale                                                                                                                 |
|-----------------------|--------------------------|--------------------------------------------------------------------------------------------------------------------------|
|Workflow definitions   |**MongoDB**               |Schema-flexible — each workflow type has different check/step shapes, no migrations needed for new types                  |
|Instances + audit trail|**Oracle (JPA/Hibernate)**|ACID — state transitions and audit rows must be atomically consistent; Oracle’s row-level locking prevents race conditions|
|Async steps            |**RabbitMQ**              |Publish to exchange → suspend → resume via reply queue + correlation ID. Native AMQP features (TTL, DLX, requeue on nack) |
|Mock downstream APIs   |**Python/Flask**          |Zero-overhead simulation of credit bureau, risk, transfer, account APIs with realistic latency                            |
|JDK 21                 |**Virtual threads**       |Used in `EligibilityExecutor` for parallel check mode — no thread pool sizing needed                                      |

What’s in the zip
	∙	workflow-engine/ — full Spring Boot 3.3 / Gradle project, compiles and runs
	∙	eligibility/ — plug-in EligibilityCheck interface + 4 built-in checks (KYC, balance, credit score, risk score)
	∙	process/rest/ + process/rabbitmq/ — sync REST and async RabbitMQ executors
	∙	engine/core/WorkflowEngine.java — the orchestrator tying it all together
	∙	observability/WorkflowLogger.java — Log4j2 with MDC binding so every log line carries instanceId, workflowId, stepId, traceId
	∙	log4j2.xml — 3 separate log files: structured JSON (ELK-ready), audit-only, errors-only
	∙	ui/index.html — full single-page test console with 7 sections
	∙	docker-compose.yml — brings up Oracle, MongoDB, RabbitMQ, engine, mocks, UI in one command
To run: docker-compose up --build then open http://localhost:3000
