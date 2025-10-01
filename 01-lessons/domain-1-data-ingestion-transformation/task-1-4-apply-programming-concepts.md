# Task Statement 1.4: Apply Programming Concepts

## 🎯 Official Exam Scope

This task statement focuses on the foundational programming and development practices that data engineers need to build, deploy, and maintain robust data solutions in AWS.

## 📚 Knowledge Areas

### 1. SQL Queries to Extract, Transform, and Load Data

#### 🏦 Banking Transaction Analysis with Complex SQL

**Business Scenario**: The bank needs to analyze customer transaction patterns, calculate monthly summaries, and identify suspicious activities using sophisticated SQL queries.

```sql
-- Complex banking analysis queries

-- 1. Customer Transaction Summary with Window Functions
WITH customer_monthly_summary AS (
    SELECT 
        customer_id,
        account_id,
        DATE_TRUNC('month', transaction_date) as month_year,
        COUNT(*) as transaction_count,
        SUM(amount) as total_amount,
        AVG(amount) as avg_transaction,
        MIN(amount) as min_transaction,
        MAX(amount) as max_transaction,
        -- Window functions for analysis
        SUM(amount) OVER (
            PARTITION BY customer_id, account_id 
            ORDER BY DATE_TRUNC('month', transaction_date)
            ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
        ) as rolling_3month_total,
        LAG(SUM(amount), 1) OVER (
            PARTITION BY customer_id, account_id 
            ORDER BY DATE_TRUNC('month', transaction_date)
        ) as previous_month_total
    FROM transactions 
    WHERE transaction_date >= CURRENT_DATE - INTERVAL '12 months'
        AND transaction_status = 'COMPLETED'
    GROUP BY customer_id, account_id, DATE_TRUNC('month', transaction_date)
)

-- 2. Anomaly Detection using Statistical Analysis
SELECT 
    c.*,
    -- Calculate percentage change from previous month
    CASE 
        WHEN previous_month_total > 0 THEN 
            ((total_amount - previous_month_total) / previous_month_total) * 100
        ELSE NULL 
    END as month_over_month_change,
    
    -- Flag unusual patterns
    CASE 
        WHEN total_amount > rolling_3month_total * 0.6 THEN 'HIGH_ACTIVITY'
        WHEN total_amount < rolling_3month_total * 0.1 THEN 'LOW_ACTIVITY'
        ELSE 'NORMAL'
    END as activity_pattern,
    
    -- Risk scoring
    CASE 
        WHEN transaction_count > 100 AND avg_transaction > 5000 THEN 'HIGH_RISK'
        WHEN max_transaction > 50000 THEN 'LARGE_TRANSACTION_RISK'
        ELSE 'NORMAL_RISK'
    END as risk_category

FROM customer_monthly_summary c
WHERE month_year = DATE_TRUNC('month', CURRENT_DATE - INTERVAL '1 month')
ORDER BY total_amount DESC;

-- 3. ETL Query for Data Warehouse Loading
INSERT INTO analytics.customer_transaction_facts (
    customer_key,
    account_key,
    date_key,
    transaction_count,
    total_amount,
    avg_amount,
    risk_score,
    activity_category,
    load_timestamp
)
SELECT 
    dim_customer.customer_key,
    dim_account.account_key,
    dim_date.date_key,
    monthly_stats.transaction_count,
    monthly_stats.total_amount,
    monthly_stats.avg_transaction,
    -- Complex risk calculation
    GREATEST(0, LEAST(100, 
        (monthly_stats.transaction_count * 0.1) +
        (CASE WHEN monthly_stats.max_transaction > 10000 THEN 25 ELSE 0 END) +
        (CASE WHEN monthly_stats.avg_transaction > 2000 THEN 15 ELSE 0 END)
    )) as risk_score,
    monthly_stats.activity_pattern,
    CURRENT_TIMESTAMP
FROM customer_monthly_summary monthly_stats
JOIN dim_customer ON monthly_stats.customer_id = dim_customer.customer_id
JOIN dim_account ON monthly_stats.account_id = dim_account.account_id  
JOIN dim_date ON monthly_stats.month_year = dim_date.month_year
WHERE monthly_stats.month_year = DATE_TRUNC('month', CURRENT_DATE - INTERVAL '1 month');
```

#### 🏪 Supermarket Sales Analysis and ETL

**Business Scenario**: Supermarket chain needs to analyze sales performance across stores, identify top products, and load summarized data for business intelligence.

