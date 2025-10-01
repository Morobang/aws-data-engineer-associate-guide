# Task Statement 1.1: Perform Data Ingestion

## 🎯 Official Exam Scope

This task statement covers 34% of Domain 1 content and focuses on how data engineers collect, import, and initially process data from various sources into AWS systems.

## 📚 Knowledge Areas

### 1. Throughput and Latency Characteristics for AWS Services

#### 🏪 Understanding with Supermarket Examples

**Throughput** = How much data you can process at once
**Latency** = How fast you can process individual pieces of data

| Scenario | Throughput Need | Latency Need | AWS Service Choice |
|----------|----------------|--------------|-------------------|
| **Black Friday Checkout** | VERY HIGH (1000s transactions/sec) | MEDIUM (few seconds OK) | Kinesis Data Streams |
| **Credit Card Fraud** | MEDIUM (100s transactions/sec) | VERY LOW (milliseconds) | Kinesis + Lambda |
| **Daily Inventory Count** | LOW (once per day) | HIGH (hours OK) | S3 + Glue |
| **Price Change Updates** | MEDIUM (hundreds/minute) | LOW (seconds) | Kinesis Data Firehose |

#### AWS Service Throughput & Latency Characteristics

```
Amazon Kinesis Data Streams:
✅ Throughput: Up to 1,000 records/second per shard
✅ Latency: Sub-second to few seconds
✅ Use for: Real-time credit card processing, live inventory tracking

Amazon Kinesis Data Firehose:
✅ Throughput: Scales automatically
✅ Latency: 60 seconds to 15 minutes (buffering)
✅ Use for: Loading data to S3/Redshift, not real-time needs

AWS Glue:
✅ Throughput: Processes GBs to TBs (scales with workers)
✅ Latency: Minutes to hours (batch processing)
✅ Use for: Nightly data transformations, large dataset processing

Amazon S3:
✅ Throughput: 3,500 PUT/second, 5,500 GET/second per prefix
✅ Latency: 100-200 milliseconds per request
✅ Use for: Data lake storage, batch file processing
```

### 2. Data Ingestion Patterns

#### 🏦 Banking Examples of Ingestion Patterns

**Pattern 1: Frequency-Based Ingestion**
```
Real-time (milliseconds): Credit card authorization
Near real-time (seconds): Account balance updates  
Micro-batch (minutes): ATM transaction logs
Batch (hours): Daily interest calculations
Monthly (days): Customer statements
```

**Pattern 2: Data History Patterns**
```
Full Historical Load: 
- Migrating 10 years of customer transaction history
- AWS Service: DMS with full load

Incremental Load:
- Only new transactions since last load
- AWS Service: DMS with CDC or Glue job bookmarks

Change Data Capture (CDC):
- Track every account balance change
- AWS Service: DMS CDC or DynamoDB Streams
```

### 3. Streaming Data Ingestion

#### 🏪 Supermarket Streaming Examples

**Real-time Inventory Tracking**:
```python
# Every product scan creates a stream record
{
    "store_id": "store-001",
    "product_id": "milk-001", 
    "quantity_sold": 1,
    "timestamp": "2024-10-01T10:30:00Z",
    "register_id": "pos-01"
}

# Kinesis processes these records continuously
# Updates inventory in real-time
# Triggers reorder alerts when stock low
```

**Customer Behavior Streaming**:
```python
# Track customer movement through store
{
    "customer_id": "cust-12345",
    "loyalty_card": "scan",
    "location": "dairy-section", 
    "timestamp": "2024-10-01T10:31:00Z",
    "duration_seconds": 45
}
```

### 4. Batch Data Ingestion

#### 🏦 Banking Batch Processing Examples

**End-of-Day Processing**:
```python
# All branch transactions collected at day end
daily_batch = {
    "processing_date": "2024-10-01",
    "branch_transactions": [
        {"account": "12345", "amount": 100.00, "type": "deposit"},
        {"account": "67890", "amount": -50.00, "type": "withdrawal"},
        # ... thousands more transactions
    ],
    "total_transactions": 15420,
    "total_amount": 2150000.00
}

# Scheduled processing patterns:
# - EventBridge triggers at 11:59 PM
# - Glue job processes all branch files
# - Results loaded to Redshift data warehouse
```

