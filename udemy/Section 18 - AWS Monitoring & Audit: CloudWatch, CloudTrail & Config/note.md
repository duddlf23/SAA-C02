# AWS CloudWatch Metrics
- CloudWatch - Provide metrics for every services in AWS

# AWS CloudWatch Dashboards
- Dashboards are global

# AWS CloudWatch Logs
- Application can send logs to CloudWatch
- CloudWatch can collect log from AWS Services, straightforwardly
- CloudWatch Logs can go to: 
    - Batch exporter to S3 for archival 
    - Stream to ElasticSearch cluster for further analytics

# CloudWatch logs for EC2
- By default, no logs from your EC2 machine will go to CloudWatch
- You need to run a CloudWatch agent on EC2 to push the log files you want
- Make sure IAM permissions are correct
- The CloudWatch log agent can be setup on-premises too

# AWS CloudWatch Alarms
- Alarm States:
    - OK
    - INSUFFICIENT_DATA -  Basically don't send enough data for your alarm, so the metric is missing some data points
    - ALARM

# AWS CloudWatch Events
- Schedule: Cron jobs
- Event Pattern: Event rules to react to a service doing something
    - Ex: CodePipeline state changes!
- Triggers to Lambda functions, SQS/SNS/Kinesis Messages

# AWS CloudTrail
- Provides governance, compliance and audit for your AWS Account
- CloudTrail is enabled by default!
- Get an history of events / API calls made within your AWS Account
- Can put logs from CloudTrail into CloudWatch Logs

# AWS Config - Overview
- Helps with auditing and recording compliance of your AWS resources
- Possibility of storing the configuration data into S3 (analyzed by Athena)

