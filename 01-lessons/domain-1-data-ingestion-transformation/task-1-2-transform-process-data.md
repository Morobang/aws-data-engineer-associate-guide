# Task Statement 1.2: Transform and Process Data

## � Real-World Data Transformation: The Shoprite Manufacturing Story

### From Raw Ingredients to Finished Products

Imagine walking into a Shoprite bakery at 4 AM. You see flour, eggs, milk, sugar, and yeast - these are raw ingredients. By 8 AM, these same ingredients have been transformed into fresh bread, pastries, and cakes that customers love to buy.

**This is exactly what data transformation does.**

Just like a bakery transforms raw ingredients into valuable products, data transformation takes raw information from various sources and converts it into useful insights that businesses can actually use to make decisions.

**Shoprite's Data Transformation Challenge:**

Every day, Shoprite collects massive amounts of raw data:
- **Sales transactions** in different formats from 2,900 stores
- **Inventory updates** from various supplier systems
- **Customer behavior data** from mobile apps and websites
- **Weather information** from external APIs
- **Social media mentions** and reviews
- **Delivery tracking data** from logistics partners

This raw data is like having flour, eggs, and milk sitting separately - useful ingredients, but not something customers can consume until they're properly combined and transformed.

**The Business Impact:**

Without proper data transformation, Shoprite's executives would be trying to make strategic decisions by reading through millions of individual transaction records. It would be like trying to understand customer preferences by manually reading every single receipt - technically possible, but practically impossible.

With proper data transformation, the same data becomes:
- **Executive dashboards** showing sales trends across regions
- **Inventory alerts** preventing stockouts before they happen  
- **Customer segmentation** enabling personalized marketing
- **Demand forecasting** optimizing purchasing and staffing
- **Performance metrics** identifying top and underperforming stores

## 🧠 Understanding Data Transformation: The Complete Picture

### What Is Data Transformation, Really?

Think of data transformation like a sophisticated restaurant kitchen. Raw ingredients (data) come in from many suppliers in different conditions and formats. The kitchen (transformation system) has multiple workstations where different chefs (processing services) work together to create finished dishes (business insights) that customers (business users) can actually consume and enjoy.

**The Three Fundamental Questions Every Business Faces:**

1. **What happened?** (Descriptive Analytics)
   - *Shoprite Example:* "We sold 50,000 loaves of bread last week"

2. **Why did it happen?** (Diagnostic Analytics)
   - *Shoprite Example:* "Bread sales increased 40% because of a competitor's supply shortage"

3. **What should we do about it?** (Prescriptive Analytics)
   - *Shoprite Example:* "Increase bread orders by 25% for the next two weeks and launch a promotion to capture market share"

Raw data can't answer these questions directly. It needs to be transformed, analyzed, and presented in ways that make business sense.

### The Volume, Velocity, and Variety Challenge

**Volume - The Overwhelming Scale**

Shoprite processes data at an almost incomprehensible scale:
- **5 million+ transactions daily** from POS systems
- **100,000+ inventory updates hourly** from stores and warehouses
- **1 million+ mobile app interactions daily** from customers
- **50,000+ social media mentions monthly** across platforms
- **Weather data updates every 15 minutes** for all store locations

*The Challenge:* Traditional database systems would crash trying to process this volume. It's like trying to bake bread for 40 million customers using a home kitchen oven.

*The Solution:* Distributed computing systems like Amazon EMR and Apache Spark that can process data across hundreds of computers simultaneously.

**Velocity - The Need for Speed**

Different business decisions require different response speeds:

*Real-Time (Seconds):*
- **Fraud Detection:** Credit card transactions must be validated within 2-3 seconds
- **Inventory Updates:** When someone buys the last item, the system must immediately show "out of stock"
- **Price Matching:** Competitor price changes need immediate response

*Near Real-Time (Minutes):*
- **Store Manager Alerts:** Low inventory warnings can wait 5-10 minutes
- **Customer Service Updates:** New complaints need attention within 15 minutes
- **Delivery Tracking:** Package location updates every few minutes

*Batch Processing (Hours/Days):*
- **Financial Reporting:** End-of-day reconciliation can wait until overnight
- **Customer Analytics:** Purchase pattern analysis can be done weekly
- **Regulatory Compliance:** Some reports only need monthly updates

**Variety - The Format Complexity**

