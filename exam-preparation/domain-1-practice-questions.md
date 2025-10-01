# Domain 1 Practice Questions: Data Ingestion and Transformation

**Time Limit**: 68 minutes (2 minutes per question)  
**Questions**: 34 questions  
**Passing Score**: 80% (27+ correct answers)

## Instructions
- Choose the best answer for each question
- Some questions may have multiple correct answers (select all that apply)
- Consider cost-effectiveness, scalability, and AWS best practices
- Mark your answers and check against the answer key at the end

---

## Question 1
A retail company needs to process customer transaction data in real-time to detect fraudulent activities. The data arrives at a rate of 50,000 records per second with each record being approximately 2KB in size. Which AWS service would be MOST appropriate for ingesting this data?

A) Amazon SQS  
B) Amazon Kinesis Data Streams  
C) AWS Glue  
D) Amazon S3 Transfer Acceleration

## Question 2
Your organization wants to migrate data from an on-premises MySQL database to Amazon RDS MySQL with minimal downtime. The database contains 500GB of data and receives continuous updates. Which AWS service should you use?

A) AWS DataSync  
B) AWS Database Migration Service (DMS)  
C) AWS Glue  
D) Amazon Kinesis Data Firehose

## Question 3
A data engineer needs to convert JSON log files stored in Amazon S3 to Parquet format for better query performance in Amazon Athena. The conversion should happen automatically when new files are uploaded. Which solution provides the LEAST operational overhead?

A) Use AWS Lambda triggered by S3 events to convert files  
B) Use Amazon Kinesis Data Firehose with data format conversion  
C) Schedule AWS Glue jobs to run every hour  
D) Use Amazon EMR with automatic scaling

## Question 4
Which of the following are key components of Amazon Kinesis Data Streams? (Select TWO)

A) Delivery streams  
B) Shards  
C) Partition keys  
D) Firehose destinations  
E) Analytics applications

## Question 5
A company streams IoT sensor data to Amazon Kinesis Data Streams. They need to ensure that data from the same sensor always goes to the same shard for ordered processing. How should they configure the partition key?

A) Use a random UUID for each record  
B) Use the sensor ID as the partition key  
C) Use the timestamp as the partition key  
D) Use a combination of sensor ID and timestamp

## Question 6
Your team is building a data lake on Amazon S3 and needs to automatically discover and catalog new datasets as they arrive. Which AWS service combination would be MOST effective?

A) AWS Glue Crawlers and AWS Glue Data Catalog  
B) Amazon Athena and AWS CloudFormation  
C) AWS Lambda and Amazon DynamoDB  
D) Amazon EMR and Apache Hive Metastore

## Question 7
A streaming data pipeline processes clickstream data and needs to handle temporary spikes in traffic that can increase data volume by 300%. Which AWS service would automatically scale to handle this without manual intervention?

A) Amazon Kinesis Data Streams  
B) Amazon Kinesis Data Firehose  
C) Amazon SQS  
D) AWS Glue

## Question 8
When using AWS Glue for ETL operations, what is the primary benefit of enabling job bookmarks?

A) Improved job performance  
B) Automatic error recovery  
C) Processing only new or changed data  
D) Better monitoring and logging

## Question 9
A company needs to replicate data changes from a PostgreSQL database to a data warehouse in real-time. Which AWS DMS feature should they enable?

A) Full Load migration  
B) Schema conversion  
C) Change Data Capture (CDC)  
D) Data validation

## Question 10
Which Amazon Kinesis Data Analytics feature allows you to analyze streaming data over specific time periods?

A) Partition keys  
B) Windowed queries  
C) Reference data  
D) Error streams

## Question 11
Your application generates large log files (500MB each) every 15 minutes. You need to store these files in S3 and make them available for analysis within 30 minutes. Which ingestion strategy would be MOST cost-effective?

A) Stream logs to Kinesis Data Streams then to S3  
B) Use Kinesis Data Firehose with appropriate buffering  
C) Upload directly to S3 using multipart upload  
D) Use AWS Transfer Family

## Question 12
An AWS Glue job fails intermittently with "out of memory" errors when processing large datasets. Which optimization strategy would be MOST effective?

A) Increase the number of workers  
B) Enable job bookmarks  
C) Use a larger worker type (G.2X instead of Standard)  
D) Partition the input data more granularly

## Question 13
Which data format conversion does Amazon Kinesis Data Firehose support natively without requiring custom transformation?

A) CSV to JSON  
B) JSON to Parquet  
C) XML to JSON  
D) Avro to ORC

## Question 14
A company wants to process streaming data with complex joins and aggregations that require maintaining state across multiple records. Which service would be MOST appropriate?

