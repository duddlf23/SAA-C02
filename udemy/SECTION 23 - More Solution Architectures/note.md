# Event Processing in AWS
- Only 3 destination out of s3 event - SNS, SQS, Lambda

# Blocking an IP Address in AWS
- There is no such thing as a sg for a NLB
- The traffic is passed through so that means that the clients originating IP is going to go all the way to our EC2 Instance even if our EC2 Instance sits within a private sublet and has a private IP.
# High Performance Computing (HPC) on AWS
- The cloud is the perfect place to perform HPC
- You can create a very high number of resources in no time
- You can speed up time to results by adding more resources
- You can pay only for the systems you have used
## Data Management & Transfer
- AWS Direct Connect:
    - Move GB/s of data to the cloud, over a private secure network
- Snowball & Snowmobile
    - Move PB of data to the cloud
- AWS DataSync
    - Move large amount of data between on-premise and S3, EFS, FSx for Windows
## Compute and Networking
- EC2 Placement Groups: Cluster for good network performance
- EC2 Enhanced Networking - Elastic Network Adapter (ENA)
- Elastic Fabric Adapter (EFA) - Improved ENA for HPC, only works for Linux

# EC2 Instance High Availability
- EBS Restore cross AZ - Using ASG Terminate lifecycle hook -> EBS Snapshot

# Bastion Host High Availability
- SSH - lower level is TCP, so use NLB for Bastion HA
