# Task Statement 1.2: Transform and Process Data

## 🎯 Official Exam Scope

This task statement focuses on the "T" in ETL - how data engineers clean, convert, structure, and prepare raw data for analysis and business use.

## 📚 Knowledge Areas

### 1. Creation of ETL Pipelines Based on Business Requirements

#### 🏪 Supermarket ETL Example: Daily Sales Analysis

**Business Requirement**: "We need to know which products are selling best in each store to optimize inventory and identify trends."

**Raw Data Sources**:
```
Point-of-Sale Systems (CSV files):
store_id,register_id,product_barcode,quantity,price,timestamp,cashier_id
001,POS01,123456789012,2,3.99,2024-10-01T10:30:15Z,emp001

Inventory System (JSON):
{
  "product_id": "123456789012",
  "product_name": "Organic Milk 1 Gallon", 
  "category": "Dairy",
  "supplier": "Local Dairy Co",
  "cost": 2.50
}

Customer Database (PostgreSQL):
customer_id | loyalty_card | first_name | last_name | signup_date
```

**ETL Pipeline Design**:
```python
# EXTRACT
raw_sales = extract_from_pos_systems()      # CSV files from S3
inventory = extract_from_inventory_db()     # JSON from DynamoDB  
customers = extract_from_customer_db()      # PostgreSQL via JDBC

# TRANSFORM
def transform_sales_data(raw_sales, inventory, customers):
    # 1. Clean and validate data
    cleaned_sales = remove_invalid_transactions(raw_sales)
    
    # 2. Enrich with product information
    enriched_sales = join_with_inventory(cleaned_sales, inventory)
    
    # 3. Add customer information
    complete_sales = join_with_customers(enriched_sales, customers)
    
    # 4. Calculate business metrics
    final_data = calculate_metrics(complete_sales)
    
    return final_data

# LOAD
load_to_redshift(transformed_data)          # Data warehouse
load_to_s3(transformed_data)                # Data lake
```

#### 🏦 Banking ETL Example: Risk Assessment Pipeline

**Business Requirement**: "We need daily risk reports for loan portfolio management and regulatory compliance."

```python
# ETL Pipeline for Risk Assessment
def create_risk_assessment_pipeline():
    
    # EXTRACT from multiple sources
    loan_data = extract_from_core_banking()     # Loan details
    payment_history = extract_from_payments()   # Payment records  
    credit_scores = extract_from_bureaus()      # External credit data
    economic_data = extract_from_fed_api()      # Economic indicators
    
    # TRANSFORM with business logic
    risk_metrics = calculate_risk_scores(
        loans=loan_data,
        payments=payment_history,
        credit=credit_scores,
        economic=economic_data
    )
    
    # Apply regulatory calculations
    regulatory_metrics = apply_basel_iii_rules(risk_metrics)
    
    # LOAD to reporting systems
    load_to_regulatory_db(regulatory_metrics)
    load_to_risk_dashboard(risk_metrics)
    
    return "Risk assessment pipeline completed"
```

### 2. Volume, Velocity, and Variety of Data (The 3 V's)

#### 🏪 Supermarket Chain: 3 V's Example

**Volume** - How much data?
```
Daily Data Volumes:
- 500 stores × 12 hours × 100 transactions/hour = 600,000 transactions/day
- Each transaction: ~500 bytes
- Daily total: ~300 MB of transaction data
- Plus: Images, videos, sensor data = ~10 GB/day total
- Annual: ~3.6 TB of data

AWS Solution: Use S3 for storage, EMR for processing large volumes
```

**Velocity** - How fast does data arrive?
```
Peak Shopping Hours (Black Friday):
- 1,000 transactions/second across all stores  
- Real-time inventory updates needed
- Fraud detection in <100ms
- Price updates propagated in <30 seconds

AWS Solution: Kinesis Data Streams for real-time, Lambda for processing
```