A) AWS Lambda  
B) Amazon Kinesis Data Analytics with Apache Flink  
C) AWS Glue Streaming  
D) Amazon EMR Notebooks

## Question 15
When configuring an AWS Glue crawler for a data source with nested JSON structures, what should you consider to optimize catalog creation?

A) Set appropriate schema change policy  
B) Configure custom classifiers  
C) Enable compression detection  
D) All of the above

## Question 16
Your organization receives daily CSV files from partners via SFTP. Each file is 2-10GB and needs to be processed and loaded into Amazon Redshift. Which architecture would be MOST efficient?

A) SFTP → Lambda → Glue → Redshift  
B) SFTP → S3 → Glue → Redshift  
C) SFTP → Kinesis → Firehose → Redshift  
D) SFTP → EMR → S3 → Redshift

## Question 17
A Kinesis Data Streams application is experiencing throttling errors. The stream has 5 shards and receives 10,000 records per second with an average record size of 1.5KB. What is the MOST likely cause?

A) Total data rate exceeds shard capacity  
B) Individual record size is too large  
C) Partition key distribution is uneven  
D) Consumer application is too slow

## Question 18
Which AWS Glue feature allows you to create ETL jobs without writing code?

A) AWS Glue DataBrew  
B) AWS Glue Studio  
C) AWS Glue Crawlers  
D) AWS Glue Development Endpoints

## Question 19
A company needs to migrate a 10TB Oracle database to Amazon Aurora PostgreSQL. The migration must complete within a maintenance window of 4 hours. Which DMS configuration would be MOST appropriate?

A) Single replication instance with full load  
B) Multiple replication instances with parallel full load  
C) Single replication instance with CDC only  
D) Schema Conversion Tool only

## Question 20
When using Amazon EMR for big data processing, which storage option provides the BEST performance for temporary data during job execution?

A) Amazon EBS  
B) Amazon S3  
C) Instance store (ephemeral storage)  
D) Amazon EFS

## Question 21
A data pipeline needs to handle both real-time streaming data and batch data from the same sources. Which architecture pattern would be MOST effective?

A) Lambda architecture  
B) Kappa architecture  
C) Microservices architecture  
D) Event-driven architecture

## Question 22
Which Kinesis Data Streams configuration provides the highest availability?

A) Single shard in one AZ  
B) Multiple shards in one AZ  
C) Single shard across multiple AZs  
D) Multiple shards across multiple AZs

## Question 23
An AWS Glue job needs to join data from two large datasets (100GB each). The job runs for 8 hours and costs $200. How can you optimize it for better performance and cost?

A) Use more workers with the same worker type  
B) Use fewer workers with larger worker type  
C) Enable Glue job bookmarks  
D) Partition the data and use broadcast joins

## Question 24
Which Amazon Kinesis service would you use to load streaming data directly into Amazon Elasticsearch Service with built-in transformation capabilities?

A) Kinesis Data Streams  
B) Kinesis Data Firehose  
C) Kinesis Data Analytics  
D) Kinesis Video Streams

## Question 25
A company processes log files that arrive irregularly throughout the day. Sometimes files arrive every few minutes, other times hours apart. They want to minimize processing latency while controlling costs. Which Kinesis Data Firehose buffer configuration would be MOST appropriate?

A) Large buffer size, short buffer interval  
B) Small buffer size, short buffer interval  
C) Large buffer size, long buffer interval  
D) Small buffer size, long buffer interval

## Question 26
When designing an ETL pipeline for a data lake, which data organization strategy would provide the BEST query performance in Amazon Athena?

A) Store all data in a single partition  
B) Partition data by date and compress with GZIP  
C) Partition data by date and use columnar format (Parquet)  
D) Use row-based format with indexing

## Question 27
An AWS Lambda function processes records from a Kinesis Data Stream. The function occasionally fails due to downstream service unavailability. How should you configure error handling?

A) Increase Lambda timeout  
B) Configure a dead letter queue  
C) Retry processing within the function  
D) Configure Kinesis retry settings

## Question 28
Which AWS service would be MOST cost-effective for running infrequent, large-scale data processing jobs (once per month, processing 1TB of data)?

A) Amazon EMR with On-Demand instances  
B) Amazon EMR with Spot instances  
C) AWS Glue with standard workers  
D) AWS Batch with Spot instances

## Question 29
A data engineer needs to ensure that sensitive data in transit between AWS services is encrypted. Which services provide encryption in transit by default? (Select TWO)

A) Amazon Kinesis Data Streams  
B) AWS Glue  
C) Amazon S3  
D) Amazon DMS  
E) Amazon EMR

