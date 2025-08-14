# Serverless Service Alternatives Report  
**CST8917 – Serverless Applications**  
**Author:** Keval Trivedi
**Due:** August 15, 2025

---


# 1) Azure Functions (Triggers & Bindings)

**Azure → AWS / GCP equivalents**  
- **AWS:** AWS Lambda  
- **GCP:** Google Cloud Functions / Cloud Run functions  

| Area | Azure Functions | AWS Lambda | GCP Cloud Functions / Cloud Run functions |
|---|---|---|---|
| Overview | FaaS platform with first-class *bindings* (input/output) | FaaS platform with broad event source integration | FaaS for event-driven functions (Cloud Functions) and container-based functions (Cloud Run functions) |
| Triggers & bindings | HTTP, Timer, Event Grid, Service Bus, Blob, Queue | HTTP, S3, DynamoDB streams, Kinesis, SNS, SQS, EventBridge, etc. | HTTP, CloudEvents, Pub/Sub, Storage triggers |
| Integration | Built-in bindings, Azure AD integration | AWS services via IAM roles & native integrations | GCP integrations with service accounts, Cloud Pub/Sub, Cloud Storage |
| Monitoring | Azure Monitor, Application Insights | CloudWatch, X-Ray, Lambda Insights | Cloud Logging/Monitoring |
| Pricing | Pay-per-execution + GB-s, premium/dedicated plans | Pay-per-execution + GB-s, provisioned concurrency | Pay-per-execution + GB-s |
| Strengths | Bindings reduce boilerplate; great .NET support | Largest ecosystem of native event sources | Simple developer experience; strong GCP product integration |
| Weaknesses | Azure-centric; cold-start on consumption plan | Complexity, many options; cold starts unless provisioned | Fewer native integrations than AWS |

---

# 2) Durable Functions (Chaining, Orchestration, Fan-out/Fan-in)

**Azure → AWS / GCP equivalents**  
- **AWS:** AWS Step Functions  
- **GCP:** Google Cloud Workflows  

| Area | Azure Durable Functions | AWS Step Functions | GCP Workflows |
|---|---|---|---|
| Overview | Azure Functions extension for stateful orchestration | Orchestration/state machine service | Managed orchestration for calling services/APIs |
| Chaining & fan-out/fan-in | First-class orchestrator + activities | Parallel & sequential workflows | Sequential & parallel flows |
| Integration | Functions, Storage, Service Bus | 220+ AWS services, HTTP | GCP services, HTTP, Cloud Functions |
| Monitoring | Durable history, Azure Monitor | Execution history, CloudWatch | Execution logs, Cloud Logging |
| Pricing | Pay for Functions + orchestration state | Per state transition | Per execution/step |
| Strengths | Code-centric workflow patterns | Mature service integrations | Simple syntax, API-first |
| Weaknesses | Azure-specific model | Per-state pricing for chatty flows | Less mature for complex workflows |

---

# 3) Azure Logic Apps

**Azure → AWS / GCP equivalents**  
- **AWS:** AWS Step Functions (with EventBridge for events)  
- **GCP:** Workflows (with Cloud Tasks / Pub/Sub)  

| Area | Azure Logic Apps | AWS Step Functions | GCP Workflows |
|---|---|---|---|
| Overview | Low-code visual workflow automation | Serverless orchestration/state machine | Programmatic orchestration |
| Connectors | 400+ connectors | AWS service integrations | HTTP APIs, cloud services |
| Integration | SaaS connectors, B2B, on-prem gateway | AWS services, API Gateway, Lambda | GCP services, HTTP endpoints |
| Monitoring | Azure Monitor | CloudWatch | Cloud Logging |
| Pricing | Per action/connector | Per state transition | Per execution/step |
| Strengths | Extensive SaaS connectors | Deep cloud integration | Lightweight API orchestration |
| Weaknesses | Vendor lock-in | Fewer SaaS connectors | Fewer low-code features |

