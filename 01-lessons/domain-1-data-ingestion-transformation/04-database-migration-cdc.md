# Lesson 4: Database Migration and Change Data Capture

## 🎯 Learning Objectives

After completing this lesson, you will be able to:
- Understand why and when businesses migrate databases
- Explain Change Data Capture (CDC) using real-world scenarios
- Distinguish between stateful and stateless data transactions
- Implement replayable data ingestion pipelines
- Configure AWS DMS for different migration scenarios

## 🏦 What is Database Migration? (Think: Moving Your Bank Account)

Imagine your local bank decides to upgrade from their 1990s computer system to a modern cloud-based system. They need to:
- **Move all customer accounts** without losing a single penny
- **Keep the bank running** during the migration (no downtime)
- **Track every transaction** that happens during the move
- **Ensure nothing gets lost** or duplicated
- **Switch to the new system** seamlessly

This is exactly what **database migration** does for businesses - moving data from old systems to new ones safely and reliably.

### 🏪 Supermarket Chain Example
A supermarket chain wants to migrate from:
- **Old system**: Individual databases in each store
- **New system**: Centralized cloud database for all stores

**Challenges**:
- 500 stores operating 24/7
- Can't shut down stores for migration
- Must track every sale during transition
- Inventory must stay accurate across all systems

## 🔄 What is Change Data Capture (CDC)?

**Simple Explanation**: CDC is like having a security camera watching your database - it records every change (insert, update, delete) so you can replay what happened.

### 🏦 Banking CDC Example

```
Original Account Balance: $1,000

During Migration:
10:00 AM - Customer deposits $500    → Balance: $1,500
10:15 AM - Customer withdraws $200   → Balance: $1,300  
10:30 AM - Interest added $5         → Balance: $1,305
10:45 AM - Monthly fee $10           → Balance: $1,295

CDC captures all these changes so the new system 
ends up with the correct final balance: $1,295
```

Without CDC, the new system might only see the original $1,000 and miss all the changes!

### 🏪 Supermarket Inventory CDC Example

```
Product: Milk (Gallon)
Starting Inventory: 100 units

During Migration:
9:00 AM - Customer buys 2 gallons    → Inventory: 98
9:30 AM - Delivery arrives +50       → Inventory: 148
10:00 AM - Customer buys 3 gallons   → Inventory: 145
10:30 AM - Return +1 gallon          → Inventory: 146

CDC ensures new system shows correct inventory: 146 units
```

## 🔄 Stateful vs Stateless Transactions

### 🏦 Stateful Transactions (Remember Previous Actions)

**Banking Example**: Account Balance
```
Account Balance: $1,000 (STATE)

Transaction 1: Withdraw $100
- Checks current balance ($1,000)
- Ensures sufficient funds
- New balance: $900 (NEW STATE)

Transaction 2: Withdraw $50  
- Checks current balance ($900) ← Depends on previous state
- Ensures sufficient funds
- New balance: $850 (NEW STATE)
```

**Key Point**: Each transaction depends on the result of previous transactions.

### 📊 Stateless Transactions (Independent Actions)

**Supermarket Example**: Item Scanning
```
Transaction 1: Scan apple ($1.50)
- Records: Product=Apple, Price=$1.50, Time=10:00

Transaction 2: Scan bread ($2.00)
- Records: Product=Bread, Price=$2.00, Time=10:01

Each scan is independent - scanning bread doesn't 
depend on whether apple was scanned first.
```

## 🔁 Replayability: Why It Matters

**Replayability** means you can "rewind" and reprocess data if something goes wrong.

### 🏦 Banking Replayability Example

```
Scenario: Migration fails at 2 PM, but bank operated all morning

Without Replayability:
❌ Lost: All transactions from 8 AM - 2 PM
❌ Result: Customer accounts show wrong balances
❌ Solution: Manual reconciliation (takes days/weeks)

With Replayability (CDC):
✅ Stored: All changes from 8 AM - 2 PM in CDC log
✅ Action: Replay all changes on fixed system
✅ Result: All accounts perfectly accurate
✅ Time: Minutes, not days
```

### 🏪 Supermarket Replayability Example

```
Scenario: Inventory system crashes during busy Saturday

CDC Log Contains:
08:00 - Milk inventory: 200 units
08:05 - Sold 3 units → 197
08:10 - Delivery +100 → 297
08:15 - Sold 5 units → 292
... (hundreds of transactions)
14:30 - System crashes

Recovery:
1. Restore system to 08:00 state (200 units)
2. Replay all CDC changes from 08:00-14:30
3. Current inventory accurate: 197 units
```

