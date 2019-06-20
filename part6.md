
###Setting up QuickSight
1. Go to [Quicksight console](https://ap-southeast-1.quicksight.aws.amazon.com/sn/start?)
2. You will be directed to the sign up page. Click *_Sign up for QuickSight_*.
3. Choose non-enterprise.
4. Click *_Continue_*.
5. Choose *_Use Role Based Federation (SSO)_*
6. Fill in details as following

>QuickSight account name = `_Any name_`

>Notification email address = `_Any email_`

>QuickSight region = Asia Pacific (Singapore)

>Auto Discovery = Select Athena and S3. Choose S3 bucket that we created in this workshop.

7. Leave others as default.
8. click *_Finish_*.
9. Once the loading is done, click *_Go to Amazon QuickSight_*.
10. Feel free to following through the introduction by clicking *_Next_*.
11. Click *_Get Started_*.


### QuickSight with Athena
1. Click *_New analysis_*
2. Click *_New data set_*
3. Choose *_Athena_*.
4. Fill in details as following

>Data source name = applicationData

5. Click *_Validate connection_*
6. Once done, click *_Create data source_*.
7. In the *_Database: contain sets of tables._* dropdown, choose *_masterdb_*.
8. In the *_Tables: contain the data you can visualize._*, choose *_transformedapachelog_* table.
9. Click *_Select_*.
10. Choose *_Directly query your data_*.
11. Click *_Edit Preview Data_*.
12. Click *_Add data_* under the search bar
13. Add *_rdstos3_multitenantapp_users_*
14. Add *_rdstos3_multitenantapp_tenants_*
15. You should see 3 blocks on the console, click on adjacent circles to start joining your data.
16. *_rdstos3_multitenantapp_users_* joins with *_transformedapachelog_* on id=userid 
17. *_rdstos3_multitenantapp_users_* joins with *_rdstos3_multitenantapp_tenants_* on tenandid=id
18. You can drag around the blocks to change tables to join to.
19. Once done, you should see a table with list of values populated.
20. Click on *_Save & Visualize_* to start visualizing.
. Once done, we can start visualizing.
