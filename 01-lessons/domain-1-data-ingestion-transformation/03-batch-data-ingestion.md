# Lesson 3: Batch Data Ingestion

## 🎯 Learning Objectives

After completing this lesson, you will be able to:
- Understand when to use batch ingestion vs streaming
- Configure scheduled data ingestion using AWS services
- Implement event-driven batch processing
- Handle large file transfers and manage throttling
- Set up proper error handling and retry mechanisms

## 🏪 What is Batch Data Ingestion? (Think: End-of-Day Store Operations)

Imagine you work at a supermarket. At the end of each day:
- **All cash registers** send their daily sales data to headquarters
- **Inventory system** compiles what was sold and what's left
- **Employee time clocks** submit all punch-in/punch-out records
- **Security system** backs up all camera footage
- **Supplier systems** receive tomorrow's order requests

This is **batch ingestion** - collecting and processing data in chunks at specific times rather than continuously.

### 🏦 Banking Batch Processing Example
Every night after banking hours:
- **All branch transactions** are consolidated
- **ATM logs** from thousands of machines are collected
- **Credit card purchases** from the day are processed
- **Interest calculations** run for millions of accounts
- **Regulatory reports** are prepared and submitted

## ⏰ When to Choose Batch Over Streaming

### 📊 Decision Matrix

| Scenario | Choose Batch If... | Choose Streaming If... |
|----------|-------------------|----------------------|
| **Data Volume** | Large files (GB/TB) | Continuous small records |
| **Processing Time** | Hours/minutes acceptable | Seconds/milliseconds required |
| **Cost Sensitivity** | Budget is tight | Real-time value justifies cost |
| **Data Sources** | Files, databases, exports | APIs, sensors, user actions |
| **Business Impact** | Delayed processing OK | Immediate action required |

### 🏪 Supermarket Examples

#### ✅ Good for Batch:
- **Daily sales analysis**: "How much milk did we sell today?"
- **Inventory reordering**: "What products need restocking this week?"
- **Customer loyalty reports**: "Which customers earned rewards this month?"
- **Financial reconciliation**: "Do our cash drawers balance?"

#### ❌ Bad for Batch:
- **Fraud detection**: Can't wait until tomorrow to stop a stolen card
- **Inventory alerts**: Need to know immediately when freezer breaks
- **Price updates**: Dynamic pricing needs real-time changes
- **Security monitoring**: Shoplifting alerts must be instant

## 📥 AWS Services for Batch Ingestion

### 1. Amazon S3 (Simple Storage Service)
**Think**: Like a massive digital warehouse for files

#### 🏪 Supermarket Use Case:
```
Daily Operation:
- POS systems → Generate sales files (CSV)
- Upload to S3 bucket: "daily-sales-data/"
- Files organized by date: "2024/10/01/store-001-sales.csv"
```

#### Key Features:
- **Unlimited storage**: Store petabytes of data
- **Event notifications**: Trigger processing when files arrive
- **Lifecycle policies**: Automatically archive old data
- **Security**: Encryption and access controls

### 2. AWS Glue (ETL Service)
**Think**: Your data cleaning and organizing assistant

#### 🏦 Banking Use Case:
```
Nightly Process:
1. Glue Crawler scans new transaction files in S3
2. Updates Data Catalog with schema changes
3. Glue Job transforms and cleans data
4. Loads processed data into Redshift warehouse
```

#### Key Capabilities:
- **Data cataloging**: Automatically discovers data structure
- **ETL jobs**: Transform data without managing servers
- **Scheduling**: Run jobs on schedule or triggers
- **Visual interface**: Build pipelines without coding

### 3. AWS Database Migration Service (DMS)
**Think**: Moving truck for databases

#### 🏪 Supermarket Use Case:
```
Scenario: Migrating from old POS system to cloud
1. DMS reads from legacy database
2. Transforms data format during migration
3. Loads into new AWS database
4. Keeps systems in sync with CDC (Change Data Capture)
```

### 4. Amazon EventBridge (Event Scheduler)
**Think**: Like setting alarms for data processing

#### Configuration Examples:
```json
{
  "ScheduleExpression": "rate(24 hours)",
  "Description": "Daily sales report generation",
  "Target": {
    "Service": "AWS Glue",
    "Job": "process-daily-sales"
  }
}
```

## 🔄 Batch Ingestion Patterns

### Pattern 1: Scheduled Ingestion
**How it works**: Process data at regular intervals

#### 🏪 Supermarket Example: Daily Inventory Report
```
Schedule: Every day at 2 AM
Process:
1. EventBridge triggers Lambda function
2. Lambda queries inventory database
3. Generates CSV report
4. Uploads report to S3
5. Sends notification to managers
```