```sql
-- Supermarket chain analysis and ETL queries

-- 1. Store Performance Analysis with Advanced Analytics
WITH store_performance AS (
    SELECT 
        s.store_id,
        s.store_name,
        s.region,
        DATE_TRUNC('week', sale_date) as week_start,
        
        -- Sales metrics
        COUNT(DISTINCT transaction_id) as transaction_count,
        COUNT(DISTINCT customer_id) as unique_customers,
        SUM(quantity * unit_price) as total_revenue,
        SUM(quantity * cost_per_unit) as total_cost,
        SUM(quantity * unit_price) - SUM(quantity * cost_per_unit) as gross_profit,
        
        -- Product mix analysis
        COUNT(DISTINCT product_id) as product_variety,
        AVG(quantity * unit_price) as avg_transaction_value,
        
        -- Performance percentiles
        PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY quantity * unit_price) as median_sale,
        PERCENTILE_CONT(0.95) WITHIN GROUP (ORDER BY quantity * unit_price) as p95_sale
        
    FROM sales_transactions st
    JOIN stores s ON st.store_id = s.store_id
    WHERE sale_date >= CURRENT_DATE - INTERVAL '8 weeks'
    GROUP BY s.store_id, s.store_name, s.region, DATE_TRUNC('week', sale_date)
),

-- 2. Product Performance Ranking
product_ranking AS (
    SELECT 
        p.product_id,
        p.product_name,
        p.category,
        SUM(st.quantity) as total_quantity_sold,
        SUM(st.quantity * st.unit_price) as total_revenue,
        COUNT(DISTINCT st.store_id) as stores_selling,
        
        -- Ranking within category
        RANK() OVER (
            PARTITION BY p.category 
            ORDER BY SUM(st.quantity * st.unit_price) DESC
        ) as revenue_rank_in_category,
        
        -- Performance ratios
        SUM(st.quantity * st.unit_price) / 
        SUM(SUM(st.quantity * st.unit_price)) OVER (PARTITION BY p.category) * 100 
        as category_revenue_share
        
    FROM sales_transactions st
    JOIN products p ON st.product_id = p.product_id
    WHERE st.sale_date >= CURRENT_DATE - INTERVAL '4 weeks'
    GROUP BY p.product_id, p.product_name, p.category
)

-- 3. Customer Segmentation Analysis
SELECT 
    customer_segments.segment_name,
    COUNT(DISTINCT customer_segments.customer_id) as customer_count,
    AVG(customer_segments.total_spent) as avg_spending,
    AVG(customer_segments.visit_frequency) as avg_visits_per_month,
    SUM(customer_segments.total_spent) as segment_revenue

FROM (
    SELECT 
        c.customer_id,
        c.customer_name,
        SUM(st.quantity * st.unit_price) as total_spent,
        COUNT(DISTINCT DATE_TRUNC('day', st.sale_date)) as visit_frequency,
        
        -- Customer segmentation logic
        CASE 
            WHEN SUM(st.quantity * st.unit_price) > 2000 AND 
                 COUNT(DISTINCT DATE_TRUNC('day', st.sale_date)) > 15 THEN 'Premium Regular'
            WHEN SUM(st.quantity * st.unit_price) > 2000 THEN 'High Value'
            WHEN COUNT(DISTINCT DATE_TRUNC('day', st.sale_date)) > 15 THEN 'Frequent Shopper'
            WHEN SUM(st.quantity * st.unit_price) > 500 THEN 'Regular Customer'
            ELSE 'Occasional Shopper'
        END as segment_name
        
    FROM customers c
    JOIN sales_transactions st ON c.customer_id = st.customer_id
    WHERE st.sale_date >= CURRENT_DATE - INTERVAL '3 months'
    GROUP BY c.customer_id, c.customer_name
) customer_segments

GROUP BY customer_segments.segment_name
ORDER BY segment_revenue DESC;

-- 4. ETL: Load to Data Warehouse with Data Quality Checks
BEGIN TRANSACTION;

-- Data quality validation
CREATE TEMP TABLE data_quality_check AS
SELECT 
    'sales_data' as table_name,
    COUNT(*) as total_records,
    COUNT(DISTINCT store_id) as unique_stores,
    MIN(sale_date) as earliest_date,
    MAX(sale_date) as latest_date,
    SUM(CASE WHEN quantity <= 0 OR unit_price <= 0 THEN 1 ELSE 0 END) as invalid_records
FROM sales_transactions 
WHERE sale_date = CURRENT_DATE - INTERVAL '1 day';

-- Only proceed if data quality is acceptable
INSERT INTO datawarehouse.daily_sales_summary (
    sale_date,
    store_id,
    region,
    total_transactions,
    total_revenue, 
    total_profit,
    unique_customers,
    avg_transaction_value,
    top_category,
    data_quality_score,
    load_timestamp
)
SELECT 
    st.sale_date,
    st.store_id,
    s.region,
    COUNT(DISTINCT st.transaction_id) as total_transactions,
    SUM(st.quantity * st.unit_price) as total_revenue,
    SUM(st.quantity * st.unit_price) - SUM(st.quantity * st.cost_per_unit) as total_profit,
    COUNT(DISTINCT st.customer_id) as unique_customers,
    AVG(st.quantity * st.unit_price) as avg_transaction_value,
    
    -- Most popular category
    (SELECT p.category 
     FROM sales_transactions st2 
     JOIN products p ON st2.product_id = p.product_id
     WHERE st2.store_id = st.store_id AND st2.sale_date = st.sale_date
     GROUP BY p.category 
     ORDER BY SUM(st2.quantity) DESC 
     LIMIT 1) as top_category,
     
    -- Data quality score (0-100)
    CASE 
        WHEN COUNT(*) > 0 AND 
             SUM(CASE WHEN st.quantity > 0 AND st.unit_price > 0 THEN 1 ELSE 0 END) = COUNT(*) 
        THEN 100
        ELSE (SUM(CASE WHEN st.quantity > 0 AND st.unit_price > 0 THEN 1 ELSE 0 END) * 100.0 / COUNT(*))
    END as data_quality_score,
    
    CURRENT_TIMESTAMP as load_timestamp

FROM sales_transactions st
JOIN stores s ON st.store_id = s.store_id
WHERE st.sale_date = CURRENT_DATE - INTERVAL '1 day'
    AND EXISTS (
        SELECT 1 FROM data_quality_check 
        WHERE invalid_records = 0
    )
GROUP BY st.sale_date, st.store_id, s.region;

COMMIT;
```

### 2. Version Control Concepts for Code Deployment

#### 🏦 Banking Data Pipeline Git Workflow

**Business Scenario**: Managing code for critical banking data pipelines requires strict version control and deployment processes.

```bash
# Banking data pipeline Git workflow structure
banking-data-pipelines/
├── .github/
│   └── workflows/
│       ├── ci-cd-dev.yml      # Development environment CI/CD
│       ├── ci-cd-prod.yml     # Production environment CI/CD
│       └── security-scan.yml  # Security and compliance scanning
├── src/
│   ├── glue-jobs/
│   │   ├── transaction-etl.py
│   │   ├── fraud-detection.py
│   │   └── regulatory-reporting.py
│   ├── lambda-functions/
│   │   ├── data-validation/
│   │   └── alert-processor/
│   └── sql-scripts/
│       ├── ddl/              # Data definition language
│       ├── etl/              # ETL procedures
│       └── reports/          # Reporting queries
├── infrastructure/
│   ├── terraform/            # Infrastructure as Code
│   └── cloudformation/       # AWS CloudFormation templates
├── tests/
│   ├── unit/
│   ├── integration/
│   └── performance/
├── config/
│   ├── dev/
│   ├── staging/
│   └── prod/
└── docs/
    ├── architecture/
    ├── deployment/
    └── troubleshooting/
```

