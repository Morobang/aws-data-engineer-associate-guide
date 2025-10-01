# Lesson 3: AWS Glue for ETL

## 🎯 Learning Objectives

After completing this lesson, you will be able to:
- Understand AWS Glue's role in data processing and cataloging
- Explain the components of AWS Glue: Data Catalog, Crawlers, and Jobs
- Design ETL pipelines using AWS Glue
- Use Glue Studio for visual ETL development

## 🔧 What is AWS Glue?

AWS Glue is a fully managed extract, transform, and load (ETL) service that makes it easy to prepare and load data for analytics. It's serverless, so you don't need to manage infrastructure.

Think of AWS Glue as your data preparation assistant - it discovers your data, catalogs it, and helps you transform it into the format you need.

## 🏗️ AWS Glue Components

### 1. AWS Glue Data Catalog
**Purpose**: A central metadata repository for all your data assets

#### What it stores:
- **Databases**: Logical groupings of tables
- **Tables**: Metadata about data structure (schema, location, format)
- **Partitions**: Information about data organization
- **Connections**: Database and data store connection information

#### Benefits:
- Single source of truth for metadata
- Integrates with other AWS services (Athena, EMR, Redshift)
- Automatic schema evolution detection

### 2. AWS Glue Crawlers
**Purpose**: Automatically discover and catalog data

#### How they work:
1. **Scan**: Examine data in specified locations
2. **Infer**: Determine schema and format
3. **Catalog**: Create/update table definitions in Data Catalog

#### Supported Data Sources:
- Amazon S3
- JDBC databases (RDS, Redshift, etc.)
- DynamoDB
- Cassandra

### 3. AWS Glue Jobs
**Purpose**: Execute ETL logic to transform data

#### Types of Jobs:
- **Spark Jobs**: For large-scale data processing
- **Python Shell Jobs**: For lightweight processing
- **Ray Jobs**: For machine learning workloads

## 📊 ETL with AWS Glue

### Extract
- Read data from various sources
- Supported formats: JSON, CSV, Parquet, ORC, Avro
- Connection to databases via JDBC

### Transform
- **Built-in Transformations**:
  - ApplyMapping: Rename/recast columns
  - Filter: Remove unwanted records
  - Join: Combine datasets
  - DropFields: Remove columns
  - Relationalize: Flatten nested structures

### Load
- Write data to target destinations
- Automatic partitioning
- Format conversion

## 🎨 AWS Glue Studio

### Visual ETL Development
- **Drag-and-drop interface**: Create ETL jobs without coding
- **Pre-built transforms**: Common transformations ready to use
- **Data preview**: See data at each transformation step
- **Job monitoring**: Track job execution and performance

### Visual Job Editor Features:
- **Sources**: S3, databases, Data Catalog tables
- **Transforms**: Join, filter, map, aggregate
- **Targets**: S3, databases, Data Catalog

## 🔄 Common ETL Patterns

### Pattern 1: Data Lake ETL
```
Raw Data (S3) → Glue Crawler → Data Catalog → Glue Job → Processed Data (S3)
```

**Use Case**: Convert raw JSON logs to Parquet format for faster queries

### Pattern 2: Database to Data Warehouse
```
Source Database → Glue Job → Transform → Redshift
```

**Use Case**: Daily ETL from operational database to analytics warehouse

### Pattern 3: Data Integration
```
Multiple Sources → Glue Jobs → Join/Merge → Unified Dataset
```

**Use Case**: Combine customer data from CRM and transaction data from e-commerce platform

## 🚀 Advanced Features

### Dynamic Frames
- **What**: Glue's enhanced DataFrame with schema flexibility
- **Benefits**: 
  - Handle schema variations
  - Automatic error handling
  - Schema evolution support

### Job Bookmarks
- **Purpose**: Track processed data to avoid reprocessing
- **How**: Stores state information about previous job runs
- **Benefits**: Incremental processing, cost optimization

### Glue DataBrew
- **Purpose**: Visual data preparation tool
- **Features**: 
  - 250+ pre-built transformations
  - Data profiling and quality assessment
  - No-code data preparation

## 📋 Best Practices

