# S3 Buckets and Objects
# S3 Versioning
- Protect against unintended deletes (ability to restore a version)
- Easy roll back to previous version
- Any file that is not versioned prior to enabling versioning will have version “null”
- Suspending versioning does not delete the previous versions - It will just make sure that the future files do not have a version assigned to it
- delete - Delete marker to version

# S3 Encryption
- There are 4 methods of encrypting objects in S3
    - SSE-S3: encrypts S3 objects using keys handled & managed by AWS
    - SSE-KMS: leverage AWS Key Management Service to manage encryption keys
    - SSE-C: when you want to manage your own encryption keys
    - Client Side Encryption
- It’s important to understand which ones are adapted to which situation for the exam

## SSE-S3
- Encrypted server side
- AES-256 encryption type
- Must set header: “x-amz-server-side-encryption": "AES256"

## SSE-KMS
- SSE-KMS: encryption using keys handled & managed by KMS
- KMS Advantages: user control + audit trail
- Object is encrypted server side
- Must set header: “x-amz-server-side-encryption": ”aws:kms"

## SSE-C
- Client side data key for encryption
- SSE-C: server-side encryption using data keys fully managed by the customer outside of AWS
- Amazon S3 does not store the encryption key you provide
- HTTPS must be used
- Encryption key must provided in HTTP headers, for every HTTP request made
- If you wanted to retrieve that file from Amazon S3 using SSE-C, you would need to provide as well, the same Client Side data key that was used.

## Client Side Encryption
- Encrypt the objects before uploading it into Amazon S3

## Encryption in transit (SSL/TLS)
- Amazon S3 exposes:
    - HTTP endpoint: non encrypted
    - HTTPS endpoint: encryption in flight
- Free to use the endpoint you want, but HTTPS is recommended
- HTTPS is mandatory for SSE-C
- Encryption in flight is also called SSL / TLS

# S3 Security & Bucket Policies
## S3 Security
- User based
    - • IAM policies - which API calls should be allowed for a specific user from IAM console
- Resource Based
    - Bucket Policies - bucket wide rules from the S3 console - allows cross account
    - Object Access Control List (ACL) – finer grain
    - Bucket Access Control List (ACL) – less common

**IAM principal can access an S3 object if the user IAM permissions allow it OR the resource policy ALLOWS it AND there’s no explicit DENY**

## S3 Bucket Policies
- JSON based policies 
    - Resources: buckets and objects 
    - Actions: Set of API to Allow or Deny 
    - Effect: Allow / Deny 
    - Principal: The account or user to apply the policy to
- Use S3 bucket for policy to: 
    - Grant public access to the bucket 
    - Force objects to be encrypted at upload 
    - Grant access to another account (Cross Account)
    
## S3 Security - Other
- Networking:
    - Supports VPC Endpoints (for instances in VPC without www internet)
- Logging and Audit:
    - S3 Access Logs can be stored in other S3 bucket
    - API calls can be logged in AWS CloudTrail
- User Security:
    - MFA Delete: MFA (multi factor authentication) can be required in versioned buckets to delete objects
    - Pre-Signed URLs: URLs that are valid only for a limited time (ex: premium video service for logged in users)

# S3 static website hosting
- index.html
- error.html

# S3 CORS
## Cross-Origin Resource Sharing
CORS headers have to be defined on the cross-origin bucket,not the first origin bucket

# S3 Consistency Model
- Amazon S3 is made up of multiple servers and so when you write to Amazon S3, the other servers are going to replicate data between each other.
- Read after write consistency for PUTS of new objects
    - As soon as a new object is written, we can retrieve it ex: (PUT 200 => GET 200)
    - This is true, except if we did a GET before to see if the object existed ex: (GET 404 => PUT 200 => GET 404) – eventually consistent
- Eventual Consistency for DELETES and PUTS of existing objects
    - If we read an object after updating, we might get the older version ex: (PUT 200 => PUT 200 => GET 200 (might be older version))
    - If we delete an object, we might still be able to retrieve it for a short time ex: (DELETE 200 => GET 200)
- Note: there’s no way to request “strong consistency” ( if you overwrite an object, you need to wait a little bit before you are certain that the GET returns the newest version of your object)
