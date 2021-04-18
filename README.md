# AWS GLUE SECURITY CONFIG RULES
 This Config Conformance Pack enforces security best practices for the AWS Glue Service. It contains the following:
 1. Glue Job Security Configuration is encrypted for S3 and CloudWatch
 2. Glue Crawler Security Configuration is encrypted for S3 and CloudWatch 
 3. Glue Data Catalog's metadata and passwords are encrypted
 4. Glue Connection requires SSL
 5. An SNS topic for notifications of Glue Job failures

There are also remediation lambdas in this package.

Please note that the Security Configuration remediating actions require creating a new KMS Key and a new security configuration. The remediation will then swap the previous security configuration for the newly created secure one. 

## Steps
1. Upload the entire 'Lambdas' Folder into an S3 bucket - so you have keys like bucket-name/Lambdas/codefile.py.zip
2. Update the template's S3BucketName parameter to your S3 bucket name
3. Create stacks from the CloudFormation templates in the CloudFormation folder

## Drawbacks
- Enforcing Glue Connection SSL will only apply to JDBC or MongoDb connections


# CLI Commands to Create Templates
**Job Security Config Check**
> aws cloudformation delete-stack --stack-name glueJobSecurityConfigEncryption
> aws cloudformation create-stack --stack-name glueJobSecurityConfigEncryption --template-body file://glue_job_security_config_encryption.yml --capabilities CAPABILITY_IAM


**Crawler Security Config Check**
> aws cloudformation delete-stack --stack-name glueCrawlerSecurityConfigEncryption
> aws cloudformation create-stack --stack-name glueCrawlerSecurityConfigEncryption --template-body file://glue_crawler_security_config_encryption.yml --capabilities CAPABILITY_IAM

**Data Catalog Encrypted**
> aws cloudformation delete-stack --stack-name glueDataCatalogEncrypted
> aws cloudformation create-stack --stack-name glueDataCatalogEncrypted --template-body file://glue_encrypt_data_catalog.yml --capabilities CAPABILITY_IAM

**Crawler SSL Check**
> aws cloudformation delete-stack --stack-name glueCrawlerSSL
> aws cloudformation create-stack --stack-name glueCrawlerSSL --template-body file://glue_crawler_enforce_connection_ssl.yml --capabilities CAPABILITY_IAM