### Crawler Configuration
1. **Scheduling**: Run crawlers regularly to detect schema changes
2. **Classification**: Use custom classifiers for specific data formats
3. **Partitioning**: Configure proper partition schemes for performance

### Job Optimization
1. **Worker Type**: Choose appropriate worker size (Standard, G.1X, G.2X)
2. **Number of Workers**: Scale based on data volume
3. **Job Bookmarks**: Enable for incremental processing
4. **Compression**: Use columnar formats (Parquet, ORC) for better performance

### Cost Optimization
1. **Right-sizing**: Monitor job metrics to optimize worker allocation
2. **Spot Instances**: Use for fault-tolerant workloads
3. **Scheduling**: Run jobs during off-peak hours when possible

## 🔍 Monitoring and Troubleshooting

### CloudWatch Metrics
- **Job Success/Failure Rates**: Monitor job reliability
- **Data Movement**: Track records read/written
- **Resource Utilization**: Monitor CPU and memory usage

### Logging
- **CloudWatch Logs**: Detailed job execution logs
- **Error Messages**: Specific error details for troubleshooting
- **Custom Logging**: Add custom log statements in job code

## 💡 Real-World Examples

### Example 1: E-commerce Data Pipeline
```python
# Glue Job to process customer orders
from awsglue.context import GlueContext
from awsglue.dynamicframe import DynamicFrame

# Read from S3
orders_df = glueContext.create_dynamic_frame.from_catalog(
    database="ecommerce",
    table_name="raw_orders"
)

# Transform: Add calculated fields
transformed_df = orders_df.apply_mapping([
    ("order_id", "string", "order_id", "string"),
    ("customer_id", "string", "customer_id", "string"),
    ("order_total", "double", "order_amount", "double"),
    ("order_date", "string", "order_date", "date")
])

# Write to data warehouse
glueContext.write_dynamic_frame.from_jdbc_conf(
    frame=transformed_df,
    catalog_connection="redshift-connection",
    connection_options={"dbtable": "processed_orders"}
)
```

### Example 2: Log Processing Pipeline
```python
# Process application logs
logs_df = glueContext.create_dynamic_frame.from_options(
    connection_type="s3",
    connection_options={"paths": ["s3://app-logs/"]},
    format="json"
)

# Filter and transform
error_logs = logs_df.filter(lambda record: record["level"] == "ERROR")
transformed_logs = error_logs.apply_mapping([
    ("timestamp", "string", "log_time", "timestamp"),
    ("message", "string", "error_message", "string"),
    ("source", "string", "application", "string")
])

# Write to S3 in Parquet format
glueContext.write_dynamic_frame.from_options(
    frame=transformed_logs,
    connection_type="s3",
    connection_options={"path": "s3://processed-logs/errors/"},
    format="parquet"
)
```

## 🔗 Integration with Other AWS Services

### Amazon Athena
- Query data cataloged by Glue
- Use Glue Data Catalog as metadata store

### Amazon EMR
- Share metadata through Glue Data Catalog
- Use Glue for lightweight ETL, EMR for complex processing

### AWS Lambda
- Trigger Glue jobs from Lambda functions
- Process small datasets with Lambda, large with Glue

## 📊 When to Use AWS Glue

### Use AWS Glue When:
- Need serverless ETL processing
- Want to catalog and discover data automatically
- Processing structured and semi-structured data
- Need visual ETL development

### Consider Alternatives When:
- Real-time streaming processing (use Kinesis Analytics)
- Complex machine learning pipelines (use SageMaker)
- Very simple data movement (use Lambda)

## 📋 Summary

AWS Glue provides a comprehensive platform for ETL operations:
- **Data Catalog**: Central metadata repository
- **Crawlers**: Automatic data discovery and cataloging
- **Jobs**: Serverless ETL processing
- **Studio**: Visual development environment

It's ideal for batch processing, data cataloging, and preparing data for analytics.

## 🔗 Next Steps

Continue with [Lesson 4: AWS Lambda for Data Processing](04-aws-lambda-data-processing.md) to learn about serverless data processing patterns.

---

**Next**: [Lesson 4: AWS Lambda for Data Processing](04-aws-lambda-data-processing.md)  
**Previous**: [Lesson 2: Amazon Kinesis Family](02-amazon-kinesis-family.md)