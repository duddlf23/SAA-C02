# AWS RDS Overview
- Managed DB service for DB use SQL as a query language
- What type of database engine
    - Postgres
    - MySQL
    - MariaDB
    - Oracle
    - Microsoft SQL Server
    - Aurora (AWS Proprietary database)

Scaling capability
    - vertical - increasing the instance type
    - horizontal - adding read replicas

Storage backed by EBS (gp2 or io1)

Can't SSH into instances

# RDS Read Replicas vs Multi AZ
Understand the difference between RDS Read Replicas, and Multi AZ

## Read Replicas
- help to scale reads
- Up to 5
- within AZ, cross AZ, cross region 
- Replication is ASYNC, so reads are eventually consistent
- Replicas can be promoted to their own DB

## RDS Read Replicas – Network Cost
- In AWS there’s a network cost when data goes from one AZ to another
- To reduce the cost, you can have your Read Replicas in the same AZ

## RDS Multi AZ
- SYNC replication
- One DNS name - automatic app failover to standby
- Increase availability
- Failover in case of loss of AZ, loss of network, instance or storage failure for the Master db
- The standby database is just for standby. No one can read to it, no one can write to it.
- No manual intervention in apps
- Not used for scaling

# Hands on
- IAM db authentication option
    - database user credentials through AWS IAM users and roles
- Deletion protection
    - we won't be able to delete our database without removing first the deletion protection.

# RDS Encryption + Security
Data at rest - data that's not in movement
- At rest encryption
    - Possibility to encrypt the master & read replicas with AWS KMS - AES-256 encryption
    - Encryption has to be defined at launch time
    - If the master is not encrypted, the read replicas cannot be encrypted
    - Transparent Data Encryption (TDE) available for Oracle and SQL Server
    
- In-flight encryption
    - While being sent from your clients into your database.
    - SSL certificates to encrypt data to RDS in flight
    - Provide SSL options with trust certificate when connecting to database
    
## Encryption RDS backups
- Snapshots of un-encrypted RDS databases are un-encrypted
- Snapshots of encrypted RDS databases are encrypted
- Can copy a snapshot un-encrypted into an encrypted one

## To encrypt an un-encrypted RDS database
- Create a snapshot of the un-encrypted database
- Copy the snapshot and enable encryption for the snapshot
- Restore the database from the encrypted snapshot
- Migrate applications to the new database, and delete the old database

## RDS Security - Network & IAM
- Network Security
    - using sg
- Access Management
    - IAM policy
    - IAM-based authentication - Mysql, Postgresql

## RDS - IAM Authentication
- Benefits:
    - Network in/out must be encrypted using SSL
    - IAM to centrally manage users instead of DB
    - Can leverage IAM Roles and EC2 Instance profiles for easy integration
    
# Aurora Overview
- Aurora is a proprietary technology from AWS (not open sourced)
- It's going to be compatible with Postgres and MySQL
- If you connect, as if you were connecting to your Postgres or a MySQL database, then it will work.
- High performance
- Automatically grows in increments
- Aurora can have 15 replicas while MySQL has 5, and the replication process is faster (sub 10 ms replica lag)
- Failover in Aurora is instantaneous. It’s HA (High Availability) native
- More cost

## Aurora HA and Read Scaling
- Multi AZ for RDS
- There's a master in Aurora and that's where we'll take writes.
- Master + up to 15 Aurora Read Replicas serve reads
- Read Replicas can become the master in case the master fails
- Support for cross region replication
- Their storage is going to be replicated, self healing, auto expanding, little blocks by little blocks.

## Aurora DB Cluster
- One writer endpoint - pointing to the master
- Can set up auto scaling of read replicas
- Reader Endpoint - Connection Load Balancing

## Aurora Security
- Same as RDS Security

## Aurora Serverless
- Unpredictable workloads
- No capacity planning needed
- If we get more load, more Amazon Aurora databases will be created for us automatically. If we get less load, then less databases will be going, all the way up to 0 Aurora databases, if we don't have any usage.

## Global Aurora
- Aurora cross region read Replicas
- Aurora Global Database
    - 1 Primary Region (read / write)
    - Up to 5 secondary (read-only) regions, replication lag is less than 1 second
    - Up to 16 Read Replicas per secondary region
    - Helps for decreasing latency
    - Promoting another region (for disaster recovery) has an RTO of < 1 minute
    
# Hands on
- Database features
    - One writer and multiple readers
    - One writer and multiple readers - Parallel query
        - Improve the performance of analytics queries
    - Multiple writers
    - Serverless
- Backtrack
    - quickly rewind the DB cluster to a specific point in time, without having to create another DB cluster
    - Charge for storing changes you make for backtracking
- Select an endpoint that is either the writer endpoint to write or the reader endpoint to read.

# ElastiCache Overview
- The same way RDS is to get managed Relational Databases…
- ElastiCache is to get managed Redis or Memcached
- Caches are in-memory databases with really high performance, low latency
- Helps reduce load off of databases for read intensive workloads
- Helps make your application stateless - by storing in a common cache
- AWS takes care of OS maintenance / patching, optimizations, setup, configuration, monitoring, failure recovery and backups
- Write Scaling using sharding
- Read Scaling using Read Replicas
- Multi AZ with Failover Capability
- RDS for caches
- **Using ElastiCache involves heavy application code changes**

## DB Cache
- Applications queries ElastiCache, if not available, get from RDS and store in ElastiCache.
- Helps relieve load in RDS
- Cache must have an invalidation strategy to make sure only the most current data is used in there. 

## User Session Store
- User logs into any of the application
- The application writes the session data into ElastiCache
- The user hits another instance of our application
- The instance retrieves the data and the user is already logged in
- All the instances can retrieve this data and make sure the user doesn't have to re-authenticate every time

## ElastiCache with solution architecture
- Relieve load off a database
- Share some states

## Redis
- Very similar to RDS
- Database

## Memcached
- Distributed memory object caching system - pure cache
- Non persistent - If your Memcached node goes down then the data is lost. 
- No backup and restore
- Multi-threaded

# Hand on
- If disable encryption transit - No options for Redis-Auth

# ElastiCache for Solution Architect
## ElastiCache – Cache Security
- All caches in ElastiCache:
    - Support SSL in flight encryption
    - Do not support IAM authentication
    - IAM policies on ElastiCache are only used for AWS API-level security
- Redis AUTH
    - You can set a “password/token” when you create a Redis cluster
    - This is an extra level of security for your cache (on top of security groups)
    - Any client that does not have this password or this token will not be able to connect to your Redis cluster and therefore will be rejected.
- Memcached
    - Supports SASL-based authentication (advanced)

- Patterns for ElastiCache
    - Lazy Loading: all the read data is cached, data can become stale in cache
    - Write Through: Adds or update data in the cache when written to a DB (no stale data)
    - Session Store: store temporary session data in a cache (using TTL features)