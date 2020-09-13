# AWS CLI ON EC2
- IAM Roles can be attached to EC2 instances
- IAM Roles can come with a policy authorizing exactly what the EC2 instance should be able to do
- EC2 Instances can then use these profiles automatically without any additional configurations
- This is the best practice on AWS and you should 100% do this

# AWS Policy Simulator

# AWS EC2 Instance Metadata
- AWS EC2 Instance Metadata is powerful but one of the least known features to developers
- It allows AWS EC2 instances to ”learn about themselves” without using an IAM Role for that purpose.
- The URL is http://169.254.169.254/latest/meta-data - It will only work from EC2 instances
- You can retrieve the IAM Role name from the metadata, but you CANNOT retrieve the IAM Policy.
- Metadata = Info about the EC2 instance
- Userdata = launch script of the EC2 instance

To perform API goals is that it queries this whole URL right here, which it gets an access key ID, a secret access key and a token. And it turns out that this is a short lived credentials. So as you can see, there is an expiration date in here and that's usually something like one hour.

# AWS SDK Overview
AWS CLI is a wrapper around the Python SDK

If you don't specify or configure a default region, then the API codes will default to the US East region when you use the SDK.

## AWS SDK Credentials Security
- It’s recommend to use the default credential provider chain
- The default credential provider chain works seamlessly with:
    -• AWS credentials at ~/.aws/credentials (only on our computers or on premise)
    - Instance Profile Credentials using IAM Roles (for EC2 machines, etc…)
    - Environment variables (AWS_ACCESS_KEY_I, AWS_SECRET_ACCESS_KEY)
- Overall, NEVER EVER STORE AWS CREDENTIALS IN YOUR CODE.
- Best practice is for credentials to be inherited from mechanisms above, and 100% IAM Roles if working from within AWS Services

