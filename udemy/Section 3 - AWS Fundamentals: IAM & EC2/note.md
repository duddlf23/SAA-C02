# 11 AWS Regions and AZs

AWS is global cloud provider and has many data centers all around the world

Region - Cluster of data centers

Each region has many AZs and They're separate from each other, so isolated from disasters

Region table - which services are available in which regions

# 12 IAM Introduction

IAM - Identity and Access Management (whole security), global view
- Users
- Groups
- Roles

Users inherit group's permissions. Roles are only for internal usage within the AWS resources and services.

IAM is best way to give users the minimal amount of permissions they need (least privilege principles).

[IAM Federation](https://docs.aws.amazon.com/ko_kr/IAM/latest/UserGuide/id_roles_providers.html)

IAM 101
- One IAM User per PHYSICAL PERSON
- One IAM Role per Application
- IAM credentials should NEVER BE SHARED
- Never write IAM credentials in code.
- Never use ROOT IAM Credentials

# 13 IAM Hands On
MFA - Multi-Factor Authentication [Ref](https://docs.aws.amazon.com/ko_kr/IAM/latest/UserGuide/id_credentials_mfa.html?icmpid=docs_iam_console)

# 15 ED2 Intro
Root device: OS going to run

Security group: firewall around the instance

EC2 stop - it will not bill

# 17 How to SSH using Linux or Mac
chmod 0400 ~.pem

# 21 EC2 Instance Connect 
EC2 connection from the browser

# 23 Security Groups Deep Dive
Locked down to a regil / VPC combination

"connection refused" error - application error

# 24 Private vs Public vs Elastic IP
Networking has two sorts of IPs. IPv4 and IPv6

EC2 stop and restart -> it can change its public IP

Fix - Elastic IP

Try to avoid using Elastic IP
- Instead, use a random public IP and register a DNS name
- Or use a Load Balancer and don't use a public IP

# 25 Hands on
Associate Elastic IP
- Instance
- Network Interface

# 27 EC2 User Data
Possible to bootstrap instances using an script

Run with the root user (sudo rights)

# 28 EC2 Instances Launch Types
Which type of launch will allow to either provide the best cost savings or the most stability.

- On Demand: short workloads, predictable pricing
- Reserved
    - Reserved Instances
    - Convertible Reserved Instances: long workloads with flexible instances
    - Scheduled Reserved Instances
- Spot: short workloads, cheap, less reliable
- Dedicated Instances: no other customers will share my hardware
- Dedicated Hosts: book an entire physical server, control instance placement

## On Demand
Highest cost but no upfront payment

short-term and un-interrupted workloads, where I can't predict how the application will behave.

## Reserved Instances
- Discount compared to On-demand
- Pay upfront for what I user with long term commitment
- period can be 1 or 3 years
- Specific instance type
- Steady state using applications (think database)
- Convertible Reserved Instance
    - Can change the EC2 Instance type
    - little expansive
- Scheduled Reserved Instance

## Spot Instances
- Useful for workloads that are resilient to failure
    - possible to retry
- No great for critical jobs or databases

## Great combo
Reserved Instances for baseline + On-demand & Spot for peaks

## EC2 Dedicated Hosts
- Useful for software that have complicated licensing model(Bring Your Own License)

[Dedicated Instances vs Dedicated Hosts](https://aws.amazon.com/ko/ec2/dedicated-hosts/)

# 29 Spot Instances & Spot Fleet
## EC2 Spot Instance Requests
- hourly spot price varies based on offer and capacity
- spot failure two option
    - stop - can restart
    - terminate
- Spot Block
    - prevent reclaim from AWS
    - "block" spot instance during a specified time frame without interruptions
    
## Pricing
Spot price based on AZ

## How to terminate Spot Instances
- how a spot request works
    - one-time: spot instance launch -> request go away
    - To cancel spot request doesn't induce terminating instances
    - Cancel spot request -> terminate associated spot instances

## [Spot Fleets](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/spot-fleet.html)
- Ultimate way to save money
- Set of Spot Instances + (optional) On-Demand Instances

# 31 EC2 Instance Types Deep Dive
- R: App that needs a lot of RAM - in memory caches
- C: App that needs good CPU - compute / DB
- M: App that ar balanced - genral / web app
I: good I/O - DB
G: GPU - video rendering / ML

## Burstable Instances (T2 / T3)
- Machine needs to process something unexpected -> It can burst, and CPU can be very good.
- burst credits
- All the credits are gone, the CPU becomes BAD

### T2/T3 Unlimited
- Unlimited burst credit balance

# 32 EC2 AMIs
- AMI Storage
    - AMI take space and they live in Amazon S3
    - But won't see them in the S3 console
    - By default, AMIs are private

- AMI Pricing
    - AMIs live in S3, so I get charged for the actual space in takes in Amazon S3

# 35 EC2 Placement Groups
- Control over the EC2 instance placement strategy
- Don' get direct interaction with the hardware of AWS
- Let AWS know how we would like EC2 to be placed compared to one another

## Cluster
- All in same rack, which means same hardware, and same AZ
- Great network
- If the rack fails, all instances fails at the same time
## Spread
- Minimize the failure risk
- All the EC2 are going to be located on diff hardware
- Can span across AZs
- Limited to 7 instances per AZ per placement group
- Use case
    - needs to HA, or must be isolated from failure from each other
## Partition
- Within the AZ, there are different partitions, which are sets of racks.
- Up to seven partitions per AZ
- Between diff partitions, there are no instances shared racks from other instances in other partitionss
- EC2 get access to the partition information as metadata

# 36 Elastic Network Interfaces (ENI) with Hands On
- Give EC2 instances access to the network
- Used outside EC2 instances as well
- Each instance gets each own ENI

# 38 EC2 Hibernate
- Stop, terminate instances
    - DeleteOnTermination - true when ebs is root, but false when ebs is not root.
- EC2 Hibernate
    - RAM state is preserved
    - Instance restart is much faster (OS is not stopped)
    - The RAM state is written to a file in the root EBS volume
    - The root EBS volume must be encrypted
    - Available for On-Demand and Reserved Instances
    