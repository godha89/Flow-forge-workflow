Now I have a clear picture of what needs changing. Let me make targeted edits:​​​​​​​​​​​​​​​​

Now update the Sync/Async tab — replace all Kafka/SQS references with RabbitMQ:​​​​​​​​​​​​​​​​

Now update the mixed pipeline section and the YAML config tab which references Kafka topics:​​​​​​​​​​​​​​​​

Now update the mixed pipeline label at the bottom of the async tab:​​​​​​​​​​​​​​​​

Now update the pipeline step labels in the mixed pipeline from generic “Queue” to “RabbitMQ”:​​​​​​​​​​​​​​​​

Also update the audit trail detail that mentions “published to risk.assessment.request”:​​​​​​​​​​​​​​​​

Done. Here’s a summary of every change made:
Ingress Layer — stripped down to just the REST API surface: POST /workflows/{type}/start, GET /workflows/{id}/status, and POST /workflows/{id}/resume. Removed API Gateway, GraphQL, gRPC, Kafka ingress, and Webhook listener.
Process Layer — replaced the generic “Async Queue Publisher” with explicit RabbitMQ Publisher and RabbitMQ Reply Consumer nodes.
Infrastructure Layer — swapped Message Broker (Kafka) for RabbitMQ Broker, and removed the Config Store (etcd).
Config YAML — process type changed from QUEUE to RABBITMQ, with topic/reply_topic replaced by RabbitMQ-native exchange, routing_key, and reply_queue.
Sync/Async Tab — “Queue Executor” renamed to “RabbitMQ Executor”, pipeline labels updated to ASYNC · RABBITMQ, and YAML snippet updated with RabbitMQ-specific fields.
Audit Trail — updated log entries to show RabbitMQ exchange routing and reply queue names.​​​​​​​​​​​​​​​​