```yaml
# .github/workflows/ci-cd-prod.yml - Production deployment workflow
name: Banking Pipeline Production Deployment

on:
  push:
    branches: [ main ]
    paths: 
      - 'src/**'
      - 'infrastructure/**'
  pull_request:
    branches: [ main ]

env:
  AWS_REGION: us-east-1
  ENVIRONMENT: prod

jobs:
  security-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Security Scan
        uses: github/super-linter@v4
        env:
          DEFAULT_BRANCH: main
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          VALIDATE_PYTHON: true
          VALIDATE_SQL: true
          
      - name: SAST Scan with Bandit
        run: |
          pip install bandit
          bandit -r src/ -f json -o bandit-report.json
          
      - name: Check for Secrets
        uses: trufflesecurity/trufflehog@v3.21.0
        with:
          path: ./
          base: main
          head: HEAD

  code-quality:
    runs-on: ubuntu-latest
    needs: security-scan
    steps:
      - uses: actions/checkout@v3
      
      - name: Python Code Quality
        run: |
          pip install pylint black pytest
          black --check src/
          pylint src/
          
      - name: SQL Quality Check
        run: |
          # SQL formatting and basic syntax check
          find src/sql-scripts -name "*.sql" -exec sqlfluff lint {} \;

  unit-tests:
    runs-on: ubuntu-latest
    needs: code-quality
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.9'
          
      - name: Install Dependencies
        run: |
          pip install -r requirements.txt
          pip install pytest pytest-cov moto boto3
          
      - name: Run Unit Tests
        run: |
          pytest tests/unit/ --cov=src/ --cov-report=xml
          
      - name: Upload Coverage
        uses: codecov/codecov-action@v3

  integration-tests:
    runs-on: ubuntu-latest
    needs: unit-tests
    services:
      localstack:
        image: localstack/localstack
        ports:
          - 4566:4566
        env:
          SERVICES: s3,glue,lambda,kinesis
    steps:
      - uses: actions/checkout@v3
      
      - name: Integration Tests
        run: |
          export AWS_ENDPOINT_URL=http://localhost:4566
          pytest tests/integration/

  deploy-staging:
    runs-on: ubuntu-latest
    needs: integration-tests
    if: github.ref == 'refs/heads/main'
    environment: staging
    steps:
      - uses: actions/checkout@v3
      
      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ env.AWS_REGION }}
          
      - name: Deploy Infrastructure
        run: |
          cd infrastructure/terraform/staging
          terraform init
          terraform plan -out=tfplan
          terraform apply tfplan
          
      - name: Deploy Glue Jobs
        run: |
          aws s3 sync src/glue-jobs/ s3://banking-pipeline-staging-scripts/
          # Update Glue job definitions
          python scripts/update-glue-jobs.py --environment staging
          
      - name: Run Smoke Tests
        run: |
          pytest tests/smoke/ --environment=staging

  manual-approval:
    runs-on: ubuntu-latest
    needs: deploy-staging
    environment: production-approval
    steps:
      - name: Request Production Deployment Approval
        uses: actions/github-script@v6
        with:
          script: |
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: '🚀 Ready for production deployment. Please review and approve.'
            })

  deploy-production:
    runs-on: ubuntu-latest
    needs: manual-approval
    environment: production
    steps:
      - uses: actions/checkout@v3
      
      - name: Blue-Green Deployment
        run: |
          # Deploy to green environment
          python scripts/blue-green-deploy.py --target=green
          
          # Run production validation tests
          pytest tests/production-validation/
          
          # Switch traffic to green
          python scripts/blue-green-deploy.py --switch-traffic
          
          # Keep blue environment for rollback capability
          echo "Deployment complete. Blue environment maintained for rollback."
```

#### 🏪 Supermarket Analytics Git Strategy

```bash
# Supermarket analytics Git branching strategy
git checkout -b feature/customer-segmentation-ml

# Feature development with proper commits
git add src/ml-models/customer-segmentation.py
git commit -m "feat: Add customer segmentation ML model

- Implement RFM analysis for customer segmentation
- Add feature engineering for purchase patterns
- Include model validation and metrics
- Add unit tests with 90% coverage

Closes #123"

# Code review process
git push origin feature/customer-segmentation-ml
# Create pull request with:
# - Detailed description of changes
# - Test results and coverage report
# - Performance impact analysis
# - Business value explanation

# After review and approval
git checkout main
git merge feature/customer-segmentation-ml --no-ff
git tag -a v2.1.0 -m "Release v2.1.0: Customer segmentation ML model"
git push origin main --tags

# Production deployment
git checkout main
git pull origin main
# Automated CI/CD pipeline triggers deployment
```

### 3. Infrastructure as Code for Data Resources

#### 🏦 Banking Infrastructure with Terraform

**Business Scenario**: Deploy secure, compliant banking data infrastructure using Infrastructure as Code.

