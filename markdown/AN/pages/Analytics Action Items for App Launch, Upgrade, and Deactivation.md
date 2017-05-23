The following are the action items that need to be taken for when a app reaches certain milestones.

## App Launch: 

* launch_date table in Redshift:  * If there are not already two lines in the table for the launching app, one for each OS, add two entries to the table, one for each OS. 
  * Set *launch_date* field for both OS lines to the PST date the app launched. 


  * Set *status* field for both OS lines to "live" 


  * Set *crashlytics_app_name* field to match the equivalent data in the crashlytics_raw_data table. As a watch out, some of the crashlytics_app_name entries will have two app names, one for each os. 
  * Set *siq_account_id* field to match the equivalent data in the siq_opportunities table. Unlike the crashlytics_app_name entries, there should only be one id for each app. 


* [Murali Blessed Google Sheet File](https://docs.google.com/spreadsheets/d/1P9YOWFd4w0M_0w690SC3pFlnwO9IMCMfP9h6Y1eSL7s/edit#gid=653490561):   * In the __Live Apps - 30-day and Life-to-Date Conversion__, add in a new row at the top of the dataset and include in the relevant data for columns A to L. 
  * Ensure that the ASN and TSN numbers are correct.



## App Upgrade to 5.0+ (including when an app is launched on 5.0+): 

* launch_date table in Redshift:   * Set *five0_launch_date* for both OS lines to the PST date the app launched or was upgraded to 5.0+. 
  * Set five0_status for both OS lines "alpha", "beta", or "gamma" , depending on the status of the app being launched. 



## App Deactivation:

* launch_date table in Redshift  * Set *deactivation_date* field for both OS lines to the PST date the app was taken out of the respective stores. 


  * Set *status* field for bos OS lines to "inactive".


* [Murali Blessed Google Sheet File](https://docs.google.com/spreadsheets/d/1P9YOWFd4w0M_0w690SC3pFlnwO9IMCMfP9h6Y1eSL7s/edit#gid=653490561):   * In the __Live Apps - 30-day and Life-to-Date Conversion__, update the deactivation date column (column F) with the appropriate deactivation date. 



