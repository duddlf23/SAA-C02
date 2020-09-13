# Route 53 Overview
- Managed DNS
- DNS - collection of rules and records which helps clients understand
how to reach a server through URLs
- Common Records
    - A: hostname(myapp.example.com) -> IPv4
    - AAAA: hostname to IPv6
    - CNAME: hostname to another hostname
    - Alias: hostname to AWS resource

## Diagram for A Record
1. DNS Request - DNS query  to a DNS system such as Route 53
2. Send back IP
3. HTTP Request to IP
4. HTTP Resopnse

Global solution, not require region seleciton

# Route 53 - EC2 Setup
- elb - locked on region
- Route 53 - cross region

# Route 53 - TTL
- DNS Records Time to Live
- TTL - A way for web browsers and clients to cache the response of a DNS query
- Response of DNS Request included TTL value
- Web browser will cache that DNS request and the response for the TTL duration

# Route 53 CNAME vs Alias
- CNAME - only for non root domain, point hostname to other any hostname
- Alias - Works for root domain or non root domain, free of charge, point hostname to AWS resource(blabla.amazonaws.com)

# Routing Policy - Simple
- Can't attach health checks to simple routing policy
- If multiple values are returned, a random one is chosen by the client

# Routing Policy - Weighted
- Control the % of the requests that go to specific endpoint
- Can be associated with Health Checks

# Routing Policy - Latency
- Redirect to the server that has the least latency close to us
- Super helpful when latency of users is a priority
- Latency is evaluated in terms of user to designated AWS Region

# Route 53 Health Checks
If an instance is unhealthy, just like an ELB, Route 53 will not send traffic to that instance
- Have X health checks failed => unhealthy (default 3)
- After X health checks passed => health (default 3)
- Default Health Check Interval: 30s (can set to 10s – higher cost)
- About 15 health checkers will check the endpoint health
- Possibility of integrating the health check with CloudWatch
- Health checks can be linked to Route53 DNS queries (They will basically change the behavior of Route 53)

# Routing Policy - Failover
- Route 53 will failover to the secondary instance when there is a DNS query

# Routing Policy - Geolocation
- Different from Latency based
- This is routing based on user location
- Here we specify: traffic from the UK should go to this specific IP
- Should create a “default” policy (in case there’s no match on location)

# Routing Policy - Multi Value
- Upgrade version of simple policy
- From the web browser perspective if I go to this URL basically my web browser would have just picked any of these three IPs at random and just used this one

# 3rd Party Domains & Route 53
- Domain Regisrtar != DNS
- If you buy your domain on 3rd party website, you can still use Route53.
    1) Create a Hosted Zone in Route 53
    2) Update NS Records on 3rd party website to use Route 53 name servers
- Domain Registrar != DNS
- (But each domain registrar usually comes with some DNS features)