# Task Statement 1.1: Perform Data Ingestion

## � Real-World Data Ingestion: The Shoprite Case Study

### Meet Shoprite - Africa's Retail Giant

**The Scale:**
- 2,900+ stores across Africa
- 40 million+ customers monthly  
- 50,000+ point-of-sale (POS) systems
- Processing 5 million+ transactions daily
- $10+ billion in annual revenue

**The Data Nightmare They Face:**
Imagine trying to coordinate 2,900 different stores, each generating sales data, inventory updates, customer transactions, and supplier information every second of every day. 

Think about it: when you buy a loaf of bread at Shoprite in Cape Town, that single transaction needs to:
- Update inventory levels instantly (so they don't oversell)
- Trigger automatic reordering when stock gets low
- Update customer loyalty points in real-time
- Feed into demand forecasting for next week's orders
- Contribute to financial reporting for corporate headquarters
- Help detect fraud patterns across all stores

This is exactly what **data ingestion** solves - it's the process of collecting, moving, and preparing all this data so businesses can actually use it to make smart decisions.

## 🧠 Understanding Data Ingestion: Why It Matters

### What Is Data Ingestion, Really?

Think of data ingestion like the circulatory system of a business. Just like your heart pumps blood to every part of your body, data ingestion pumps information to every part of a business that needs it.

**For Shoprite specifically:**
When a customer scans their loyalty card and buys groceries, that single action creates multiple streams of data that need to flow to different systems:

1. **The POS System** needs to process the payment immediately
2. **The Inventory System** needs to reduce stock counts in real-time  
3. **The Loyalty System** needs to add points to the customer's account
4. **The Analytics System** needs this data for sales reporting
5. **The Supply Chain System** needs to know what's selling fast
6. **The Fraud Detection System** needs to check for suspicious patterns

Without proper data ingestion, each system would be isolated - like having organs in your body that can't communicate with each other. The result? Chaos.

### The Two Fundamental Patterns: Streaming vs Batch

**Streaming Data Ingestion (Real-Time)**
This is like having a conversation - data flows continuously, and you need to respond immediately.

*Shoprite Example:* When someone buys milk at a store in Johannesburg, the inventory system needs to know RIGHT NOW so that:
- The store doesn't accidentally sell milk they don't have
- The mobile app shows accurate stock levels
- Automatic reordering can trigger if they're running low

This happens thousands of times per second across all Shoprite stores. It's like having 2,900 people all talking to you at once, and you need to understand and respond to each conversation immediately.

**Batch Data Ingestion (Scheduled)**
This is like getting a detailed report at the end of each day - data is collected over time and processed all at once.

*Shoprite Example:* Every night, all the day's transactions from every store get gathered together to:
- Calculate total daily sales across all stores
- Update the corporate financial reports
- Analyze shopping patterns to predict future demand
- Generate reports for store managers about their performance

Think of it like a teacher collecting all homework at the end of the week to grade - it's not urgent like a streaming conversation, but it needs to happen regularly and thoroughly.

## 📚 Knowledge Areas Explained Through Shoprite

### 1. Throughput and Latency Characteristics

Let me explain these terms in simple language:

**Throughput** = How many customers can go through the checkout lines at the same time
**Latency** = How long each individual customer has to wait in line

At Shoprite, this matters HUGELY:

**During Normal Shopping Hours:**
- Maybe 100 customers per hour per store
- Each transaction takes 2-3 minutes
- People are patient, latency isn't critical
- **AWS Solution:** You could use simple batch processing overnight

**During Black Friday Sales:**
- Suddenly 1,000+ customers per hour per store  
- Each transaction still takes 2-3 minutes, but now you have MASSIVE lines
- Customers get angry if they wait too long
- **AWS Solution:** You need Amazon Kinesis Data Streams to handle the flood

**For Fraud Detection:**
- You might only have 10 suspicious transactions per hour
- But you need to catch them in SECONDS, not minutes
- If someone steals a credit card, every second matters
- **AWS Solution:** Amazon Kinesis + AWS Lambda for instant processing

**Here's the key insight:** Different business situations need different combinations of throughput and latency. Shoprite can't use the same system for overnight inventory reports (high throughput, high latency OK) as they use for fraud detection (low throughput, but ultra-low latency required).

### 2. Data Ingestion Patterns

**Frequency Patterns - The Shoprite Reality:**

Think about how often different types of data need to be collected:

**Every Second (Continuous Streaming):**
- Customer transactions at checkout
- Inventory levels changing
- Credit card payments processing
- Store temperature sensors (for frozen food sections)

*Why this matters:* If Shoprite's freezer temperature rises above safe levels, they need to know immediately, not tomorrow morning. Spoiled food could cost them thousands of dollars and make customers sick.

**Every Hour (Micro-Batch):**
- Sales summaries for each store
- Inventory reorder suggestions
- Customer traffic patterns
- Staff scheduling adjustments

*Why this matters:* Store managers need regular updates to make decisions during the day, but they don't need second-by-second updates that would overwhelm them.

**Every Day (Daily Batch):**
- Complete financial reconciliation
- Supplier payment processing
- Deep customer behavior analysis
- Store performance comparisons

*Why this matters:* Corporate executives need the big picture, but they don't need to be distracted by every individual transaction.

**Data History Patterns:**

This is about how much historical data you need to keep and access:

**Hot Data (Immediate Access Required):**
- Current inventory levels
- Today's sales figures
- Active customer loyalty points
- Current supplier contracts

*Shoprite stores need this data instantly.* When a customer asks "Do you have this product in stock?" the employee needs an answer in 2 seconds, not 2 minutes.

**Warm Data (Quick Access Helpful):**
- Last 3 months of sales data
- Customer purchase history
- Supplier delivery patterns
- Store performance trends

*This data is used for decision-making.* A store manager planning next week's staff schedule needs to see patterns from recent months, but they can wait 30 seconds for this information.

**Cold Data (Archive Access Acceptable):**
- Sales data from 2+ years ago
- Old customer records
- Historical supplier agreements
- Regulatory compliance documents

*This data is kept for legal reasons and long-term analysis.* If there's a lawsuit or regulatory audit, Shoprite might need 5-year-old transaction records, but they can wait hours or even days to retrieve them.

### 3. Streaming Data Ingestion

**What Streaming Really Means:**

Imagine you're standing at the entrance of a busy Shoprite store, and your job is to count every customer who walks in. You can't wait until the end of the day to count them all at once - you need to count them one by one as they arrive. That's streaming data ingestion.

**Real Shoprite Streaming Scenarios:**

**Scenario 1: The Bread Shortage Crisis**
It's Saturday morning, and every Shoprite store is selling bread faster than expected. Without streaming data ingestion:
- Store A runs out of bread at 10 AM
- Store B runs out at 10:15 AM  
- Store C runs out at 10:30 AM
- Customers get angry and go to competitors
- Shoprite loses sales and reputation

With streaming data ingestion:
- As bread sales increase beyond normal patterns, the system automatically alerts managers
- Emergency bread deliveries can be arranged before stores run out
- Customer satisfaction stays high
- Shoprite keeps the sales

**Scenario 2: The Payment System Overload**
During a major promotion, credit card transactions spike dramatically. Without streaming monitoring:
- Payment systems get overwhelmed
- Customers can't pay for their groceries
- Long lines form, people abandon their shopping
- Shoprite loses millions in sales

With streaming data ingestion:
- The system detects payment volume spikes in real-time
- Additional payment processing capacity is automatically activated
- Transactions continue smoothly
- Customers have a good experience

### 4. Batch Data Ingestion

**What Batch Processing Really Means:**

Think of batch processing like doing laundry. You don't wash one sock at a time throughout the day - you collect all your dirty clothes and wash them together in batches. It's more efficient that way.

**Shoprite Batch Processing Examples:**

**Event-Driven Batch Processing:**
This happens when something specific triggers the batch job.

*Example:* Every time Shoprite receives a delivery truck, they need to update inventory across multiple systems. They don't do this item by item - instead, when the truck arrives (the "event"), they process the entire delivery manifest as one batch.

**Scheduled Batch Processing:**
This happens on a regular schedule, regardless of events.

*Example:* Every night at 2 AM, when stores are closed, Shoprite runs massive batch jobs to:
- Reconcile all the day's transactions
- Update corporate financial systems
- Generate reports for store managers
- Plan next day's staffing and deliveries
- Analyze customer behavior patterns

**Why Batch Processing Makes Sense for Some Things:**

1. **Efficiency:** It's much faster to process 1 million transactions together than to process them one at a time
2. **Cost:** AWS charges less for batch processing than real-time processing
3. **Complexity:** Some analyses (like "what products should we promote together?") require looking at ALL the data, not just individual transactions
4. **Timing:** Some business processes naturally happen at specific times (like end-of-day reconciliation)

### 5. Replayability of Data Ingestion Pipelines

**What This Means in Simple Terms:**

Imagine you're recording a TV show, but something goes wrong and you lose the last 10 minutes. With a good recording system, you can "replay" those 10 minutes from the beginning. Data pipeline replayability works the same way.

**Why Shoprite Desperately Needs This:**

**The Disaster Scenario:**
It's December 23rd (huge shopping day), and at 3 PM, Shoprite's main data processing system crashes. Without replayability:
- All sales data from 3 PM onwards is lost forever
- They can't track inventory properly
- Financial reports are wrong
- Suppliers don't know what to deliver
- Complete business chaos

**With Replayability:**
- All transaction data is safely stored in Amazon Kinesis (even during the crash)
- When systems come back online, they can "replay" all the missed data
- It's like the crash never happened
- Business continues normally

**Real-World Implementation:**
Shoprite's systems are designed so that even if their main processing fails for 6 hours, they can replay all 6 hours of data in about 30 minutes once systems are restored. This means a 6-hour outage only costs them 30 minutes of actual business impact.

### 6. Stateful vs Stateless Transactions

This is a bit technical, but I'll explain it through Shoprite examples:

**Stateless Transactions (Simple):**
Each transaction is independent and self-contained.

*Example:* When you buy a Coke at Shoprite, the transaction contains everything needed:
- Product: Coca-Cola 500ml
- Price: R15.99
- Store: Shoprite Sandton
- Time: 2024-10-01 14:30
- Customer: Loyalty card #12345

This transaction doesn't depend on any previous transactions. Even if the system forgot everything else, it could still process this single purchase.

**Stateful Transactions (Complex):**
Each transaction depends on information from previous transactions.

*Example:* Customer loyalty points system:
- Transaction 1: Customer buys R100 worth of groceries, earns 10 points
- Transaction 2: Customer buys R200 worth of groceries, earns 20 points  
- Transaction 3: Customer wants to redeem 25 points for a discount

Transaction 3 can only work if the system "remembers" (maintains state about) Transactions 1 and 2. It needs to know the customer has 30 points available before allowing them to redeem 25.

**Why This Matters for Data Ingestion:**
- Stateless systems are simpler and more reliable
- Stateful systems are more complex but enable richer business logic
- Shoprite uses both: stateless for basic sales tracking, stateful for loyalty programs and inventory management

## 🛠️ Skills Implementation Through Shoprite Examples

### 1. Reading Data from Streaming Sources

**Amazon Kinesis Data Streams - The Shoprite Transaction Highway**

Think of Amazon Kinesis like a multi-lane highway where each lane (called a "shard") can handle thousands of transactions per second flowing from Shoprite stores to their headquarters.

**How It Works for Shoprite:**
Every time someone makes a purchase at any Shoprite store, that transaction gets sent to a Kinesis stream. It's like having a dedicated express lane for data that never gets stuck in traffic.

**Why Shoprite Chose Kinesis:**
- **Scale:** Can handle millions of transactions per day from 2,900 stores
- **Speed:** Transactions are available for processing within seconds
- **Reliability:** Even if one part of the system fails, data keeps flowing
- **Cost-Effective:** Only pay for what you use

**Amazon MSK (Managed Streaming for Kafka) - For Complex Message Handling**

While Kinesis is like a highway, MSK is like a sophisticated mail system that can handle complex routing rules.

**Shoprite Use Case:**
When a product goes on promotion, they need to send different messages to different systems:
- Inventory system needs to prepare for higher demand
- Marketing system needs to update advertising displays
- Pricing system needs to update POS terminals
- Analytics system needs to track promotion effectiveness

MSK ensures each system gets exactly the messages it needs, in the right order, without any getting lost.

**DynamoDB Streams - Tracking Changes in Real-Time**

This is like having a security camera that records every time someone changes something in a database.

**Shoprite Example:**
When inventory levels change in DynamoDB (their product database), DynamoDB Streams automatically captures:
- What product changed
- What the old quantity was  
- What the new quantity is
- When the change happened
- Which store made the change

This information then flows to other systems that need to react to inventory changes, like automatic reordering systems and store manager alerts.

### 2. Reading Data from Batch Sources

**Amazon S3 - The Shoprite Data Warehouse**

Think of S3 like a massive, infinitely expandable storage warehouse where Shoprite can store any type of data - sales reports, customer photos, supplier contracts, security camera footage - and it never runs out of space.

**What makes S3 special:**
- **Unlimited Storage:** Shoprite can store petabytes of data without worrying about running out of space
- **Durability:** Data is automatically copied to multiple locations, so it's virtually impossible to lose
- **Cost-Effective:** Different storage classes for different needs - frequently accessed data costs more, archived data costs very little
- **Global Access:** Store managers in Kenya can access the same data as executives in South Africa

**Shoprite's S3 Strategy:**
- **Daily Sales Reports:** Stored in S3 Standard for immediate access
- **Monthly Analytics:** Moved to S3 Standard-IA after 30 days
- **Historical Data:** Archived to S3 Glacier after 1 year for long-term compliance

**AWS Glue - The Smart Data Organizer**

Imagine you have a brilliant assistant who can look at any messy pile of data and automatically organize it, clean it up, and make it useful. That's AWS Glue.

**How Shoprite Uses Glue:**
Every night, Shoprite stores generate hundreds of different file formats:
- CSV files from old POS systems
- JSON files from newer mobile payment systems  
- XML files from supplier integration systems
- Database dumps from various store management systems

AWS Glue automatically:
- Discovers what type of data is in each file
- Creates a catalog so people can find data later
- Converts everything into a standard format (usually Parquet)
- Cleans up errors and inconsistencies
- Makes the data available for analysis

**Amazon EMR - The Heavy-Duty Data Processor**

When Shoprite needs to analyze massive amounts of data - like figuring out shopping patterns across all 2,900 stores for the past 5 years - they need serious computing power. EMR provides clusters of computers that can work together on huge data processing jobs.

**Example Analysis:**
"Which products should we stock more of during load-shedding periods?"

This analysis requires:
- Processing 5 years of sales data (billions of transactions)
- Correlating with historical load-shedding schedules
- Analyzing customer behavior changes during power outages
- Identifying products that sell better during load-shedding
- Calculating optimal inventory levels per store

A single computer would take months to do this analysis. With EMR's cluster of 100 computers working together, it takes a few hours.

### 3. Implementing Batch Ingestion Configurations

**The Art of Timing - When to Process What**

Different types of data need different processing schedules, just like different household chores happen at different times.

**Shoprite's Processing Schedule:**

**Every 15 Minutes (High Priority):**
- Inventory level updates for fast-moving items
- Fraud detection pattern updates
- Customer loyalty point calculations
- Critical system health checks

*Why this frequency:* These things directly impact customer experience. If the system says a product is in stock but it's actually sold out, customers get frustrated.

**Every Hour (Medium Priority):**
- Sales performance summaries for store managers
- Supplier reorder recommendations
- Staff productivity reports
- Energy usage optimization

*Why this frequency:* Store managers need regular updates to make good decisions, but every minute would be overwhelming.

**Every Day (Lower Priority but Comprehensive):**
- Complete financial reconciliation
- Deep customer behavior analysis
- Supplier payment processing
- Corporate reporting and dashboards

*Why this frequency:* These processes need complete, accurate data and can afford to wait for the most comprehensive analysis.

**Configuration Best Practices Shoprite Learned:**

1. **Start Small:** Begin with daily processing, then add hourly, then add real-time only where truly needed
2. **Plan for Failures:** Every batch job needs a "what if this fails?" plan
3. **Monitor Everything:** Set up alerts for when jobs take too long or fail
4. **Test with Real Data:** Batch jobs that work with sample data might fail with real volumes

### 4. Consuming Data APIs

**What APIs Mean for Shoprite**

An API (Application Programming Interface) is like a waiter in a restaurant. You don't go into the kitchen and cook your own food - you tell the waiter what you want, and they bring it to you. APIs work the same way with data.

**External APIs Shoprite Uses:**

**Weather APIs:**
- Shoprite automatically adjusts inventory based on weather forecasts
- Hot weather = more ice cream and cold drinks ordered
- Rainy weather = more umbrella and indoor entertainment products
- Cold weather = more soup and warm clothing

**Financial APIs:**
- Real-time currency exchange rates for stores in different countries
- Credit card processing and validation
- Banking integration for supplier payments
- Economic indicators that affect pricing strategies

**Supplier APIs:**
- Automatic inventory level sharing with major suppliers
- Price update notifications
- Delivery scheduling and tracking
- Product catalog updates

**Government APIs:**
- Tax rate updates for different regions
- Regulatory compliance checking
- Import/export documentation
- Business license renewals

**How Shoprite Manages API Consumption:**

**Rate Limiting Management:**
Many APIs limit how often you can request data. Shoprite learned to:
- Batch multiple requests together when possible
- Cache frequently requested data to reduce API calls
- Distribute API calls across time to avoid hitting limits
- Have backup plans when APIs are unavailable

**Data Quality Handling:**
APIs sometimes return incomplete or incorrect data. Shoprite's systems:
- Validate all incoming API data against business rules
- Have fallback procedures when APIs return errors
- Log all API interactions for troubleshooting
- Monitor API response times and reliability

### 5. Setting Up Schedulers

**Amazon EventBridge - The Smart Scheduler**

Think of EventBridge like a highly intelligent personal assistant who can watch for specific events and automatically trigger appropriate responses.

**Shoprite EventBridge Examples:**

**Event:** "New supplier delivery arrives at Store #47"
**Automatic Actions Triggered:**
- Update inventory system with new stock levels
- Notify store manager about products that arrived
- Update supplier payment system
- Trigger quality control inspection workflow
- Update sales forecasting models with new availability

**Event:** "Customer complaint received via social media"
**Automatic Actions Triggered:**
- Create customer service ticket
- Notify store manager if complaint is about specific store
- Add to customer's profile for personalized follow-up
- Trigger review of related products or services
- Update reputation monitoring dashboard

**Apache Airflow - The Complex Workflow Manager**

For more complex workflows that involve multiple steps and dependencies, Shoprite uses Airflow. Think of it like a project manager who ensures all tasks happen in the right order.

**Example: New Store Opening Workflow**

This is a complex process that involves dozens of steps that must happen in a specific order:

1. **Legal and Regulatory Setup** (Week 1-2)
   - Register business with local authorities
   - Obtain necessary permits and licenses
   - Set up tax registration
   - Complete environmental compliance

2. **Infrastructure Setup** (Week 3-4)
   - Install POS systems and test connectivity
   - Set up network infrastructure
   - Install security systems
   - Connect to Shoprite's central systems

3. **Inventory and Staffing** (Week 5-6)
   - Initial inventory delivery and setup
   - Staff hiring and training
   - System access provisioning
   - Store layout and merchandising

4. **Go-Live Preparation** (Week 7)
   - Final system testing
   - Soft opening with limited customers
   - Issue resolution and fine-tuning
   - Marketing campaign launch

Airflow ensures each step completes successfully before the next one begins, and if any step fails, it alerts the right people and provides options for recovery.

### 6. Setting Up Event Triggers

**Amazon S3 Event Notifications - Automatic File Processing**

Every time a Shoprite store uploads their daily sales file to S3, it automatically triggers processing workflows without any human intervention.

**The Process:**
1. **Store Upload:** At closing time, each store automatically uploads their daily sales file to S3
2. **Event Triggered:** S3 detects the new file and sends a notification
3. **Processing Begins:** AWS Lambda function starts processing the file
4. **Validation:** System checks the file format and completeness
5. **Integration:** Valid data gets integrated into corporate systems
6. **Notification:** Store manager gets confirmation that their data was processed successfully

**What happens if something goes wrong:**
- Corrupted files trigger alerts to technical support
- Missing files trigger follow-up with store managers
- Processing errors get logged for investigation
- Backup procedures ensure no data is lost

### 7. Lambda Functions with Kinesis

**Real-Time Response to Real-Time Data**

When transactions flow through Kinesis streams, Lambda functions can process each transaction immediately and take instant action.

**Shoprite Fraud Detection Example:**

As credit card transactions flow through Kinesis:

**Normal Transaction Pattern:**
- Customer #12345 typically spends R300-500 per visit
- Usually shops at Store #23 in Cape Town
- Typically buys groceries between 6 PM and 8 PM
- Uses the same credit card every time

**Suspicious Transaction Detected:**
- Same Customer #12345
- Suddenly spending R3,000 (10x normal)
- At Store #156 in Durban (500km from normal location)
- At 2 AM (unusual time)
- Different credit card number

**Lambda Function Response (within 2 seconds):**
1. Flag transaction for manual review
2. Send alert to fraud investigation team
3. Temporarily hold the transaction from processing
4. Send SMS to customer asking for confirmation
5. Log all details for investigation
6. Update fraud detection model with new patterns

**Why Lambda + Kinesis Works So Well:**
- **Speed:** Response within seconds, not minutes
- **Scale:** Can handle thousands of transactions simultaneously
- **Cost:** Only pay when fraud is actually detected
- **Reliability:** Automatic retries if processing fails

### 8. Managing IP Allowlists

**Security Without Blocking Business**

Shoprite needs to protect their systems from unauthorized access, but they also need to ensure legitimate systems can connect reliably.

**The Challenge:**
- 2,900 stores with different internet connections
- Suppliers connecting from various locations
- Staff working from multiple offices
- Third-party services that need system access
- All while blocking hackers and unauthorized access

**Shoprite's IP Allowlist Strategy:**

**Store Networks:**
- Each store's internet connection gets added to the allowlist
- Backup connections (mobile hotspots) are pre-approved
- New stores get temporary access during setup
- Old store addresses are removed when stores close

**Supplier Access:**
- Major suppliers get dedicated IP ranges for their integration systems
- Smaller suppliers use secure VPN connections
- Emergency access procedures for critical suppliers
- Regular review and cleanup of unused access

**Geographic Considerations:**
- South African stores have different IP ranges than Kenyan stores
- Country-specific regulations affect access patterns
- Cross-border data transfer considerations
- Disaster recovery sites in different regions

### 9. Implementing Throttling and Rate Limits

**Protecting Systems from Overload**

Just like a highway has speed limits to prevent accidents, data systems need rate limits to prevent crashes.

**DynamoDB Throttling at Shoprite:**

**The Problem:** During Black Friday sales, transaction volumes spike 10x normal levels, potentially overwhelming the customer database.

**The Solution:**
- **Gradual Scaling:** DynamoDB automatically increases capacity as demand grows
- **Request Buffering:** Excess requests wait in queue rather than being rejected
- **Priority Handling:** Critical operations (like payment processing) get priority over less critical ones (like loyalty point updates)
- **Graceful Degradation:** If systems get overwhelmed, non-essential features temporarily stop working, but core functions continue

**Amazon RDS Rate Limiting:**

**The Challenge:** Shoprite's reporting system could overwhelm their operational database if not properly controlled.

**The Approach:**
- **Read Replicas:** Heavy reporting queries run against copies of data, not the main database
- **Connection Pooling:** Limit total number of simultaneous database connections
- **Query Timeouts:** Long-running queries get cancelled automatically
- **Resource Monitoring:** Automatic alerts when database load gets too high

**Kinesis Rate Management:**

**Scaling Strategy:**
- **Shard Monitoring:** Watch for shards approaching their limits
- **Automatic Scaling:** Add more shards when needed
- **Load Distribution:** Spread data evenly across shards
- **Burst Handling:** Temporary capacity increases during peak times

### 10. Managing Fan-in and Fan-out Patterns

**Fan-out: One Source, Many Destinations**

Think of fan-out like a sprinkler system - one water source (transaction) sprays to many different areas (systems) that need it.

**Shoprite Transaction Fan-out Example:**

When a customer makes a purchase, that single transaction needs to go to multiple systems:

**Single Transaction Fans Out To:**
1. **Payment System** - Process the credit card charge
2. **Inventory System** - Reduce stock levels
3. **Loyalty System** - Add points to customer account
4. **Analytics System** - Update sales reports
5. **Fraud Detection** - Check for suspicious patterns
6. **Supply Chain** - Update demand forecasting
7. **Marketing System** - Update customer preferences
8. **Financial System** - Record revenue
9. **Tax System** - Calculate tax obligations

**Why This Matters:**
If the fan-out system fails, a customer might get charged but their loyalty points don't get added, or inventory doesn't get updated. This creates customer service nightmares and business problems.

**Fan-in: Many Sources, One Destination**

Fan-in is like a funnel - many streams of data flow into one system for processing.

**Shoprite Analytics Fan-in Example:**

To understand overall business performance, Shoprite needs to combine data from many sources:

**Multiple Sources Fan Into Analytics:**
1. **POS Systems** from 2,900 stores
2. **Online Shopping** data
3. **Mobile App** usage data
4. **Supplier Delivery** information
5. **Weather Data** from external APIs
6. **Economic Indicators** from government sources
7. **Social Media** sentiment analysis
8. **Competitor Pricing** data
9. **Staff Performance** metrics

**All This Flows Into:** Business Intelligence system that creates executive dashboards showing complete business health.

**The Technical Challenge:**
Different sources send data in different formats, at different times, with different levels of reliability. The fan-in system needs to handle all this complexity and still produce accurate, timely reports.

**Shoprite's Fan-in Solution:**
- **Data Standardization:** Convert all incoming data to common formats
- **Timing Coordination:** Wait for all sources before generating reports
- **Missing Data Handling:** Proceed with partial data if some sources are delayed
- **Quality Checks:** Validate data from each source before combining
- **Error Recovery:** Handle cases where some data sources are unavailable
```

### 6. Stateful vs Stateless Data Transactions



#### 🏪 Product Price Lookup (Stateless)


## 🛠️ Skills Implementation

### 1. Reading Data from Streaming Sources

#### Amazon Kinesis Data Streams

**🏦 Banking Transaction Processing**:


#### Amazon MSK (Managed Streaming for Apache Kafka)

**🏪 Multi-Store Data Collection**:

#### DynamoDB Streams - Tracking Every Change

**Understanding Change Tracking:**

Think of DynamoDB Streams like a detailed diary that automatically records every change that happens to your data. Every time someone updates a customer's information or changes an account balance, DynamoDB writes down exactly what changed, when it changed, and what the old and new values were.

**Shoprite Customer Profile Tracking Example:**

When a customer updates their address in the mobile app:

**What Happens Behind the Scenes:**
1. **Original Data:** Customer #12345 lives at "123 Main Street, Cape Town"
2. **Change Made:** Customer updates address to "456 New Avenue, Durban"
3. **Stream Record Created:** DynamoDB automatically creates a record showing:
   - Customer ID: #12345
   - Field Changed: Address
   - Old Value: "123 Main Street, Cape Town"
   - New Value: "456 New Avenue, Durban"
   - Timestamp: When the change happened
   - Who Made Change: Mobile app on behalf of customer

**Automatic Actions Triggered:**
- **Marketing System:** Update regional marketing campaigns for this customer
- **Delivery Service:** Update default delivery location for online orders
- **Store Recommendations:** Suggest stores near the new address
- **Analytics:** Update customer location analytics and regional trends
- **Compliance:** Ensure tax calculations use correct regional rates

**Why This Matters for Business:**
Without change tracking, Shoprite wouldn't know when customers move, change phone numbers, or update preferences. This means they might keep sending promotions for the Cape Town store to someone who now lives in Durban, or miss opportunities to recommend nearby stores.

### 2. Reading Data from Batch Sources

#### Amazon S3 with AWS Glue - Processing Large Data Files

**Understanding Batch Processing:**


**Shoprite Daily Sales Processing Example:**

Every night after all stores close, Shoprite needs to process the day's sales data to prepare reports for management, update inventory systems, and plan for the next day.

**The Daily Process:**
1. **Data Collection:** Each of the 2,900 stores uploads their daily sales file to S3 cloud storage
2. **File Organization:** Files are organized by date and region (e.g., "sales/2024/10/15/western-cape/store-123.csv")
3. **Processing Begins:** At midnight, AWS Glue starts processing all the files
4. **Data Transformation:** Raw sales data gets cleaned, validated, and summarized
5. **Report Generation:** Summary reports are created for different departments
6. **Data Storage:** Processed data goes into the data warehouse for long-term analysis

**What AWS Glue Does for Shoprite:**

**Data Discovery:** Glue automatically identifies that sales files contain columns like:
- Store ID, Product Code, Quantity Sold, Price, Customer ID, Timestamp, Payment Method

**Data Cleaning:** Glue automatically:
- Removes duplicate transactions
- Fixes formatting issues (like dates in different formats)
- Handles missing data (like when a barcode scanner fails)
- Validates data (like ensuring prices are positive numbers)

**Data Transformation:** Glue converts raw transaction data into useful business information:
- Daily sales totals by store
- Best-selling products by region
- Peak shopping hours analysis
- Payment method preferences by location

**Why Glue Instead of Manual Processing:**
- **Speed:** Can process 2,900 store files in 30 minutes instead of days
- **Reliability:** Never forgets to process a file or makes calculation errors
- **Scalability:** Can handle Black Friday volumes (10x normal data) without changes
- **Cost:** Only pay for processing time used, not 24/7 server costs

#### AWS Lambda for Small File Processing

**Understanding Serverless Processing:**

Lambda is like having a team of temporary workers who show up exactly when you need them, do their specific job, and then disappear until needed again. You only pay for the actual work they do.

**Shoprite Store Manager Report Example:**

Every hour, each store manager uploads a small report about their store's status - things like staff levels, temperature readings for refrigeration units, and any issues that need attention.

**The Automatic Process:**
1. **Upload Trigger:** Store manager uploads report to S3
2. **Instant Response:** Lambda function immediately starts processing
3. **File Processing:** Lambda reads the report and extracts important information
4. **Alert Generation:** If there are issues (like a refrigeration unit running too hot), Lambda immediately sends alerts
5. **Dashboard Update:** Lambda updates the regional manager's dashboard with current store status
6. **Clean Up:** Lambda finishes and disappears until the next report arrives

**Why Lambda Works Well for Small Files:**
- **Instant Response:** Starts processing within milliseconds of file upload
- **No Waste:** Only runs when needed, not sitting idle waiting for files
- **Automatic Scaling:** Can handle one file or 1,000 files simultaneously
- **Cost Effective:** For small files, costs less than $0.01 per processing event

### 3. Understanding Different Types of Schedulers

#### Time-Based Scheduling - Running Tasks at Specific Times

**Shoprite's Scheduled Operations:**

Just like how stores have opening and closing times, data processing systems need to run different tasks at optimal times throughout the day.

**Why Specific Timing Matters:**
- **System Performance:** Heavy processing during low-usage hours
- **Business Impact:** Price updates before stores open prevent customer confusion
- **Data Accuracy:** Processing complete days of data for accurate reports
- **Cost Optimization:** Using cheaper computing hours during off-peak times

#### Event-Based Scheduling - Responding to Real Events

**Understanding Event-Driven Processing:**

Instead of running tasks at specific times, event-based systems respond immediately when something important happens. It's like having a smart alarm system that knows the difference between a cat walking by and an actual intruder.

**Shoprite Event Examples:**

**Event:** "New supplier delivery truck arrives at distribution center"
**Immediate Actions:**
- Automatically create receiving paperwork
- Alert warehouse staff about incoming delivery
- Update expected inventory levels
- Schedule quality inspection
- Prepare shipping instructions for stores

**Event:** "Customer complaint received via social media"
**Immediate Actions:**
- Create customer service ticket
- Alert appropriate store manager
- Check customer's purchase history
- Flag for follow-up within 2 hours
- Update brand monitoring dashboard

**Event:** "Store refrigeration temperature rises above safe level"
**Immediate Actions:**
- Alert store manager and maintenance team
- Lock affected products from sale
- Schedule emergency repair service
- Begin moving products to backup refrigeration
- Document incident for health department compliance

### 4. Event Triggers - Automatic Responses to Data Events

#### File Upload Triggers - Instant Processing

**The Challenge:** Shoprite can't have stores wait for their data to be processed. Whether it's a daily sales report or an urgent inventory update, the system needs to respond immediately when files are uploaded.

**How Automatic File Processing Works:**

**Step 1: File Detection**
- Store manager uploads monthly inventory file to cloud storage
- System immediately detects the new file
- Identifies file type and which store it came from

**Step 2: Automatic Validation**
- Checks if file format is correct
- Ensures all required data fields are present
- Validates data makes sense (no negative quantities, reasonable prices)

**Step 3: Processing Decision**
- Small files (under 100MB): Process immediately with Lambda
- Large files (over 100MB): Queue for batch processing with Glue
- Urgent files: Process immediately regardless of size

**Step 4: Results and Notifications**
- Store manager gets confirmation email: "Your inventory file was processed successfully"
- If problems found: "Your inventory file has 5 items that need attention" with detailed list
- System automatically updates corporate inventory databases

**Why This Matters:**
Without automatic triggers, someone would need to manually check for new files every few minutes, 24/7. Files could sit unprocessed for hours, delaying important business decisions.

### 5. Lambda Integration with Kinesis - Real-Time Processing

**Understanding the Partnership:**

Kinesis is like a massive conveyor belt moving data, and Lambda functions are like skilled workers stationed along the belt who can examine each item and take immediate action when needed.

**Shoprite Real-Time Inventory Example:**

**The Flow:**
1. **Customer buys milk at Store #45:** Transaction enters Kinesis stream
2. **Lambda immediately processes:** Checks current milk inventory at Store #45
3. **Inventory Status:** Only 12 cartons left (normally stock 200)
4. **Automatic Action:** Lambda immediately:
   - Orders emergency milk delivery from nearest distribution center
   - Alerts store manager about low stock
   - Updates central inventory system
   - Checks if other nearby stores are also low on milk
   - Adjusts delivery truck route to prioritize milk delivery

**Real-Time Fraud Detection Example:**

**The Scenario:** 
1. **Transaction Enters Stream:** Customer #789 attempts to buy R5,000 worth of gift cards
2. **Lambda Analysis (within 2 seconds):**
   - Normal spending pattern: R200-400 per visit
   - Purchase is 12x higher than normal
   - Customer usually buys groceries, never gift cards
   - Transaction at unusual time (2 AM)
   - Different payment method than usual

3. **Immediate Response:**
   - Hold transaction for manual review
   - Send SMS to customer: "Unusual transaction detected, please confirm"
   - Alert fraud investigation team
   - Log all details for investigation
   - Continue monitoring customer's account for additional suspicious activity

**Why Real-Time Processing Matters:**
- **Fraud Prevention:** Stop fraudulent transactions before money is lost
- **Inventory Management:** Prevent stockouts that lose sales
- **Customer Experience:** Address issues before customers notice problems
- **Operational Efficiency:** Automate responses that would otherwise require manual intervention
### 6. Managing System Load and Rate Limits

#### Understanding System Throttling

**What is Throttling?**

Think of throttling like traffic lights at a busy intersection. Without traffic lights, cars would crash into each other. With properly timed lights, traffic flows smoothly even during rush hour. In data systems, throttling prevents crashes by controlling how much work the system does at once.

**Shoprite Black Friday Example:**

During Black Friday sales, Shoprite might experience 10 times normal transaction volume. Without proper throttling:
- **Crashes:** Systems could crash from overload
- **Lost Sales:** Customers can't complete purchases
- **Data Loss:** Transactions might not be recorded properly
- **Customer Frustration:** Long wait times and error messages

**With Proper Throttling:**
- **Smooth Operations:** Systems handle the load gracefully
- **Customer Experience:** Slightly slower response times, but no crashes
- **Data Integrity:** All transactions are recorded correctly
- **Business Continuity:** Sales continue throughout the peak period

#### DynamoDB Throttling - Database Load Management

**Understanding Database Capacity:**

DynamoDB is like a restaurant kitchen. It can serve a certain number of meals per hour based on:
- Number of chefs (provisioned capacity)
- Complexity of dishes (data operations)
- Kitchen equipment (infrastructure)
- Available ingredients (system resources)

**Shoprite Customer Database Example:**

**Normal Operations:**
- 1,000 customer profile updates per minute
- System handles this easily with standard capacity

**Black Friday Rush:**
- 15,000 customer profile updates per minute
- Without throttling: Database crashes
- With throttling: Database processes requests as fast as safely possible, queues the rest

**How Throttling Helps:**

**Request Queuing:** Instead of rejecting excess requests, they wait in line
**Automatic Scaling:** System gradually increases capacity as demand grows
**Priority Handling:** Critical operations (like payments) get priority over less critical ones (like loyalty point updates)
**Graceful Degradation:** Non-essential features temporarily slow down to keep essential ones running

**Business Impact Without Throttling:**
- Lost revenue from failed transactions
- Customer service complaints about system errors
- Data corruption from overloaded systems
- Reputation damage from poor system performance

**Business Benefits With Throttling:**
- Consistent system performance even during peaks
- No lost transactions or data corruption
- Customer confidence in system reliability
- Ability to handle seasonal volume spikes
#### Kinesis Data Streams - Managing High-Volume Data Flow

**Understanding Stream Capacity:**

Kinesis streams are like highways with multiple lanes (called shards). Each shard can handle a specific amount of traffic. When traffic exceeds capacity, you get congestion.

**Shoprite Data Stream Management:**

**Normal Traffic:**
- 1,000 transactions per second across all stores
- 2 shards easily handle this volume
- Smooth data flow to all processing systems

**Black Friday Traffic:**
- 15,000 transactions per second
- Original 2 shards become overwhelmed
- System automatically adds more shards (like opening more highway lanes)
- Traffic continues flowing smoothly with increased capacity

**Retry Strategy When Systems Get Overwhelmed:**

Instead of losing data when systems are busy, Shoprite uses a "polite retry" approach:

1. **First Attempt:** Try to send data immediately
2. **If Busy:** Wait 0.1 seconds and try again
3. **Still Busy:** Wait 0.2 seconds and try again
4. **Still Busy:** Wait 0.4 seconds and try final attempt
5. **If All Fail:** Store data safely for later processing (nothing gets lost)

This approach ensures no data is lost even during the busiest shopping periods.

### 7. Fan-in and Fan-out Patterns - Managing Data Distribution

#### Fan-in: Collecting Data from Multiple Sources

**Understanding Data Collection:**

Fan-in is like a funnel - many streams of data flowing into one central location for processing. This allows Shoprite to get a complete picture of their business by combining data from all sources.

**Shoprite Fan-in Example - Daily Business Intelligence:**

**Multiple Data Sources Flow Into Central Analytics:**

**Source 1 - Store Sales Data:**
- 2,900 stores each send their daily sales
- Data includes: products sold, quantities, prices, timestamps
- Shows which products are popular at which locations

**Source 2 - Online Shopping Data:**
- Website and mobile app transactions
- Customer browsing behavior and preferences
- Shows online vs in-store shopping patterns

**Source 3 - Supply Chain Data:**
- Delivery schedules and inventory levels
- Supplier performance metrics
- Cost and pricing information

**Source 4 - Customer Service Data:**
- Support tickets and complaints
- Customer satisfaction scores
- Common issues and resolutions

**Source 5 - External Market Data:**
- Competitor pricing information
- Economic indicators and trends
- Weather data affecting shopping patterns

**Central Processing Result:**
All this data combines to create executive dashboards showing complete business health, helping management make informed decisions about pricing, inventory, staffing, and strategy.

#### Fan-out: Distributing Single Events to Multiple Systems

**Understanding Event Broadcasting:**

Fan-out is like a sprinkler system - one water source (event) spreads to many different areas (systems) that each need that information.

**Shoprite Transaction Fan-out Example:**

When Mrs. Johnson buys groceries worth R350 at Store #123, that single transaction triggers updates across multiple systems:

**Transaction Event Fans Out To:**

**System 1 - Payment Processing:**
- Charge Mrs. Johnson's credit card
- Handle payment confirmation or decline
- Update payment processing records

**System 2 - Inventory Management:**
- Reduce stock levels for purchased items
- Check if any items need reordering
- Update shelf availability displays

**System 3 - Customer Loyalty:**
- Add points to Mrs. Johnson's loyalty account
- Check if she qualifies for any rewards
- Update her purchase history and preferences

**System 4 - Financial Records:**
- Record revenue for Store #123
- Update daily/monthly sales totals
- Prepare data for accounting and tax systems

**System 5 - Analytics and Reporting:**
- Update sales trend analysis
- Contribute to customer behavior models
- Feed into business intelligence dashboards

**System 6 - Supply Chain:**
- Update demand forecasting models
- Adjust automatic reordering quantities
- Inform supplier relationship management

**Why Fan-out Matters:**
Without proper fan-out, each system would need to separately track the same transaction, leading to inconsistencies, duplicate work, and potential errors. With fan-out, one event automatically updates all relevant systems consistently.
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