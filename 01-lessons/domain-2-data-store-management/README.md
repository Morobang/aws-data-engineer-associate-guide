# Domain 2: Data Store Management

Welcome to Domain 2 of the AWS Certified Data Engineer Associate (DEA-C01) exam preparation! This domain focuses on choosing, implementing, and managing different data storage solutions in AWS.

## 🎯 Domain Overview

**Exam Weight**: 26% of total exam  
**Focus**: Understanding and implementing appropriate data storage solutions for different use cases, managing data catalogs, lifecycle management, and schema design.

## 🏪 Why This Matters: Real-World Context

Imagine you're the data engineer for both a **global supermarket chain** and a **major bank**:

- **Supermarket Chain**: You need to store real-time sales data, historical transaction records, customer profiles, inventory data, and supply chain information - each with different access patterns and cost requirements.

- **Banking Institution**: You must manage customer account data, transaction histories, regulatory reports, fraud detection models, and audit trails - all with strict compliance, security, and performance requirements.

This domain teaches you how to choose the right AWS storage service for each scenario, organize your data effectively, manage costs through lifecycle policies, and evolve your data models as business needs change.

## 📖 Learning Approach

Every concept in this domain is explained through practical scenarios you can relate to:
- **Shopping experiences** → Data store selection decisions
- **Banking operations** → Compliance and performance requirements
- **Business growth** → Schema evolution and lifecycle management
- **Cost optimization** → Storage tier strategies

## 📋 Task Statement Structure

Each Task Statement is covered in a dedicated comprehensive lesson that maps directly to the official AWS exam guide. Every concept is explained with real-world supermarket and banking examples that build your understanding from the ground up.

---

## 🗄️ Task Statement 2.1: Choose a Data Store

### [📦 Task Statement 2.1: Choose a Data Store](task-2-1-choose-data-store.md)
**Complete Coverage**: All knowledge areas and skills from the official exam guide

**Knowledge Areas Covered**:
- Storage platforms and their characteristics (OLTP vs OLAP, relational vs NoSQL)
- Storage services and configurations for specific performance demands
- Data storage formats (.csv, .txt, Parquet, JSON, Avro) and their trade-offs
- Aligning data storage with data migration requirements
- Determining appropriate storage solutions for specific access patterns
- Managing locks to prevent concurrent access issues

**Skills Covered**:
- Implementing storage services for cost and performance requirements
- Configuring storage services for specific access patterns
- Applying storage services to appropriate use cases
- Integrating migration tools into data processing systems
- Implementing data migration and remote access methods

---

## 📚 Task Statement 2.2: Understand Data Cataloging Systems

### [🔍 Task Statement 2.2: Understand Data Cataloging Systems](task-2-2-understand-data-cataloging-systems.md)
**Complete Coverage**: All knowledge areas and skills from the official exam guide

**Knowledge Areas Covered**:
- Creating and maintaining data catalogs
- Data classification based on business and compliance requirements
- Components of metadata and data catalogs
- Schema discovery and management

**Skills Covered**:
- Using data catalogs to consume data from various sources
- Building and referencing data catalogs (AWS Glue Data Catalog, Apache Hive)
- Discovering schemas and using AWS Glue crawlers
- Synchronizing partitions with data catalogs
- Creating source and target connections for cataloging

---

## ⏳ Task Statement 2.3: Manage the Lifecycle of Data

### [🔄 Task Statement 2.3: Manage the Lifecycle of Data](task-2-3-manage-lifecycle-of-data.md)
**Complete Coverage**: All knowledge areas and skills from the official exam guide

**Knowledge Areas Covered**:
- Storage solutions for hot and cold data requirements
- Cost optimization based on data lifecycle
- Data deletion for business and legal compliance
- Data retention policies and archiving strategies
- Data protection with appropriate resiliency and availability

**Skills Covered**:
- Load and unload operations between S3 and Redshift
- Managing S3 Lifecycle policies for storage tier transitions
- Expiring data using S3 Lifecycle policies
- Managing S3 versioning and DynamoDB TTL

---

## 🏗️ Task Statement 2.4: Design Data Models and Schema Evolution

### [📐 Task Statement 2.4: Design Data Models and Schema Evolution](task-2-4-design-data-models-schema-evolution.md)
**Complete Coverage**: All knowledge areas and skills from the official exam guide

**Knowledge Areas Covered**:
- Data modeling concepts for different storage systems
- Ensuring data accuracy and trustworthiness through data lineage
- Indexing, partitioning, compression, and optimization techniques
- Modeling structured, semi-structured, and unstructured data
- Schema evolution techniques and backward compatibility

**Skills Covered**:
- Designing schemas for Redshift, DynamoDB, and Lake Formation
- Addressing changes to data characteristics
- Performing schema conversion with AWS SCT and DMS
- Establishing data lineage using AWS tools

---

## 🎯 Key AWS Services Covered

### Storage Services
- **Amazon S3** (Object storage, lifecycle policies, storage classes)
- **Amazon Redshift** (Data warehouse, columnar storage)
- **Amazon RDS** (Relational databases, Multi-AZ, read replicas)
- **Amazon DynamoDB** (NoSQL, TTL, streams)

### Catalog and Metadata Services
- **AWS Glue Data Catalog** (Centralized metadata repository)
- **AWS Glue Crawlers** (Schema discovery and cataloging)
- **Apache Hive Metastore** (Integration with EMR)

### Migration and Integration Services
- **AWS DMS** (Database migration service)
- **AWS Transfer Family** (File transfer protocols)
- **AWS Schema Conversion Tool** (Schema migration)

### Analytics and Lake Services
- **AWS Lake Formation** (Data lake governance)
- **Amazon EMR** (Big data processing)
- **Amazon Kinesis** (Streaming data storage)

## 📈 Exam Weight: 26%

This domain is the second most heavily weighted in the exam. Focus on understanding:
1. **When to use which storage service** based on access patterns and requirements
2. **Cost optimization strategies** using appropriate storage tiers and lifecycle policies
3. **Data catalog management** for data discovery and governance
4. **Schema design and evolution** patterns for different data types

## 🔗 Navigation

- **Next**: [Task Statement 2.1: Choose a Data Store](task-2-1-choose-data-store.md)
- **Previous**: [Domain 1: Data Ingestion and Transformation](../domain-1-data-ingestion-transformation/README.md)
- **Back to**: [Main Course](../../README.md)

---

## 🚀 Getting Started

Start with Task Statement 2.1 to understand how to choose the right data store for different scenarios. Each lesson builds upon the previous one, using consistent real-world examples that make complex storage decisions easy to understand and remember.

Remember: The key to mastering data store management is understanding that **different access patterns require different storage solutions**. A supermarket's real-time inventory updates have very different requirements than historical sales analysis - and AWS provides the right tools for both!