**Variety** - What types of data?
```
Structured Data:
- Transaction records (rows/columns)
- Customer database (relational)
- Inventory counts (numerical)

Semi-Structured Data:  
- Product catalogs (JSON/XML)
- Web logs (key-value pairs)
- API responses (nested JSON)

Unstructured Data:
- Security camera footage (video)
- Customer reviews (text)
- Receipt images (scanned documents)

AWS Solution: Glue for structured/semi-structured, Rekognition for images, Comprehend for text
```

#### 🏦 Banking 3 V's Example

```python
# Banking data characteristics
banking_data_profile = {
    "volume": {
        "daily_transactions": 50_000_000,      # 50M transactions/day
        "customer_records": 25_000_000,        # 25M customers
        "historical_data": "10_years",         # Decade of history
        "storage_needs": "100_TB_annually"     # Massive storage
    },
    
    "velocity": {
        "credit_card_auth": "50ms",            # Must be instant
        "fraud_detection": "100ms",            # Real-time alerts
        "account_updates": "1_second",         # Near real-time
        "batch_reports": "overnight"           # Can wait hours
    },
    
    "variety": {
        "structured": [
            "account_balances",     # Financial data
            "transaction_logs",     # Payment records
            "customer_profiles"     # Personal information
        ],
        "semi_structured": [
            "atm_logs",            # JSON from ATM machines
            "mobile_app_data",     # User interaction logs
            "api_responses"        # Third-party data feeds
        ],
        "unstructured": [
            "check_images",        # Scanned documents
            "customer_emails",     # Support communications
            "call_recordings"      # Audio files
        ]
    }
}
```

### 3. Cloud Computing and Distributed Computing

#### 🏪 Why Distributed Computing for Retail?

**Problem**: Single computer can't handle Black Friday data volume
```
Black Friday Challenge:
- 1000 stores × 500 transactions/hour × 12 hours = 6 million transactions
- Each transaction needs validation, enrichment, and storage
- Single computer: 12+ hours to process
- Business need: Results in 1 hour maximum
```

**Solution**: Distribute work across multiple computers
```python
# Distributed processing with Apache Spark on EMR
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("BlackFridayProcessing") \
    .config("spark.executor.instances", "20") \
    .config("spark.executor.memory", "8g") \
    .getOrCreate()

# Read data from S3 (distributed across cluster)
transactions = spark.read.csv("s3://retail-data/black-friday/")

# Process in parallel across 20 machines
processed_transactions = transactions \
    .filter(col("amount") > 0) \
    .join(product_catalog, "product_id") \
    .groupBy("store_id", "category") \
    .agg(sum("amount").alias("total_sales"))

# Results ready in 30 minutes instead of 12 hours!
processed_transactions.write.mode("overwrite").parquet("s3://processed-data/")
```

#### 🏦 Banking Distributed Computing Example

```python
# Monthly interest calculation for 25 million accounts
def distributed_interest_calculation():
    
    # Problem: Single machine would take 15 hours
    # Solution: Distribute across EMR cluster
    
    spark = SparkSession.builder \
        .appName("MonthlyInterestCalculation") \
        .config("spark.executor.instances", "50") \
        .getOrCreate()
    
    # Load account data (distributed across nodes)
    accounts = spark.read.jdbc(
        url="jdbc:postgresql://bank-db:5432/accounts",
        table="customer_accounts",
        numPartitions=50  # Split across 50 partitions
    )
    
    # Calculate interest in parallel
    interest_calculations = accounts \
        .filter(col("account_type") == "savings") \
        .withColumn("monthly_interest", 
                   col("balance") * col("interest_rate") / 12) \
        .withColumn("new_balance", 
                   col("balance") + col("monthly_interest"))
    
    # Write results back (distributed write)
    interest_calculations.write \
        .mode("overwrite") \
        .jdbc(url="jdbc:postgresql://bank-db:5432/accounts",
              table="monthly_interest_updates")
    
    # Completed in 45 minutes instead of 15 hours!
```

### 4. Apache Spark for Data Processing

#### 🏪 Supermarket Customer Segmentation with Spark

