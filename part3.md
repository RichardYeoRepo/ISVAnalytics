# Getting started with Data Analytics on AWS

## Section 3: Using SQL against data on S3

### Creating Glue Job
1. Go to [Glue Job console](https://ap-southeast-1.console.aws.amazon.com/glue/home?region=ap-southeast-1#etl:tab=jobs).
2. Click *_Add job_*.
3. Fill in details as following

>Name = ApachelogJob

>IAM role = AWSGlueServiceRole_S3FullAccess

>Type = Python shell

>This job runs = A new script to be authored by you

4. Leave other settings as default.
5. Click *_Next_*.
6. Click *_Save job and edit script_*
7. Copy and paste the code. *_MAKE SURE TO REPLACE `<REPLAC ME>` WITH YOUR BUCKET NAME. It should be `<yourname>awsworkshop`_*

```Python
import boto3 
import json
import re

conn = boto3.client('s3')
count = 0
size = len(conn.list_objects(Bucket='datatoshare')['Contents'])
for key in conn.list_objects(Bucket='datatoshare')['Contents']:
    obj = conn.get_object(Bucket='datatoshare', Key=key['Key'])
    folder,filename = key['Key'].split("/")
    body = obj["Body"].read().decode('utf-8')
    body = re.sub('\?', ' ', body)
    body = re.sub('userID=', '', body)
    body = re.sub(' HTTP/1.0\"', ' HTTP/1.0', body)
    body = re.sub('] \"', '] ', body)
    boto3.resource('s3').Object('<REPLACE ME>', 'apachelog/'+ filename).put(Body=body)
    count = count + 1
    print(str(count) + "/" + str(size))
```
8. Click *_Save_*
9. Click *_Run job_*
10. Leave all settings at default.
11. Click *_Run job_*
12. It takes about 5 minutes for it to complete.
13. You can close the editor by clicking on large X on top right side.
14. Click on the name of your job on the console to monitor progress.

### Using Athena to look at files again.
1. Go to [Athena console](https://ap-southeast-1.console.aws.amazon.com/athena/home?region=ap-southeast-1#query)
2. On the left pane, look for *_Database_*.
3. Click on the drop down list and select *_masterdb_*.
4. Clear the query field.
5. Copy and paste the code. *_MAKE SURE TO REPLACE `<REPLAC ME>` WITH YOUR BUCKET NAME. It should be `<yourname>awsworkshop`_*

```sql
CREATE EXTERNAL TABLE `transformedApachelog`(
  `requestip` string COMMENT '', 
  `time` string COMMENT '', 
  `method` string COMMENT '', 
  `uri` string COMMENT '', 
  `userid` int COMMENT '', 
  `httpversion` string COMMENT '', 
  `status` int COMMENT '', 
  `size` int COMMENT '', 
  `referrer` string COMMENT '', 
  `browser` string COMMENT '', 
  `system` string COMMENT '')
ROW FORMAT SERDE 
  'org.apache.hadoop.hive.serde2.RegexSerDe' 
WITH SERDEPROPERTIES ( 
  'input.regex'='^([0-9]+.[0-9]+.[0-9]+.[0-9]+) - - \\[([0-9]{2}/[a-zA-Z]+/[0-9]+:[0-9]+:[0-9]+:[0-9]+ \\+[0-9]+)\\] ([A-Z]+) ([/a-z/.]+) ([0-9]+) (HTTP/1.[0-1]) ([0-9]{3}) ([0-9]+) \"(.+)\" \"(\\S+) (.+)\"$') 
LOCATION
  's3://<REPLAC ME>/apachelog'
```

6. Run your query
7. The console should refresh automatically and you will see new table *_transformedApachelog_* created.
8. Click on three vertical dots on the right of the table name and select *_Preview table_*. Your result should look like below.
9. If you are familiar with SQL, try querying data.

Once done, go to [part 4](https://github.com/RichardYeoRepo/ISVAnalytics/blob/master/part4)
