## Part 1 - Victorious Data Sources

The main repository for data and querying is **__RedShift__** (accessed either through the [Data Cortex web interface](https://report.data-cortex.com/home) or through [DBVisualizer](https://docs.google.com/document/d/1JF-u0P8BAf4Z_tUdd0gFT6khhlXKWx2HnLd9dr7xTLU/edit)). It stores data for all relevant events and is considered the source of truth for all Victorious's numbers. It should be used for ad hoc queries for data needed not immediately available on Tableau. 

Currently, there are additional data sources that are used in analyses with the ultimate goal is to move the relevant data to Redshift so that Redshift can be used as our sole source of truth. Current secondary sources include: 

* __MySQL__ - Used to populate the CMS and app content and has more detailed sequence/content data but does not contain other event based information like installs, daily activity, and session specific data.
* __Crashlytics__ - Contains crash data and affected user counts by app, os, date, and app version.
* __JIRA__ - Contains information on sprint data used to analyses internal efficiency.
* __SFIQ__ - Includes information on the deal status of creators we've signed with. 
* __Apple and Google Store Reports__ - Includes financial transaction details provided from the respective stores.  
* __Elastic Search/Sumo/AppDynamics__ - App Performance/speed at a given time. (no current connectivity to Redshift)
* __AppAnnie Intelligence__ - Install and Revenue data by app, os, and date. (no current connectivity to Redshift)

For more information and resources on SQL and current data sources see: [SQL Training](https://docs.google.com/presentation/d/1-msOKCDr6m-w2h0_JUhOsvx1Md2xaKhS9ghWtyT25Sk/edit?ts=56b25722#slide=id.ga24205b28_0_0)

For common queries for the main app performance metrics, these can be found on .

## Part 2 - Redshift Querying 

In order to derive data that can creating meaningful insights and actions, it is important to understand the database structure. The relevant data structure of the Redshift database falls into two schemas: Public and Vicky. The Public schema is the source of the KPIs where the Vicky schema feeds the CMS with more granular information on specific sequences and interactions.* In the *Public* schema, the main tables are Install, DAU, and Event. The Event table is the heaviest and contains all of the data on sessions, creations, views, engagements, and more. Examples of how to pull certain information from these three main tables can be found in the attached training deck below. 
* The *Vicky* schema is the data coming from MySQL and contains copies of the the main tables that are used on the backend/CMS. The main tables are Sequences, Users, and Apps. Sequences is the individual pieces of content posted within the app. Users are all users registered within the apps. Apps are a listing of all of our apps including their app_ids, which will help when pulling from the former two tables. In addition, there is a field in the Apps table called redshift_app_name that can be used to join data between the two schemas. Additional secondary tables are also available in redshift that include follows, tags, and sequence engagements. 
  

Additional information, including the basics of SQL, the essential queries we use, and some of the tricks you need to know before you start to pull the data can be found here: [SQL Training Part 2](https://docs.google.com/presentation/d/1A64IHMlmx3TMrnguzrXCW2j6dwtzJzPStpHGaAQeN98/edit#slide=id.ga24205b28_0_0)


  