## 🚀 AWS Database Migration Service (DMS)

AWS DMS is like a moving company that specializes in database migrations - they handle the heavy lifting and make sure nothing gets lost.

### 🏗️ DMS Components

#### 1. **Replication Instance**
**Think**: The moving truck
- Performs the actual data migration
- Handles the workload of copying and synchronizing data
- Can be sized based on migration needs

#### 2. **Source Endpoint**  
**Think**: Your old house address
- Points to the original database
- Could be on-premises or in cloud
- Supports many database types (Oracle, MySQL, PostgreSQL, etc.)

#### 3. **Target Endpoint**
**Think**: Your new house address  
- Points to the destination database
- Often an AWS database service
- Can be different type than source (e.g., Oracle → PostgreSQL)

### 🏦 Banking Migration Architecture

```
Old Bank System (Source)          DMS Replication Instance          New Cloud System (Target)
┌─────────────────────┐          ┌──────────────────────┐          ┌─────────────────────┐
│   Oracle Database   │   ───→   │   DMS reads changes  │   ───→   │   Amazon RDS        │
│   - Customer accounts│          │   - Full load first  │          │   PostgreSQL        │
│   - Transaction logs │          │   - Then CDC for     │          │   - Modern schema   │
│   - 20 years of data │          │     ongoing changes  │          │   - Better performance│
└─────────────────────┘          └──────────────────────┘          └─────────────────────┘
```

## 📋 Migration Strategies

### 1. **Full Load Migration**
**Best for**: Small databases or acceptable downtime

#### 🏪 Small Store Example:
```
Single Store Database Migration (Weekend Closure)
Friday 6 PM: Store closes, migration starts
Saturday: DMS copies all data (products, sales, customers)
Sunday: Test new system, train staff  
Monday 8 AM: Store opens with new system
```

### 2. **Full Load + CDC (Most Common)**
**Best for**: Large databases needing minimal downtime

#### 🏦 Large Bank Example:
```
Phase 1: Full Load (Background)
- DMS copies all historical data while bank operates normally
- Takes several days but doesn't impact customers

Phase 2: CDC Sync
- DMS captures and applies ongoing changes
- Gap between old and new systems gets smaller

Phase 3: Cutover (Brief Downtime)
- Stop old system for 5-10 minutes
- Apply final changes  
- Switch to new system
- Resume operations
```

### 3. **CDC Only**
**Best for**: Real-time replication between systems

#### 🏪 Multi-Store Replication:
```
Headquarters ←→ Store Databases
- Each store updates local database
- CDC replicates changes to headquarters in real-time
- Headquarters has consolidated view of all stores
- No downtime, continuous synchronization
```

## 🛠️ DMS Configuration Examples

### Setting Up CDC for Banking System

```python
# DMS Task Configuration
{
    "SourceEndpoint": {
        "EndpointType": "source",
        "EngineName": "oracle",
        "ServerName": "bank-legacy-db.company.com",
        "Port": 1521,
        "DatabaseName": "BANKPROD",
        "Username": "dms_user"
    },
    
    "TargetEndpoint": {
        "EndpointType": "target", 
        "EngineName": "postgres",
        "ServerName": "bank-rds.amazonaws.com",
        "Port": 5432,
        "DatabaseName": "modernbank",
        "Username": "postgres"
    },
    
    "ReplicationTask": {
        "MigrationType": "full-load-and-cdc",
        "TableMappings": {
            "rules": [
                {
                    "rule-type": "selection",
                    "rule-id": "1",
                    "rule-name": "customer-accounts",
                    "object-locator": {
                        "schema-name": "banking",
                        "table-name": "customer_accounts"
                    },
                    "rule-action": "include"
                },
                {
                    "rule-type": "transformation",
                    "rule-id": "2", 
                    "rule-name": "rename-column",
                    "rule-target": "column",
                    "object-locator": {
                        "schema-name": "banking",
                        "table-name": "customer_accounts",
                        "column-name": "cust_id"
                    },
                    "rule-action": "rename",
                    "value": "customer_id"
                }
            ]
        }
    }
}
```

### Monitoring Migration Progress