```hcl
# main.tf - Banking data infrastructure
terraform {
  required_version = ">= 1.0"
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
  
  backend "s3" {
    bucket = "banking-terraform-state"
    key    = "banking-data-platform/terraform.tfstate"
    region = "us-east-1"
    encrypt = true
    kms_key_id = "arn:aws:kms:us-east-1:123456789012:key/12345678-1234-1234-1234-123456789012"
  }
}

# Data lake S3 buckets with encryption and compliance
module "data_lake" {
  source = "./modules/s3-data-lake"
  
  environment = var.environment
  
  buckets = {
    raw_data = {
      name = "banking-raw-data-${var.environment}"
      encryption = "aws:kms"
      kms_key_id = module.kms.data_key_arn
      versioning = true
      lifecycle_rules = [
        {
          id = "transition_to_ia"
          status = "Enabled"
          transition = [
            {
              days = 30
              storage_class = "STANDARD_IA"
            },
            {
              days = 90
              storage_class = "GLACIER"
            }
          ]
        }
      ]
    }
    
    processed_data = {
      name = "banking-processed-data-${var.environment}"
      encryption = "aws:kms"
      kms_key_id = module.kms.data_key_arn
      versioning = true
    }
    
    sensitive_data = {
      name = "banking-sensitive-data-${var.environment}"
      encryption = "aws:kms"
      kms_key_id = module.kms.sensitive_data_key_arn
      versioning = true
      access_logging = true
      object_lock = true  # Regulatory compliance
    }
  }
  
  tags = local.common_tags
}

# KMS keys for different data sensitivity levels
module "kms" {
  source = "./modules/kms"
  
  keys = {
    data_key = {
      description = "Key for general banking data encryption"
      key_usage = "ENCRYPT_DECRYPT"
      key_spec = "SYMMETRIC_DEFAULT"
      deletion_window = 30
    }
    
    sensitive_data_key = {
      description = "Key for sensitive customer data (PII, account numbers)"
      key_usage = "ENCRYPT_DECRYPT"
      key_spec = "SYMMETRIC_DEFAULT"
      deletion_window = 30
      key_rotation = true
    }
  }
  
  tags = local.common_tags
}

# Kinesis data streams for real-time processing
resource "aws_kinesis_stream" "transaction_stream" {
  name             = "banking-transactions-${var.environment}"
  shard_count      = var.environment == "prod" ? 10 : 2
  retention_period = 168  # 7 days
  
  shard_level_metrics = [
    "IncomingRecords",
    "OutgoingRecords"
  ]
  
  encryption_type = "KMS"
  kms_key_id      = module.kms.data_key_arn
  
  tags = merge(local.common_tags, {
    Purpose = "Real-time transaction processing"
  })
}

# Glue catalog and databases
resource "aws_glue_catalog_database" "banking_data" {
  name        = "banking_data_${var.environment}"
  description = "Banking data catalog for ${var.environment} environment"
  
  create_table_default_permission {
    permissions = ["SELECT"]
    principal   = aws_iam_role.data_analyst.arn
  }
}

# EMR cluster for big data processing
module "emr_cluster" {
  source = "./modules/emr"
  
  cluster_name = "banking-analytics-${var.environment}"
  release_label = "emr-6.15.0"
  
  master_instance_type = var.environment == "prod" ? "m5.xlarge" : "m5.large" 
  core_instance_type   = var.environment == "prod" ? "m5.large" : "m5.medium"
  core_instance_count  = var.environment == "prod" ? 5 : 2
  
  # Security configuration
  security_configuration = {
    encryption_at_rest = true
    encryption_in_transit = true
    kms_key_id = module.kms.data_key_arn
  }
  
  # Applications
  applications = [
    "Spark",
    "Hadoop",
    "Hive",
    "Zeppelin"
  ]
  
  # Bootstrap actions
  bootstrap_actions = [
    {
      name = "Install additional Python packages"
      path = "s3://banking-scripts-${var.environment}/bootstrap/install-packages.sh"
    }
  ]
  
  tags = local.common_tags
}

# RDS for metadata and configuration
resource "aws_db_instance" "metadata_db" {
  identifier = "banking-metadata-${var.environment}"
  
  engine         = "postgres"
  engine_version = "15.3"
  instance_class = var.environment == "prod" ? "db.r5.large" : "db.t3.medium"
  
  allocated_storage     = var.environment == "prod" ? 100 : 20
  max_allocated_storage = var.environment == "prod" ? 1000 : 100
  storage_type         = "gp3"
  storage_encrypted    = true
  kms_key_id          = module.kms.data_key_arn
  
  db_name  = "banking_metadata"
  username = "admin"
  password = random_password.db_password.result
  
  # Security
  vpc_security_group_ids = [aws_security_group.rds.id]
  db_subnet_group_name   = aws_db_subnet_group.main.name
  
  # Backup and maintenance
  backup_retention_period = var.environment == "prod" ? 7 : 3
  backup_window          = "03:00-04:00"
  maintenance_window     = "sun:04:00-sun:05:00"
  
  # Monitoring
  performance_insights_enabled = true
  monitoring_interval         = 60
  monitoring_role_arn        = aws_iam_role.rds_monitoring.arn
  
  deletion_protection = var.environment == "prod" ? true : false
  
  tags = local.common_tags
}

# Lambda functions for data processing
module "lambda_functions" {
  source = "./modules/lambda"
  
  functions = {
    transaction_validator = {
      filename = "transaction-validator.zip"
      handler = "lambda_function.lambda_handler"
      runtime = "python3.9"
      timeout = 300
      memory_size = 512
      environment_variables = {
        ENVIRONMENT = var.environment
        METADATA_DB_ENDPOINT = aws_db_instance.metadata_db.endpoint
      }
    }
    
    fraud_detector = {
      filename = "fraud-detector.zip"
      handler = "lambda_function.lambda_handler"
      runtime = "python3.9"
      timeout = 900
      memory_size = 3008  # Maximum for CPU-intensive ML operations
      environment_variables = {
        MODEL_BUCKET = module.data_lake.buckets["processed_data"].bucket
        KINESIS_STREAM = aws_kinesis_stream.transaction_stream.name
      }
    }
  }
  
  tags = local.common_tags
}

# CloudWatch alarms and monitoring
module "monitoring" {
  source = "./modules/cloudwatch"
  
  alarms = {
    kinesis_high_utilization = {
      metric_name = "IncomingRecords"
      namespace = "AWS/Kinesis"
      statistic = "Sum"
      period = "300"
      evaluation_periods = "2"
      threshold = "1000000"
      comparison_operator = "GreaterThanThreshold"
      alarm_description = "Kinesis stream receiving high volume of records"
      dimensions = {
        StreamName = aws_kinesis_stream.transaction_stream.name
      }
    }
    
    emr_cluster_failure = {
      metric_name = "IsIdle"
      namespace = "AWS/ElasticMapReduce"
      statistic = "Average"
      period = "300"
      evaluation_periods = "3"
      threshold = "1"
      comparison_operator = "LessThanThreshold"
      alarm_description = "EMR cluster is not processing jobs"
    }
  }
  
  tags = local.common_tags
}

# Local values for common tags
locals {
  common_tags = {
    Project = "Banking Data Platform"
    Environment = var.environment
    ManagedBy = "Terraform"
    Owner = "Data Engineering Team"
    CostCenter = "Technology"
    Compliance = "SOX, PCI-DSS"
  }
}

# Variables
variable "environment" {
  description = "Environment name (dev, staging, prod)"
  type        = string
  validation {
    condition     = contains(["dev", "staging", "prod"], var.environment)
    error_message = "Environment must be dev, staging, or prod."
  }
}

# Outputs
output "data_lake_buckets" {
  value = {
    for name, bucket in module.data_lake.buckets : name => bucket.bucket
  }
  description = "Data lake S3 bucket names"
}

output "kinesis_stream_arn" {
  value = aws_kinesis_stream.transaction_stream.arn
  description = "Kinesis stream ARN for transaction processing"
}

output "emr_cluster_id" {
  value = module.emr_cluster.cluster_id
  description = "EMR cluster ID for big data processing"
}
```

