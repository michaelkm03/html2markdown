With the launch of the VIP product, it becomes necessary to collect data on individual users and aggregate them accurately across all reporting. However, all of the data we are interested in collecting is not located in a singular location. The following includes how this data is collected as of December 2nd, 2016 and how that data is queried for reporting purposes.

# Collecting Subscriber Information: 

Run the following query against the primary redshift database to determine all new subscribers. It finds the first instance of a successful validation for a victorious user id. This process ignores any user who subscribed, had churned and resubscribed, which will need to be addressed in the future.Take the pertinent information for all new subscribers and copy and paste that data into this file under the appropriate app's tab: [https://docs.google.com/spreadsheets/d/1K3jGaR39wKur8MZtbTQudAaYedFH__s4xcl96S6-rcg/edit#gid=188992149](https://docs.google.com/spreadsheets/d/1K3jGaR39wKur8MZtbTQudAaYedFH__s4xcl96S6-rcg/edit#gid=188992149)Determine these new subscribers' email and usernames from the user dimension table (currently using mySQL) and include those in the file as well.Determine the new subscribers' purchased skus, subscription end date, and either the purchase token for Android subs or the original transaction ID for iOS subs and copy those into the google sheets file as well.Once this data is collected, determine if any of these users are internal subscribers that are employees or otherwise affiliated with Victorious. This should be clear from the user names and emails used determined in step 3.In the google sheets file, if the subscribers is a victorious user, rather than including the SKU that is listed on their receipt, the field should show "victorious" and column K should show the sku that was purchased for this user. If the email associated with any internal account ends in "@victorious" , "@getvictorious" , "@example.com" , OR "@something.net", then no additional work needs to be done. If not, this user will have to be removed from scheduled data pulls for determining external subscribers. This process is being done each morning.

# Removing Non Conforming Internal Subscribers & other non-External Subscribers

While there are not many non conforming internal subscribers or otherwise non-External Subs, all instances need to be removed from the queries that users to populate reported numbers. All instances of where there is a subscriber query need to be updated to remove this non-external subscriber. Which workbooks these are found, and how often they should be updated are listed below: 

* [5.0 Initial Comprehensive Workbook](https://10ay.online.tableau.com/#/site/victorious/workbooks/330876/views): "Subscriber" data source should be updated daily if there is a non-conforming internal user or non-external subscriber.
* [App and Functional Scorecard](https://10ay.online.tableau.com/#/site/victorious/workbooks/344897/views): "Subscriber" data source should be updated at the beginning of each week, at a minimum.
* [5.0 Apps Dashboard](https://10ay.online.tableau.com/#/site/victorious/workbooks/332123/views): "Subscriber" data source should be updated at the beginning of each week, at a minimum.

In addition, the user_summary_master table needs to be updated. This is run each day, even when there are no instances of a non-conforming internal user or non-external subscriber that needs to be removed. 

[https://github.com/Victorious/analytics/blob/master/user_summary_master%20table%20updates.sql](https://github.com/Victorious/analytics/blob/master/user_summary_master%20table%20updates.sql)

EDIT: This page is outdated by [vic_comp_subscriber table update](https://wiki.victorious.com/display/AN/vic_comp_subscriber+table+update) as of 4/1/2017. Page describes the automation of the manual process described here 

