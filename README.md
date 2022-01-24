# AWS Provider Terraform Shared Credentials Test
This code snippet is to test AWS Provider Shared Credential using terraform which is typically AWS cloud control. Cloud Control API is essentially a set of common Application Programming Interfaces — or APIs — that are designed to make it easy for developers to manage their cloud infrastructure consistently and leverage the latest AWS capabilities faster. Typically on the day of launch. So, testing their access to the AWS cloud account prior to launching or deploying AWS resources such as OpenShift Container Platform, ROSA, AWS resources: S3, EC2, Subnets, and rest of the AWS resources is now a mandatory requirement from Cloud service providers.  

AWS provider block in infrastructure as Code innovation introduces consistent APIs access state for managing the end-to-end lifecycle of AWS resources. Hundreds of AWS resources are supported on Cloud Control API, which continues to keep adding support for new AWS to resources. And when I say resources or resource type, I mean a resource type essentially, that has a set of properties and permissions that Control API interactions with underlying AWS or third-party services; such as S3, EC2, et al. 

## Why this AWS Provider Shared Credentials Test?
This is important because it is easy for user not to have access to the targeted AWS account because of some corporate policy but attention will not be paid to the access issue rather attention will be on the project in general. This accessibility test will help to solve the AWS account access issues when user is using Shared Credentials that includes: aws_access_key, aws_secret_key, and aws_session_token.  

This test will test four distinct parts:

1. That User has AWS access from the tools machine. 
2. Thats User setup the terraform correctly within the tools machine. 
3. That User has the ability to create S3 bucket within the intended AWS account.
4. That User can initiate AWS account STS within the environment that will deploy AWS resources.
 ## AWS Configure Security Best Practice
 Although in the settings of AWS configure or in your corporate scripts, the keys and token can be stored in the "config" file, but for security reasons in the IT industry, AWS recommend that you store this in the "credentials" file. You receive this values as part of the temporary credentials returned by successful requests to assume a role. [See AWS Security best practice in configuring the AWS access keys, aws secret keys, and aws session token:](https://docs.aws.amazon.com/sdkref/latest/guide/setting-global-aws_session_token.html) . Therefore, this test is based on AWS security best practices and the IT industry security best practices adopted by IEEE.

## AWS Provider block expected on the root module.
```bash
  provider "aws" {
    region  = var.region
    shared_credentials_file = var.aws_shared_credentials_file
    profile  = var.aws_profile
}
```
## AWS Keys & Token expected in "~/.aws/credentials"
```bash
    [default]
    aws_access_key_id = xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    aws_secret_access_key = xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    aws_session_token = xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

## Testing Steps
1. Step-1: Initiate.
```bash
   a. git clone https://github.com/schijioke-uche/aws-shared-credentials-test.git
   b. cd aws-terraform-shared-access-test
   c. Initiate your AWS Account session: either by running: 
        i. aws configure,
        ii. aws configure sso, or 
        iii. running your own corporate configured script for AWS STS initiation.
   And then, locate aws_access_key, aws_secret_key,and aws_session_token in ~/.aws/credentials
```

2. Step-2: Edit these two files
```bash
   a. Open test.tfvars file and add: aws_access_key, aws_secret_key,and aws_session_token
   b. Open variable.tf file and add add: aws_access_key, aws_secret_key,and aws_session_token
```
3. Step-3: Run the test. 
```bash
   bash test-create.sh
```
If create is successful, you will see similar output below:
```bash
    Plan: 2 to add, 0 to change, 0 to destroy.
    random_id.buc: Creating...
    random_id.buc: Creation complete after 0s [id=syrcWTHVdno]
    aws_s3_bucket.buc: Creating...
    aws_s3_bucket.buc: Creation complete after 8s [id=test-s3-b32adc5931d5767a]

    Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
```
4. Step-4: Destroy the created test s3. 
```bash
   bash test-destroy.sh
```
If destroy is successful, you will see similar output below:
```bash
    Plan: 0 to add, 0 to change, 2 to destroy.
    aws_s3_bucket.buc: Destroying... [id=test-s3-b32adc5931d5767a]
    aws_s3_bucket.buc: Destruction complete after 0s
    random_id.buc: Destroying... [id=syrcWTHVdno]
    random_id.buc: Destruction complete after 0s

    Destroy complete! Resources: 2 destroyed.
```
