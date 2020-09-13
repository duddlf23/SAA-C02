# Instantiating applications quickly
- EC2 Instances:
    - Use a Golden AMI: Install your applications, OS dependencies etc.. beforehand and launch your EC2 instance from the Golden AMI
    - Bootstrap using User Data: For dynamic configuration, use User Data scripts
    - Hybrid: mix Golden AMI and User Data (Elastic Beanstalk)
- RDS Databases:
    - Restore from a snapshot: the database will have schemas and data ready!
- EBS Volumes:
    - Restore from a snapshot: the disk will already be formatted and have data!

# Beanstalk Overview
- Elastic Beanstalk is a developer centric view of deploying an application on AWS
- It uses all the component’s we’ve seen before: EC2, ASG, ELB, RDS, etc…
- But it’s all in one view that’s easy to make sense of!
- We still have full control over the configuration
- Beanstalk is free but you pay for the underlying instances
- Elastic Beanstalk has three components
    - Application
    - Application version: each deployment gets assigned a version
    - Environment name (dev, test, prod…): free naming