#### AWS Implementation:
```python
import boto3
import json
from datetime import datetime

def lambda_handler(event, context):
    # Triggered by EventBridge daily at 2 AM
    
    # 1. Query database for inventory data
    inventory_data = query_inventory_database()
    
    # 2. Generate report
    report = generate_inventory_report(inventory_data)
    
    # 3. Upload to S3
    s3 = boto3.client('s3')
    filename = f"inventory-report-{datetime.now().strftime('%Y-%m-%d')}.csv"
    s3.put_object(
        Bucket='supermarket-reports',
        Key=f'inventory/{filename}',
        Body=report
    )
    
    # 4. Send notification
    sns = boto3.client('sns')
    sns.publish(
        TopicArn='arn:aws:sns:us-east-1:123456789012:inventory-alerts',
        Message=f'Daily inventory report generated: {filename}'
    )
```

### Pattern 2: Event-Driven Ingestion
**How it works**: Processing triggered by events (file uploads, database changes)

#### 🏦 Banking Example: Transaction File Processing
```
Trigger: Bank branch uploads transaction file to S3
Process:
1. S3 event notification triggers Lambda
2. Lambda validates file format
3. Starts Glue job to process transactions
4. Loads results into data warehouse
5. Sends confirmation to branch
```

#### AWS S3 Event Configuration:
```json
{
  "Rules": [{
    "Name": "ProcessTransactionFiles",
    "EventPattern": {
      "source": ["aws.s3"],
      "detail-type": ["Object Created"],
      "detail": {
        "bucket": {
          "name": ["bank-transaction-files"]
        },
        "object": {
          "key": [{
            "prefix": "daily-transactions/"
          }]
        }
      }
    },
    "Targets": [{
      "Id": "1",
      "Arn": "arn:aws:lambda:us-east-1:123456789012:function:ProcessTransactions"
    }]
  }]
}
```

### Pattern 3: Large File Transfer with Multipart Upload
**How it works**: Break large files into chunks for reliable transfer

#### 🏪 Supermarket Example: Security Camera Footage
```
Challenge: Upload 10GB of daily camera footage
Solution: Multipart upload
1. Split 10GB file into 100MB chunks
2. Upload chunks in parallel
3. Combine chunks in S3
4. Verify integrity with checksums
```

## 🚦 Managing Throttling and Rate Limits

### Understanding Rate Limits
Different AWS services have different speed limits:

| Service | Default Limit | What Happens When Exceeded |
|---------|---------------|----------------------------|
| **S3 PUT requests** | 3,500/second | 503 SlowDown errors |
| **DynamoDB writes** | 1,000 WCU/table | ProvisionedThroughputExceededException |
| **Lambda invocations** | 1,000 concurrent | TooManyRequestsException |
| **RDS connections** | Database-dependent | Connection timeout |

### 🏦 Banking Example: Handling High Volume
```python
import boto3
import time
from botocore.exceptions import ClientError

def upload_with_retry(s3_client, bucket, key, data, max_retries=3):
    """Upload with exponential backoff retry"""
    
    for attempt in range(max_retries):
        try:
            s3_client.put_object(
                Bucket=bucket,
                Key=key,
                Body=data
            )
            return True
            
        except ClientError as e:
            if e.response['Error']['Code'] == 'SlowDown':
                # S3 is throttling us, wait and retry
                wait_time = (2 ** attempt)  # Exponential backoff
                print(f"Rate limited, waiting {wait_time} seconds...")
                time.sleep(wait_time)
            else:
                raise e
    
    return False

# Process batch of transaction files
for transaction_file in transaction_files:
    success = upload_with_retry(
        s3_client, 
        'bank-transactions', 
        f'processed/{transaction_file.name}',
        transaction_file.data
    )
    
    if not success:
        # Send to dead letter queue for later processing
        send_to_dlq(transaction_file)
```

## 🛡️ Error Handling and Reliability

### Dead Letter Queues (DLQ)
**Think**: Lost and found for failed data processing

#### 🏪 Supermarket Example:
```
Problem: Daily sales file is corrupted
Solution: DLQ workflow
1. Processing fails due to bad data
2. File goes to DLQ instead of being lost
3. Alert sent to IT team
4. Manual investigation and reprocessing
5. Fixed data continues through pipeline
```

### Retry Mechanisms
```python
def process_sales_file(file_path, max_retries=3):
    """Process sales file with retry logic"""
    
    for attempt in range(max_retries):
        try:
            # Attempt to process file
            data = read_sales_file(file_path)
            validate_data(data)
            transform_data(data)
            load_to_warehouse(data)
            
            # Success!
            return True
            
        except ValidationError as e:
            # Data quality issue - don't retry
            send_to_manual_review(file_path, str(e))
            return False
            
        except TransientError as e:
            # Network/system issue - retry with backoff
            if attempt < max_retries - 1:
                wait_time = (2 ** attempt) * 60  # 1, 2, 4 minutes
                time.sleep(wait_time)
            else:
                send_to_dlq(file_path, str(e))
                return False
```

## 📋 Best Practices for Batch Ingestion

### 1. File Organization in S3
```
Recommended structure:
s3://company-data-lake/
├── raw/                    # Incoming data
│   ├── sales/
│   │   ├── year=2024/
│   │   │   ├── month=10/
│   │   │   │   ├── day=01/
│   │   │   │   │   └── store-001.csv
├── processed/             # Clean, validated data
├── failed/               # Processing failures
└── archive/              # Long-term storage
```