Shoprite receives data in dozens of different formats:

*Structured Data (Easy to Process):*
- **Database records** from POS systems with clear columns and rows
- **CSV files** from supplier systems with predictable formats
- **XML files** from payment processors with standard schemas

*Semi-Structured Data (Moderate Complexity):*
- **JSON logs** from mobile apps with nested information
- **XML feeds** from suppliers with varying structures
- **API responses** with different field arrangements

*Unstructured Data (Highly Complex):*
- **Customer emails** and support tickets with free-form text
- **Social media posts** with images, hashtags, and emotions
- **Security camera footage** from stores
- **Audio recordings** from call center interactions

Each format requires different processing approaches, like needing different cooking methods for vegetables versus meat versus pastries.

## 📚 Knowledge Areas Explained Through Shoprite

### 1. Creation of ETL Pipelines Based on Business Requirements

**Understanding ETL: Extract, Transform, Load**

Think of ETL like Shoprite's supply chain process:

**Extract (Gathering Ingredients):**
- Collect fresh produce from farms
- Pick up manufactured goods from suppliers
- Gather imported items from ports

**Transform (Processing and Packaging):**
- Wash and package fresh vegetables
- Check expiration dates and quality
- Price items and add barcodes
- Organize products by category

**Load (Stocking Shelves):**
- Place items in appropriate store sections
- Ensure proper display and accessibility
- Update inventory systems
- Make products available to customers

**Real Shoprite ETL Pipeline Example: Customer Analytics**

**Business Requirement:** "We need to understand which customers are most likely to switch to competitors so we can target them with retention offers."

**Extract Phase:**
- **Transaction Data:** Pull purchase history from all 2,900 stores
- **Customer Service Data:** Extract complaint records and resolution times
- **App Usage Data:** Gather mobile app interaction patterns
- **Competitor Data:** Collect pricing information from external sources
- **Social Media Data:** Extract brand mentions and sentiment

**Transform Phase:**
- **Data Cleaning:** Remove duplicate records and fix formatting errors
- **Customer Scoring:** Calculate loyalty scores based on purchase frequency and value
- **Risk Assessment:** Identify customers with declining engagement
- **Segmentation:** Group customers by demographics and behavior patterns
- **Prediction Modeling:** Use machine learning to predict churn probability

**Load Phase:**
- **Marketing Database:** Load customer segments for targeted campaigns
- **CRM System:** Update customer profiles with risk scores
- **Executive Dashboard:** Load summary metrics for management review
- **Store Systems:** Provide store managers with at-risk customer lists
### 2. Cloud Computing and Distributed Computing

**Why Traditional Computing Isn't Enough**

Imagine trying to count all the money in all 2,900 Shoprite stores using just one person with a calculator. Even working 24/7, it would take months to finish, and by then the numbers would be completely outdated.

**Traditional Computing Limitations:**
- **Single Point of Failure:** If one server crashes, everything stops
- **Limited Scalability:** Can't easily handle 10x more data
- **Fixed Capacity:** Stuck with the same processing power regardless of workload
- **High Costs:** Need to buy expensive hardware for peak capacity, even if used only occasionally

**Cloud Computing Benefits for Shoprite:**

**Elastic Scaling:**
During Black Friday, Shoprite's data processing needs increase 10x. With cloud computing:
- **Automatic Scaling:** System automatically adds more processing power
- **Cost Efficiency:** Only pay for extra capacity during peak times
- **Instant Availability:** New servers available in minutes, not weeks
- **Global Reach:** Process data close to where it's generated (South Africa, Kenya, etc.)

**Distributed Computing in Action:**

**Example: Analyzing 5 Years of Sales Data**

*Traditional Approach:*
- One powerful computer processes all data sequentially
- Takes 30 days to complete analysis
- If computer crashes on day 29, start over from day 1
- Costs $50,000 in server hardware

*Distributed Approach with Amazon EMR:*
- 100 smaller computers work on different parts simultaneously
- Analysis completes in 6 hours instead of 30 days
- If one computer fails, others continue working
- Costs $200 for the 6-hour job

**Real Shoprite Distributed Computing Example:**

**Challenge:** Analyze customer purchasing patterns across all stores to optimize product placement

