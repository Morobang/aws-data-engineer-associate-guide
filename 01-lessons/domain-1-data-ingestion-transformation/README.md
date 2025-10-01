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

## 🏪 Task Statement 1.1: Data Ingestion
*"How do businesses collect and receive data?"*

### [Lesson 1: Understanding Data Ingestion (The Basics)](01-understanding-data-ingestion.md)
**Real-world focus**: How supermarkets and banks collect data every second
- What is data ingestion? (Think: every swipe of your credit card)
- Streaming vs Batch ingestion (Real-time vs end-of-day processing)
- Throughput and latency (Why some data needs to be instant, others can wait)
- Data patterns and frequency (Rush hour vs midnight data flows)

### [Lesson 2: Streaming Data Ingestion with Amazon Kinesis](02-streaming-data-kinesis.md)
**Real-world focus**: Credit card transactions and store sensors in real-time
- Kinesis Data Streams (Like a conveyor belt for live data)
- Kinesis Data Firehose (Automatic delivery to storage)
- Kinesis Data Analytics (Analyzing data as it flows)
- Fan-in and fan-out patterns (Many sources, many destinations)

### [Lesson 3: Batch Data Ingestion](03-batch-data-ingestion.md)
**Real-world focus**: Daily sales reports and monthly bank statements
- Reading from S3, databases, and files
- Scheduled ingestion with EventBridge (Like setting an alarm clock)
- Event-driven ingestion (Triggered by file uploads)
- Managing large file transfers and throttling

### [Lesson 4: Database Migration and Change Data Capture](04-database-migration-cdc.md)
**Real-world focus**: Moving bank records without losing a single transaction
- AWS DMS for database migration
- Change Data Capture (CDC) - tracking every change
- Replayability (Being able to "rewind" and process again)
- Stateful vs stateless transactions

---

## 🔄 Task Statement 1.2: Data Transformation
*"How do we clean, convert, and prepare data for use?"*

### [Lesson 5: ETL Fundamentals with AWS Glue](05-etl-fundamentals-glue.md)
**Real-world focus**: Converting messy store data into clean business reports
- What is ETL? (Extract, Transform, Load explained simply)
- AWS Glue overview (Your data cleaning assistant)
- Data Catalog (Like a library catalog for your data)
- Volume, velocity, variety (The 3 V's with supermarket examples)

### [Lesson 6: Data Processing with Apache Spark](06-spark-data-processing.md)
**Real-world focus**: Processing millions of transactions quickly
- What is Apache Spark? (Parallel processing explained)
- EMR for big data (When you have LOTS of data)
- Distributed computing concepts (Many computers working together)
- Container optimization (ECS/EKS for processing)

### [Lesson 7: Data Format Transformations](07-data-format-transformations.md)
**Real-world focus**: Converting receipts from different formats into one standard
- CSV to Parquet (Why format matters for speed)
- JSON transformations (Handling complex nested data)
- Connecting different data sources (JDBC/ODBC)
- Cost optimization strategies

### [Lesson 8: Integration and API Creation](08-integration-apis.md)
**Real-world focus**: Making bank data available to mobile apps
- Integrating multiple data sources
- Creating data APIs (Making data accessible to applications)
- Troubleshooting transformation failures
- Performance optimization techniques

---

## 🎼 Task Statement 1.3: Data Pipeline Orchestration
*"How do we coordinate and manage complex data workflows?"*

### [Lesson 9: Event-Driven Architecture](09-event-driven-architecture.md)
**Real-world focus**: When a customer buys something, what happens next?
- EventBridge for connecting systems
- Lambda functions triggered by events
- Building responsive data pipelines
- Notification services (SNS/SQS)

### [Lesson 10: Workflow Orchestration](10-workflow-orchestration.md)
**Real-world focus**: Coordinating end-of-day bank processing
- AWS Step Functions (Like a flowchart for data processing)
- Amazon MWAA (Managed Airflow)
- Building fault-tolerant pipelines
- Serverless workflows

---

## 💻 Task Statement 1.4: Programming Concepts
*"What coding skills do data engineers need?"*

### [Lesson 11: SQL for Data Engineers](11-sql-data-engineers.md)
**Real-world focus**: Querying customer purchase patterns and fraud detection
- SQL fundamentals for data transformation
- Query optimization techniques
- Redshift stored procedures
- Complex joins and aggregations

### [Lesson 12: Infrastructure as Code and CI/CD](12-infrastructure-cicd.md)
**Real-world focus**: Automatically deploying data pipelines safely
- CloudFormation templates (Building infrastructure with code)
- AWS CDK (Cloud Development Kit)
- CI/CD for data pipelines
- Git version control for data engineers

### [Lesson 13: Performance Optimization and Serverless](13-performance-serverless.md)
**Real-world focus**: Making data processing fast and cost-effective
- Lambda function optimization
- AWS SAM for serverless deployment
- Code optimization techniques
- Concurrency and performance tuning

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