#### 🏪 Supermarket Infrastructure with CloudFormation

```yaml
# supermarket-analytics-infrastructure.yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: 'Supermarket analytics data platform infrastructure'

Parameters:
  Environment:
    Type: String
    Default: dev
    AllowedValues: [dev, staging, prod]
    Description: Environment name
    
  StoreCount:
    Type: Number
    Default: 500
    Description: Number of stores in the chain
    
  ExpectedTPS:
    Type: Number
    Default: 1000
    Description: Expected transactions per second

Mappings:
  EnvironmentSettings:
    dev:
      KinesisShards: 2
      EMRInstanceType: m5.large
      EMRInstanceCount: 2
      RedshiftNodeType: dc2.large
      RedshiftNodeCount: 1
    staging:
      KinesisShards: 5
      EMRInstanceType: m5.xlarge
      EMRInstanceCount: 3
      RedshiftNodeType: dc2.large
      RedshiftNodeCount: 2
    prod:
      KinesisShards: 20
      EMRInstanceType: m5.xlarge
      EMRInstanceCount: 10
      RedshiftNodeType: ra3.xlplus
      RedshiftNodeCount: 3

Resources:
  # Data Lake S3 Buckets
  RawDataBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub 'supermarket-raw-data-${Environment}-${AWS::AccountId}'
      VersioningConfiguration:
        Status: Enabled
      BucketEncryption:
        ServerSideEncryptionConfiguration:
          - ServerSideEncryptionByDefault:
              SSEAlgorithm: AES256
      LifecycleConfiguration:
        Rules:
          - Id: TransitionToIA
            Status: Enabled
            Transitions:
              - TransitionInDays: 30
                StorageClass: STANDARD_IA
              - TransitionInDays: 90
                StorageClass: GLACIER
              - TransitionInDays: 365
                StorageClass: DEEP_ARCHIVE
      PublicAccessBlockConfiguration:
        BlockPublicAcls: true
        BlockPublicPolicy: true
        IgnorePublicAcls: true
        RestrictPublicBuckets: true

  ProcessedDataBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub 'supermarket-processed-data-${Environment}-${AWS::AccountId}'
      VersioningConfiguration:
        Status: Enabled
      BucketEncryption:
        ServerSideEncryptionConfiguration:
          - ServerSideEncryptionByDefault:
              SSEAlgorithm: AES256

  # Kinesis Data Streams
  TransactionStream:
    Type: AWS::Kinesis::Stream
    Properties:
      Name: !Sub 'supermarket-transactions-${Environment}'
      ShardCount: !FindInMap [EnvironmentSettings, !Ref Environment, KinesisShards]
      RetentionPeriodHours: 168  # 7 days
      StreamModeDetails:
        StreamMode: PROVISIONED
      Tags:
        - Key: Environment
          Value: !Ref Environment
        - Key: Purpose
          Value: Real-time transaction processing

  InventoryStream:
    Type: AWS::Kinesis::Stream
    Properties:
      Name: !Sub 'supermarket-inventory-${Environment}'
      ShardCount: !FindInMap [EnvironmentSettings, !Ref Environment, KinesisShards]
      RetentionPeriodHours: 24
      StreamModeDetails:
        StreamMode: PROVISIONED

  # Kinesis Data Firehose for S3 delivery
  TransactionFirehose:
    Type: AWS::KinesisFirehose::DeliveryStream
    Properties:
      DeliveryStreamName: !Sub 'supermarket-transactions-to-s3-${Environment}'
      DeliveryStreamType: KinesisStreamAsSource
      KinesisStreamSourceConfiguration:
        KinesisStreamARN: !GetAtt TransactionStream.Arn
        RoleARN: !GetAtt FirehoseDeliveryRole.Arn
      S3DestinationConfiguration:
        BucketARN: !GetAtt RawDataBucket.Arn
        Prefix: 'transactions/year=!{timestamp:yyyy}/month=!{timestamp:MM}/day=!{timestamp:dd}/hour=!{timestamp:HH}/'
        ErrorOutputPrefix: 'errors/transactions/'
        BufferingHints:
          SizeInMBs: 5
          IntervalInSeconds: 300
        CompressionFormat: GZIP
        RoleARN: !GetAtt FirehoseDeliveryRole.Arn
        ProcessingConfiguration:
          Enabled: true
          Processors:
            - Type: Lambda
              Parameters:
                - ParameterName: LambdaArn
                  ParameterValue: !GetAtt DataTransformationLambda.Arn

  # Lambda function for data transformation
  DataTransformationLambda:
    Type: AWS::Lambda::Function
    Properties:
      FunctionName: !Sub 'supermarket-data-transformation-${Environment}'
      Runtime: python3.9
      Handler: lambda_function.lambda_handler
      Timeout: 300
      MemorySize: 512
      Code:
        ZipFile: |
          import json
          import base64
          import boto3
          from datetime import datetime
          
          def lambda_handler(event, context):
              output = []
              
              for record in event['records']:
                  # Decode the data
                  payload = json.loads(base64.b64decode(record['data']))
                  
                  # Add metadata
                  payload['processed_timestamp'] = datetime.utcnow().isoformat()
                  payload['processing_version'] = '1.0'
                  
                  # Data quality checks
                  if validate_transaction(payload):
                      result = 'Ok'
                  else:
                      result = 'ProcessingFailed'
                  
                  # Encode the result
                  output_record = {
                      'recordId': record['recordId'],
                      'result': result,
                      'data': base64.b64encode(
                          json.dumps(payload).encode('utf-8')
                      ).decode('utf-8')
                  }
                  output.append(output_record)
              
              return {'records': output}
          
          def validate_transaction(payload):
              required_fields = ['store_id', 'transaction_id', 'amount', 'timestamp']
              return all(field in payload for field in required_fields)
      Role: !GetAtt DataTransformationLambdaRole.Arn

  # EMR Cluster for big data processing
  EMRCluster:
    Type: AWS::EMR::Cluster
    Properties:
      Name: !Sub 'supermarket-analytics-${Environment}'
      ReleaseLabel: emr-6.15.0
      Applications:
        - Name: Spark
        - Name: Hadoop
        - Name: Hive
        - Name: Zeppelin
      ServiceRole: !Ref EMRServiceRole
      JobFlowRole: !Ref EMRInstanceProfile
      Instances:
        MasterInstanceGroup:
          InstanceCount: 1
          InstanceType: !FindInMap [EnvironmentSettings, !Ref Environment, EMRInstanceType]
          Market: ON_DEMAND
        CoreInstanceGroup:
          InstanceCount: !FindInMap [EnvironmentSettings, !Ref Environment, EMRInstanceCount]
          InstanceType: !FindInMap [EnvironmentSettings, !Ref Environment, EMRInstanceType]
          Market: SPOT
          BidPrice: "0.10"
        Ec2SubnetId: !Ref PrivateSubnet
        Ec2KeyName: !Ref KeyPairName
        KeepJobFlowAliveWhenNoSteps: false
        TerminationProtected: false
      BootstrapActions:
        - Name: Install additional packages
          ScriptBootstrapAction:
            Path: !Sub 's3://supermarket-scripts-${Environment}/bootstrap/install-packages.sh'
      LogUri: !Sub 's3://supermarket-logs-${Environment}/emr-logs/'
      Tags:
        - Key: Environment
          Value: !Ref Environment
        - Key: Purpose
          Value: Big data analytics processing

  # Redshift cluster for data warehousing
  RedshiftCluster:
    Type: AWS::Redshift::Cluster
    Properties:
      ClusterIdentifier: !Sub 'supermarket-analytics-${Environment}'
      DBName: analytics
      MasterUsername: admin
      MasterUserPassword: !Ref RedshiftPassword
      NodeType: !FindInMap [EnvironmentSettings, !Ref Environment, RedshiftNodeType]
      NumberOfNodes: !FindInMap [EnvironmentSettings, !Ref Environment, RedshiftNodeCount]
      ClusterSubnetGroupName: !Ref RedshiftSubnetGroup
      VpcSecurityGroupIds:
        - !Ref RedshiftSecurityGroup
      Encrypted: true
      PubliclyAccessible: false
      AutomatedSnapshotRetentionPeriod: 7
      PreferredMaintenanceWindow: sun:05:00-sun:06:00
      Tags:
        - Key: Environment
          Value: !Ref Environment
        - Key: Purpose
          Value: Data warehouse for analytics

  # DynamoDB tables for metadata and configuration
  ConfigurationTable:
    Type: AWS::DynamoDB::Table
    Properties:
      TableName: !Sub 'supermarket-configuration-${Environment}'
      AttributeDefinitions:
        - AttributeName: config_key
          AttributeType: S
        - AttributeName: config_category
          AttributeType: S
      KeySchema:
        - AttributeName: config_key
          KeyType: HASH
        - AttributeName: config_category
          KeyType: RANGE
      BillingMode: PAY_PER_REQUEST
      PointInTimeRecoverySpecification:
        PointInTimeRecoveryEnabled: true
      SSESpecification:
        SSEEnabled: true
      Tags:
        - Key: Environment
          Value: !Ref Environment
        - Key: Purpose
          Value: Application configuration storage

Outputs:
  RawDataBucket:
    Description: S3 bucket for raw data storage
    Value: !Ref RawDataBucket
    Export:
      Name: !Sub '${AWS::StackName}-RawDataBucket'
      
  TransactionStreamArn:
    Description: Kinesis stream ARN for transactions
    Value: !GetAtt TransactionStream.Arn
    Export:
      Name: !Sub '${AWS::StackName}-TransactionStream'
      
  EMRClusterId:
    Description: EMR cluster ID
    Value: !Ref EMRCluster
    Export:
      Name: !Sub '${AWS::StackName}-EMRCluster'
      
  RedshiftEndpoint:
    Description: Redshift cluster endpoint
    Value: !GetAtt RedshiftCluster.Endpoint.Address
    Export:
      Name: !Sub '${AWS::StackName}-RedshiftEndpoint'
```