```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import *
from pyspark.ml.feature import VectorAssembler
from pyspark.ml.clustering import KMeans

def customer_segmentation_pipeline():
    spark = SparkSession.builder.appName("CustomerSegmentation").getOrCreate()
    
    # Load customer transaction history
    transactions = spark.read.parquet("s3://retail-data/transactions/")
    
    # Calculate customer metrics
    customer_metrics = transactions.groupBy("customer_id").agg(
        sum("amount").alias("total_spent"),
        count("transaction_id").alias("visit_frequency"),
        avg("amount").alias("avg_transaction"),
        countDistinct("product_category").alias("category_diversity"),
        datediff(max("transaction_date"), min("transaction_date")).alias("customer_lifetime")
    )
    
    # Prepare features for machine learning
    assembler = VectorAssembler(
        inputCols=["total_spent", "visit_frequency", "avg_transaction", "category_diversity"],
        outputCol="features"
    )
    
    feature_data = assembler.transform(customer_metrics)
    
    # Apply K-means clustering to segment customers
    kmeans = KMeans(k=5, seed=1)  # 5 customer segments
    model = kmeans.fit(feature_data)
    
    # Generate customer segments
    segmented_customers = model.transform(feature_data)
    
    # Interpret segments for business use
    segments = segmented_customers.groupBy("prediction").agg(
        avg("total_spent").alias("avg_spending"),
        avg("visit_frequency").alias("avg_visits"),
        count("customer_id").alias("segment_size")
    )
    
    # Save results for marketing team
    segments.write.mode("overwrite").parquet("s3://analytics/customer-segments/")
    
    return "Customer segmentation completed"
```

### 5. Intermediate Data Staging Locations

#### 🏦 Banking Data Pipeline with Staging

```python
# Multi-stage data processing for regulatory compliance
def banking_etl_with_staging():
    
    # Stage 1: Raw Data Landing (S3)
    raw_data_location = "s3://bank-data-lake/raw/"
    
    # Stage 2: Validated Data (S3)  
    validated_data_location = "s3://bank-data-lake/validated/"
    
    # Stage 3: Enriched Data (S3)
    enriched_data_location = "s3://bank-data-lake/enriched/"
    
    # Stage 4: Business Ready (Redshift)
    final_location = "redshift://bank-warehouse/reporting"
    
    # Processing pipeline with checkpoints
    pipeline_stages = {
        "raw_ingestion": {
            "source": "core_banking_system",
            "destination": raw_data_location,
            "validation": "file_format_check",
            "checkpoint": True
        },
        
        "data_validation": {
            "source": raw_data_location,
            "destination": validated_data_location,  
            "transformations": [
                "remove_duplicates",
                "validate_account_numbers", 
                "check_transaction_amounts"
            ],
            "checkpoint": True
        },
        
        "data_enrichment": {
            "source": validated_data_location,
            "destination": enriched_data_location,
            "transformations": [
                "add_customer_demographics",
                "calculate_risk_scores",
                "apply_business_rules"
            ],
            "checkpoint": True
        },
        
        "final_load": {
            "source": enriched_data_location,
            "destination": final_location,
            "transformations": [
                "aggregate_daily_summaries",
                "apply_data_masking",
                "create_audit_trails"
            ]
        }
    }
    
    return pipeline_stages
```

## 🛠️ Skills Implementation

### 1. Container Optimization (EKS/ECS)

#### 🏪 Retail Data Processing with EKS

```yaml
# Kubernetes deployment for retail data processing
apiVersion: apps/v1
kind: Deployment
metadata:
  name: retail-data-processor
spec:
  replicas: 10  # Scale based on Black Friday demand
  selector:
    matchLabels:
      app: retail-processor
  template:
    metadata:
      labels:
        app: retail-processor
    spec:
      containers:
      - name: data-processor
        image: retail-analytics:latest
        resources:
          requests:
            memory: "2Gi"
            cpu: "1000m"
          limits:
            memory: "4Gi"  
            cpu: "2000m"
        env:
        - name: S3_BUCKET
          value: "retail-transaction-data"
        - name: REDSHIFT_CLUSTER
          value: "retail-analytics-cluster"
```

