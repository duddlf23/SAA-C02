# 54. EBS Intro
- EC2 machine, it will lose its root volume by defaults
- An EBS (Elastic Block Store) Volume is a network drive you can attach
to your instances while they run
- It allows your instances to persist data 

## It is a network drive
- a bit of latency
- Detach & Attach quickly
- Locked to AZ
- To move a volume across, need to snapshot
- Have a provisioned capacity (size in GBs, and IOPS)
    - You get billed for all the provisioned capacity
    - You can increase the capacity of the drive over time
- EBS volume types
    - GP2, IO1, ST1, SC1
    - Only GP2 and IO1 can be used as boot volumes
    
# 56. EBS Volume Types Deep Dive
- GP2 - Recommended for most workloads
    - 3 IOPA per GB, minumum of 100 IOPS 
- IO1 - Critical business applications that require sustained IOPS performance, or more than 16,000 IOPS per volume
    - Large database workloads
    - IOPS is provisioned
- Need to increase the size as increase the IOPS
- ST1 - Streaming workloads requiring consistent, fast throughput at a low price, throughput optimized HDD
- SC1 - Throughput-oriented storage for large volumes of data that is
infrequently accessed
    - Scenarios where the lowest storage cost is important
    
# EBS Operation: Snapshots
## EBS Snapshots
- Incremental – only backup changed blocks
- EBS backups use IO and you shouldn’t run them while your application is
handling a lot of traffic
- Snapshots will be stored in S3 (but you won’t directly see them)
- Not necessary to detach volume to do snapshot, but recommended
- Max 100,000 snapshots
- Can copy snapshots across AZ or Region
- Can make Image (AMI) from Snapshot
- EBS volumes restored by snapshots need to be pre-warmed (using fio or dd command to read the entire volume)
- Snapshots can be automated using Amazon Data Lifecycle Manager

# EBS Operation: Volume Migration
- Locked to a AZ
- Snapshot migration!

# EBS Operation: Volume Encryption
- When you copy an unencrypted snapshot, then you enable encryption.
- How encrypt an unencrypted EBS volumes?
    - Create an EBS snapshot of the volume
    - Encrypt the EBS snapshot ( using copy )
    - Create new ebs volume from the snapshot ( the volume will also be encrypted )
    - Now you can attach the encrypted volume to the original instance

# EBS vs Instance Store
- Instance Store - Ephemeral Storage
    - Physically attached, so very high IOPS
    - Can't resize
- EBS
    - Limited IOPS up to 64,000 IOPS (IO1)
    
# EBS RAID configurations
- RAID (Redundant Array of Independent Disks)
- RAID 5, 6, 10 - not recommended for EBS
- RAID 0
    - An application that needs a lot of IOPS and doesn’t need fault-tolerance
    - very big disk with a lot of IOPS
- RAID 1
    - increase fault tolerance
    - Mirroring a volume to another
    - If one disk fails, our logical volume is still working
    - Application that need increase volume fault tolerance
 
# EFS Overview
Elastic File System
- Managed NFS that can be mounted on many EC2 across many different AZs
- EFS works with EC2 instances in multi-AZ
- Only pay for what you use, so if you don't store that much data, it may actually be cheaper to use EFS than EBS based on how well you manage your data set and size on your EFS drive
- Can attach security group 
- Compatible with Linux based AMI (not Windows)
## EFS Scale
- Can grow to Petabyte-scale network file system, automatically
## Storage Tiers (lifecycle management feature – move file after N days)
- Standard: for frequently accessed files
- Infrequent access (EFS-IA): cost to retrieve files, lower price to store (need retrieval fee)
## Throughput mode
- Bursting - scales with file system size
- Provisioned

For each AZ, you should define a security group.

## Mount EFS on a Linux instance
- via DNS
- via IP
- Add NFS type inbound rules to sg of EFS

# EFS vs EBS
