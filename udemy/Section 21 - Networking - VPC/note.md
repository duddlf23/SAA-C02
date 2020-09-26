# CIDR, Private vs Public IP
- 2 Components - base IP, Subnet Mask

# Default VPC Overview
- Default VPC have internet connectivity and all instances have public IP

# Subnet Overview and Hands On
- Subnets are going to be tied to specific availability zones.
- AWS reserves 5 Ips address in each Subnet

# Internet Gateways & Route Tables
- Only one IGW per VPC

# NAT Instances
- Must disable source/destination checks

# NAT Gateways
- Better alter to NAT Instances

# DNS Resolution Options & Route 53 Private Zones
- enableDnsHostname: (= DNS Hostname setting)
    - Won’t do anything unless enableDnsSupport=true
    - If True, Assign public hostname to EC2 instance if it has a public
- If you use custom DNS domain names in a private zone in Route 53, you must set both these attributes to true

# NACL & Security Groups
- NACL is at a subnet level, so it's before traffic even gets into our subnet or EC2 instance.

# VPC Endpoints
- Endpoints allow you to connect to AWS Services using a private network instead of the public www network
- Interface - provisions an ENI (private IP address) as an entry point (must attach security group) – most AWS services
- Gateway - provisions a target and must be used in a route table – S3 and DynamoDB

# VPC Flow Logs + Athena
- Capture information about IP traffic going into your interfaces:
    - VPC Flow Logs
    - Subnet Flow Logs
    - Elastic Network Interface Flow Logs
- Helps to monitor & troubleshoot connectivity issues
- Flow logs data can go to S3 / CloudWatch Logs
- Captures network information from AWS managed interfaces too: ELB, RDS, ElastiCache, Redshift, WorkSpaces

# Site to Site VPN, Virtual Private Gateway & Customer Gateway
- Connect with our corporate data center
- How does that work?
    1. Create a customer gateway onto corporate DC
    1. On the VPC side, provision VPN gateway
    1. In between the VPN gateway and the customer gateway, we will set up a site to site VPN connection
- On the Customer Gateway side - IP Address
    - Use static, internet-routable IP address for your customer gateway device.
    - If behind a CGW behind NAT (with NAT-T), use the public IP address of the NAT

# Direct Connect & Direct Connect Gateway
- Don't use public internet
- If you want to setup a Direct Connect to one or more VPC in many different regions (same account), you must use a Direct Connect Gateway
- Dedicated Connections - AWS has to provision
- Hosted Connections - Capacity can be added or removed on demand

# Egress Only Internet Gateway
- Works for IPv6 only

# AWS PrivateLink - VPC Endpoint Services
- Want to expose application to other VPCs in other accounts
- Requires a network load balancer (Service VPC) and ENI (Customer VPC)
- If the NLB is in multiple AZ, and the ENI in multiple AZ, the solution is fault tolerant!

# VPN CloudHub
- Provide secure communication between sites, if you have multiple VPN connections
- It’s a VPN connection so it goes over the public internet

# Transit Gateway
- All the things connected to the transit gateway if set up correctly can connect with one another.
- Regional resource, can work cross-region
- Share cross-account using Resource Access Manager (RAM)
- You can peer Transit Gateways across regions 
- Route Tables: limit which VPC can talk with other VPC 
- Works with Direct Connect Gateway, VPN connections
 
# Networking Costs in AWS
- Use Private IP instead of Public IP for good savings and better network performance
- Use same AZ for maximum savings (at the cost of high availability)