```python
# ECS task definition for banking batch processing
ecs_task_definition = {
    "family": "banking-batch-processor",
    "networkMode": "awsvpc",
    "requiresCompatibilities": ["FARGATE"],
    "cpu": "2048",    # 2 vCPU
    "memory": "4096", # 4 GB RAM
    "containerDefinitions": [
        {
            "name": "risk-calculator",
            "image": "bank-analytics:risk-v1.2",
            "memory": 4096,
            "environment": [
                {"name": "DB_HOST", "value": "bank-rds.amazonaws.com"},
                {"name": "BATCH_SIZE", "value": "10000"},
                {"name": "PARALLEL_WORKERS", "value": "8"}
            ],
            "logConfiguration": {
                "logDriver": "awslogs",
                "options": {
                    "awslogs-group": "/ecs/banking-batch",
                    "awslogs-region": "us-east-1"
                }
            }
        }
    ]
}
```

### 2. Connecting to Different Data Sources (JDBC/ODBC)

#### 🏦 Banking Multi-Database Connections

```python
# Glue job connecting to multiple banking systems
from awsglue.context import GlueContext
from awsglue.dynamicframe import DynamicFrame

def connect_banking_systems():
    glueContext = GlueContext(SparkContext())
    
    # Connect to Core Banking (Oracle via JDBC)
    core_banking = glueContext.create_dynamic_frame.from_options(
        connection_type="oracle",
        connection_options={
            "url": "jdbc:oracle:thin:@core-banking:1521:PROD",
            "dbtable": "accounts",
            "user": "glue_user",
            "password": "secure_password"
        }
    )
    
    # Connect to Credit Bureau (SQL Server via JDBC)  
    credit_data = glueContext.create_dynamic_frame.from_options(
        connection_type="sqlserver",
        connection_options={
            "url": "jdbc:sqlserver://credit-bureau:1433",
            "dbtable": "credit_scores",
            "user": "bureau_user",
            "password": "bureau_password"
        }
    )
    
    # Connect to Legacy System (DB2 via JDBC)
    legacy_data = glueContext.create_dynamic_frame.from_options(
        connection_type="db2",
        connection_options={
            "url": "jdbc:db2://legacy-system:50000/LEGACY",
            "dbtable": "historical_loans",
            "user": "legacy_user", 
            "password": "legacy_password"
        }
    )
    
    return core_banking, credit_data, legacy_data
```

#### 🏪 Retail System Integration

```python
# Connect to various retail systems
def integrate_retail_systems():
    
    # POS Systems (PostgreSQL)
    pos_connection = {
        "connection_type": "postgresql",
        "connection_options": {
            "url": "jdbc:postgresql://pos-db:5432/transactions",
            "dbtable": "daily_sales",
            "user": "pos_reader",
            "password": "pos_password",
            "partitionColumn": "store_id",
            "lowerBound": "1",
            "upperBound": "500", 
            "numPartitions": "10"  # Parallel reading
        }
    }
    
    # Inventory System (MySQL)
    inventory_connection = {
        "connection_type": "mysql",
        "connection_options": {
            "url": "jdbc:mysql://inventory-db:3306/products",
            "dbtable": "current_inventory",
            "user": "inventory_reader",
            "password": "inventory_password"
        }
    }
    
    # Customer Loyalty (DynamoDB via Spark connector)
    loyalty_connection = {
        "connection_type": "dynamodb",
        "connection_options": {
            "dynamodb.input.tableName": "customer_loyalty",
            "dynamodb.throughput.read.percent": "0.5"  # Don't overwhelm production
        }
    }
    
    return pos_connection, inventory_connection, loyalty_connection
```

### 3. Integrating Data from Multiple Sources

#### 🏪 Complete Retail Data Integration

