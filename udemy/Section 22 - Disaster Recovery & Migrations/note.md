# Disaster Recovery in AWS
- Disaster - Any event that has a negative impact on a company’s business continuity or finances 
- Disaster recovery (DR) is about preparing for and recovering from a disaster
- RPO: Recovery Point Objective
- RTO: Recovery Time Objective
## RPO
How much of a data loss are you willing to accept in case of a disaster happens?
## RTO
When you recover from disaster, downtime

## DR Strategies
- Backup and Restore 
    - High RPO, RTO, cheap
- Pilot Light
    - A small version of the app is always running in the cloud
    - Faster than Backup and Restore as critical systems are already up
- Warm Standby
    - Full system is up and running, but at minimum size 
    - Upon disaster, we can scale to production load
- Multi Site / Hot Site Approach
    - Very low RTO (minutes or seconds) – very expensive 
    - Full Production Scale is running AWS and On Premise

# DMS
## AWS Schema Conversion Tool (SCT)
Convert your Database’s Schema from one engine to another

# On-Premises Strategies with AWS
- AWS SMS - Incremental replication of on-premise liver servers to AWS

# DataSync
- Move large amount of data from on-premise to AWS
- AWS DataSync agent which we have to install on-premise
- Can use EFS to EFS

# Transferring Large Datasets into AWS