### 5. Replayability of Data Ingestion Pipelines

#### 🏪 Why Replayability Matters: Store System Crash

```
Scenario: Point-of-sale system crashes during busy Saturday

Without Replayability:
❌ Lost sales data from 10 AM - 2 PM crash
❌ Inventory counts incorrect  
❌ Customer loyalty points missing
❌ Revenue reports wrong

With Replayability:
✅ All sales data stored in Kinesis (24-hour retention)
✅ Replay processing from 10 AM crash point
✅ Inventory and loyalty points corrected
✅ Revenue reports accurate
```

**AWS Implementation**:
```python
# Kinesis Data Streams with extended retention
kinesis_stream = {
    "StreamName": "store-pos-transactions",
    "RetentionPeriod": 168,  # 7 days (max 365)
    "ShardCount": 10
}

# Glue jobs with bookmarks for replayability
glue_job = {
    "Name": "process-store-transactions",
    "DefaultArguments": {
        "--job-bookmark-option": "job-bookmark-enable",
        "--enable-continuous-cloudwatch-log": "true"
    }
}
```

### 6. Stateful vs Stateless Data Transactions

#### 🏦 Banking Account Balance (Stateful)

```python
# Account balance depends on previous transactions (STATE)
class BankAccount:
    def __init__(self, account_id, initial_balance):
        self.account_id = account_id
        self.balance = initial_balance  # Current STATE
    
    def process_transaction(self, amount, transaction_type):
        # Each transaction depends on current state
        if transaction_type == "withdrawal":
            if self.balance >= amount:  # Check current STATE
                self.balance -= amount  # Update STATE
                return {"status": "success", "new_balance": self.balance}
            else:
                return {"status": "insufficient_funds", "balance": self.balance}
        
        elif transaction_type == "deposit":
            self.balance += amount  # Update STATE
            return {"status": "success", "new_balance": self.balance}

# AWS Implementation: DynamoDB for state management
account_table = {
    "account_id": "12345",
    "current_balance": 1500.00,  # State stored in database
    "last_transaction_id": "txn-999"
}
```

#### 🏪 Product Price Lookup (Stateless)

```python
# Each price lookup is independent (NO STATE)
def get_product_price(product_id):
    # This function doesn't depend on previous calls
    price_catalog = {
        "milk-001": 3.99,
        "bread-002": 2.49, 
        "apple-003": 0.99
    }
    return price_catalog.get(product_id, 0.00)

# Each call is independent:
price1 = get_product_price("milk-001")  # Returns 3.99
price2 = get_product_price("bread-002") # Returns 2.49
# Order doesn't matter, no state between calls

# AWS Implementation: Lambda function
def lambda_handler(event, context):
    product_id = event['product_id']
    # Stateless - no memory of previous invocations
    return get_price_from_catalog(product_id)
```

## 🛠️ Skills Implementation

### 1. Reading Data from Streaming Sources

#### Amazon Kinesis Data Streams

**🏦 Banking Transaction Processing**:
```python
import boto3
import json

def process_banking_transactions():
    kinesis = boto3.client('kinesis')
    
    # Read from transaction stream
    response = kinesis.get_records(
        ShardIterator=shard_iterator,
        Limit=100
    )
    
    for record in response['Records']:
        transaction_data = json.loads(record['Data'])
        
        # Real-time fraud detection
        if detect_fraud(transaction_data):
            alert_security_team(transaction_data)
            block_transaction(transaction_data['card_number'])
        
        # Update account balance
        update_account_balance(
            transaction_data['account_id'],
            transaction_data['amount']
        )
```

#### Amazon MSK (Managed Streaming for Apache Kafka)

