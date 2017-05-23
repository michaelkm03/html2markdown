While we have automated many of the previously manually datapulls, the Crashlytics data source is one that may require a return to manual updating. The following is the previously used methodology and will remain here if such a process needs to be undertaken once again 

## "Crash Data" data source

To update the ["Crash Data"](https://drive.google.com/a/getvictorious.com/file/d/0B1N-_25FQNl0NkVObGpRTTRhZUU/view?usp=sharing) data source, you have to determine the top 5 apps for the week by WAU. The queries to the right will run off the redshift database on both overall WAU as well as spliting it out by os.* If you are pulling this data on a Monday, it is important you either pull the data on the cluster or the that data source includes up to the Sunday at 11:59 PM. *

Once the data has populated, add the top 5 overall apps by WAU and their respective os WAU into the data excel file: 

![](wiki-attachment:Screen Shot 2016-03-04 at 11.47.57 AM.png)

![](wiki-attachment:Screen Shot 2016-03-04 at 11.50.55 AM.png)

Then, you will need to open up Fabric ([https://fabric.io/dashboard](https://fabric.io/dashboard)) and select an app and os that is listed in the top 5 apps you just pulled.

 ![](wiki-attachment:Screen Shot 2016-03-04 at 11.39.43 AM.png)

Once you have selected one of the apps, make sure you change the date range to reflect the week you are interested in highlighted in the red box in the picture below. Once you have the date range set, take the crashes and users affected into the excel file for the relevant app and os. Make sure you do this for every app and os. You should have to pull 10 sets of data from Fabric, two for each of the top 5 apps for the week. 

![](wiki-attachment:Screen Shot 2016-03-04 at 11.56.17 AM.png)

Once you are done, save, close, and upload the revised version in the Google Drive folder. 

WAU Queries: 


```
--Lists WAU in descending order by App overall and by OS. The date filter in bold needs to be changed to the date of the Sunday before the week in question.
```
 

SELECT app

, week_ending

, SUM(WAU) as WAU

, SUM(CASE WHEN os='Android' THEN WAU ELSE 0 END) as Android_WAU

, SUM(CASE WHEN os='iOS' THEN WAU ELSE 0 END) as iOS_WAU

FROM

(

SELECT dau.app

, dau.os

, date_trunc('week', dau.local_datetime)+6 as week_ending

, count(distinct dau.user_map_tag) as WAU

FROM vic_dau dau

WHERE dau.local_datetime::date>'2016-04-24'

GROUP BY 1,2,3

)

GROUP BY 1,2

ORDER BY 3 desc