---

# 4) Azure Service Bus (Queues & Topics)

**Azure → AWS / GCP equivalents**  
- **AWS:** Amazon SQS + SNS / Amazon MQ  
- **GCP:** Google Cloud Pub/Sub  

| Area | Azure Service Bus | AWS SQS/SNS/MQ | GCP Pub/Sub |
|---|---|---|---|
| Overview | Enterprise messaging with queues & topics | SQS for queues, SNS for pub/sub, MQ for broker | Global pub/sub messaging |
| Features | FIFO, sessions, transactions, DLQ | FIFO/Standard queues, pub/sub | Ordering keys, DLQ |
| Integration | Azure services | AWS services | GCP services |
| Monitoring | Azure Monitor | CloudWatch | Cloud Monitoring |
| Pricing | Per operation + retention | Per request | Per message |
| Strengths | Rich enterprise features | Simple, scalable | Global scale |
| Weaknesses | Azure-specific | Combine services for parity | Fewer enterprise broker features |

---

# 5) Azure Event Grid

**Azure → AWS / GCP equivalents**  
- **AWS:** Amazon EventBridge  
- **GCP:** Eventarc  

| Area | Azure Event Grid | AWS EventBridge | GCP Eventarc |
|---|---|---|---|
| Overview | Push-based event routing | Event bus & routing | Event routing with CloudEvents |
| Event model | CloudEvents | Custom events, SaaS partner events | CloudEvents |
| Integration | Azure services, HTTP/webhooks | AWS services, SaaS | GCP services, Pub/Sub |
| Monitoring | Azure Monitor | CloudWatch | Cloud Logging |
| Pricing | Per event | Per event | Per matched event |
| Strengths | Simple, scalable | Rich partner integrations | Cloud-native integration |
| Weaknesses | High-rate pricing | Rules complexity | Fewer SaaS partners |

---

# 6) Azure Event Hubs

**Azure → AWS / GCP equivalents**  
- **AWS:** Amazon Kinesis Data Streams / Firehose  
- **GCP:** Google Cloud Pub/Sub (or Pub/Sub Lite)  

| Area | Azure Event Hubs | AWS Kinesis | GCP Pub/Sub |
|---|---|---|---|
| Overview | Event ingestion for telemetry/streaming | Real-time streams (Kinesis), delivery (Firehose) | Global messaging/streaming |
| Scaling | Throughput units | Shards | Auto-scaling (Lite for reserved capacity) |
| Integration | Blob/ADLS, Kafka endpoint | AWS analytics tools | Dataflow, BigQuery |
| Monitoring | Azure Monitor | CloudWatch | Cloud Monitoring |
| Pricing | Throughput + retention | Shard-hour + payloads | Per message |
| Strengths | Kafka endpoint, big-data scale | Rich analytics integration | BigQuery integration |
| Weaknesses | Capacity planning | Shard management | Ordering trade-offs |

---

## High-level migration & pricing overview
- **Functions:** Replace bindings with explicit SDK calls on AWS/GCP.  
- **Durable orchestration:** Map to Step Functions or Workflows.  
- **Messaging:** Service Bus → SQS/SNS or Pub/Sub (adjust for sessions/transactions).  
- **Eventing:** Event Grid ↔ EventBridge/Eventarc.  
- **Streaming:** Event Hubs ↔ Kinesis/Pub/Sub.  

---

## High-level Migration & Pricing Overview

Migrating from Azure serverless services to AWS or GCP requires mapping both **capabilities** and **patterns** — not all features have a direct equivalent, so design adjustments are often necessary.

### Functions
- **Migration path:**  
  - Replace Azure Functions *bindings* (input/output decorators) with explicit SDK calls or service client integrations.  
  - Example: An Azure Function triggered by a Service Bus queue with an output binding to Blob Storage will need:  
    - **AWS:** Lambda triggered by SQS → SDK call to S3.  
    - **GCP:** Cloud Function triggered by Pub/Sub → SDK call to Cloud Storage.  