## Question 30
When using AWS Glue Dynamic Frames, what is the primary advantage over standard Apache Spark DataFrames?

A) Better performance  
B) Lower cost  
C) Schema flexibility and error handling  
D) Easier debugging

## Question 31
A company wants to implement a data pipeline that processes streaming data with SQL queries and delivers results to multiple destinations. Which service combination would be MOST appropriate?

A) Kinesis Data Streams + Lambda + S3  
B) Kinesis Data Analytics + Kinesis Data Firehose  
C) Glue Streaming + Step Functions  
D) EMR + Kafka + S3

## Question 32
Which factor MOST significantly affects the cost of Amazon Kinesis Data Streams?

A) Number of records processed  
B) Size of individual records  
C) Number of shards provisioned  
D) Number of consumer applications

## Question 33
An ETL process needs to handle schema evolution where new columns may be added to source data over time. Which AWS Glue feature would be MOST helpful?

A) Job bookmarks  
B) Dynamic frames  
C) Custom transforms  
D) Development endpoints

## Question 34
A data pipeline ingests social media data in JSON format and needs to extract specific fields for analytics. The processing logic is simple but the data volume is high (1TB/day). Which service would provide the BEST balance of cost and simplicity?

A) AWS Lambda with S3 triggers  
B) AWS Glue with Python shell jobs  
C) Amazon EMR with Spark  
D) AWS Glue with Spark jobs

---

## Answer Key

1. **B** - Kinesis Data Streams can handle high-throughput streaming data (50K records/sec × 2KB = 100MB/sec is within capacity with proper sharding)

2. **B** - AWS DMS supports continuous replication with minimal downtime using CDC

3. **B** - Kinesis Data Firehose provides automatic format conversion from JSON to Parquet with minimal operational overhead

4. **B, C** - Shards and partition keys are core components of Kinesis Data Streams

5. **B** - Using sensor ID as partition key ensures data from same sensor goes to same shard

6. **A** - Glue Crawlers automatically discover data and populate the Data Catalog

7. **B** - Kinesis Data Firehose automatically scales without manual intervention

8. **C** - Job bookmarks track processed data to enable incremental processing

9. **C** - Change Data Capture enables real-time replication of data changes

10. **B** - Windowed queries allow analysis over specific time periods

11. **B** - Kinesis Data Firehose with appropriate buffering is cost-effective for this use case

12. **C** - Larger worker types provide more memory to handle large datasets

13. **B** - Firehose natively supports JSON to Parquet conversion

14. **B** - Kinesis Data Analytics with Flink supports complex stateful stream processing

15. **D** - All options are important for optimizing crawler performance with nested JSON

16. **B** - Standard pattern: land in S3, process with Glue, load to Redshift

17. **C** - Uneven partition key distribution causes hot shards and throttling

18. **B** - Glue Studio provides visual, no-code ETL development

19. **B** - Multiple replication instances enable parallel processing for faster migration

20. **C** - Instance store provides the best performance for temporary data

21. **A** - Lambda architecture handles both real-time and batch processing

22. **D** - Multiple shards across AZs provides highest availability

23. **D** - Data partitioning and broadcast joins optimize large dataset processing

24. **B** - Kinesis Data Firehose can load directly to Elasticsearch with transformations

25. **B** - Small buffer, short interval minimizes latency for irregular arrivals

26. **C** - Date partitioning with Parquet format provides best Athena performance

27. **B** - Dead letter queue handles failed records appropriately

28. **B** - EMR with Spot instances is most cost-effective for infrequent large jobs

29. **A, B** - Kinesis and Glue provide encryption in transit by default

30. **C** - Dynamic Frames handle schema variations and errors better

31. **B** - Analytics for SQL processing, Firehose for multi-destination delivery

32. **C** - Number of shards is the primary cost factor for Data Streams

33. **B** - Dynamic frames handle schema evolution effectively

34. **D** - Glue with Spark jobs provides good balance for high-volume simple processing

---

## Scoring Guide

- **27-34 correct (79-100%)**: Excellent! You're ready for this domain
- **24-26 correct (71-78%)**: Good understanding, review missed topics
- **21-23 correct (62-70%)**: Fair knowledge, significant review needed
- **Below 21 (< 62%)**: Substantial study required, revisit lessons

## Next Steps

Based on your score:
- **80%+**: Proceed to [Domain 2 Practice Questions](domain-2-practice-questions.md)
- **Below 80%**: Review [Domain 1 Lessons](../lessons/domain-1-data-ingestion-transformation/README.md) and retake this test