```python
import boto3
import time

def monitor_migration_task(task_arn):
    """Monitor DMS task progress with real-world context"""
    
    dms = boto3.client('dms')
    
    while True:
        response = dms.describe_replication_tasks(
            ReplicationTaskArns=[task_arn]
        )
        
        task = response['ReplicationTasks'][0]
        status = task['Status']
        
        if 'ReplicationTaskStats' in task:
            stats = task['ReplicationTaskStats']
            
            print(f"""
            Migration Status: {status}
            
            Customer Records Migrated: {stats.get('TablesLoaded', 0):,}
            Current Transfer Rate: {stats.get('FullLoadProgressPercent', 0)}%
            
            Real-time Changes Applied: {stats.get('FreshStartDate', 'N/A')}
            
            Banking Operations Impact: {'MINIMAL' if status == 'running' else 'NONE'}
            """)
        
        if status in ['stopped', 'failed', 'ready']:
            break
            
        time.sleep(60)  # Check every minute
```

## 🚨 Common Migration Challenges and Solutions

### Challenge 1: Data Inconsistency During Migration

**🏦 Problem**: Customer sees different balance in mobile app vs ATM during migration

**Solution**: 
```python
# Implement read preference routing
def get_account_balance(account_id, migration_status):
    if migration_status == 'pre-migration':
        return query_legacy_system(account_id)
    elif migration_status == 'post-migration':
        return query_new_system(account_id)
    else:  # during migration
        # Always read from source of truth
        return query_legacy_system(account_id)
```

### Challenge 2: Large Table Migration

**🏪 Problem**: Product catalog has 50 million items, too large for single migration

**Solution**: Partition migration
```sql
-- Migrate in chunks by date
SELECT * FROM products WHERE created_date >= '2020-01-01' AND created_date < '2021-01-01'
SELECT * FROM products WHERE created_date >= '2021-01-01' AND created_date < '2022-01-01'
-- ... continue by year
```

### Challenge 3: Schema Changes

**🏦 Problem**: New system has different database structure

**Solution**: DMS Transformation Rules
```json
{
    "rule-type": "transformation",
    "rule-id": "3",
    "rule-name": "split-name-column", 
    "rule-target": "column",
    "object-locator": {
        "schema-name": "customers",
        "table-name": "accounts"
    },
    "rule-action": "split-column",
    "old-column-name": "full_name",
    "new-columns": ["first_name", "last_name"],
    "delimiter": " "
}
```

## 📊 Best Practices for Database Migration

### 1. **Pre-Migration Planning**
```
✅ Assess source database size and complexity
✅ Test migration with sample data
✅ Plan for schema differences  
✅ Set up monitoring and alerting
✅ Prepare rollback procedures
✅ Train users on new system
```

### 2. **During Migration**
```
✅ Monitor replication lag closely
✅ Watch for error messages
✅ Verify data integrity with checksums
✅ Test critical business functions
✅ Keep stakeholders informed
```

### 3. **Post-Migration**
```
✅ Compare row counts between source and target
✅ Validate critical data samples
✅ Monitor application performance
✅ Plan for cleanup of old system
✅ Document lessons learned
```

## 🔍 Real-World Success Story: Regional Bank Migration

**Challenge**: Regional bank with 50 branches needed to migrate from mainframe to AWS RDS

**Solution**:
1. **Phase 1 (2 months)**: DMS full load of historical data
2. **Phase 2 (1 month)**: CDC testing and validation  
3. **Phase 3 (1 weekend)**: Final cutover

**Results**:
- ✅ Zero data loss
- ✅ 15-minute total downtime
- ✅ 300% performance improvement
- ✅ 60% cost reduction
- ✅ Enhanced security and compliance

**Key Success Factors**:
- Extensive testing with production-like data
- Gradual migration approach
- Strong monitoring and alerting
- Clear rollback procedures
- Executive sponsorship and user training

## 📋 Summary

Database migration and CDC are critical for:
- **Moving from legacy systems** to modern cloud databases
- **Maintaining data consistency** during transitions  
- **Enabling real-time replication** between systems
- **Ensuring zero data loss** during system upgrades
- **Supporting business continuity** during changes

Key AWS DMS capabilities:
- **Multiple database engine support**: Oracle, MySQL, PostgreSQL, SQL Server, etc.
- **Schema conversion**: Transform data structure during migration
- **Continuous replication**: Keep systems in sync with CDC
- **Minimal downtime**: Migrate without stopping business operations

Remember: Successful migrations require careful planning, thorough testing, and robust monitoring.

## 🔗 Next Steps

Now that you understand how to safely move and track data changes, let's explore how to clean, transform, and prepare that data for analysis with [Lesson 5: ETL Fundamentals with AWS Glue](05-etl-fundamentals-glue.md).

---

**Next**: [Lesson 5: ETL Fundamentals with AWS Glue](05-etl-fundamentals-glue.md)  
**Previous**: [Lesson 3: Batch Data Ingestion](03-batch-data-ingestion.md)