### 4. Software Development Lifecycle Concepts

#### 🏦 Banking Data Platform SDLC

**Complete development lifecycle for mission-critical banking systems:**

```python
# Banking SDLC implementation with governance and compliance
class BankingDataPlatformSDLC:
    """
    Complete Software Development Lifecycle for banking data platforms
    with compliance, security, and governance requirements
    """
    
    def __init__(self):
        self.phases = {
            "planning": self.planning_phase,
            "analysis": self.analysis_phase,
            "design": self.design_phase,
            "implementation": self.implementation_phase,
            "testing": self.testing_phase,
            "deployment": self.deployment_phase,
            "maintenance": self.maintenance_phase
        }
        
    def planning_phase(self):
        """
        Project planning with regulatory and compliance considerations
        """
        planning_activities = {
            "requirements_gathering": {
                "business_requirements": [
                    "Process 1M transactions/day with <100ms latency",
                    "Generate regulatory reports within 4 hours of day-end",
                    "Detect fraud patterns in real-time",
                    "Maintain 99.99% uptime during business hours"
                ],
                "regulatory_requirements": [
                    "SOX compliance for financial reporting",
                    "PCI-DSS for payment data handling",
                    "GDPR for customer data protection",
                    "Basel III for risk reporting"
                ],
                "technical_requirements": [
                    "Multi-region deployment for disaster recovery",
                    "End-to-end encryption for sensitive data",
                    "Audit trails for all data access and modifications",
                    "Real-time monitoring and alerting"
                ]
            },
            
            "risk_assessment": {
                "technical_risks": [
                    {
                        "risk": "Data corruption during high-volume processing",
                        "probability": "Medium",
                        "impact": "High",
                        "mitigation": "Implement checksums and data validation at every stage"
                    },
                    {
                        "risk": "System failure during regulatory reporting",
                        "probability": "Low", 
                        "impact": "Critical",
                        "mitigation": "Multi-AZ deployment with automated failover"
                    }
                ],
                "compliance_risks": [
                    {
                        "risk": "Unauthorized access to customer data", 
                        "probability": "Medium",
                        "impact": "Critical",
                        "mitigation": "Role-based access control with MFA"
                    }
                ]
            },
            
            "project_timeline": {
                "phase_1_foundation": "8 weeks",
                "phase_2_core_processing": "12 weeks", 
                "phase_3_analytics": "8 weeks",
                "phase_4_compliance": "6 weeks",
                "phase_5_go_live": "4 weeks",
                "total_timeline": "38 weeks"
            }
        }
        
        return planning_activities
    
    def analysis_phase(self):
        """
        Detailed system analysis and architecture design
        """
        analysis_deliverables = {
            "current_state_analysis": {
                "existing_systems": [
                    "Legacy mainframe transaction processing",
                    "Multiple disconnected reporting systems", 
                    "Manual regulatory report generation",
                    "Batch-only fraud detection (24-hour delay)"
                ],
                "pain_points": [
                    "6-hour end-of-day processing window",
                    "Limited real-time analytics capabilities",
                    "Manual data reconciliation processes",
                    "Difficulty meeting new regulatory timelines"
                ]
            },
            
            "future_state_architecture": {
                "data_ingestion": "Real-time streaming with Kinesis",
                "data_processing": "Serverless and managed services",
                "data_storage": "S3 data lake with Redshift warehouse",
                "analytics": "Real-time dashboards and ML models",
                "reporting": "Automated regulatory report generation"
            },
            
            "data_flow_design": {
                "real_time_path": [
                    "Transaction → Kinesis Data Streams",
                    "Kinesis → Lambda (validation/enrichment)",
                    "Lambda → DynamoDB (customer state)",
                    "Lambda → Kinesis Analytics (fraud detection)",
                    "Results → SNS (alerts) / S3 (storage)"
                ],
                "batch_path": [
                    "S3 → Glue (ETL processing)",
                    "Glue → Redshift (data warehouse)",
                    "Redshift → QuickSight (reporting)",
                    "Scheduled jobs → Regulatory reports"
                ]
            }
        }
        
        return analysis_deliverables
    
    def design_phase(self):
        """
        Detailed technical design with security and compliance
        """
        design_specifications = {
            "architecture_components": {
                "ingestion_layer": {
                    "service": "Amazon Kinesis Data Streams",
                    "configuration": "10 shards, 7-day retention", 
                    "encryption": "KMS encryption at rest and in transit",
                    "monitoring": "CloudWatch metrics and alarms"
                },
                
                "processing_layer": {
                    "real_time": "Lambda functions with reserved concurrency",
                    "batch": "AWS Glue with Spark for large-scale ETL",
                    "ml_inference": "SageMaker endpoints for fraud detection",
                    "orchestration": "Step Functions for complex workflows"
                },
                
                "storage_layer": {
                    "data_lake": "S3 with lifecycle policies and cross-region replication",
                    "data_warehouse": "Redshift with automated backups",
                    "operational_data": "DynamoDB with point-in-time recovery",
                    "metadata": "AWS Glue Data Catalog"
                }
            },
            
            "security_design": {
                "network_security": "VPC with private subnets, NAT gateways",
                "access_control": "IAM roles with least privilege principle",
                "encryption": "KMS keys for different data sensitivity levels",
                "monitoring": "CloudTrail, Config, GuardDuty integration",
                "secrets_management": "AWS Secrets Manager for credentials"
            },
            
            "data_governance": {
                "data_classification": "Public, Internal, Confidential, Restricted",
                "data_lineage": "AWS Glue Data Catalog with custom metadata",
                "data_quality": "Great Expectations framework integration",
                "privacy_controls": "Automated PII detection and masking"
            }
        }
        
        return design_specifications
    
    def implementation_phase(self):
        """
        Agile development with continuous integration
        """
        implementation_approach = {
            "development_methodology": "Agile with 2-week sprints",
            
            "sprint_structure": {
                "sprint_1_2": "Foundation infrastructure and CI/CD pipeline",
                "sprint_3_4": "Data ingestion and basic processing",
                "sprint_5_6": "Real-time processing and fraud detection",
                "sprint_7_8": "Batch processing and data warehousing",
                "sprint_9_10": "Reporting and dashboard development",
                "sprint_11_12": "Security hardening and compliance features",
                "sprint_13_14": "Performance optimization and testing",
                "sprint_15_16": "User acceptance testing and documentation"
            },
            
            "coding_standards": {
                "python": "PEP 8 compliance with Black formatter",
                "sql": "SQL style guide with consistent naming",
                "infrastructure": "Terraform modules with validation",
                "documentation": "Sphinx for Python, README for each component"
            },
            
            "quality_gates": {
                "code_review": "All code requires 2 approvals",
                "unit_tests": "Minimum 80% code coverage",
                "security_scan": "SAST and DAST on every commit",
                "performance_test": "Load testing for each component"
            }
        }
        
        return implementation_approach
    
    def testing_phase(self):
        """
        Comprehensive testing strategy for financial systems
        """
        testing_strategy = {
            "test_pyramid": {
                "unit_tests": {
                    "coverage": "80% minimum",
                    "tools": "pytest, moto for AWS mocking",
                    "execution": "Automated on every commit"
                },
                
                "integration_tests": {
                    "scope": "API endpoints, database connections, service interactions",
                    "environment": "Dedicated testing environment",
                    "tools": "pytest, testcontainers, LocalStack"
                },
                
                "system_tests": {
                    "scope": "End-to-end transaction processing",
                    "scenarios": "Happy path, error conditions, edge cases",
                    "data": "Synthetic data matching production patterns"
                },
                
                "performance_tests": {
                    "load_testing": "1M transactions/day simulation",
                    "stress_testing": "150% of expected load",
                    "endurance_testing": "72-hour continuous operation",
                    "tools": "JMeter, Artillery, AWS Load Testing Solution"
                }
            },
            
            "compliance_testing": {
                "security_testing": [
                    "Penetration testing by third-party security firm",
                    "Vulnerability scanning with AWS Inspector",
                    "OWASP Top 10 validation",
                    "Access control verification"
                ],
                
                "regulatory_testing": [
                    "SOX controls testing",
                    "PCI-DSS compliance validation", 
                    "Data privacy controls verification",
                    "Audit trail completeness testing"
                ]
            },
            
            "disaster_recovery_testing": {
                "backup_restoration": "Monthly backup restore tests",
                "failover_testing": "Quarterly multi-region failover",
                "data_corruption_recovery": "Annual chaos engineering exercises",
                "business_continuity": "Semi-annual full DR drills"
            }
        }
        
        return testing_strategy
    
    def deployment_phase(self):
        """
        Production deployment with blue-green strategy
        """
        deployment_strategy = {
            "environments": {
                "development": {
                    "purpose": "Active development and unit testing",
                    "data": "Synthetic data, small volume",
                    "infrastructure": "Single AZ, minimal resources"
                },
                
                "testing": {
                    "purpose": "Integration and system testing",
                    "data": "Production-like synthetic data",
                    "infrastructure": "Multi-AZ, scaled-down production"
                },
                
                "staging": {
                    "purpose": "User acceptance testing and final validation",
                    "data": "Anonymized production data subset",
                    "infrastructure": "Identical to production"
                },
                
                "production": {
                    "purpose": "Live transaction processing",
                    "data": "Real customer and transaction data",
                    "infrastructure": "Multi-region, high availability"
                }
            },
            
            "deployment_process": {
                "blue_green_deployment": {
                    "blue_environment": "Current production environment",
                    "green_environment": "New version deployment target",
                    "traffic_routing": "Gradual traffic shift using Route 53",
                    "rollback_plan": "Immediate traffic switch to blue if issues"
                },
                
                "deployment_steps": [
                    "Deploy infrastructure to green environment",
                    "Deploy application code to green environment", 
                    "Run smoke tests on green environment",
                    "Route 10% of traffic to green environment",
                    "Monitor metrics and error rates",
                    "Gradually increase traffic to 50%, then 100%",
                    "Keep blue environment for 24 hours before decommission"
                ]
            },
            
            "rollback_procedures": {
                "automatic_rollback": [
                    "Error rate > 1% for 5 minutes",
                    "Response time > 500ms for 3 minutes",
                    "Failed health checks on 2 consecutive polls"
                ],
                "manual_rollback": "Decision point at each traffic increase",
                "rollback_time": "< 5 minutes for complete rollback"
            }
        }
        
        return deployment_strategy
    
    def maintenance_phase(self):
        """
        Ongoing maintenance and continuous improvement
        """
        maintenance_activities = {
            "monitoring_and_alerting": {
                "business_metrics": [
                    "Transaction processing rate and latency",
                    "Fraud detection accuracy and false positive rate",
                    "Report generation time and completeness",
                    "Customer satisfaction scores"
                ],
                
                "technical_metrics": [
                    "System uptime and availability",
                    "Error rates and exception counts", 
                    "Resource utilization and cost metrics",
                    "Security event detection and response time"
                ],
                
                "compliance_metrics": [
                    "Audit trail completeness",
                    "Data retention policy compliance",
                    "Access control violations",
                    "Regulatory report timeliness"
                ]
            },
            
            "maintenance_schedules": {
                "daily": [
                    "System health checks",
                    "Backup verification",
                    "Performance metric review",
                    "Security alert triage"
                ],
                
                "weekly": [
                    "Capacity planning review",
                    "Cost optimization analysis",
                    "Security patch assessment",
                    "Performance trend analysis"
                ],
                
                "monthly": [
                    "Disaster recovery testing",
                    "Security compliance review",
                    "Business metric analysis",
                    "System optimization review"
                ],
                
                "quarterly": [
                    "Architecture review and updates",
                    "Technology roadmap assessment",
                    "Compliance audit preparation",
                    "Team training and skill development"
                ]
            },
            
            "continuous_improvement": {
                "performance_optimization": [
                    "Regular performance benchmarking",
                    "Cost optimization initiatives",
                    "Technology stack upgrades",
                    "Process automation enhancements"
                ],
                
                "feature_enhancements": [
                    "Business requirement gathering",
                    "User feedback incorporation",
                    "Regulatory requirement updates",
                    "Technology innovation adoption"
                ]
            }
        }
        
        return maintenance_activities

# Usage example
banking_sdlc = BankingDataPlatformSDLC()
project_plan = banking_sdlc.planning_phase()
system_design = banking_sdlc.design_phase()
testing_plan = banking_sdlc.testing_phase()
```

