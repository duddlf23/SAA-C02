# Choosing the right database
## Database Types
- RDBMS (=SQL / OLTP): RDS, Aurora - great for joins
- NoSQl - no joins
- Object Store: S3 (for big objects) / Glacier (for backups / archives)
- Data Warehouse (= SQL Analytics / BI): Redshift (OLAP), ATHEna
- Search: ES - free text, unstructured searches
- Graphs: Neptune - displays relationships between data

# RDS
- Managed PostgreSQL / MySQL / Oracle / SQL Server
- Must provision an EC2 instance & EBS Volume type and size
- Support for Read Replicas and Multi AZ
- Security through IAM, Security Groups, KMS , SSL in transit
- IAM authentication (Postgres, MySQL)
- Use case: Store relational datasets (RDBMS / OLTP), perform SQL queries, transactional inserts / update / delete is available 

# Aurora
- Auto healing capability
- Data is held in 6 replicas, across 3 AZ
- Multi AZ, Auto Scaling Read Replicas
- Read Replicas can be Global
- Auto scaling of storage
- Define EC2 instance type
- "Aurora Serverless" option
- Use case: same as RDS, but with less maintenance / more flexibility / more performance / more cost

# ElastiCache
- In-memory K-V data store, sub-millisecond latency
- Must provision EC2 instance type
- Redis Auth instead IAM Auth
- Use Case: Key/Value store, Frequent reads, less writes, cache results for DB queries, store session data for websites, cannot use SQL

# DynamoDB
- DAX (DynamoDB accelerated cluster) for read cache
- Use Case: Serverless applications development (small documents 100s KB), distributed serverless cache, doesn’t have SQL query language available, has transactions capability from Nov 2018

# S3
- Use Case: static files, key value store for big files, website hosting

# Athena
- Query engine on top of S3
- Pay per query
- Use Case: one time SQL queries, serverless queries on S3, log analytics

# Redshift
- Not replacement for RDS
- OLAP
- Columnar storage
- Massively Parallel Query Execution
- Pay based on the instances provisioned
- Can configure Amazon Redshift to automatically copy snapshots (automated or manual) of a cluster to another AWS Region

# Neptune
- Fully managed graph database
- Highly available across 3 AZ, with up to 15 read replicas

# ElasticSearch
- DynamoDB - Can only find pk or indexes, when searching, It's not flexible