**🏪 Multi-Store Data Collection**:
```python
from kafka import KafkaConsumer
import json

# MSK consumer for store data
consumer = KafkaConsumer(
    'store-transactions',
    bootstrap_servers=['msk-cluster.amazonaws.com:9092'],
    value_deserializer=lambda x: json.loads(x.decode('utf-8'))
)

for message in consumer:
    store_data = message.value
    
    # Process transaction from any store in chain
    update_inventory(store_data['product_id'], store_data['quantity'])
    update_loyalty_points(store_data['customer_id'], store_data['amount'])
    trigger_restock_if_needed(store_data['product_id'])
```

#### DynamoDB Streams

**🏦 Account Change Tracking**:
```python
def lambda_handler(event, context):
    """Process DynamoDB stream records for account changes"""
    
    for record in event['Records']:
        if record['eventName'] == 'MODIFY':
            # Account balance changed
            old_balance = record['dynamodb']['OldImage']['balance']['N']
            new_balance = record['dynamodb']['NewImage']['balance']['N']
            account_id = record['dynamodb']['Keys']['account_id']['S']
            
            # Check for large withdrawals (potential fraud)
            if float(old_balance) - float(new_balance) > 5000:
                send_fraud_alert(account_id, old_balance, new_balance)
            
            # Update data warehouse
            update_data_warehouse(account_id, new_balance)
```

### 2. Reading Data from Batch Sources

#### Amazon S3 with AWS Glue

**🏪 Daily Sales Report Processing**:
```python
# Glue job to process daily sales files
from awsglue.context import GlueContext
from awsglue.dynamicframe import DynamicFrame
from pyspark.context import SparkContext

sc = SparkContext()
glueContext = GlueContext(sc)

# Read daily sales files from S3
sales_data = glueContext.create_dynamic_frame.from_options(
    connection_type="s3",
    connection_options={
        "paths": ["s3://supermarket-data/daily-sales/2024/10/01/"]
    },
    format="csv",
    format_options={
        "withHeader": True,
        "separator": ","
    }
)

# Transform and aggregate
daily_summary = sales_data.apply_mapping([
    ("store_id", "string", "store", "string"),
    ("product_id", "string", "product", "string"), 
    ("quantity", "int", "qty_sold", "int"),
    ("amount", "double", "revenue", "double")
])

# Write to data warehouse
glueContext.write_dynamic_frame.from_jdbc_conf(
    frame=daily_summary,
    catalog_connection="redshift-connection",
    connection_options={"dbtable": "daily_sales_summary"}
)
```

#### AWS Lambda for Small File Processing

**🏦 Branch Report Processing**:
```python
import boto3
import pandas as pd

def lambda_handler(event, context):
    """Process small branch report files"""
    
    s3 = boto3.client('s3')
    
    # Triggered by S3 event when branch uploads report
    bucket = event['Records'][0]['s3']['bucket']['name']
    key = event['Records'][0]['s3']['object']['key']
    
    # Download and process file
    obj = s3.get_object(Bucket=bucket, Key=key)
    df = pd.read_csv(obj['Body'])
    
    # Basic validation and transformation
    df['processing_date'] = pd.Timestamp.now()
    df['branch_id'] = key.split('/')[1]  # Extract from file path
    
    # Upload processed data
    processed_key = f"processed/{key}"
    df.to_csv(f"/tmp/processed.csv", index=False)
    
    s3.upload_file("/tmp/processed.csv", bucket, processed_key)
    
    return {"status": "success", "processed_records": len(df)}
```

### 3. Setting Up Schedulers

#### Amazon EventBridge Scheduling

**🏪 Daily Inventory Check**:
```json
{
    "Rules": [{
        "Name": "DailyInventoryCheck",
        "ScheduleExpression": "cron(0 2 * * ? *)",
        "Description": "Run inventory check at 2 AM daily",
        "State": "ENABLED",
        "Targets": [{
            "Id": "1",
            "Arn": "arn:aws:glue:us-east-1:123456789012:job/inventory-check",
            "RoleArn": "arn:aws:iam::123456789012:role/EventBridgeRole",
            "Input": "{\"date\": \"${aws.events.event.ingestion-time}\"}"
        }]
    }]
}
```