### 2. Monitoring and Alerting
```python
# CloudWatch metrics for monitoring
cloudwatch = boto3.client('cloudwatch')

# Track successful file processing
cloudwatch.put_metric_data(
    Namespace='Batch-Processing',
    MetricData=[{
        'MetricName': 'FilesProcessed',
        'Value': files_processed,
        'Unit': 'Count',
        'Dimensions': [
            {'Name': 'DataType', 'Value': 'sales'},
            {'Name': 'Store', 'Value': store_id}
        ]
    }]
)

# Alert on processing delays
if processing_time > threshold:
    sns.publish(
        TopicArn='arn:aws:sns:us-east-1:123456789012:processing-alerts',
        Subject='Batch Processing Delayed',
        Message=f'Sales processing took {processing_time} minutes (threshold: {threshold})'
    )
```

### 3. Cost Optimization
- **Use S3 Intelligent Tiering**: Automatically move old data to cheaper storage
- **Schedule Glue jobs during off-peak hours**: Lower compute costs
- **Right-size EMR clusters**: Don't over-provision resources
- **Use Spot instances**: Save up to 90% on compute costs

## 🔍 Real-World Implementation Example

### 🏪 Complete Supermarket Daily Sales Processing Pipeline

```python
"""
Daily Sales Processing Pipeline
Runs every night at 2 AM to process the day's sales data
"""

import boto3
import pandas as pd
from datetime import datetime, timedelta

def lambda_handler(event, context):
    """Main function triggered by EventBridge"""
    
    try:
        # 1. List all sales files from today
        s3 = boto3.client('s3')
        today = datetime.now().strftime('%Y/%m/%d')
        
        files = s3.list_objects_v2(
            Bucket='supermarket-raw-data',
            Prefix=f'sales/{today}/'
        )
        
        if 'Contents' not in files:
            send_alert("No sales files found for today")
            return
        
        # 2. Start Glue job to process files
        glue = boto3.client('glue')
        job_run = glue.start_job_run(
            JobName='process-daily-sales',
            Arguments={
                '--input-path': f's3://supermarket-raw-data/sales/{today}/',
                '--output-path': f's3://supermarket-processed-data/sales/{today}/',
                '--job-date': today
            }
        )
        
        # 3. Monitor job completion
        monitor_glue_job(job_run['JobRunId'])
        
        # 4. Generate business reports
        generate_daily_reports(today)
        
        # 5. Send success notification
        send_success_notification(today, len(files['Contents']))
        
    except Exception as e:
        send_error_notification(str(e))
        raise

def monitor_glue_job(job_run_id):
    """Monitor Glue job and handle completion"""
    glue = boto3.client('glue')
    
    while True:
        response = glue.get_job_run(JobName='process-daily-sales', RunId=job_run_id)
        status = response['JobRun']['JobRunState']
        
        if status == 'SUCCEEDED':
            break
        elif status in ['FAILED', 'ERROR', 'TIMEOUT']:
            raise Exception(f"Glue job failed with status: {status}")
        
        time.sleep(30)  # Check every 30 seconds
```

## 📊 Monitoring Batch Ingestion

### Key Metrics to Track:
1. **Processing Time**: How long does each batch take?
2. **Success Rate**: Percentage of successful file processing
3. **Data Volume**: Amount of data processed daily
4. **Error Rate**: Failed processing attempts
5. **Cost per Batch**: Resource utilization costs

### Sample CloudWatch Dashboard:
```json
{
  "widgets": [
    {
      "type": "metric",
      "properties": {
        "metrics": [
          ["Batch-Processing", "FilesProcessed", "DataType", "sales"],
          [".", "ProcessingTime", ".", "."],
          [".", "ErrorCount", ".", "."]
        ],
        "period": 86400,
        "stat": "Sum",
        "region": "us-east-1",
        "title": "Daily Sales Processing"
      }
    }
  ]
}
```

## 📋 Summary

Batch data ingestion is perfect when:
- ✅ You can process data in chunks rather than continuously
- ✅ Some delay in processing is acceptable
- ✅ You're working with large files or datasets
- ✅ Cost optimization is important
- ✅ You need to process data from multiple sources together

Key AWS services for batch ingestion:
- **Amazon S3**: Store and organize data files
- **AWS Glue**: Transform and process data
- **Amazon EventBridge**: Schedule and trigger processing
- **AWS Lambda**: Lightweight processing and orchestration
- **AWS DMS**: Database migration and replication

Remember: The goal is to build reliable, cost-effective pipelines that process your business data accurately and on time.

## 🔗 Next Steps

Now that you understand batch ingestion, let's explore how to handle scenarios where you need to move entire databases while keeping track of every change with [Lesson 4: Database Migration and Change Data Capture](04-database-migration-cdc.md).

---

**Next**: [Lesson 4: Database Migration and Change Data Capture](04-database-migration-cdc.md)  
**Previous**: [Lesson 2: Streaming Data Ingestion with Amazon Kinesis](02-streaming-data-kinesis.md)