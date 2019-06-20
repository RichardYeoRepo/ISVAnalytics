# Getting started with Data Analytics on AWS

## Section 5: Verifying results from ETL

### Creating Glue Crawler
1. Go to [Glue Crawler console](https://ap-southeast-1.console.aws.amazon.com/glue/home?region=ap-southeast-1#catalog:tab=crawlers).
1. Click *_Add crawler_*.
1. Fill in details as following

>Crawler name:: Section5Crawler

>Leave other settings as default.

4. Click *_Next_*.
5. Fill in details as following

>Crawler source type = Data stores

6. Click *_Next_*.
7. Fill in details as following

>Include path = `<YOUR BUCKET>/users`

8. Leave other details as default.
9. Click *_Next_*.
10. At *_Add antoher data source_* page, *_choose Yes_*
11. Click *_Next_*.
12. Fill in details as following

>Include path = `<YOUR BUCKET>/tenants`

13. Leave other details as default.
14. Click *_Next_*.
15. At *_Add antoher data source_* page, *_choose No_*
11. Click *_Next_*.12. Fill in details as following

>Choose an existing IAM role = `tick`

>IAM Role = AWSGlueServiceRoleS3FullAccess

13. Leave other details as default.
14. Click *_Next_*.
15. At *_Create a schedule for this crawler_*, leave as default (Run on demand).
16. Click *_Next_*.
17. Fill in details as following

>Choose database = masterdb

>on pop up, enter *_masterdb_*. Leave others as default and click *Create*.

>Prefix added to tables = *_rdstos3___*

18. Leave other details as default.
19. Click *_Next_*.
20. Look through the summary
21. Click *_Finish_*.
22. Once the crawler is done, you can use Athena to review the data.

Once done, go to [part 6](https://github.com/RichardYeoRepo/ISVAnalytics/blob/master/part6.md)
