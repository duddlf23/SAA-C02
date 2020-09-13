#  Snowball Overview
- Physical data transport solution that helps moving TBs or PBs of data in or out of AWS
- Alternative to moving data over the network (and paying network fees)
- Use cases: large data cloud migrations, DC decommission, disaster recovery
- If it takes more than a week to transfer over the network, use Snowball devices!
- Snowball cannot import to Glacier directly
- You have to use Amazon S3 first, and an S3 lifecycle policy

## Process
1. Request snowball devices from the AWS console for delivery
2. Install the snowball client on your servers
3. Connect the snowball to your servers and copy files using the client
4. Ship back the device when you’re done (goes to the right AWS
facility)
5. Data will be loaded into an S3 bucket
6. Snowball is completely wiped
7. Tracking is done using SNS, text messages and the AWS console

## Snowball Edge
- Snowball Edges add computational capability to the device
- Very useful to pre-process the data while moving 
- Use case: data migration, image collation, IoT capture, machine learning

## AWS Snowmobile
- Each Snowmobile has 100 PB of capacity (use multiple in parallel)
- Better than Snowball if you transfer more than 10 PB

# Storage Gateway Overview
## AWS Storage Gateway
- Bridge between on-premise data and cloud data in S3
- Use cases: disaster recovery, backup & restore, tiered storage
- 3 types of Storage Gateway:
    - File Gateway
    - Volume Gateway
    - Tape Gateway

## File Gateway
- Configured S3 buckets are accessible using the NFS and SMB protocol
- Most recently used data is cached in the file gateway
- It seems like we're talking to a local network file system

## Volume Gateway
- Block storage using iSCSI protocol backed by S3
- Backed by EBS snapshots which can help restore on-premise volumes!
- Cached volumes: low latency access to most recent data
- Stored volumes: entire dataset is on premise, scheduled backups to S3

## Tape Gateway
- If you see anything around backup and Virtual Tapes
- Some companies have backup processes using physical tapes (!)
- With Tape Gateway, companies use the same processes but in the cloud
- Virtual Tape Library (VTL) backed by Amazon S3 and Glacier
- Back up data using existing tape-based processes (and iSCSI interface)
- Works with leading backup software vendors

# Amazon FSx - Overview
## FSx for Windows
- EFS is a shared POSIX system for Linux systems.
- FSx for Windows is a fully managed Windows file system share drive
- Anytime you have shared storage for your Windows instances, this is Amazon FSx for Windows

# Amazon FSx for Lustre
- High Performance Computing (HPC)
- Lustre is a type of parallel distributed file system, for large-scale computing
- The name Lustre is derived from “Linux” and “cluster”