#### Time-based Glue Crawler Schedule

```python
import boto3

glue = boto3.client('glue')

# Schedule crawler to run every Sunday at midnight
response = glue.create_crawler(
    Name='weekly-sales-data-crawler',
    Role='arn:aws:iam::123456789012:role/GlueServiceRole',
    DatabaseName='supermarket-data',
    Targets={
        'S3Targets': [
            {
                'Path': 's3://supermarket-data/weekly-sales/',
                'Exclusions': ['temp/*', '*.tmp']
            }
        ]
    },
    Schedule='cron(0 0 ? * SUN *)'  # Every Sunday at midnight
)
```

### 4. Event Triggers

#### S3 Event Notifications

**🏦 Automatic Transaction File Processing**:
```json
{
    "Rules": [{
        "Id": "ProcessTransactionFiles",
        "Filter": {
            "Key": {
                "FilterRules": [{
                    "Name": "prefix",
                    "Value": "incoming/transactions/"
                }, {
                    "Name": "suffix", 
                    "Value": ".csv"
                }]
            }
        },
        "Status": "Enabled",
        "Configuration": {
            "LambdaConfiguration": {
                "LambdaFunctionArn": "arn:aws:lambda:us-east-1:123456789012:function:ProcessTransactions",
                "Events": ["s3:ObjectCreated:*"]
            }
        }
    }]
}
```

### 5. Lambda Integration with Kinesis

**🏪 Real-time Inventory Updates**:
```python
def lambda_handler(event, context):
    """Process Kinesis records for inventory updates"""
    
    dynamodb = boto3.resource('dynamodb')
    inventory_table = dynamodb.Table('store-inventory')
    
    for record in event['Records']:
        # Decode Kinesis data
        payload = json.loads(
            base64.b64decode(record['kinesis']['data']).decode('utf-8')
        )
        
        product_id = payload['product_id']
        quantity_sold = payload['quantity_sold']
        store_id = payload['store_id']
        
        # Update inventory in real-time
        try:
            response = inventory_table.update_item(
                Key={'product_id': product_id, 'store_id': store_id},
                UpdateExpression='ADD quantity_available :qty',
                ExpressionAttributeValues={':qty': -quantity_sold},
                ReturnValues='UPDATED_NEW'
            )
            
            # Check if restock needed
            new_quantity = response['Attributes']['quantity_available']
            if new_quantity < 10:  # Restock threshold
                send_restock_alert(product_id, store_id, new_quantity)
                
        except Exception as e:
            # Handle errors gracefully
            send_to_dlq(record, str(e))
    
    return {'statusCode': 200, 'processed': len(event['Records'])}
```

### 6. Managing Throttling and Rate Limits

#### DynamoDB Throttling Management

**🏦 High-Volume Transaction Processing**:
```python
import boto3
import time
from botocore.exceptions import ClientError

def write_transaction_with_retry(transaction_data, max_retries=3):
    """Write to DynamoDB with exponential backoff"""
    
    dynamodb = boto3.resource('dynamodb')
    table = dynamodb.Table('bank-transactions')
    
    for attempt in range(max_retries):
        try:
            table.put_item(Item=transaction_data)
            return True
            
        except ClientError as e:
            if e.response['Error']['Code'] == 'ProvisionedThroughputExceededException':
                # DynamoDB is throttling - wait and retry
                wait_time = (2 ** attempt) + random.uniform(0, 1)
                print(f"Throttled, waiting {wait_time:.2f} seconds...")
                time.sleep(wait_time)
            else:
                raise e
    
    # If all retries failed, send to DLQ
    send_to_dlq(transaction_data)
    return False
```

#### Kinesis Throttling Management

