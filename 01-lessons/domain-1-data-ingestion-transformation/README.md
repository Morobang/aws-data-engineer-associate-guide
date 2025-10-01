# Domain 1: Data Ingestion and Transformation (34%)

This domain covers the fundamental concepts and AWS services for ingesting, processing, and transforming data. It represents the largest portion of the exam content.

> **� Teaching Philosophy**: Every concept is explained using real-world examples from supermarkets and banks - places you interact with daily. If you can understand how your grocery store processes your purchase or how your bank handles your transactions, you can understand data engineering!

## 🎯 What You'll Master

By completing this domain, you'll understand how to:
- **Collect data** from various sources (like how stores collect sales data)
- **Process data in real-time** (like fraud detection when you swipe your card)
- **Transform data** (like converting raw transaction logs into monthly statements)
- **Orchestrate workflows** (like coordinating all the steps in processing your loan application)
- **Optimize performance** (like making sure your mobile banking app loads instantly)

## 📖 Lesson Structure

Each lesson includes:
- **Real-world scenarios** using supermarket and banking examples
- **Beginner-friendly explanations** with no assumed prior knowledge
- **Visual diagrams** showing data flow and architecture
- **Hands-on examples** you can relate to daily experiences
- **AWS service deep-dives** with practical configurations

---

## 📋 Task Statement Structure

Each Task Statement is covered in a dedicated comprehensive lesson that maps directly to the official AWS exam guide. Every concept is explained with real-world supermarket and banking examples.

---

## 🏪 Task Statement 1.1: Perform Data Ingestion

### [📥 Task Statement 1.1: Perform Data Ingestion](task-1-1-perform-data-ingestion.md)
**Complete Coverage**: All knowledge areas and skills from the official exam guide

**Knowledge Areas Covered**:
- Throughput and latency characteristics for AWS services
- Data ingestion patterns (frequency, data history)
- Streaming vs batch data ingestion
- Replayability of data ingestion pipelines
- Stateful and stateless data transactions

**Skills Covered**:
- Reading from streaming sources (Kinesis, MSK, DynamoDB Streams, DMS, Glue, Redshift)
- Reading from batch sources (S3, Glue, EMR, DMS, Redshift, Lambda, AppFlow)
- Implementing batch ingestion configurations
- Consuming data APIs and setting up schedulers
- Event triggers and Lambda integration
- IP allowlists and throttling management
- Fan-in and fan-out patterns

---

## 🔄 Task Statement 1.2: Transform and Process Data

### [🔧 Task Statement 1.2: Transform and Process Data](task-1-2-transform-process-data.md)
**Complete Coverage**: All knowledge areas and skills from the official exam guide

**Knowledge Areas Covered**:
- ETL pipeline creation based on business requirements
- Volume, velocity, and variety of data (structured, unstructured)
- Cloud computing and distributed computing concepts
- Apache Spark for data processing
- Intermediate data staging locations

**Skills Covered**:
- Container optimization (EKS, ECS) for performance
- Connecting to different data sources (JDBC, ODBC)
- Integrating data from multiple sources
- Cost optimization while processing data
- Implementing transformation services (EMR, Glue, Lambda, Redshift)
- Format transformations (CSV to Parquet, etc.)
- Troubleshooting transformation failures
- Creating data APIs for system integration

---

## 🎼 Task Statement 1.3: Orchestrate Data Pipelines

### [🎯 Task Statement 1.3: Orchestrate Data Pipelines](task-1-3-orchestrate-data-pipelines.md)
**Complete Coverage**: All knowledge areas and skills from the official exam guide

**Knowledge Areas Covered**:
- AWS service integration for ETL pipelines
- Event-driven architecture concepts
- Service configuration for schedules and dependencies
- Serverless workflow patterns

**Skills Covered**:
- Orchestration services (Lambda, EventBridge, MWAA, Step Functions, Glue workflows)
- Building pipelines for performance, availability, scalability, resiliency
- Implementing and maintaining serverless workflows
- Notification services (SNS, SQS) for alerts

---

## 💻 Task Statement 1.4: Apply Programming Concepts

### [👨‍💻 Task Statement 1.4: Apply Programming Concepts](task-1-4-apply-programming-concepts.md)
**Complete Coverage**: All knowledge areas and skills from the official exam guide

**Knowledge Areas Covered**:
- CI/CD for data pipelines (implementation, testing, deployment)
- SQL queries for data sources and transformations
- Infrastructure as Code (CDK, CloudFormation)
- Distributed computing concepts
- Data structures and algorithms
- SQL query optimization

**Skills Covered**:
- Code optimization for runtime reduction
- Lambda function configuration for concurrency and performance
- SQL queries for data transformation (Redshift stored procedures)
- Structuring SQL queries for pipeline requirements
- Git commands (creating, updating, cloning, branching)
- AWS SAM for serverless deployment
- Storage volume usage and mounting in Lambda

### [Lesson 7: Data Transformation Techniques](07-data-transformation-techniques.md)
- Common transformation patterns
- Data quality and validation
- Schema evolution
- Performance optimization

### [Lesson 8: Streaming Data Processing](08-streaming-data-processing.md)
- Real-time processing concepts
- Stream processing patterns
- Windowing and aggregations
- Fault tolerance

### [Lesson 9: Batch Processing](09-batch-processing.md)
- Batch processing patterns
- Scheduling and orchestration
- Error handling and retry logic
- Monitoring batch jobs

### [Lesson 10: Data Pipeline Orchestration](10-data-pipeline-orchestration.md)
- AWS Step Functions
- Apache Airflow on AWS
- EventBridge for event-driven architecture
- Workflow best practices

## 🎯 Key AWS Services Covered

- **Amazon Kinesis** (Data Streams, Firehose, Analytics)
- **AWS Glue** (ETL, Data Catalog, Crawlers)
- **AWS Lambda** (Serverless processing)
- **Amazon EMR** (Big data frameworks)
- **AWS DMS** (Database migration)
- **AWS Step Functions** (Workflow orchestration)
- **Amazon EventBridge** (Event-driven architecture)

## 📈 Exam Weight: 34%

This domain is the most heavily weighted in the exam, so ensure you have a solid understanding of all concepts and hands-on experience with the key services.

## 🔗 Navigation

- **Next**: [Lesson 1: Introduction to Data Ingestion](01-introduction-to-data-ingestion.md)
- **Back to**: [Main Course](../../README.md)