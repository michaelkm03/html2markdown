The following tables lists any relevant outages or other bugs with the analytics service and resulting cleanups to the Redshift databases' main tables (vic_raw_event, vic_install, vic_dau, vic_event). All times are listed in UTC. 

|Date of Issue Start|Date of Issue End|Tables Impacted|Issue Description|Clean Up Done|
|-------------------|-----------------|---------------|-----------------|-------------|
|2016-07-08 22:53:58|2016-07-11 20:32:59|vic_raw_event, vic_install, vic_dau, vic_event|The analytics service went down and no analytics events were making it to the database during this time period.|All tracking events had been logged so these logs were just replayed. Once the logs were replayed, the local_datetime was updated to correctly reflect the PST version of the UTC event_datetime field.Events included in the replay:* session start/end
* tile view
* view
* component view
* app performance
* ftue

Full details can be found [here.](https://docs.google.com/spreadsheets/d/1QP7oPajZPWpm6OYV26zIXWqqRS8243PpMAuW8-Cw6u0/edit#gid=0)|
|2016-07-12 17:54:50|2016-07-13 02:18:12|vic_raw_event, vic_install, vic_dau|The events during this period were not ingesting a PST local_datetime field.|Updated to include PST local_datetime using the UTC event_datetime field.|
|2016-07-08 21:27:19|2016-07-08 22:53:58|vic_install|The install events were not being sent to the vic_install table, though they were logged in the vic_raw_event tables.|Made copies of the installs in the vic_raw_event tables to the vic_install table.|
|2016-07-11 20:32:59|2016-07-12 18:44:52|vic_install|The install events were not being sent to the vic_install table, though they were logged in the vic_raw_event tables.|Made copies of the installs in the vic_raw_event tables to the vic_install table.|
|2016-09-12 07:00:00|2016-09-14 19:00:00|vic_raw_event, vic_install, vic_dau, vic_event|The analytics service was sending events using the mySQL app name rather than the redshift app name in the app field.|Updated the app field to the redshift app name across all 4 tables.|
|2016-09-12 23:43:57|2016-09-14 21:40:53|vic_raw_event, vic_install, vic_dau, vic_event|The analytics service sent the os version of the client device for both the os and app version fields in the database.5.1 launched during the outage (2016-09-13 18:15:00), complicating the cleanup.  |Only the first 7 (wassabi, eleventh gorgeous, katie price, joey graceffa, fitness blender, wong fu, and rajiv internal) 5.0 apps were cleaned up.Depending on the time of the event before or after the 5.1 launch, we used the most recent or subsequent correct app version to determine the app version for those impacted events.|
|2016-09-23 15:20:00|2016-09-23 15:35:00|vic_raw_event, vic_install, vic_dau, vic_event|Due to an issue with AWS, events were taking a long time to ingest into the database, resulting in time outs for the ingestion and duplicate events and a period of time (~7 min) where production was completely down and there were no events being sent to the database.|The duplicate events were deduped. No action taken on 7 minute outage.|
|2016-09-29 22:00:00|2016-10-05 07:00:00|vic_raw_event, vic_install, vic_dau, vic_event|5.2 on iOS was not making install, session start, session end, and ftue calls. 5.2.2 was created and released to the store with the appropriate calls.|Database events were not replaced. Rather, the vic_comp tables and the two main 5.0 tableau workbooks (5.0 comprehensive and 5.0 Scorecards) that utilize these events were updated to pull aggregated data using workarounds.|

