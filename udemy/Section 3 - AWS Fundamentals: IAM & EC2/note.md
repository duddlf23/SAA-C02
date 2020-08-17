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


