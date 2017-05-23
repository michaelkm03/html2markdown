The below details how these tables are created and adjustments we have had to perform in order to accurately capture data in periods of outages or other issues. If you need to rebuild table or make adjustments, the below is meant to ensure that data accuracy is maintained but user should ensure that any changes they make to these tables that impact historical data aligns with the scripts provided here. 

# Initial Table Creation

There are 11 tables that have been created prior to the launch of VIP in Oct 2016. These tables are created using three tables in the redshift public cluster: install, dau, and event. These tables were created to cover the period from the launch of our first apps through 4/15/2016, inclusive. Further inserts are discussed below.

* vic_comp_install
* vic_comp_registration
* vic_comp_dau
* vic_comp_engagement
* vic_comp_view
* vic_comp_tileview
* vic_comp_creation
* vic_comp_timeinapp
* vic_comp_repost
* vic_comp_retention

There are two additional tables that were created as a result of the launch of VIP that measures new subscribers and churned subscribers starting with the launch of Rajiv on Oct 26, 2016

* vic_comp_subscriber

## Table Inserts:

Because of database outages and data error issues, there are adjustments that have been made to specific tables to ensure accurate reporting. The following details the scripts and upstream tables that populate the vic_comp tables: [Detailed tables to use to populate vic_comp tables](https://docs.google.com/spreadsheets/d/1pa-cQvIQ-9eee1jPqe_-ArZ8jrjXJo4BIdw-brUkbJA/edit#gid=0). Adjustments outside of the daily script that are included in this file but more details are provided below to share logic and context around dates that require adjustment by table.  these daily updates. 

VIC_COMP_INSTALL

* From 7/8/2016 to 7/13/2016, there were numerous database issues that impacted the table. Action was taken at the time to ensure that the vic_install table accurately measured the events and current state is accurate. Use the vic_install table though these dates will be missing app_ver.
* 9/28/2016 22:00:00 to 10/05/2016 07:00:00, iOS version 5.2 was not firing install events. Use dedupe on all events for a user for these dates on iOS to replicate the install events in addition to the regular script pulling the non 5.2 iOS and Android installs.

VIC_COMP_REGISTRATION

* From 7/8/2016 to 7/12/2016, the analytics outage resulted in the loss of registration events and the replay did not include these lost registration events. As a result, sort to include the first instance that a 'ut' user map tag appears in the vic_dau table and consider that a registration event.  

VIC_COMP_DAU

* From 7/8/2016 to 7/12, the analytics outage impacted these events. The session start events which are used to calculate DAU were replayed and all data is present in the vic_dau table but is missing app_ver. Continue to use the vic_dau table through this period.
* From 9/28/2016 22:00:00 to 10/05/2016 07:00:00, iOS version 5.2 was not firing session start events. Use the regular script for Android, and non 5.0 iOS apps. DAU for active 5.0 iOS apps calculated DAU but switching the table over to vic_event and calculating the distinct user_map_tags present for those apps on those days for any event.

VIC_COMP_ENGAGEMENT

* From 7/8/2016 to 7/12/2016, the analytics outage resulted in the loss of engagements events and the replay did not include these lost engagement events. As a result, use the mySQL tables to populate the 5 components of engagements: like, comment, emotive ballistics, poll_response, reposts. 

VIC_COMP_VIEW

* From 7/8/2016 to 7/12/2016, the analytics outage resulted in the loss of view events but were all restored with the replay. Continue to use the standard update script pulling from the vic_event table. 

VIC_COMP_TILEVIEW

* From 7/8/2016 to 7/12/2016, the analytics outage resulted in the loss of tile view events but were all restored with the replay. Continue to use the standard update script pulling from the vic_event table. 

VIC_COMP_CREATION

* From 3/1/2016 to approximately 8/1/2016, creations made on iOS devices were sent without a device_tag and with the os = 'victorious'. Needed amend query to categories creations that fall into this category as iOS.
* From 7/8/2016 to 7/12/2016, the analytics outage resulted in the loss of creation events and were NOT restored by the replay. As a result, use the mySQL tables to populate the creations made in any of the apps during this period from sequences table where category NOT LIKE '%repost%'.

VIC_COMP_TIMEINAPP

* From 7/8/2016 to 7/12/2016, the analytics outage impacted these metrics. The session end events which contain the time spent in app by the user was replayed and all data is present in the vic_event table but is missing app_ver. Continue to the use the vic_event table to populate data through this period. 
* From 9/29/2016 to 10/05/2016, iOS version 5.2 was not firing session start events. Use the regular script for Android, and non 5.0 iOS apps. time in app for active 5.0 iOS apps is calculated by taking the difference between the earliest and latest timestamps associated with a session_id and aggregating them across the standard dimensions. 

VIC_COMP_REPOST

* From 7/8/2016 to 7/12/2016, the analytics outage resulted in the loss of creation and repost events and were NOT restored by the replay. As a result, use the mySQL tables to populate the creations made in any of the apps during this period from sequences table where category LIKE '%repost%'.

VIC_COMP_RETENTION

* From 7/8/2016 to 7/12/2016, the analytics outage resulted in impacted the install and session_start events. The replay restored these events so the retention query and vic_install / vic_dau tables can continued to be used to populate data across this period. 
* From 9/28/2016 to 10/5/2016, iOS version 5.2 failed to fire session start and install events. Continue to use the regular script for Android and non 5.0 iOS apps. Retention for active 5.0 iOS apps is calculated by using the registration event as the install for registrations on 5.2 on any of the seven 5.0 apps and unique UT user_map_tag that occur on any of these days for these apps. 

VIC_COMP_SUBSCRIBER

Details on the vic_comp_subscriber table can be found here: 