- **Considerations:**  
  - Bindings in Azure simplify boilerplate — expect to write more glue code in AWS/GCP.  
  - Cold start behaviour varies — AWS supports *Provisioned Concurrency*; GCP supports *min instances*; Azure has Premium Plan for always-warm.  
- **Pricing:**  
  - All platforms use pay-per-invocation + GB-seconds; watch for additional request or data egress charges.

---

### Durable Orchestration
- **Migration path:**  
  - **Azure Durable Functions** orchestrators become:  
    - **AWS:** Step Functions workflows (using Map State for fan-out/fan-in).  
    - **GCP:** Workflows YAML/JSON with `parallel` branches or multiple `call` steps.  
- **Considerations:**  
  - Durable Functions is *code-first*, Step Functions and Workflows are *declarative JSON/YAML*.  
  - Long-running workflows: check max execution times (Durable supports days/months, Step Functions Standard workflows up to 1 year, GCP Workflows up to 1 year).  
  - Error handling and retries are configurable in all three — syntax differs.  
- **Pricing:**  
  - Azure: Functions consumption + orchestration state storage.  
  - AWS: Per state transition (Standard workflow ~$0.025 per 1,000 transitions).  
  - GCP: Per execution + step ($0.000025 per step after free tier).

---

### Messaging (Queues & Topics)
- **Migration path:**  
  - **Azure Service Bus** features like sessions, transactions, and topics map to:  
    - **AWS:** Combination of SQS (queues) + SNS (topics) or Amazon MQ for JMS/AMQP broker semantics.  
    - **GCP:** Cloud Pub/Sub for pub/sub; Cloud Tasks for point-to-point task queues.  
- **Considerations:**  
  - Transactions & sessions: Directly supported in Service Bus, but require custom patterns in SQS/SNS and Pub/Sub.  
  - FIFO support: Native in Service Bus and SQS FIFO queues; Pub/Sub uses ordering keys.  
- **Pricing:**  
  - Azure: Per operation + message size/retention.  
  - AWS: SQS per request, SNS per publish/delivery.  
  - GCP: Per message ingestion/delivery; Lite option for lower cost.

---

### Eventing
- **Migration path:**  
  - **Azure Event Grid** → **AWS EventBridge** or **GCP Eventarc**.  
  - Migrate event subscriptions to equivalent routing rules (EventBridge) or triggers (Eventarc).  
- **Considerations:**  
  - Event schemas: Event Grid & Eventarc use CloudEvents; EventBridge supports its own schema registry plus CloudEvents.  
  - Partner integrations: EventBridge often has more SaaS sources; Eventarc more focused on GCP services.  
  - Latency & delivery guarantees: All support at-least-once delivery; retry/backoff configuration differs.  
- **Pricing:**  
  - Per event published/delivered; large-scale workloads can accumulate costs quickly.

---

### Streaming
- **Migration path:**  
  - **Azure Event Hubs** → **AWS Kinesis Data Streams** (real-time) / **Firehose** (delivery) or **GCP Pub/Sub** (for streaming pipelines).  
- **Considerations:**  
  - Scaling units: Event Hubs uses throughput units; Kinesis uses shards; Pub/Sub auto-scales (Lite uses reserved capacity).  
  - Kafka compatibility: Event Hubs supports Kafka API; AWS offers MSK; GCP supports Kafka via Confluent Cloud.  
  - Downstream analytics integration:  
    - Azure → ADLS, Synapse.  
    - AWS → Kinesis Data Analytics, S3, Redshift.  
    - GCP → Dataflow, BigQuery.  
- **Pricing:**  
  - Azure: Per throughput unit-hour + data retention.  
  - AWS: Per shard-hour + payload size.  
  - GCP: Per message size; Lite is per throughput reservation.
