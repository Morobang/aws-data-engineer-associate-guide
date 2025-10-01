# Lab 1: Setting Up Your Data Engineering Environment

**Duration**: 45 minutes  
**Cost**: Free (Free Tier eligible)  
**Services**: AWS CLI, S3, IAM, CloudFormation

## 🎯 Learning Objectives

By the end of this lab, you will be able to:
- Configure AWS CLI with appropriate credentials
- Create and manage S3 buckets for data storage
- Set up IAM roles and policies for data engineering tasks
- Establish a consistent naming convention for AWS resources
- Create a development environment template for future labs

## 📋 Prerequisites

- AWS account with administrator access (or appropriate permissions)
- Computer with internet access  
- Basic command line familiarity

## 🛠️ Setup Instructions

### Step 1: Install AWS CLI

#### Windows
```powershell
# Download and install AWS CLI v2
# Visit: https://aws.amazon.com/cli/
# Or use PowerShell:
Invoke-WebRequest -Uri "https://awscli.amazonaws.com/AWSCLIV2.msi" -OutFile "AWSCLIV2.msi"
Start-Process msiexec.exe -Wait -ArgumentList '/I AWSCLIV2.msi /quiet'
```

#### macOS
```bash
# Using Homebrew
brew install awscli

# Or download installer
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
```

#### Linux
```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

#### Verify Installation
```bash
aws --version
# Expected output: aws-cli/2.x.x Python/3.x.x ...
```

### Step 2: Create IAM User for Development

1. **Log into AWS Console**
   - Navigate to the IAM service
   - Click "Users" → "Add users"

2. **Create User**
   - Username: `data-engineer-dev`
   - Access type: ✅ Programmatic access
   - Click "Next: Permissions"

3. **Attach Policies**
   For this course, attach these managed policies:
   - `AmazonS3FullAccess`
   - `AWSGlueConsoleFullAccess`  
   - `AmazonKinesisFullAccess`
   - `AmazonRedshiftFullAccess`
   - `AmazonRDSFullAccess`
   - `AWSLambdaFullAccess`
   - `CloudWatchFullAccess`
   - `IAMReadOnlyAccess`

   > **Note**: In production, you would create more restrictive policies

4. **Download Credentials**
   - Save the Access Key ID and Secret Access Key
   - ⚠️ **Security**: Never commit these to version control

### Step 3: Configure AWS CLI

```bash
aws configure
```

Enter your credentials:
```
AWS Access Key ID [None]: YOUR_ACCESS_KEY_ID
AWS Secret Access Key [None]: YOUR_SECRET_ACCESS_KEY  
Default region name [None]: us-east-1
Default output format [None]: json
```

#### Verify Configuration
```bash
aws sts get-caller-identity
```

Expected output:
```json
{
    "UserId": "AIDACKCEVSQ6C2EXAMPLE",
    "Account": "123456789012", 
    "Arn": "arn:aws:iam::123456789012:user/data-engineer-dev"
}
```

### Step 4: Create S3 Buckets

We'll create several S3 buckets for different purposes throughout the course.

#### Create Bucket Creation Script

Create a file named `create-buckets.sh` (or `create-buckets.ps1` for Windows):

```bash
#!/bin/bash

# Set variables
ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
REGION="us-east-1"
TIMESTAMP=$(date +%Y%m%d-%H%M%S)

# Create bucket names with account ID for uniqueness
RAW_BUCKET="data-engineering-raw-${ACCOUNT_ID}-${TIMESTAMP}"
PROCESSED_BUCKET="data-engineering-processed-${ACCOUNT_ID}-${TIMESTAMP}"
CURATED_BUCKET="data-engineering-curated-${ACCOUNT_ID}-${TIMESTAMP}"
LOGS_BUCKET="data-engineering-logs-${ACCOUNT_ID}-${TIMESTAMP}"

echo "Creating S3 buckets..."

# Create buckets
aws s3 mb s3://$RAW_BUCKET --region $REGION
aws s3 mb s3://$PROCESSED_BUCKET --region $REGION  
aws s3 mb s3://$CURATED_BUCKET --region $REGION
aws s3 mb s3://$LOGS_BUCKET --region $REGION

# Enable versioning on important buckets
aws s3api put-bucket-versioning --bucket $PROCESSED_BUCKET --versioning-configuration Status=Enabled
aws s3api put-bucket-versioning --bucket $CURATED_BUCKET --versioning-configuration Status=Enabled

# Set up bucket structure
aws s3api put-object --bucket $RAW_BUCKET --key "landing/" --content-length 0
aws s3api put-object --bucket $RAW_BUCKET --key "archive/" --content-length 0
aws s3api put-object --bucket $PROCESSED_BUCKET --key "transformed/" --content-length 0
aws s3api put-object --bucket $CURATED_BUCKET --key "analytics/" --content-length 0

echo "Buckets created successfully:"
echo "Raw Data: $RAW_BUCKET"
echo "Processed Data: $PROCESSED_BUCKET"  
echo "Curated Data: $CURATED_BUCKET"
echo "Logs: $LOGS_BUCKET"