**Traditional Single-Computer Approach:**
- **Data Volume:** 5 million transactions × 365 days × 5 years = 9.1 billion records
- **Processing Time:** 45 days of continuous processing
- **Risk:** If processing fails on day 44, lose all work
- **Cost:** $80,000 in dedicated server hardware

**Distributed Cloud Approach:**
- **Cluster:** 200 virtual computers working in parallel
- **Processing Time:** 4 hours total
- **Resilience:** If some computers fail, others continue working
- **Cost:** $320 for the 4-hour analysis
- **Scalability:** Can instantly add more computers if needed

### 3. Apache Spark for Data Processing

**Understanding Spark Through Shoprite's Kitchen Analogy**

Imagine Shoprite's central bakery needs to prepare 1 million sandwiches for a special promotion. They have two options:

**Option 1: Traditional Sequential Processing**
- One chef makes sandwiches one at a time
- Each sandwich takes 30 seconds
- Total time: 1,000,000 × 30 seconds = 347 days

**Option 2: Spark-Style Parallel Processing**
- 1,000 chefs work simultaneously
- Each chef makes 1,000 sandwiches
- Total time: 1,000 × 30 seconds = 8.3 hours

**How Spark Transforms Shoprite's Data Processing:**

**Real Example: Daily Sales Analysis**

*Traditional Database Approach:*
- Process each store's data one at a time
- Query each table separately
- Wait for each calculation to complete before starting the next
- Total processing time: 8-12 hours overnight

*Spark Approach:*
- Process all 2,900 stores simultaneously
- Perform multiple calculations in parallel
- Keep frequently used data in memory for faster access
- Total processing time: 45 minutes

**Why Spark is Perfect for Shoprite:**

**In-Memory Processing:**
Instead of repeatedly reading data from slow disk drives, Spark keeps frequently used data in fast memory (RAM). It's like a chef keeping commonly used ingredients on the counter instead of walking to the storage room for each recipe.

**Fault Tolerance:**
If one worker fails during processing, Spark automatically reassigns that work to another worker. The job continues without starting over.

**Flexible Data Sources:**
Spark can read from databases, files, streaming sources, and APIs simultaneously, combining all the data seamlessly.

**Real Shoprite Spark Success Story:**

**Business Challenge:** Create personalized product recommendations for 40 million customers

**Data Requirements:**
- Customer purchase history (2 billion transactions)
- Product catalog information (500,000 products)
- Customer demographic data (40 million profiles)
- Seasonal and geographic preferences
- Real-time inventory levels

**Traditional Approach Would Take:**
- 15 days of processing time
- $25,000 in computing costs
- Risk of failure requiring complete restart

**Spark Solution:**
- **Processing Time:** 6 hours using 500-node Spark cluster
- **Cost:** $1,200 for the entire analysis
- **Reliability:** Automatic failure recovery
- **Results:** Personalized recommendations for every customer
- **Business Impact:** 23% increase in cross-selling revenue

### 4. Intermediate Data Staging Locations

**The Staging Area Concept**

Think of data staging like the prep area in a restaurant kitchen. Raw ingredients don't go directly from delivery trucks to customer plates. Instead, they go through a staging area where they're washed, cut, seasoned, and organized before being cooked into final dishes.

**Why Shoprite Needs Data Staging:**

**Quality Control:**
- **Raw Data Issues:** Store systems sometimes send corrupted files, missing data, or incorrect formats
- **Staging Solution:** Check and clean all data before it goes into production systems
- **Business Impact:** Prevents wrong inventory counts or inaccurate sales reports

**Processing Optimization:**
- **Challenge:** Different systems need data in different formats
- **Staging Solution:** Convert data once in staging, then distribute in multiple formats
- **Efficiency Gain:** Process once, use many times

**Recovery and Backup:**
- **Risk:** If final processing fails, don't lose a whole day's work
- **Staging Solution:** Keep processed data in staging until final loading succeeds
- **Business Continuity:** Can reprocess quickly if something goes wrong

**Shoprite's Staging Strategy:**

**Landing Zone (Raw Data Storage):**
- **Amazon S3 Standard:** Incoming data from all stores
- **Retention:** Keep for 30 days for troubleshooting
- **Access:** Technical teams only

**Processing Zone (Cleaned Data):**
- **Amazon S3 Standard-IA:** Data after initial cleaning and validation
- **Retention:** Keep for 90 days for reprocessing if needed
- **Access:** Data engineering teams

