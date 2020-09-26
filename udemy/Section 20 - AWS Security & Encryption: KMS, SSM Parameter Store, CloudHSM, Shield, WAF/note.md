# Encryption 101
## Encryption in flight (SSL)
- Data is encrypted before sending and decrypted after receiving
- SSL certificates help with encryption (HTTPS)
- Encryption in flight ensures no MITM (man in the middle attack) can happen

## Server side encryption at rest
- Data will be stored in an encrypted form in the server

## Client side encryption
- The server should not be able to decrypt the data

# SSM Parameter Store Overview
- Secure storage for config and secrets
- optional seamless encryption using KMS
- Integration with CloudFormation
- Configuration management using path & IAM

# AWS Secrets Manger
- Another service in which can helpfully store secrets in AWS
- Newer service
- Capability to force rotation of secrets every X days
- Automate generation of secrets on rotation (uses Lambda)
- Integration with Amazon RDS (MySQL, PostgreSQL, Aurora)
- Secrets are encrypted using KMS
- Mostly meant for RDS integration

# CloudHSM
- HSM = Hardware Security Module

# Shield - DDoS Protection
- Protect from DDoS attacks
- Standard - free
- Advanced - Optional DDoS mitigation service 

# Web Application Firewall (WAF)
- Protects your web applications from common web exploits (Layer 7)
- Layer 7 is HTTP (vs Layer 4 is TCP)
- Deploy on Alb, API Gateway, CloudFront
## AWS Firewall Manager
Manage rules in all accounts of an AWS Organization

# Shared Responsibility Model