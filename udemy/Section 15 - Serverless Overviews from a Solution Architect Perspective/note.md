# Lambda Overview
- Limited by time - short executions
- Run on-demand - then you don't use Lambda, your Lambda function is not running, and you only are going to be billed when your function is running
- Docker is not for AWS Lambda 

## Serverless CRON job
- Can create a CloudWatch event rule, or an EventBridge rule, and Lambda functions are serverless too.

# Lambda Limits
- Execution:
    - Memory allocation: 128 MB – 3008 MB (64 MB increments) - When increase the memory, more vCPU
    - Maximum execution time: 900 seconds (15 minutes)
    - Environment variables (4 KB)
    - Disk capacity in the “function container” (in /tmp): 512 MB
    - Concurrency executions: 1000 (can be increased)
- Deployment:
    - Lambda function deployment size (compressed .zip): 50 MB
    - Size of uncompressed deployment (code + dependencies): 250 MB
    - Can use the /tmp directory to load other files at startup
    - Size of environment variables: 4 KB

# Lambda@Edge
Another type of synchronous invocation of Lambda.
- Run a global lambda function alongside each edge locations
- How to implement request filtering before reaching application?
-For this, you can use Lambda@Edge deploy Lambda functions alongside your CloudFront CDN
    - Build more responsive applications
    - You don’t manage servers, Lambda is deployed globally
    - Customize the CDN content
    - Pay only for what you use

# DynamoDB Overview
- Fully Managed, HA with replication across 3 AZ by default
- NoSQL

## DynamoDB - provisioned Throughput
- Provisioned read and write capacity units
- Read Capacity Units (RCU): throughput for reads 
- Write Capacity Units (WCU): throughput for writes
- Option to setup auto-scaling of throughput to meet demand
- Throughput can be exceeded temporarily using “burst credit”
- If burst credit are empty, you’ll get a “ProvisionedThroughputException”.
- It’s then advised to do an exponential back-off retry

# DynamoDB Advanced Features
## DAX
- DynamoDB Accelerator
- Seamless cache for DynamoDB
- Writes go through DAX to DynamoDB

## DynamoDB Streams
- Mostly used with Lambda function to be integrated

## Security
- VPC Endpoints available to access DynamoDB without internet

## Global Tables
- Cross region replication
    - Active Active replication, many regions
    - Must enable streams
    - low latency, recovery purposes
    
# API Gateway Overview
## Integrations
- Lambda
- HTTP
- AWS Service
## Endpoint Types
- Edge-Optimized (default): For global clients
    - Requests are routed through the CloudFront Edge locations (improves latency)
    - The API Gateway still lives in only one region
- Regional:
    - For clients within the same region
    - Could manually combine with CloudFront (more control over the caching strategies and the distribution)
- Private:
    - Can only be accessed from your VPC using an interface VPC endpoint (ENI)
    - Use a resource policy to define access

# API Gateway Security
## IAM Permissions
- Call API Gateway with Sig v4 -> API Gateway calls IAM verifies the policies -> Make sure it all checks out
- If you give access to users outside of your AWS then you can't use IAM permissions
## Lambda Authorizer
- Custom Authorizer
- Uses AWS Lambda to validate the token in header being passed
- Option to cache result of authentication
- Helps to use OAuth / SAML / 3rd party type of authentication
- Lambda must return an IAM policy for the user
- Cache available
## Cognito User Pools
- Cognito fully manages user lifecycle 
- API gateway verifies identity automatically from AWS Cognito 
- No custom implementation required 
- Cognito only helps with authentication, not authorization

# AWS Cognito Overview
- We want to give our users an identity so that they can interact with our application.
- Cognito User Pools:
    - Sign in functionality for app users
    - Integrate with API Gateway
- Cognito Identity Pools (Federated Identity):
    - Provide AWS credentials to users so they can access AWS resources directly
    - Integrate with Cognito User Pools as an identity provider
- Cognito Sync:
    - Synchronize data from device to Cognito.
    - May be deprecated and replaced by AppSync 

## AWS Cognito User Pools (CUP)
- Create serverless database of user
- Can enable Federated Identities(FB, Google, SAML)
- Sends back a JWT

## Federated Identity Pools
- Provide direct access to AWS Resources from the Client Side
- How
    - Log in to federated identity provider – or remain anonymous
    - Get temporary AWS credentials back from the Federated Identity Pool
    - These credentials come with a pre-defined IAM policy stating their permissions
- Example
    - Provide (temporary) access to write to S3 bucket using Facebook Login
- User login Identity Provider -> Get token -> Authenticate to FIP using token -> Federated Identity will talk to the STS service to get temporary credentials for AWS -> Pass on the temporary credentials back to application

# Serverless Application Model (SAM) Overview
## AWS SAM
- All the configuration is YAML code

