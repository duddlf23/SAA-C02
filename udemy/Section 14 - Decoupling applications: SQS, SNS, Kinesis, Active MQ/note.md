Synchronous between applications can be problematic if there are sudden spikes of traffic

What if you need to suddenly encode 1000 videos but usually it’s 10?

- In that case, it’s better to decouple your applications,
    - using SQS: queue model
    - using SNS: pub/sub model
    - using Kinesis: real-time streaming model
- These services can scale independently from our application!

# Amazon SQS - Standard Queues Overview
- Producer - Send messages to SQS
- You can have multiple producers sending many messages into an SQS queue
- Consumer - Poll the messages from the queue
- It will process it and then delete it back from the queue
- May have multiple consumers consuming messages from an SQS queue

## Producing Messages
- Messages that are up to 256kb
- It is persisted in SQS until a consumer reads that message and deletes it, which signifies that the message has been processed.
- Message retention - default 4 days, up to 14 days
- Can have duplicate messages (at least once delivery, occasionally)
- Can have out of order messages (best effort ordering)
- Example: send an order to be processed
    - Order id
    - Customer id
    - Any attributes you want

## Consuming Messages
- Poll -> Process -> Delete

## SQS with ASG
- ApproximateNumberOfMessages - CloudWatch Metric - Queue Length

## SQS - Security
- Encryption:
    - In-flight encryption using HTTPS API
    - At-rest encryption using KMS keys
    - Client-side encryption if the client wants to perform encryption/decryption itself
- Access Controls: IAM policies to regulate access to the SQS API
- SQS Access Policies (similar to S3 bucket policies)
    - Useful for cross-account access to SQS queues
    - Useful for allowing other services (SNS, S3…) to write to an SQS queue

# Message Visibility Timeout
- After a message is polled by a consumer, it becomes invisible to other consumers
- After the message visibility timeout is over, the message is “visible” in SQS

# SQS - Dead Letter Queues
- We can set a threshold of how many times a message can go back to the queue
- After the MaximumReceives threshold is exceeded, the message goes into a dead letter queue (DLQ)
- Make sure to process the messages in the DLQ before they expire (DLQ is also SQS):
    - Good to set a retention of 14 days in the DLQ
    
# SQS - Delay Queue
- Delay a message (consumers don’t see it immediately) up to 15 minutes
- Default is 0 seconds (message is available right away)
- Can set a default at queue level
- Can override the default on send using the DelaySeconds parameter

# SQS - FIFO Queues
- More ordering guarantee that we can get from a standard queue
- Limited throughput
- Exactly-once send capability (by removing duplicates)
- Messages are processed in order by the consumer

# Amazon SNS - Overview
- Pub/Sub model
- Each subscriber to the topic will get all the messages 
- Subscribers can be:
    - SQS
    - HTTP / HTTPS (with delivery retries – how many times)
    - Lambda
    - Emails
    - SMS messages
    - Mobile Notifications
- Many AWS services can send data directly to SNS for notifications

## Security
Very similar with SQS

# SNS and SQS - Fan Out Pattern
- SNS + SQS: Fan Out - Very common pattern
- You want to send the same message into many different SQS queues but to do so, you're going to use SNS.
- Push once in SNS, receive in all SQS queues that are subscribers
- Fully decoupled, no data loss
- SQS allows for: data persistence, delayed processing and retries of work
- Ability to add more SQS subscribers over time
- SNS cannot send messages to SQS FIFO queues (AWS limitation)

# Kinesis Data Streams Overview
- Managed alternative to Apache Kafka
- The data is by the way automatically replicated to 3 Availability Zones.
- Kinesis Streams: low latency streaming ingest at scale
- Kinesis Analytics: perform real-time analytics on streams using SQL
- Kinesis Firehose: load streams into S3, Redshift, ElasticSearch

## Kinesis Streams
- Streams are divided in ordered Shards / Partitions
- Wanna scale up - Add shards
- Data retention - 1 day by default - The data is still there against with SQS
- Ability to reprocess / replay data
- Multiple applications can consume the same stream
- Once data is inserted in Kinesis, it can’t be deleted 

## Kinesis Streams Shards
- Billing is per shard provisioned
- The number of shards can evolve over time (reshard / merge)
- Records are ordered per shard
- ProvisionedThroughputExceeded Exceptions
    - Happens when sending more data (exceeding MB/s or TPS for any shard)
    - Make sure you don’t have a hot shard (such as your partition key is bad and too much data goes to that partition)
- Solution:
    - Retries with backoff
    - Increase shards (scaling)
    - Ensure your partition key is a good one

## AWS Kinesis API – Consumers
- Can use Kinesis Client Library (in Java, Node, Python, Ruby, .Net)
    - KCL uses DynamoDB to checkpoint offsets
    - KCL uses DynamoDB to track other workers and share the work amongst shards

## Kinesis Security
- Control access / authorization using IAM policies 
- Encryption in flight using HTTPS endpoints 
- Encryption at rest using KMS 
- Possibility to encrypt / decrypt data client side (harder)
- VPC Endpoints available for Kinesis to access within VPC

# Kinesis Firehose & Kinesis Data Analytics
- Fully Managed Service, no administration, automatic scaling, serverless
- Load data into Redshift / Amazon S3 / ElasticSearch / Splunk
- Near Real Time
    - 60 seconds latency minimum for non full batches
    - Or minimum 32 MB of data at a time
- Supports many data formats, conversions, transformations, compression
- Pay for the amount of data going through Firehose

## Kinesis Data Analytics
Conceptually Kinesis Analytics can take data from kinesis data streams and kinesis data firehose and perform some queries. Some rather complex queries.

# Data Ordering for Kinesis vs SQS FIFO
Sometimes is going to be better to use SQS FIFO. If you want to have a dynamic number of consumers based on the number of group IDs, and sometimes it could be better to use Kinesis Data Stream if you have say 10,000 trucks and you need to send it lot of data, and also have data ordering per shard in your Kinesis Data Stream

# Amazon MQ
- Managed Apache ActiveMQ
- When migrating to the cloud, instead of re-engineering the application to use SQS and SNS, we can use Amazon MQ
- Amazon MQ doesn’t “scale” as much as SQS / SNS
- Amazon MQ runs on a dedicated machine, can run in HA with failover
- Amazon MQ has both queue feature (~SQS) and topic features (~SNS)
 