## 📋 Summary

Task Statement 1.4 covers the essential programming and development practices for data engineering:

**Key Programming Concepts**:
- **SQL Mastery**: Complex queries with window functions, CTEs, and statistical analysis
- **Version Control**: Git workflows with branching strategies and CI/CD integration
- **Infrastructure as Code**: Terraform and CloudFormation for reproducible deployments
- **SDLC**: Complete development lifecycle with testing, deployment, and maintenance

**Critical Development Practices**:
- **Code Quality**: Automated testing, code reviews, and quality gates
- **Security**: SAST/DAST scanning, secrets management, and compliance validation
- **Deployment**: Blue-green deployments with automated rollback capabilities
- **Monitoring**: Comprehensive observability and alerting strategies

**Real-world Applications**:
- Banking transaction processing with complex SQL analytics
- Supermarket data platform with Infrastructure as Code
- Regulatory compliance with automated testing and validation
- Production deployment strategies for mission-critical systems

**Best Practices**:
- Maintain high code coverage with comprehensive testing
- Use Infrastructure as Code for consistent environments
- Implement proper CI/CD pipelines with quality gates
- Design for maintainability and continuous improvement

Mastering these programming concepts enables you to build robust, scalable, and maintainable data engineering solutions that meet enterprise requirements for quality, security, and compliance.

---

**Next**: [Domain 2: Data Store Management](../domain-2-data-store-management/README.md)  
**Previous**: [Task Statement 1.3: Orchestrate Data Pipelines](task-1-3-orchestrate-data-pipelines.md)