# CST8917 – Serverless Applications  
## Serverless & Event-Driven Services – Azure vs AWS vs GCP  

---

##  Goal of the Report  
This document compares **Azure’s serverless and event-driven services** used in the course with their closest equivalents in **Amazon Web Services (AWS)** and **Google Cloud Platform (GCP)**.  
For each service, we cover:  

- Overview of what the service does  
- Core features (triggers, bindings, messaging/eventing)  
- Integration options with other services and CI/CD tools  
- Monitoring and observability capabilities  
- High-level pricing model  
- Strengths and weaknesses from a serverless perspective  

---

##  Quick Reference Mapping  

| #  | Azure Service                        | AWS Equivalent(s)                                  | GCP Equivalent(s)                                 |
|----|---------------------------------------|-----------------------------------------------------|---------------------------------------------------|
| 1  | Azure Functions                       | AWS Lambda                                          | Cloud Functions                                   |
| 2  | Azure Durable Functions               | AWS Step Functions (with Lambda)                   | Cloud Workflows                                   |
| 3  | Azure Logic Apps                      | Step Functions + EventBridge (+ Lambda)            | Cloud Workflows + Eventarc                        |
| 4  | Azure Blob Storage                    | Amazon S3                                           | Cloud Storage                                     |
| 5  | Azure Cosmos DB                       | Amazon DynamoDB                                     | Firestore / Cloud Spanner                         |
| 6  | Azure Service Bus (Queues & Topics)   | Amazon SQS + Amazon SNS                             | Pub/Sub                                           |
| 7  | Azure Event Hubs                      | Amazon Kinesis Data Streams / Amazon MSK            | Pub/Sub                                           |
| 8  | Azure Event Grid                      | Amazon EventBridge                                  | Eventarc                                          |
| 9  | Azure Relay                           | API Gateway private integration / AWS PrivateLink   | No direct equivalent – mix of API Gateway + VPC   |
| 10 | Azure IoT Hub                         | AWS IoT Core                                        | Partner IoT platforms + Pub/Sub                   |

---

## Service 1 – Azure Functions vs AWS Lambda vs Google Cloud Functions  

**Overview**  
Azure Functions is a serverless compute service that runs small pieces of code triggered by events, scaling automatically with demand and only charging for runtime.  

**Core Features**  
- **Azure:** Triggers include HTTP, Timer, Blob, Queue, Service Bus, Event Hub, Cosmos DB, Event Grid, SignalR. Bindings simplify connecting to other services.  
- **AWS:** Lambda can be triggered by API Gateway, S3, DynamoDB Streams, EventBridge, SQS/SNS, Kinesis, CloudWatch, IoT events.  
- **GCP:** Cloud Functions respond to HTTP, Pub/Sub, Cloud Storage, Firestore, and Eventarc events.  

**Integration Options**  
- Azure: GitHub Actions, Azure DevOps, ARM/Bicep/Terraform deployments.  
- AWS: CodePipeline, SAM, CDK, Terraform.  
- GCP: Cloud Build, Cloud Deploy, Terraform.  

**Monitoring & Observability**  
- Azure: Application Insights, Azure Monitor, Live Metrics.  
- AWS: CloudWatch logs/metrics, X-Ray tracing.  
- GCP: Cloud Logging, Cloud Monitoring, Trace.  

**Pricing Model**  
Per execution and GB-seconds of runtime, with a free allowance; provisioned capacity billed separately.  

**Strengths & Weaknesses**  
- **Strengths:** Rich native triggers, easy service integration.  
- **Weaknesses:** Cold start delays, time limits for execution.  

---

## Service 2 – Azure Durable Functions vs AWS Step Functions vs Google Cloud Workflows  

**Overview**  
Provides orchestration capabilities for coordinating multiple serverless functions into long-running workflows with built-in state management.  