```python
def integrate_retail_data_sources():
    """Combine data from all retail systems for complete customer view"""
    
    spark = SparkSession.builder.appName("RetailIntegration").getOrCreate()
    
    # Source 1: Transaction data from POS systems
    transactions = spark.read.jdbc(
        url="jdbc:postgresql://pos-db:5432/sales",
        table="transactions",
        properties={"user": "reader", "password": "password"}
    )
    
    # Source 2: Product information from inventory system
    products = spark.read.jdbc(
        url="jdbc:mysql://inventory-db:3306/catalog", 
        table="products",
        properties={"user": "reader", "password": "password"}
    )
    
    # Source 3: Customer data from CRM (Salesforce API)
    customers = spark.read.format("csv") \
        .option("header", "true") \
        .load("s3://retail-data/salesforce-export/customers.csv")
    
    # Source 4: Online behavior from web analytics (JSON)
    web_behavior = spark.read.json("s3://retail-data/web-logs/")
    
    # Source 5: Social media mentions (from third-party API)
    social_data = spark.read.format("parquet") \
        .load("s3://retail-data/social-mentions/")
    
    # INTEGRATION: Create unified customer view
    integrated_data = transactions \
        .join(products, "product_id") \
        .join(customers, "customer_id") \
        .join(web_behavior, "customer_id", "left_outer") \
        .join(social_data, "customer_id", "left_outer")
    
    # Calculate unified metrics
    customer_360 = integrated_data.groupBy("customer_id").agg(
        sum("transaction_amount").alias("total_spending"),
        countDistinct("product_id").alias("products_purchased"),
        avg("product_rating").alias("avg_rating_purchased"),
        count("web_session_id").alias("website_visits"),
        collect_list("social_sentiment").alias("social_sentiments")
    )
    
    # Save integrated data
    customer_360.write.mode("overwrite") \
        .parquet("s3://retail-analytics/customer-360-view/")
    
    return "Data integration completed"
```

### 4. Cost Optimization While Processing Data

#### 🏦 Banking Cost Optimization Strategies

```python
def optimize_banking_data_costs():
    """Implement cost optimization for banking data processing"""
    
    # Strategy 1: Use Spot Instances for EMR (up to 90% savings)
    emr_cluster_config = {
        "Name": "BankingDataProcessor",
        "Instances": {
            "MasterInstanceType": "m5.xlarge",
            "SlaveInstanceType": "m5.large", 
            "InstanceCount": 10,
            "Ec2KeyName": "banking-keypair",
            "Placement": {"AvailabilityZone": "us-east-1a"},
            "InstanceGroups": [
                {
                    "Name": "Master",
                    "Market": "ON_DEMAND",  # Keep master stable
                    "InstanceRole": "MASTER",
                    "InstanceType": "m5.xlarge",
                    "InstanceCount": 1
                },
                {
                    "Name": "Workers", 
                    "Market": "SPOT",       # Use Spot for workers (90% savings)
                    "InstanceRole": "CORE",
                    "InstanceType": "m5.large",
                    "InstanceCount": 9,
                    "BidPrice": "0.05"      # Max bid price
                }
            ]
        }
    }
    
    # Strategy 2: Process during off-peak hours
    schedule_config = {
        "processing_schedule": "cron(0 2 * * ? *)",  # 2 AM when usage is low
        "auto_shutdown": True,
        "max_runtime_hours": 4
    }
    
    # Strategy 3: Use appropriate storage classes
    s3_lifecycle_policy = {
        "Rules": [
            {
                "Status": "Enabled",
                "Filter": {"Prefix": "banking-data/raw/"},
                "Transitions": [
                    {
                        "Days": 30,
                        "StorageClass": "STANDARD_IA"  # Cheaper for infrequent access
                    },
                    {
                        "Days": 90, 
                        "StorageClass": "GLACIER"      # Archive old data
                    }
                ]
            }
        ]
    }
    
    # Strategy 4: Compress data for storage savings
    compression_settings = {
        "input_format": "csv",
        "output_format": "parquet",      # Columnar format
        "compression": "snappy",         # Fast compression
        "partition_by": ["year", "month", "day"]  # Improve query performance
    }
    
    return {
        "cluster_config": emr_cluster_config,
        "schedule": schedule_config, 
        "storage_lifecycle": s3_lifecycle_policy,
        "compression": compression_settings
    }
```

### 5. Data Format Transformations

#### 🏪 Supermarket Data Format Conversions

