While investigating the sequential numbering analytics tracking designed to track the sequence of user actions while in app, it was discovered that there are a number of sessions that are missing either the corresponding session start and/or the session end event. These events both are used in reporting, with the session start event used to determine the active user metrics and the session start to determine the time in app by a user. In order to determine the full impact of these missing events, the following analysis was done to determine what % of these are events are missing starting from 4/16/2016 through 5/18/2016.

![](wiki-attachment:Starts and Ends by OS.png)

Missing Session Starts impact the two clients differently. For iOS, 99% of sessions have corresponding session starts, essentially indicating that the missing session start events impact Android solely, which only have just under 70% of the corresponding session starts. However, there both clients are missing a significant volume of the session ends, iOS missing more, about 47% of all sessions end events, while Android is missing about 35% of all session end events. 

A deep dive into both clients show that the newer App Versions drive many of the missing session starts as well as session ends on the affected clients. 

*Rates of missing events by App Version by Android and iOS, respectively: *

*![](wiki-attachment:Starts and Ends By App Ver for Android.png)  ![](wiki-attachment:Starts and Ends By App Ver for iOS.png)*

In contrast to the sessions that are missing either the session start or the session end events, 1.2% of all Android sessions are missing both the session start and session end event, with that number dropping to 0.4% for all iOS sessions, aggregating to 99% for the network. This means, that most of these sessions are capturing either start or end event, necessary for the Back End team's purposes. 

The number of sessions that have BOTH start and end events are significantly lower, as is to be expected. This is occurring at a rate of 36% for Android sessions and 53% for iOS. 

Further analysis was done on app, date, user country, and types of session starts to identify any potential drivers. All categorization did not yield any clear trends with the exception of the types of session starts. Compared to iOS, Android see a lower share of session starts considered Cold direct starts (organic start versus opened from a push notification). Additional analysis is being conducted to determine is there is any historical drops or peaks in these rates and will be updated. 

The following tickets were created to fix these on both clients: 

Android Missing Session Starts: 

Android Missing Session Ends: 

iOS Missing Session Ends: 

 

**Query: **


```
SELECT sessions.app
, sessions.os
, sessions.app_ver
, first_event::date as date
, count(sessions.group_tag) as total_sessions 
, count(starts.group_tag) as sessions_with_starts
, count(ends.group_tag) as sessions_with_ends
FROM
 (
 SELECT app
 , os
 , app_ver
 , min(local_datetime) as first_event
 , group_tag
 FROM vic_event
 WHERE local_datetime::date>='2016-04-16'
 AND local_datetime::date<getdate()::date 
 AND os NOT LIKE 'victorious'
 GROUP BY 1,2,3,5
 ) sessions
LEFT JOIN 
 (
 SELECT distinct app
 , os
 , app_ver
 , group_tag
 FROM vic_event
 WHERE local_datetime::date>='2016-04-16'
 AND local_datetime::date<getdate()::date
 AND kingdom='session'
 AND phylum IN ('session_start')
 ORDER BY 1,2,3
 ) starts
ON sessions.os=starts.os
AND sessions.app=starts.app
AND sessions.group_tag=starts.group_tag
AND sessions.app_ver=starts.app_ver
LEFT JOIN 
 (
 SELECT distinct app
 , os
 , app_ver
 , group_tag
 FROM vic_event
 WHERE local_datetime::date>='2016-04-16'
 AND local_datetime::date<getdate()::date
 AND kingdom='session'
 AND phylum IN ('session_end')
 ORDER BY 1,2,3
 ) ends
ON sessions.os=ends.os
AND sessions.app=ends.app
AND sessions.group_tag=ends.group_tag
AND sessions.app_ver=ends.app_ver
GROUP BY 1,2,3,4
ORDER BY 4,3,1
```
 