**Core Features**  
- **Azure:** Chaining, fan-out/fan-in, human interaction, sub-orchestrations, state persistence in Azure Storage.  
- **AWS:** Sequential/parallel workflows, retries, error handling, choice states, callbacks.  
- **GCP:** YAML/JSON-based orchestration, retries, conditionals, API integration.  

**Integration Options**  
- Azure: Native with Functions, deploy via Bicep/Terraform.  
- AWS: Works with Lambda, ECS, Glue, API Gateway.  
- GCP: Integrates with Cloud Functions, Cloud Run, external APIs.  

**Monitoring & Observability**  
- Azure: Instance tracking APIs, Application Insights.  
- AWS: CloudWatch logs, execution history, X-Ray.  
- GCP: Cloud Logging, Cloud Monitoring.  

**Pricing Model**  
Charged per state transition plus costs for services invoked.  

**Strengths & Weaknesses**  
- **Strengths:** Deep function integration in Azure, visual editor in AWS, API-first in GCP.  
- **Weaknesses:** Vendor-specific orchestration formats limit portability.  

---

## Service 3 – Azure Logic Apps vs AWS Step Functions & EventBridge vs Google Cloud Workflows & Eventarc  

**Overview**  
A no-code workflow automation platform connecting hundreds of services with predefined triggers and actions.  

**Core Features**  
- **Azure:** 400+ connectors, supports HTTP, event, and scheduled triggers.  
- **AWS:** Step Functions for orchestration + EventBridge for event routing, with Lambda for custom logic.  
- **GCP:** Workflows combined with Eventarc for event-based automation.  

**Integration Options**  
- Azure: Connects to SaaS and on-prem systems; deployable via ARM/Bicep/Terraform.  
- AWS/GCP: Connect through event buses and functions.  

**Monitoring & Observability**  
- Azure: Run history, Azure Monitor logs.  
- AWS: CloudWatch metrics/logs.  
- GCP: Cloud Logging, Monitoring.  

**Pricing Model**  
Billed per trigger/action, plus premium connector fees.  

**Strengths & Weaknesses**  
- **Strengths:** Rapid integration without heavy coding.  
- **Weaknesses:** Costs increase with workflow complexity.  

---

## Service 4 – Azure Blob Storage vs Amazon S3 vs Google Cloud Storage  

**Overview**  
Highly available, secure object storage for unstructured data and static files.  

**Core Features**  
- Tiered storage, versioning, lifecycle management, event triggers, WORM.  

**Integration Options**  
- Azure Functions, AWS Lambda, and GCP Cloud Functions triggers.  
- Storage backend for analytics and CDN delivery.  

**Monitoring & Observability**  
- Usage metrics and access logging via native monitoring tools.  

**Pricing Model**  
Pay for storage, operations, and data egress.  

**Strengths & Weaknesses**  
- **Strengths:** Durable and scalable, supports event-driven workflows.  
- **Weaknesses:** Egress charges can be high.  

---

## Service 5 – Azure Cosmos DB vs Amazon DynamoDB vs Google Firestore / Cloud Spanner  

**Overview**  
Managed, globally distributed NoSQL databases with low latency.  

**Core Features**  
- Cosmos DB: Multi-model APIs, global writes, tunable consistency, change feed.  
- DynamoDB: Key-value/document store, Streams, global tables, caching with DAX.  
- Firestore: Document-based, real-time sync; Spanner: globally consistent SQL.  

**Integration Options**  
- Serverless triggers, analytics pipelines, IaC deployment.  

**Monitoring & Observability**  
- Query insights, usage metrics, error logs.  

**Pricing Model**  
Provisioned or on-demand capacity (RU/s, read/write units, document ops).  

**Strengths & Weaknesses**  
- **Strengths:** Global scale, built-in triggers.  
- **Weaknesses:** Partitioning challenges, replication cost.  

---

## Service 6 – Azure Service Bus vs Amazon SQS & SNS vs Google Pub/Sub  

**Overview**  
Messaging services for decoupling applications using queue and publish/subscribe models.  