```python
def transform_retail_data_formats():
    """Convert various retail data formats for optimal processing"""
    
    spark = SparkSession.builder.appName("FormatTransformation").getOrCreate()
    
    # Transform 1: CSV to Parquet (faster queries)
    csv_transactions = spark.read.format("csv") \
        .option("header", "true") \
        .option("inferSchema", "true") \
        .load("s3://retail-raw/transactions/*.csv")
    
    # Optimize for analytics queries
    csv_transactions.write \
        .mode("overwrite") \
        .format("parquet") \
        .option("compression", "snappy") \
        .partitionBy("store_id", "transaction_date") \
        .save("s3://retail-processed/transactions-parquet/")
    
    # Transform 2: JSON to Delta format (with versioning)
    json_inventory = spark.read.json("s3://retail-raw/inventory/*.json")
    
    json_inventory.write \
        .format("delta") \
        .mode("overwrite") \
        .save("s3://retail-processed/inventory-delta/")
    
    # Transform 3: XML to structured format
    xml_supplier_data = spark.read \
        .format("com.databricks.spark.xml") \
        .option("rowTag", "supplier") \
        .load("s3://retail-raw/suppliers/*.xml")
    
    # Flatten nested XML structure
    flattened_suppliers = xml_supplier_data.select(
        col("supplier_id"),
        col("company_info.name").alias("company_name"),
        col("company_info.address.street").alias("street"),
        col("company_info.address.city").alias("city"),
        explode(col("products.product")).alias("product")
    )
    
    flattened_suppliers.write \
        .format("parquet") \
        .mode("overwrite") \
        .save("s3://retail-processed/suppliers-structured/")
    
    return "Format transformations completed"
```

### 6. Troubleshooting Transformation Failures

#### 🏦 Banking ETL Error Handling

```python
def robust_banking_etl():
    """Banking ETL with comprehensive error handling"""
    
    try:
        # Main ETL process
        raw_data = extract_banking_data()
        
        # Validate data quality
        validation_results = validate_data_quality(raw_data)
        if validation_results['error_rate'] > 0.05:  # More than 5% errors
            raise DataQualityException(f"High error rate: {validation_results['error_rate']}")
        
        # Transform data
        transformed_data = transform_banking_data(raw_data)
        
        # Load to warehouse
        load_results = load_to_redshift(transformed_data)
        
        # Success notification
        send_success_notification(load_results)
        
    except DataQualityException as e:
        # Data quality issue - notify data team
        handle_data_quality_error(e)
        send_to_quarantine(raw_data)
        
    except TransformationException as e:
        # Transformation logic error - notify engineering team
        handle_transformation_error(e)
        log_transformation_failure(e, raw_data)
        
    except DatabaseException as e:
        # Database connectivity issue - retry with backoff
        retry_count = 0
        while retry_count < 3:
            try:
                time.sleep(2 ** retry_count)  # Exponential backoff
                load_results = load_to_redshift(transformed_data)
                break
            except DatabaseException:
                retry_count += 1
        
        if retry_count == 3:
            # All retries failed - send to dead letter queue
            send_to_dlq(transformed_data)
            alert_on_call_engineer(e)
    
    except Exception as e:
        # Unexpected error - comprehensive logging
        log_unexpected_error(e, raw_data, transformed_data)
        create_support_ticket(e)
        
    finally:
        # Always clean up resources
        cleanup_temp_files()
        update_job_status()

def validate_data_quality(data):
    """Comprehensive data validation for banking data"""
    
    validation_checks = {
        "null_account_ids": data.filter(col("account_id").isNull()).count(),
        "negative_balances": data.filter(col("balance") < 0).count(),
        "invalid_dates": data.filter(col("transaction_date") > datetime.now()).count(),
        "duplicate_transactions": data.count() - data.dropDuplicates(["transaction_id"]).count()
    }
    
    total_records = data.count()
    total_errors = sum(validation_checks.values())
    error_rate = total_errors / total_records if total_records > 0 else 0
    
    return {
        "total_records": total_records,
        "validation_checks": validation_checks,
        "error_rate": error_rate
    }
```