**Analytics Zone (Business-Ready Data):**
- **Amazon S3 Standard:** Final processed data ready for business use
- **Retention:** Keep for 7 years for compliance
- **Access:** Business analysts and executives

**Real Staging Example: Black Friday Data Processing**

**Challenge:** Process 10x normal data volume without overwhelming systems

**Staging Approach:**
1. **Landing (2 AM - 4 AM):** Collect all Black Friday transaction data
2. **Validation (4 AM - 5 AM):** Check data quality and completeness
3. **Processing (5 AM - 7 AM):** Transform data for different business needs
4. **Distribution (7 AM - 8 AM):** Load into analytics and reporting systems
5. **Verification (8 AM - 9 AM):** Confirm all systems received correct data

**Without Staging (Previous Year's Disaster):**
- Processing failed at 6:30 AM due to corrupted file from one store
- Had to restart entire process from beginning
- Executive reports delayed until 2 PM
- Lost critical insights for next-day inventory decisions

**With Staging (Current Success):**
- Corrupted file detected in validation stage
- Fixed the issue without affecting other processing
- All reports delivered on time at 8 AM
- Executive team made informed decisions for weekend sales strategy

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
## 🛠️ Skills Implementation Through Shoprite Examples

### 1. Optimizing Container Usage for Performance

**Understanding Containers Through Shoprite's Distribution Model**

Think of containers like standardized shipping containers that Shoprite uses to move products from suppliers to stores. Whether the container holds bread, milk, or electronics, it's the same size and shape, making it easy to transport, stack, and manage.

**Software containers work the same way:**
- Each application runs in its own container
- Containers are standardized and portable
- Can move between different servers easily
- Multiple containers can run on the same server

**Shoprite's Container Strategy:**

**Peak Shopping Periods (Black Friday, Christmas):**
*The Challenge:* Normal computing capacity can't handle 10x traffic increase

*Container Solution:*
- **Amazon EKS (Kubernetes):** Automatically launches 50 additional containers when traffic spikes
- **Cost Efficiency:** Containers shut down automatically when traffic returns to normal
- **Performance:** Each container handles specific tasks (payment processing, inventory updates, customer service)
- **Reliability:** If one container crashes, others continue working

**Real Example: Mobile App Backend**

*Normal Day:*
- 10 containers handle mobile app requests
- Each container serves 1,000 customers
- Total capacity: 10,000 concurrent users

*Black Friday:*
- System detects increasing load
- Automatically launches 40 more containers
- Each container still serves 1,000 customers optimally
- Total capacity: 50,000 concurrent users
- Containers shut down automatically after the rush

**Amazon ECS vs Amazon EKS:**

**Amazon ECS (Simpler Management):**
*Best for:* Shoprite's straightforward applications like inventory tracking
- **Easy Setup:** Less complex configuration
- **AWS Integration:** Works seamlessly with other AWS services
- **Cost:** Lower management overhead

**Amazon EKS (Advanced Features):**
*Best for:* Shoprite's complex applications like real-time analytics
- **Flexibility:** More control over container orchestration
- **Community Support:** Large ecosystem of tools and add-ons
- **Multi-Cloud:** Can work across different cloud providers if needed

### 2. Connecting to Different Data Sources

**The Universal Translator Challenge**

Imagine Shoprite needs to communicate with suppliers from 20 different countries, each speaking different languages and using different business practices. They need universal translators (connection protocols) to enable smooth communication.

**JDBC and ODBC: The Universal Data Translators**

**JDBC (Java Database Connectivity):**
Think of JDBC like a multilingual interpreter who specializes in database languages.

*Shoprite Example:*
- **Oracle Database:** Legacy supplier management system
- **MySQL Database:** Store management systems
- **PostgreSQL:** Customer analytics database
- **SQL Server:** Financial reporting system

*JDBC Solution:*
One Java application can connect to all these different databases using the same code structure, just with different "interpreters" (drivers) for each database type.

**ODBC (Open Database Connectivity):**
ODBC is like JDBC but works with many programming languages, not just Java.

*Shoprite Use Case:*
- **Excel Reports:** Store managers create reports connecting directly to databases
- **Python Analytics:** Data scientists analyze trends using Python scripts
- **Power BI Dashboards:** Executives view real-time dashboards
- **Legacy Applications:** Old systems still need database access

**Real Connection Challenges Shoprite Faces:**

**Challenge 1: Legacy POS Systems**
- **Problem:** 500 older stores use 15-year-old POS systems with proprietary databases
- **Solution:** Custom ODBC drivers that translate old data formats into modern SQL
- **Business Impact:** Can include legacy stores in company-wide analytics

**Challenge 2: Supplier System Integration**
- **Problem:** Major suppliers use different ERP systems (SAP, Oracle, Microsoft)
- **Solution:** JDBC connections with custom adapters for each supplier's API
- **Business Impact:** Automated ordering and inventory management

**Challenge 3: Real-Time Data Streaming**
- **Problem:** Traditional database connections too slow for real-time fraud detection
- **Solution:** Apache Kafka connectors for streaming data
- **Business Impact:** Stop fraudulent transactions within 2 seconds

### 3. Integrating Data from Multiple Sources

**The Orchestra Conductor Approach**

Imagine Shoprite's data integration like conducting a symphony orchestra. You have dozens of musicians (data sources) each playing different instruments (data formats) at different times (various schedules). The conductor (integration system) must coordinate everyone to create beautiful music (useful business insights).

**Shoprite's Data Integration Challenge:**

**Source 1: Store POS Systems**
- **Format:** Real-time transaction streams
- **Volume:** 5 million transactions daily
- **Update Frequency:** Continuous
- **Data Quality:** High accuracy, standardized format

**Source 2: Online Shopping Platform**
- **Format:** JSON web logs
- **Volume:** 2 million page views daily
- **Update Frequency:** Every few seconds
- **Data Quality:** Variable, needs cleaning

**Source 3: Supplier Inventory Systems**
- **Format:** Daily CSV files
- **Volume:** 500,000 product updates daily
- **Update Frequency:** Once per day at midnight
- **Data Quality:** Good but different formats per supplier

**Source 4: Social Media Monitoring**
- **Format:** Unstructured text and images
- **Volume:** 10,000 mentions monthly
- **Update Frequency:** Continuous
- **Data Quality:** Highly variable, needs significant processing

**Source 5: Weather Services**
- **Format:** XML API responses
- **Volume:** Weather updates for 2,900 store locations
- **Update Frequency:** Every 15 minutes
- **Data Quality:** High accuracy

**Integration Strategy: The Master Data Pipeline**

**Step 1: Data Collection (Morning Gathering)**
- **Time:** 2:00 AM - 4:00 AM daily
- **Process:** Collect all data from previous day
- **Staging:** Store in Amazon S3 landing zones
- **Validation:** Check that all expected data sources delivered files

**Step 2: Data Standardization (Format Harmonization)**
- **Time:** 4:00 AM - 5:00 AM daily
- **Process:** Convert all data to common formats (mostly Parquet)
- **Cleaning:** Remove duplicates, fix formatting errors
- **Enrichment:** Add calculated fields and business context

**Step 3: Data Integration (Combining Sources)**
- **Time:** 5:00 AM - 6:00 AM daily
- **Process:** Join related data from different sources
- **Business Rules:** Apply logic like "match online customers with store purchases"
- **Quality Checks:** Ensure integrated data makes business sense

**Step 4: Business Intelligence Loading (Final Delivery)**
- **Time:** 6:00 AM - 7:00 AM daily
- **Process:** Load integrated data into analytics systems
- **Distribution:** Make available to dashboards, reports, and APIs
- **Notification:** Alert business users that fresh data is available

**Real Integration Example: Customer 360 View**

*Business Goal:* Understand complete customer behavior across all touchpoints

*Data Sources Integration:*
- **In-Store Purchases:** POS transaction data
- **Online Shopping:** Website and mobile app activity
- **Customer Service:** Support tickets and call logs
- **Social Media:** Brand mentions and sentiment
- **Loyalty Program:** Points earned and redeemed
- **Marketing:** Email opens, clicks, and responses

*Integration Challenge:*
The same customer might appear as:
- Customer ID #12345 in the POS system
- Email address "john.doe@email.com" online
- Phone number "0821234567" in customer service
- Twitter handle "@johndoe" on social media
- Loyalty card #987654321 in the rewards program

*Integration Solution:*
Create a master customer record that links all these identities:
- Use machine learning to match records with high probability
- Manual verification for uncertain matches
- Continuous learning to improve matching over time
- Single customer view showing all interactions across channels
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