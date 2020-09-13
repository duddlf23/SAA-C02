# CloudFront Overview
- Content Delivery Network (CDN)
- Improves read performance, content is going to be distributed, and cached at the edge location
- DDoS protection, integration with Shield, AWS Web Application Firewall
- Good way to front your applications when you deploy them globally
- Can expose external HTTPS and can talk to internal HTTPS backends

## Origin
It could be an S3 buckets or any HTTP endpoints

- S3 bucket 
    - For distributing files and caching them at the edge 
    - Enhanced security with CloudFront Origin Access Identity (OAI) 
    - CloudFront can be used as an ingress (to upload files to S3) 
- Custom Origin (HTTP) 
    - Application Load Balancer 
    - EC2 instance 
    - S3 website (must first enable the bucket as a static S3 website) 
    - Any HTTP backend you want

OAI is IAM role for CloudFront origin

## ALB or EC2 as an origin
- EC2
    - EC2 instances must be public (They must be accessible from HTTP standpoint)
    - Allow Public IP of Edge Locations by sg
- ALB
    - ALB must be public, backend EC2 can be private
    - ALB's sg must allow public IP of the edge location

## CloudFront Geo Restriction
- Whitelist or Blacklist from the countries

## CloudFront vs S3 Cross Region Replication
- CloudFront
    - Great for static content that must be available everywhere
- S3 Cross Region Replication
    - Great for dynamic content that needs to be available at low-latency in few regions
    
# Hands on
Must limit the S3 bucket to be accessed only using this identity

DNS propagation

# CloudFront Signed URL / Cookies
- You want to give access to people to premium paid shared content all over the world.
- You want to be able to see and know who has access to what on your CloudFront distribution.

- Distribute paid shared content to premium users over the world
• We can use CloudFront Signed URL / Cookie. We attach a policy with:
    - Includes URL expiration
    - Includes IP ranges to access the data from
    - Trusted signers (which AWS accounts can create signed URLs)
- How long should the URL be valid for?
    - Shared content (movie, music): make it short (a few minutes)
    - Private content (private to the user): you can make it last for years

- Signed URL = access to individual files (one signed URL per file)
- Signed Cookies = access to multiple files (one signed cookie for many files)

# AWS Global Accelerator - Overview
- Global user go over the public internet, which can add a lot of latency due to many hops
- We wish to go as fast as possible through AWS network to minimize latency
## Unicast vs Anycast
- Unicast IP: one server holds one IP address
- Anycast IP: all servers hold the same IP address and the client is routed to the nearest one

## AWS Global Accelerator
- Uses that Anycast IP concept to work
- Leverage the AWS internal global network to route to application
- Instead of sending it through the public internet, it's going to come to the closest edge location. And from edge location, it's going to go all the way straight to our ALB through the internal AWS network.
- 2 Anycast IP(HA) are created for your application
- The Anycast IP send traffic directly to Edge Locations
- The Edge locations send the traffic to your application
- Very expensive
