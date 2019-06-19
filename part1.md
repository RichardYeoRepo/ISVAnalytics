# Getting started with Data Analytics on AWS

## Section 1: Introduction

### What are we building?

In this workshop, we will look at how we can build visualization with data from different sources.
The sources that we will be looking at today are

1. Static files, e.g. CSV, Excel files
2. RDBMS datasources, e.g. MySQL, SQL Server
3. Data produced by application, e.g. click data


*_All_* data that we are using today is completely fictional and any similiaries are coincidential.

Following are the list of services that we will look at today

- https://aws.amazon.com/kinesis/data-firehose/[Kinesis Firehose]
- https://aws.amazon.com/athena/[Athena]
- https://aws.amazon.com/quicksight/[QuickSight]
- https://aws.amazon.com/s3/[S3]
- https://aws.amazon.com/glue/[Glue]

### Setting up data lake
1. Go to [S3 console](https://s3.console.aws.amazon.com/s3/home?region=ap-southeast-1#).
1. Click **Create bucket**
1. Fill in details as following

>Bucket name = `yourname`analyticsworkshop

>Region = Asia Pacific (Singapore)

4. Leave other settings at default
5. Click **Create button** at the left bottom corner.

### Setting up IAM roles
1. Go to AWS [IAM role console](https://console.aws.amazon.com/iam/home?region=ap-southeast-1#/roles).
1. Click *_Create role_*
1. Click *_Glue_* in the list.
1. Click *_Next: Permissions_*
1. In the search box, type in *_AmazonS3FullAccess_*.
1. Click the *_checkbox_* next to the policy name
1. Clear the search box and type in *_AWSGlueServiceRole_*
1. Click the *_checkbox_* next to the policy name
1. Click *_Next: Tags_*
1. Leave as default
1. Click *_Next: Review_*
1. Fill in details as following

>Role name = AWSGlueServiceRole_S3FullAccess

13. Click *_Create role_*

### Setting up VPC
1. Go to AWS [Elastic IP console](https://ap-southeast-1.console.aws.amazon.com/vpc/home?region=ap-southeast-1#Addresses:sort=PublicIp)
2. Click *_Allocate new address_*
3. Click *_Allocate_*
4. Take note of the Elastic IP address and do let us know. (This IP address is used to whitelist access to Database)
5. Click *_Close_*
***
1. Go to AWS [VPC Wizard](https://ap-southeast-1.console.aws.amazon.com/vpc/home?region=ap-southeast-1#wizardSelector:).
2. Select *_VPC with Public and Private Subnets_*.
3. Click *_Select_*.
4. *_Leave all settings as default_*. Only change settings mentioned below

>VPC name: = AWSWorkshopVPC

>Elastic IP Allocation ID:* = *_Click on the text box to automatically populate_*

5. Click *_Create VPC_*

