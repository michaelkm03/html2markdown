### **Overview**

Initial analytics service has previously been provided by an outsourced provider (Data Cortex). As a company we are transitioning to an in house analytics service. Data Validation is needed in order to verify that the new service will provide the same quality of data that was previously provided as well as identify major discrepancies and their causes. This page details the row and counts on unique user ID comparions on from the three main tables (raw_install, raw_event, raw_dau) against the new vic_raw_event table. 

### **Methodology**

The data from the new analytics service began feeding into the vic_raw_event table starting on Feb 17, 2016. All the following comparisons are intended to compare the vic_raw_event for two time periods, before and after Feb 17. 

Comparing the number of rows in the vic_raw_event table to the raw tables for Install, DAU, and Event. 

Comparing the number of rows as well as the number of distinct device_tag and user_map_tag in the vic_raw_event table to the distinct user_id in the main tables. 

Pulling the metrics indexing on device_tag from vic_raw_event and comparing to the current metrics used for the comprehensive metrics.

Additionally, views will be generated using the vic_raw_event to mimic the current materialized views as vic_install, vic_dau, and vic_event, adding in a PST time stamp and utilizing user_map_tag assignment convention with the aim to replicate the current user_id assignment process. We will be comparing these to the existing materialized tables once they have been generated.

### Findings

When comparing all events in the vic_raw_event table to the current views in all of their current iterations, there is alignment between most of the metrics including registrations, creations, reposts, engagements, session time, and tile views. After additional adjustments following some preliminary data comparisons, there is also alignment in session starts and views. 

Installs and DAU are both determined by pulling the number of distinct users that install or appear in the app for a given time period based on the user_id field. In the vic_raw_event table, there is no such "User_ID" field so these distinct users are determined using the device_tag or user_map_tag. When capturing installs using device tag, there are two spikes in installs in June 2015 driven by a known data error that occurred during that time. Additionally, since June, there are rising percentage of installs using the vic_raw_event table as compared to the installs table currently holding between 10% to 20% driven by individual users installed the app on multiple devices. The trend can be seen in the below graph.

![](https://lh3.googleusercontent.com/xoSzni3YskzhsBqosN97t0aCoXCPhFhnajKnHhUaxHElxjkc_3beuBgH87YKA96rSQNIltLDJdN8X3ft3c8QSAFFwUJM2O8ox4qvMGav8_71TWxtvhbywXTg6dWtBtD4kfe2QOlt)

For DAU, there are less active users in the vic_raw_event table compared with the current tables with spikes in discrepancies in the beginning of 2015 and during the fall months of 2015. However, starting in October and November 2015, we start to see slightly higher dau from the vic_raw_event table holding at around 4% to 5% driven by individual users accessing the app on multiple devices within the same day. This trend can be seen in the below graph.

![Comparing DAU using user_id from the DAU table to the device_tag from the vic_raw_event table](wiki-attachment:Screen Shot 2016-03-10 at 1.58.18 PM.png)

All major KPIs are updated daily and can be compared using the following Tableau report: [Victorious Analytics Comparisons](https://10ay.online.tableau.com/#/site/victorious/workbooks/249492/views)

Complete and detailed findings can be found [here](https://drive.google.com/open?id=1xyReNfhQ5tr5G_WhPP7ybP_KW5B0I-RF0Cl39AazVRI). 

