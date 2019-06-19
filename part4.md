# Getting started with Data Analytics on AWS

## Section 4: ETL MySQL DB to S3

### Creating Security Group
1. Go to [Security Group console](https://ap-southeast-1.console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#SecurityGroups:sort=vpcId)
2. Click *_Create Security Group_*
3. Fill in details as following

>Security group name = AWSGlueConnectionSGWorkshop

>Description = AWSGlueConnectionSGWorkshop

>VPC = AWSWorkshop

4. Click *_Create_*
5. Locate your Security Group by searching for *_AWSGlueConnectionSGWorkshop_*
6. Click on *_Inbound_* tab
7. Click *_Edit_*
8. Fill in details as following

>Type = All Traffic

>Source = In textbox, type *_sg_*. In the dropdown, choose *_AWSGlueConnectionSGWorkshop_*

9. Click *_Save_*

### Creating Glue Connection
1. Go to [Glue Connection console](https://ap-southeast-1.console.aws.amazon.com/glue/home?region=ap-southeast-1#catalog:tab=connections).
2. Click *_Add connection_*.
3. Fill in details as following

>Connection name = connectionToDB

>Connection type = JDBC

4. Leave other settings as default.
5. Click *_Next_*.
6. Fill in details as following

>JDBC URL = `ask us`

>User name = JDBC

>Password = `ask us`

>VPC = awsworkshop

>Subnet = private subnet

>Security groups = AWSGlueConnectionSGWorkshop

7. Click *_Next_*
8. Click *_Finish_*
9. Click checkbox on the name of your connection, click *_Test connection_*
10. Choose the role
11. Click *_Test connection_*
12. Wait for the test to be completed. You should see green pop up box once it is done.

### Creating Glue Crawler
1. Go to [Glue Crawler console](https://ap-southeast-1.console.aws.amazon.com/glue/home?region=ap-southeast-1#catalog:tab=crawlers).
1. Click *_Add crawler_*.
1. Fill in details as following

>Crawler name:: Section4DBCrawler

>Leave other settings as default.

4. Click *_Next_*.
5. Fill in details as following

>Crawler source type = Data stores

6. Click *_Next_*.
7. Fill in details as following

>Choose a data store = JDBC

>Connection = connectionToDB

>Include path = multitenantapp/%

8. Leave other details as default.
9. Click *_Next_*.
10. At *_Add antoher data source_* page, leave as default.
11. Click *_Next_*.
12. Fill in details as following

>IAM Role = AWSGlueServiceRoleS3FullAccess

14. Click *_Next_*.
15. At *_Create a schedule for this crawler_*, leave as default (Run on demand).
16. Click *_Next_*.
17. Fill in details as following

>Choose database = masterdb

>on pop up, enter *_masterdb_*. Leave others as default and click *Create*.

>Prefix added to tables = *_mysql___*

18. Leave other details as default.
19. Click *_Next_*.
20. Look through the summary
21. Click *_Finish_*.

### Running Glue Crawler
1. After creating Glue Crawler, a green box pops up
2. Click *_Run it now?_*.
3. It takes about 5 minutes to run
4. Once it finishes, you should see another green box pop up. Make sure you see *_2 tables created_*

### Using Glue Spark job to move data from MySQL to S3
1. Go to [Glue Job console](https://ap-southeast-1.console.aws.amazon.com/glue/home?region=ap-southeast-1#etl:tab=jobs).
2. Click *_Add job_*.
3. Fill in details as following

>Name = MySQLtoS3

>IAM role = AWSGlueServiceRole_S3FullAccess

4. Leave other settings as default.
5. Click *_Next_*.
6. Choose *_mysql_multitenantapp_users_* 
7. Click *_Next_*.

>Click on Create tables in your data target

>Data store = Amazon S3

>Format = Parquet

>Target path = s3://<Your bucket name>/users

8. Click *_Next_*.
9. Click *_Save job and edit script_*
10. Replace the given script with the script below. (This is to reduce the number of steps)

```python
import sys
from awsglue.transforms import *
from awsglue.utils import getResolvedOptions
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from awsglue.job import Job

## @params: [JOB_NAME]
args = getResolvedOptions(sys.argv, ['JOB_NAME'])

sc = SparkContext()
glueContext = GlueContext(sc)
spark = glueContext.spark_session
job = Job(glueContext)
job.init(args['JOB_NAME'], args)

datasource0 = glueContext.create_dynamic_frame.from_catalog(database = "masterdb", table_name = "mysql_multitenantapp_tenants", transformation_ctx = "datasource0")
datasink0 = glueContext.write_dynamic_frame.from_options(frame = datasource0, connection_type = "s3", connection_options = {"path": "s3://<YOUR BUCKET>/tenants/"}, format = "parquet", transformation_ctx = "datasink0")

datasource1 = glueContext.create_dynamic_frame.from_catalog(database = "masterdb", table_name = "mysql_multitenantapp_users", transformation_ctx = "datasource1")
datasink1 = glueContext.write_dynamic_frame.from_options(frame = datasource1, connection_type = "s3", connection_options = {"path": "s3://<YOUR BUCKET>/users/"}, format = "parquet", transformation_ctx = "datasink1")


job.commit()
```
11. Make sure to replace `<YOUR BUCKET>` (at line 18 and 21) with your actual bucketname in the format of `<yourbucket>awsworkshop`.
8. Click *_Save_*
9. Click *_Run job_*
10. Leave all settings at default.
11. Click *_Run job_*
12. It takes about 5 minutes for it to complete.
13. You can close the editor by clicking on large X on top right side.
14. Click on the name of your job on the console to monitor progress.