```python
def put_record_with_retry(kinesis_client, stream_name, data, partition_key):
    """Put record to Kinesis with retry logic"""
    
    max_retries = 3
    for attempt in range(max_retries):
        try:
            response = kinesis_client.put_record(
                StreamName=stream_name,
                Data=json.dumps(data),
                PartitionKey=partition_key
            )
            return response
            
        except ClientError as e:
            if 'ProvisionedThroughputExceededException' in str(e):
                # Shard is hot - implement backoff
                time.sleep((2 ** attempt) * 0.1)  # 0.1, 0.2, 0.4 seconds
            else:
                raise e
    
    raise Exception(f"Failed to put record after {max_retries} attempts")
```

### 7. Fan-in and Fan-out Patterns

#### Fan-in: Multiple Sources to Single Destination

**🏪 Collecting Data from All Store Locations**:
```python
# Multiple stores sending data to central Kinesis stream
stores = ['store-001', 'store-002', 'store-003', 'store-004']

for store_id in stores:
    # Each store sends sales data
    sales_data = {
        'store_id': store_id,
        'timestamp': datetime.now().isoformat(),
        'daily_sales': get_store_sales(store_id),
        'inventory_status': get_inventory_status(store_id)
    }
    
    # All go to same central stream (FAN-IN)
    kinesis_client.put_record(
        StreamName='central-retail-stream',
        Data=json.dumps(sales_data),
        PartitionKey=store_id
    )
```

#### Fan-out: Single Source to Multiple Destinations

**🏦 Broadcasting Transaction Updates**:
```python
def process_transaction(transaction_data):
    """Single transaction fans out to multiple systems"""
    
    # Original transaction record
    account_id = transaction_data['account_id']
    amount = transaction_data['amount']
    
    # FAN-OUT to multiple destinations:
    
    # 1. Update account balance (DynamoDB)
    update_account_balance(account_id, amount)
    
    # 2. Fraud detection system (Kinesis Analytics)
    send_to_fraud_detection(transaction_data)
    
    # 3. Customer notification (SNS)
    send_customer_notification(account_id, transaction_data)
    
    # 4. Regulatory reporting (S3)
    store_for_compliance(transaction_data)
    
    # 5. Analytics warehouse (Kinesis Firehose → Redshift)
    send_to_analytics(transaction_data)
```

#### Enhanced Fan-out with Kinesis

```python
# Kinesis Enhanced Fan-out for dedicated throughput
def setup_enhanced_fanout():
    kinesis = boto3.client('kinesis')
    
    # Create dedicated consumer for fraud detection
    fraud_consumer = kinesis.register_stream_consumer(
        StreamARN='arn:aws:kinesis:us-east-1:123456789012:stream/bank-transactions',
        ConsumerName='fraud-detection-consumer'
    )
    
    # Create dedicated consumer for analytics
    analytics_consumer = kinesis.register_stream_consumer(
        StreamARN='arn:aws:kinesis:us-east-1:123456789012:stream/bank-transactions',
        ConsumerName='analytics-consumer'
    )
    
    # Each consumer gets dedicated 2 MB/sec throughput per shard
    return fraud_consumer, analytics_consumer
```

## 📋 Summary

Task Statement 1.1 covers the foundational skills for getting data into AWS systems:

**Key Concepts**:
- **Throughput vs Latency**: Choose services based on speed and volume needs
- **Streaming vs Batch**: Real-time for immediate action, batch for efficiency
- **Stateful vs Stateless**: Understanding transaction dependencies
- **Replayability**: Building systems that can recover from failures

**Critical AWS Services**:
- **Kinesis**: Real-time streaming (fraud detection, live inventory)
- **S3 + Glue**: Batch processing (daily reports, data lakes)
- **DMS**: Database migration and change tracking
- **Lambda**: Event-driven processing (file uploads, small datasets)
- **EventBridge**: Scheduling and event routing

**Real-world Applications**:
- Credit card transaction processing
- Retail inventory management
- Banking system migrations
- Multi-store data consolidation

Mastering these concepts enables you to design robust, scalable data ingestion solutions that meet business requirements while handling the complexities of real-world data systems.

---

**Next**: [Task Statement 1.2: Transform and Process Data](task-1-2-transform-process-data.md)  
**Previous**: [Domain 1 Overview](README.md)