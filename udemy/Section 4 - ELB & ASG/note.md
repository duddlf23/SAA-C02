# 41. High Availability and Scalability
## Vertical Scalability
- Increasing the size of the instance
- Very common for nom distributed systems, such as a database
- RDS, ElastiCache
- Hardware limit

## Horizontal Scalability
- Increasing the number of instances / systems for application
- Imply distributed systems
- Web / modern application

## High Availability
- Running application in at least 2 data centers
- Active-Passive
    - RDS Multi AZ 
- Active-Active
    - Horizontal Scaling

# 42. Elastic Load Balancing (ELB) Overview
- Servers that forward internet traffic to multiple servers downstream
- Users just interface with the single point of entry which is a Load Balancer
- In the backend, can scale EC2 instances
- Spread load across multiple downstream
- Expose a single point af access (DNS)
- Seamlessly handle failures of downstream instance
- Regular health checks
_ HA across multiple AZs

## Health Checks
- Crucial for Load Balancers
- Healthy - available to reply to requests
- Done on a port and a route
- Response is not 200 -> unhealthy
- Not healthy -> Stop sending traffic

Classic LB - HTTP, HTTPS, TCP

ALB(HTTP, HTTPS, WebSocket), NLB(TCP, TLS, UDP) - new generation

Internal(private) or External(public)

## Troubleshooting
- 4xx errors are client induced errors
- 5xx errors are application induced errors
- 503: Load Balancers Errors

## Monitoring
- ELB access logs will log all access requests
- CloudWatch Metrics will give aggregate statistics

# 43. Classic Load Balancer (CLB) with Hands On
- Supports TCP (Layer 4), HTTP & HTTPS (Layer 7)

# 44. Application Load Balancer (ALB) with Hands On
- Layer 7 (HTTP)
- Multiple HTTP applications across machines (target groups)
- Multiple applications on the same machine (ex: container)
- Support for HTTP/2 and WebSocket
- Support redirects (from HTTP to HTTPS for example)
- Root routing - routing tables to different target groups
    - Routing based on path in URL
    - Routing based on hostname in URL
    - Routing based on Query string, Headers
- Great fit for micro svcs & container-based application

## Target Groups
- EC2 (can be ASG) - HTTP
- ECS tasks - HTTP
- Lambda functions - HTTP request is translated into a JSON event
- IP Addresses - must be private IPs
_ Health checks are at the target group level
- Fixed hostname
- The application servers don't see the IP of the client directly
    - True IP of the client is inserted in the header called X-Forwarded-For
    - Can get Port and Protocol
    
# 45. Network Load Balancer (NLB) with Hands On
- Layer 4 - Allow to forward TCP & UDP traffic to instances, it's lower level
- Handle millions of request per seconds
- Less latency ~ 100ms (vs 400 ms for ALB)
- Support for static IP addresses for the load balancer. You can also assign one Elastic IP address per subnet enabled for the load balancer.
- There is no security group

# 46. Elastic Load Balancer - Stickiness
- Stickiness - Same client always redirected to the same instance behind a load balancer
- Work for CLB & ALB
- Cookie used for stickiness as long as the cookie remains the same
- Use case: make sure the user doesn't lose session data
- May bring imbalance to the load over the EC2

## ALB - stickiness with target group level
- Enable stickiness duration

# 47. Elastic Load Balancer - Cross Zone Load Balancing
- Each load balancer instance distributes evenly across all registered instances in all AZ
- CLB - default: disabled, No charges for inter AZ data if enabled
- ALB - Always on(con't be disabled), No charges for inter AZ data
- NLB - Disabled by default, pay chrges

# 48. Elastic Load Balancer - SSL Certificates
- SSL Certificate allows traffic between clients and LB to be encrypted in transit (in-flight encryption)
- It means that data, as it goes through a network is going to be encrypted and only going to be able to be decrypted by the sender and the receiver
- SSL: refers to Secure Sockets Layer, used to encrypt connections
- TLS:  Transport Layer Security, which is a newer version of SSL
- Public SSL certificates are issued by CA
- SSL certificates have an expiration date and must be renewed

## SNI (Sever Name Indication)
- Solve the problem of loading multiple SSL certificates onto one web server
- Server will know what certificate to load from client
- Only works for ALB & NLB
- CLB: Must user multiple CLB for multiple hostname with multiple SSL certificates
- ALB: Supports multiple listeners with multiple SSL cert, uses SNI to make it work
- NLB: Same with ALB

# 49. Elastic Load Balancer - Connection Draining
- Feature naming
    - CLB: connection draining
    - target group: Deregistration delay (for ALB & NLB)
- Process
    1. EC2 maybe is being terminated or is unhealthy
    1. It's going to go into draining mode
    1. During the draining mode, existing connections will be waited for the duration of the connection draining period to be completed (default: 300s)
    1. Set to a low value if requests are short
    1. If disable it, set the value to zero, and connection must be dropped while EC2 instance is being killed, then users will retrieve an error.
    
# 50. Auto Scaling Groups (ASG) Overview
- Scale out
- Scale in
- Ensure minimum and maximum number of machines running
- Automatically register new instances to a load balancer

## Attributes
- Launch configuration
    - AMI + Instance Type
    - EC2 User Data
    - EBS Volumes
    - SG
    - SSH Key Pair
- Min / Max Size / Initial Capacity
- Network + Subnets
- Load Balancer Information
- Scaling Policies (What will trigger a Scale In / Out)

## Auto Scaling Alarms
- Scale ASG based on CloudWatch alarms
- Monitor a metric (such as Average CPU)
- Metrics are computed for the overall ASG instances

## Auto Scaling Custom Metric
1. Send custom metric from application on EC2 to CloudWatch (PutMetric API) 
1. Create CloudWatch alarm to react to low / high values
1. Use alarm as the scaling policy for ASG

## ASG Brain Dump
- Launch Templates - newer version of Launch configuration
- IAM roles attached to an ASG will get assigned to EC2
- ASG are free
- ASG can terminate instances marked as unhealthy by an LB (and hence replace them)

# 51. Auto Scaling Groups Hands On
- Launch templates allow to use a spot fleet, and launch configuration allow to just specify one instance type

# 52. Auto Scaling Groups - Scaling Policies
- Target Tracking Scaling
    - ex) I want the average ASG CPU to stay at around 40%
- Simple / Step Scaling

- Scheduled Actions
- Anticipate a scaling based on known usage patterns

# 53. Auto Scaling Groups - for Solutions Architects
- ASG default termination policy
    1. Find the AZ which has the most number of instances
    1. If there are multiple instances in the AZ to choose from, delete the one with the oldest launch config  
- Lifecycle Hooks
    - Perform extra steps before the instance goes in service or the instance is terminated

 