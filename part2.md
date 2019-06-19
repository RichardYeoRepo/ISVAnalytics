# Getting started with Data Analytics on AWS

## Section 2: Using SQL against data on S3

### Creating Glue Crawler
1. Go to [Glue Crawler console](https://ap-southeast-1.console.aws.amazon.com/glue/home?region=ap-southeast-1#catalog:tab=crawlers).
1. Click *_Add crawler_*.
1. Fill in details as following

>Crawler name:: Section2S3Crawler

>Leave other settings as default.

4. Click *_Next_*.
5. Fill in details as following

>Crawler source type = Data stores

6. Click *_Next_*.
7. Fill in details as following

>Crawl data in = Specified path in another account

>Include path = s3://datatoshare/fakedata/

8. Leave other details as default.
9. Click *_Next_*.
10. At *_Add antoher data source_* page, leave as default.
11. Click *_Next_*.
12. Fill in details as following

>Choose an existing IAM role = `tick`

>IAM Role = AWSGlueServiceRoleS3FullAccess

13. Leave other details as default.
14. Click *_Next_*.
15. At *_Create a schedule for this crawler_*, leave as default (Run on demand).
16. Click *_Next_*.
17. Fill in details as following

>C1ick *_Add database_*

>on pop up, enter *_masterdb_*. Leave others as default and click *Create*.

>Prefix added to tables = *_apachelog__*

18. Leave other details as default.
19. Click *_Next_*.
20. Look through the summary
21. Click *_Finish_*.

### Running Glue Crawler
1. After creating Glue Crawler, a green box pops up
2. Click *_Run it now?_*.
3. It takes about 5 minutes to run
4. Once it finishes, you should see another green box pop up. Make sure you see *_1 tables created_*

### Verifying the result of the Glue Crawler
5. On the left pane, click on Databases.
6. Locate the *_masterdb_* and click on it.
7. Click on the link *_Tables in masterdb_*.
8. Click on the name of the table.
9. Look through the details.

### Using Athena to run SQL queries
1. Go to [Athena console](https://ap-southeast-1.console.aws.amazon.com/athena/home?region=ap-southeast-1#query)
2. On the left pane, look for *_Database_*.
3. Click on the drop down list and select *_masterdb_*.
4. Below your database, you should see the *_apachelog_fakedata_*
5. Click on three vertical dots on the right of the table name and select *_Preview table_*. Your result should look like below.
6. If you are familiar with SQL, try querying data.

