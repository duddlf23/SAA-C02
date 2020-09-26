# Security Token Service (STS) Overview
- Allows to grant limited and temporary access to AWS resources.
- Token is valid for up to one hour
- API in STS
    - AssumeRole
        - Enable Cross Account Access
    - AssumeRoleWithWebIdentity
        - AWS recommends against using this, and using Cognito instead

# Identity Federation & Cognito
- Identity provided access role
- Using federation, you don’t need to create IAM users (user management is outside of AWS)
## SAML 2.0 Federation

# Directory Services - Overview
## 
# IAM - Advanced
- aws:SourceIP: restrict the client IP from which the API calls are being made
- Aws:RequestedRegion: restrict the region The API calls are made to
- Restrict based on tags
- aws:MultiFactorAuthPresent: true

# IAM - Policy Evaluation Logic
## IAM Permission Boundaries
Advanced feature to use a managed policy to set the maximum permissions an IAM entity can get.