**Core Features**  
- Service Bus: Queues, Topics, DLQs, filtering, scheduling.  
- AWS: SQS for queues, SNS for topics, DLQs, filtering.  
- GCP: Pub/Sub push/pull, ordering keys, DLQs.  

**Integration Options**  
- Functions, Lambda, Cloud Functions.  

**Monitoring & Observability**  
- Queue depth, processing rate, error counts.  

**Pricing Model**  
Per message/request, plus throughput costs.  

**Strengths & Weaknesses**  
- **Strengths:** Reliable delivery, advanced messaging patterns.  
- **Weaknesses:** Ordering and poison message handling add complexity.  

---

## Service 7 – Azure Event Hubs vs Amazon Kinesis / MSK vs Google Pub/Sub  

**Overview**  
Ingests massive volumes of event streams for analytics and processing.  

**Core Features**  
- Partitioned ingestion, consumer groups, checkpointing, Kafka support.  

**Integration Options**  
- Stream analytics, serverless triggers, SIEM pipelines.  

**Monitoring & Observability**  
- Lag metrics, throughput, partition status.  

**Pricing Model**  
Charged by throughput units or shard hours.  

**Strengths & Weaknesses**  
- **Strengths:** High throughput, replay capability.  
- **Weaknesses:** Consumer scaling can be tricky.  

---

## Service 8 – Azure Event Grid vs Amazon EventBridge vs Google Eventarc  

**Overview**  
Event routing service for building loosely coupled, event-driven architectures.  

**Core Features**  
- Cloud and custom event sources, filtering, retries, CloudEvents support.  

**Integration Options**  
- Azure: Functions, Logic Apps, Event Hubs, Service Bus.  
- AWS: Lambda, Step Functions, SQS/SNS.  
- GCP: Cloud Run, Functions, Workflows.  

**Monitoring & Observability**  
- Delivery metrics, error logs, dead-letter tracking.  

**Pricing Model**  
Per million events processed.  

**Strengths & Weaknesses**  
- **Strengths:** Easy event fan-out.  
- **Weaknesses:** No built-in event history; pair with streaming service.  

---

## Service 9 – Azure Relay vs Amazon API Gateway PrivateLink vs GCP Hybrid API Gateway  

**Overview**  
Securely expose on-premises services to the cloud without opening inbound firewall ports.  

**Core Features**  
- Hybrid connections over HTTP/TCP, outbound from on-prem, authentication controls.  

**Integration Options**  
- Often paired with Logic Apps, Functions, API Management.  

**Monitoring & Observability**  
- Connection logs, usage metrics.  

**Pricing Model**  
Billed by listener count and data processed.  

**Strengths & Weaknesses**  
- **Strengths:** Simple hybrid connectivity.  
- **Weaknesses:** Limited protocol support, added latency.  

---

## Service 10 – Azure IoT Hub vs AWS IoT Core vs Google IoT Alternatives  

**Overview**  
Managed platform for secure IoT device connectivity and messaging.  

**Core Features**  
- Device registry, twin state, MQTT/AMQP/HTTP messaging, cloud↔device communication.  

**Integration Options**  
- Azure: Event Hubs, Functions, Stream Analytics.  
- AWS: Kinesis, Lambda, S3.  
- GCP: Partner IoT → Pub/Sub → processing services.  

**Monitoring & Observability**  
- Device metrics, message counts, diagnostic logs.  

**Pricing Model**  
Per device/message, with provisioning features billed separately.  

**Strengths & Weaknesses**  
- **Strengths:** Scalable device messaging, secure protocols.  
- **Weaknesses:** Network reliability challenges for remote devices.  

---

##  Final Notes  
Although these services are conceptually similar across Azure, AWS, and GCP, **differences in integration options, pricing, and unique features** can significantly affect design decisions.  
In multi-cloud scenarios, adopting **open standards** (e.g., CloudEvents) and keeping orchestration logic portable will help maintain flexibility.  

---