### 7. Creating Data APIs

#### 🏪 Retail Data API with AWS Lambda

```python
import json
import boto3
from decimal import Decimal

def lambda_handler(event, context):
    """API to provide retail analytics data to mobile app and dashboards"""
    
    # Parse API request
    http_method = event['httpMethod']
    path = event['path']
    query_params = event.get('queryStringParameters', {}) or {}
    
    try:
        if path == '/api/store-performance':
            return get_store_performance(query_params)
        elif path == '/api/product-recommendations':
            return get_product_recommendations(query_params)
        elif path == '/api/inventory-status':
            return get_inventory_status(query_params)
        else:
            return create_response(404, {"error": "API endpoint not found"})
            
    except Exception as e:
        return create_response(500, {"error": str(e)})

def get_store_performance(params):
    """Get store performance metrics"""
    
    store_id = params.get('store_id')
    date_range = params.get('date_range', '7')  # Default 7 days
    
    # Query Redshift data warehouse
    redshift = boto3.client('redshift-data')
    
    query = f"""
    SELECT 
        store_id,
        SUM(total_sales) as revenue,
        COUNT(DISTINCT customer_id) as unique_customers,
        AVG(transaction_amount) as avg_transaction
    FROM daily_store_summary 
    WHERE store_id = '{store_id}'
    AND date >= CURRENT_DATE - INTERVAL '{date_range} days'
    GROUP BY store_id
    """
    
    result = execute_redshift_query(query)
    
    return create_response(200, {
        "store_id": store_id,
        "performance_metrics": result,
        "date_range": f"Last {date_range} days"
    })

def get_product_recommendations(params):
    """Get personalized product recommendations"""
    
    customer_id = params.get('customer_id')
    
    # Query customer purchase history from DynamoDB
    dynamodb = boto3.resource('dynamodb')
    table = dynamodb.Table('customer-purchase-history')
    
    response = table.get_item(Key={'customer_id': customer_id})
    
    if 'Item' not in response:
        return create_response(404, {"error": "Customer not found"})
    
    # Get recommendations from ML model
    recommendations = get_ml_recommendations(response['Item'])
    
    return create_response(200, {
        "customer_id": customer_id,
        "recommendations": recommendations
    })

def create_response(status_code, body):
    """Create standardized API response"""
    return {
        'statusCode': status_code,
        'headers': {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*'
        },
        'body': json.dumps(body, default=decimal_default)
    }

def decimal_default(obj):
    """Handle Decimal objects in JSON serialization"""
    if isinstance(obj, Decimal):
        return float(obj)
    raise TypeError
```

## 📋 Summary

Task Statement 1.2 focuses on the core data engineering skill of transforming raw data into business-ready information:

**Key Transformation Concepts**:
- **ETL Pipeline Design**: Extract, Transform, Load based on business requirements
- **3 V's Management**: Handle Volume, Velocity, and Variety of data
- **Distributed Processing**: Use multiple computers for large-scale processing
- **Apache Spark**: Parallel processing framework for big data

**Critical AWS Services**:
- **AWS Glue**: Serverless ETL service for data transformation
- **Amazon EMR**: Managed Spark/Hadoop for big data processing
- **ECS/EKS**: Container orchestration for custom processing
- **AWS Lambda**: Serverless functions for lightweight transformations

**Real-world Applications**:
- Converting supermarket transaction logs into business intelligence
- Banking risk assessment and regulatory reporting
- Customer 360-degree view creation
- Real-time data APIs for mobile applications

**Best Practices**:
- Cost optimization through Spot instances and storage tiering
- Robust error handling and data quality validation
- Multi-source data integration
- Appropriate data format selection for performance

Mastering these transformation skills enables you to convert raw business data into actionable insights that drive decision-making.

---

**Next**: [Task Statement 1.3: Orchestrate Data Pipelines](task-1-3-orchestrate-data-pipelines.md)  
**Previous**: [Task Statement 1.1: Perform Data Ingestion](task-1-1-perform-data-ingestion.md)