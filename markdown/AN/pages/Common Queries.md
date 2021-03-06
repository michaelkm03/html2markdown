These queries are meant to work with the data cortex interface or using DB Visualizer. Please let  know if there are additional queries that you would like to see added. 

**Installs:**

SELECT 
  A.app
  , [apps.name](http://apps.name) as SQL_app_name 
  , cast(TO_CHAR(local_datetime::DATE, 'YYYY-MM-DD') as date) as date
  , A.os
  , datediff(day,L.launch_date,cast(TO_CHAR(local_datetime::DATE, 'YYYY-MM-DD') as date)) as days_since_launch
  ,COUNT(DISTINCT user_id) as installs
  FROM 
  (SELECT ROW_NUMBER() OVER(PARTITION BY app, os, user_id ORDER BY event_datetime) as row_num
  , *
  FROM install
  ) A
  join public.launch_date L
  on A.app = L.app
  and A.os=L.os
  and cast(TO_CHAR(local_datetime::DATE, 'YYYY-MM-DD') as date)>=L.launch_date
  INNER JOIN vicky.apps
  on A.app=apps.redshift_app_name
  WHERE row_num = 1
  GROUP BY 1,2,3,4,5
  ORDER BY 1,2,3,4,5

**Registrations: **

SELECT 
  app,
  [apps.name](http://apps.name) as SQL_app_name, 
  cast(TO_CHAR(local_datetime::DATE, 'YYYY-MM-DD') as date) as date,
  os,
  COUNT(*) AS registrations,
  COUNT(DISTINCT event.user_id) AS unique_registered_users 
  FROM event as event
  INNER JOIN vicky.apps
  ON event.app=apps.redshift_app_name
  WHERE event."kingdom" LIKE 'registration'
  GROUP BY 1,2,3,4

DAU

SELECT 
  dau.app 
  , l.SQL_app_name
  , cast(TO_CHAR(dau.local_datetime::DATE, 'YYYY-MM-DD') as date) as date
  , dau.os
  , datediff(day,l.launch_date, CAST(TO_CHAR(dau.local_datetime::date, 'YYYY-MM-DD') as date)) as days_since_launch
  , COUNT(DISTINCT dau.user_id) as dau 
  FROM 
  (SELECT ROW_NUMBER() OVER(PARTITION BY dau.app, dau.os, dau.user_id, dau.local_datetime::DATE ORDER BY dau.event_datetime) as row_num
  , *
  FROM dau
  ) dau
  JOIN install
  on dau.app = install.app
  and dau.user_id = install.user_id
  and dau.event_datetime>=install.event_datetime
  JOIN launch_date l 
  on dau.app=l.app
  AND dau.os=l.os
  WHERE row_num = 1
  AND l.app_status IN ('live','inactive')
  GROUP BY 1,2,3,4,5

**Engagements**

SELECT app, cast(TO_CHAR(local_datetime::DATE, 'YYYY-MM-DD') as date) as date
  ,event."os",COUNT(*) AS engagements,COUNT(DISTINCT event.user_id) AS engagers

FROM event event

WHERE event."kingdom" LIKE 'engagement'
  GROUP BY 1,2,3

**Views and Video Views**

SELECT app
  , cast(TO_CHAR(local_datetime::DATE, 'YYYY-MM-DD') as date) as date
  , event."os"
  , COUNT(*) AS views,COUNT(DISTINCT event.user_id) AS viewers
  , sum(case when event."class"='video' then 1 else 0 end) as video_views
  , count(distinct case when event."class"='video' then user_id else null end) as video_viewers
  FROM event event

WHERE event."kingdom" LIKE 'view'
  and event."phylum" LIKE 'start'
  GROUP BY 1,2,3

**Tile Views**

SELECT app, cast(TO_CHAR(local_datetime::DATE, 'YYYY-MM-DD') as date) as date
  ,event."os",COUNT(*) AS tile_views,COUNT(DISTINCT event.user_id) AS tile_viewers

FROM event event

WHERE event."kingdom" LIKE 'tile_view'
  and event."phylum" LIKE 'start'
  GROUP BY 1,2,3

**Creations**

SELECT 
  app,
  cast(TO_CHAR(local_datetime::DATE, 'YYYY-MM-DD') as date) as date,
  event."os",
  COUNT(*) AS creations,
  COUNT(DISTINCT event.user_id) AS creators

FROM event event

WHERE event."kingdom" LIKE '%creation%'
  GROUP BY 1,2,3

**Time in App**

SELECT 
  app,
  cast(TO_CHAR(local_datetime::DATE, 'YYYY-MM-DD') as date) as date,
  os
  ,COUNT(*) AS sessions
  ,COUNT(DISTINCT event.user_id) AS session_starters,
  SUM(float1) AS time_in_msec 
  FROM event event
  WHERE event."kingdom" LIKE 'session' 
  AND event."phylum" LIKE 'session_end'
  AND event.float1>=0
  GROUP BY 1,2,3

**Reposts**

SELECT app, cast(TO_CHAR(local_datetime::DATE, 'YYYY-MM-DD') as date) as date
  ,event."os",COUNT(*) AS reposts,COUNT(DISTINCT event.user_id) AS reposters

FROM event

WHERE event."kingdom" LIKE 'engagement'
  and event."phylum" LIKE 'repost'
  GROUP BY 1,2,3

**Retention**

SELECT
  app,
  [apps.name](http://apps.name) as SQL_app_name,
  os,
  cast(first_date as date) as first_date,
  MAX(case when daysAfterInstall = 0 then start_count end) as start_count,
  MAX(case when daysAfterInstall = 1 then return_count end) as D1_Retention,
  MAX(case when daysAfterInstall = 2 then return_count end) as D2_Retention,
  MAX(case when daysAfterInstall = 3 then return_count end) as D3_Retention,
  MAX(case when daysAfterInstall = 4 then return_count end) as D4_Retention,
  MAX(case when daysAfterInstall = 5 then return_count end) as D5_Retention,
  MAX(case when daysAfterInstall = 6 then return_count end) as D6_Retention,
  MAX(case when daysAfterInstall = 7 then return_count end) as D7_Retention,
  MAX(case when daysAfterInstall = 8 then return_count end) as D8_Retention,
  MAX(case when daysAfterInstall = 9 then return_count end) as D9_Retention,
  MAX(case when daysAfterInstall = 10 then return_count end) as D10_Retention,
  MAX(case when daysAfterInstall = 11 then return_count end) as D11_Retention,
  MAX(case when daysAfterInstall = 12 then return_count end) as D12_Retention,
  MAX(case when daysAfterInstall = 13 then return_count end) as D13_Retention,
  MAX(case when daysAfterInstall = 14 then return_count end) as D14_Retention,
  MAX(case when daysAfterInstall = 30 then return_count end) as D30_Retention,
  MAX(case when daysAfterInstall = 60 then return_count end) as D60_Retention,
  MAX(case when daysAfterInstall = 90 then return_count end) as D90_Retention

FROM

(

SELECT 
  start_count.app,
  start_count.os,
  To_char(start_count.first_date, 'YYYY-MM-DD') AS first_date, 
  return_count.nday AS daysAfterInstall, 
  start_count.start_count, 
  return_count.return_count
  
  FROM

(
  SELECT app, os, first_date, Count(user_id) AS start_count 
  FROM
   (
   SELECT app, os, start_count.user_id, local_datetime :: DATE AS first_date 
   FROM 
   (
   SELECT * 
   FROM 
   (
   SELECT Row_number() over(PARTITION BY app,os,user_id ORDER BY local_datetime) AS row_num, 
   local_datetime :: DATE AS first_date, 
   * 
   FROM install 
   ) installs_with_row_num 
   WHERE row_num = 1 
   ) start_count 
   WHERE 1 = 1
   ) a 
  GROUP BY 1,2,3
  ) start_count 
  left join 
  (
  SELECT r.app,
   r.os,
   r.first_date, 
   r.nday, 
   Count(r.user_id) AS return_count 
   FROM 
   (
   SELECT starters.app,
   starters.os,
   starters.user_id, 
   starters.first_date, 
   returners.return_date, 
   returners.return_date - starters.first_date AS nday 
   FROM 
   (
   SELECT app,os,start_count.user_id, local_datetime :: DATE AS first_date 
   FROM 
   (
   SELECT * 
   FROM 
   (
   SELECT Row_number() over(PARTITION BY app,os,user_id ORDER BY local_datetime) AS row_num, 
   local_datetime :: DATE AS first_date, 
   * 
   FROM install 
   ) installs_with_row_num 
   WHERE row_num = 1 
   ) start_count 
   WHERE 1 = 1
   ) starters 
   inner join 
   (
   SELECT DISTINCT app,os,user_id, local_datetime :: DATE AS return_date 
   FROM dau 
   WHERE local_datetime :: DATE < Getdate() :: DATE
   )returners 
   ON starters.user_id = returners.user_id
   and starters.app = returners.app
   and starters.os=returners.os 
   AND returners.return_date >= starters.first_date
   ) r 
  GROUP BY 1, 2,3,4
  ) return_count
  
  ON start_count.app = return_count.app
  AND start_count.os = return_count.os
  AND start_count.first_date = return_count.first_date


  ORDER BY 1,2,3,4
  )
  INNER JOIN vicky.apps
  ON app=apps.redshift_app_name
  GROUP BY 1,2,3,4
  ORDER BY 1,2,3,4
  
  

