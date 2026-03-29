Situation:
I want to design a generic and easily expandable workflow product
Tasks:
It should be capable to perform eligibility checks based on different workflow type. These Cheka should be generic and plug and play types mapped to each workflow
It should be able to maintain status and have proper observability for tracking statuses
Post eligibility, it can also have further processes which would be APIs which support REST and Queues for supporting both sync and async function
Acceptance:
	1.	Dynamic engine
	2.	Easily configurable
	3.	Tracking and observability
	4.	Support sync and async functions for processes in workflow

simplify solution by only supporting Rest API and Rabbit MQ for async support, also remove API gateway from ingress layer

Design looks good, now help me implement the solution using JDK21, springboot gradle, oracle/mongoDB(choose based on requirements), python(if required), Docker for packaging, and a test UI which can be used to configure rules and track processes for testing purpose. Use log4J for logging which can be used for observability
