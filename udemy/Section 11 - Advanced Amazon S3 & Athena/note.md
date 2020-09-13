# S3 MFA Delete
- MFA (multi factor authentication) forces user to generate a code on a device (usually a mobile phone or hardware) before doing important operations on S3
- To use MFA-Delete, enable Versioning on the S3 bucket
- You will need MFA to
    - permanently delete an object version
    - suspend versioning on the bucket
- You won’t need MFA for
    - enabling versioning
    - listing deleted versions
- Only the bucket owner (root account) can enable/disable MFA-Delete
- MFA-Delete currently can only be enabled using the CLI

# S3 Default Encryption vs Bucket Policies
- The old way to enable default encryption was to use a bucket policy and refuse any HTTP command without the proper headers
- The new way is to use the “default encryption” option in S3
- Note: Bucket Policies are evaluated before “default encryption”

# S3 Access Logs
- For audit purpose, you may want to log all access to S3 buckets
- Any request made to S3, from any account, authorized or denied, will be logged into another S3 bucket
- That data can be analyzed using data analysis tools…
- Or Amazon Athena as we’ll see later in this section!

## Infinite logging loop
- Do not set your logging bucket to be the monitored bucket
- It will create a logging loop, and your bucket will grow in size exponentially

# S3 Replication (Cross Region and Same Region)
- Asynchronous replication from bucket to another same or cross region bucket
- CRR - Use cases: compliance, lower latency access, replication across accounts
- SRR – Use cases: log aggregation, live replication between production and test accounts

After you activate S3 replication, only the new objects are replicated. So it's not retroactive. It will not copy your existing states of your S3 buckets.

Any delete operation is not replicated.

There is no "chaining" of replication

# S3 Pre-signed URLs
- Can generate using SDK or CLI
- Inherit the permissions of the person who generated the URL for GET / PUT

# S3 Storage Tiers + Glacier
## S3 Standard – General Purpose
- Sustain 2 concurrent facility failures
- Multi AZs

## S3 Standard – Infrequent Access (IA)
- Suitable for data that is less frequently accessed, but requires rapid access when needed
- Low cost compared to Amazon S3 Standard
- Sustain 2 concurrent facility failures
- Use Cases: As a data store for disaster recovery, backups…
- Multi AZ

## S3 One Zone - Infrequent Access (IA)
- Data is stored in a single availability zone
- High durability (99.999999999%) of objects in a single AZ; data lost when AZ is destroyed
- 99.5% Availability
- Low latency and high throughput performance
- Supports SSL for data at transit and encryption at rest
- Low cost compared to IA (by 20%)
- Use Cases: Storing secondary backup copies of on-premise data, or storing data you can recreate

## S3 Intelligent Tiering
- Same low latency and high throughput performance of S3 Standard
- Small monthly monitoring and auto-tiering fee
- Automatically moves objects between two access tiers based on changing access patterns

So it will move objects between S3 general purpose, S3 IA. And so it will choose for you if your object is less frequently accessed or not.

## Amazon Glacier 
- Cold archive
- Each item is called "Archive" (not object)
- Archives are stored in "Vaults" (not bucket)
- Low cost object storage meant for archiving / backup 
- Data is retained for the longer term (10s of years) • Alternative to on-premise magnetic tape storage 
- Average annual durability is 99.999999999% 
- Cost per storage per month ($0.004 / GB) + retrieval cost

## Amazon Glacier & Glacier Deep Archive
- There two tiers within Amazon Glacier
- Amazon Glacier Deep Archive - for long term storage & cheaper

# S3 Lifecycle Policies
- You can transition objects between storage classes
- For infrequently accessed object, move them to STANDARD_IA
- For archive objects you don’t need in real-time, GLACIER or DEEP_ARCHIVE
- Moving objects can be automated using a lifecycle configuration

## S3 Lifecycle Rules
- Transition actions: It defines when objects are transitioned to another storage class.
    - Move objects to Standard IA class 60 days after creation
    - Move to Glacier for archiving after 6 months
- Expiration actions: configure objects to expire (delete) after some time
    - Access log files can be set to delete after a 365 days
    - Can be used to delete old versions of files (if versioning is enabled)
    - Can be used to delete incomplete multi-part uploads
- Rules can be created for a certain prefix (ex - s3://mybucket/mp3/*)
- Rules can be created for certain objects tags (ex - Department: Finance)

# S3 Performance
## Baseline Performance
- Amazon S3 automatically scales to high request rates, latency 100-200 ms
- Your application can achieve at least 3,500 PUT/COPY/POST/DELETE and 5,500 GET/HEAD requests per second per prefix in a bucket.
- There are no limits to the number of prefixes in a bucket

## S3 – KMS Limitation
- If you use SSE-KMS, you may be impacted by the KMS limits
- When you upload, it calls the GenerateDataKey KMS API
- When you download, it calls the Decrypt KMS API
- Count towards the KMS quota per second (5500, 10000, 30000 req/s based on region)
- As of today, you cannot request a quota increase for KMS

## How we can optimize it
- Multi-Part upload - can help parallelize uploads
- S3 Transfer Acceleration
    - File -> (public) Edge location -> (Fast, private AWS) S3 Bucket
- S3 Byte-Range Fetches
    - Parallelize GETs by requesting specific byte ranges
    - Better resilience in case of failures
- Can be used to retrieve only partial data (for example the head of a file)

# S3 Select & Glacier Select
- Retrieve less data using SQL by performing server side filtering
- Can filter by rows & columns (simple SQL statements)
- Less network transfer, less CPU cost client-side
- For more complex querying, that's gonna be server less on S3, Amazon Athena

# S3 Event Notifications
- Object name filtering possible (*.jpg)
- Use case: generate thumbnails of images uploaded to S3
- Can create as many “S3 events” as desired
- S3 event notifications typically deliver events in seconds but can sometimes take a minute or longer
- If two writes are made to a single non-versioned object at the same time, it is possible that only a single event notification will be sent
- If you want to ensure that an event notification is sent for every successful write, you can enable versioning on your bucket.

# Athena Overview
- Serverless service to perform analytics directly against S3 files
- Use cases: Business intelligence / analytics / reporting, analyze & query VPC Flow Logs, ELB Logs, CloudTrail trails, etc...
- Exam Tip: Analyze data directly on S3 => use Athena

# S3 Lock Policies & Glacier Vault Lock
- S3 Object Lock
    - Adopt a WORM (Write Once Read Many) model
    - Block an object version deletion or modification for a specified amount of time
- Glacier Vault Lock
    - Adopt a WORM (Write Once Read Many) model
    - Lock the policy for future edits (can no longer be changed)
    