# Save bucket names for future use
cat > bucket-names.txt << EOF
RAW_BUCKET=$RAW_BUCKET
PROCESSED_BUCKET=$PROCESSED_BUCKET
CURATED_BUCKET=$CURATED_BUCKET
LOGS_BUCKET=$LOGS_BUCKET
EOF

echo "Bucket names saved to bucket-names.txt"
```

#### Run the Script
```bash
chmod +x create-buckets.sh
./create-buckets.sh
```

### Step 5: Create IAM Role for AWS Services

Create an IAM role that AWS services can assume:

```bash
# Create trust policy
cat > trust-policy.json << EOF
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "glue.amazonaws.com",
                    "lambda.amazonaws.com",
                    "kinesis.amazonaws.com",
                    "firehose.amazonaws.com"
                ]
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
EOF

# Create the role
aws iam create-role \
    --role-name DataEngineerServiceRole \
    --assume-role-policy-document file://trust-policy.json

# Attach policies
aws iam attach-role-policy \
    --role-name DataEngineerServiceRole \
    --policy-arn arn:aws:iam::aws:policy/service-role/AWSGlueServiceRole

aws iam attach-role-policy \
    --role-name DataEngineerServiceRole \
    --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess

aws iam attach-role-policy \
    --role-name DataEngineerServiceRole \
    --policy-arn arn:aws:iam::aws:policy/CloudWatchLogsFullAccess
```

### Step 6: Test Your Setup

#### Upload Sample Data
```bash
# Create sample data file
cat > sample-data.json << EOF
{"customer_id": 1, "order_id": 1001, "product": "laptop", "amount": 999.99, "timestamp": "2024-01-01T10:00:00Z"}
{"customer_id": 2, "order_id": 1002, "product": "mouse", "amount": 29.99, "timestamp": "2024-01-01T10:05:00Z"}
{"customer_id": 1, "order_id": 1003, "product": "keyboard", "amount": 79.99, "timestamp": "2024-01-01T10:10:00Z"}
EOF

# Upload to raw bucket
source bucket-names.txt
aws s3 cp sample-data.json s3://$RAW_BUCKET/landing/sample-data.json

# Verify upload
aws s3 ls s3://$RAW_BUCKET/landing/
```

#### List Your Resources
```bash
echo "=== S3 Buckets ==="
aws s3 ls

echo "=== IAM Roles ==="
aws iam list-roles --query 'Roles[?contains(RoleName, `DataEngineer`)].RoleName' --output table
```

## 🔍 Verification Steps

### 1. AWS CLI Configuration
```bash
aws configure list
```
Should show your credentials and region.

### 2. S3 Bucket Access
```bash
source bucket-names.txt
aws s3 ls s3://$RAW_BUCKET
```
Should list the folders you created.

### 3. IAM Role Creation
```bash
aws iam get-role --role-name DataEngineerServiceRole
```
Should return role details without errors.

## 📊 Expected Outcomes

After completing this lab, you should have:

✅ AWS CLI installed and configured  
✅ Four S3 buckets with proper folder structure  
✅ IAM role for AWS services  
✅ Sample data uploaded to raw bucket  
✅ Resource naming convention established

## 🧹 Cleanup Instructions

**DO NOT cleanup these resources yet** - they will be used in subsequent labs.

When you complete all labs in the course, you can cleanup with:

```bash
# Delete S3 buckets (this will delete all contents!)
source bucket-names.txt
aws s3 rb s3://$RAW_BUCKET --force
aws s3 rb s3://$PROCESSED_BUCKET --force  
aws s3 rb s3://$CURATED_BUCKET --force
aws s3 rb s3://$LOGS_BUCKET --force

# Delete IAM role
aws iam detach-role-policy --role-name DataEngineerServiceRole --policy-arn arn:aws:iam::aws:policy/service-role/AWSGlueServiceRole
aws iam detach-role-policy --role-name DataEngineerServiceRole --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
aws iam detach-role-policy --role-name DataEngineerServiceRole --policy-arn arn:aws:iam::aws:policy/CloudWatchLogsFullAccess
aws iam delete-role --role-name DataEngineerServiceRole
```

## 🚨 Troubleshooting

### Common Issues

#### AWS CLI Not Found
**Solution**: Restart your terminal/command prompt after installation

#### Access Denied Errors
**Solution**: Verify IAM user has necessary permissions

#### Bucket Name Already Exists
**Solution**: S3 bucket names are globally unique. The script includes timestamp to avoid conflicts.

#### Region Consistency
**Solution**: Ensure all resources are created in the same region (us-east-1)

## 📚 Review Questions

1. What is the purpose of each S3 bucket we created?
2. Why do we need an IAM role for AWS services?
3. What security best practices should you follow with AWS credentials?
4. How does the folder structure in S3 support data organization?

## 🔗 Next Steps

Great job! You've successfully set up your data engineering environment. 

Continue with [Lab 2: Real-Time Data Streaming with Kinesis](../lab-02-kinesis-streaming/README.md) to start building data pipelines.

---

**Estimated Cost**: $0 (Free Tier)  
**Time to Complete**: 45 minutes  
**Difficulty**: Beginner