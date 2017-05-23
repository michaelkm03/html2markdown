There are four jenkins jobs which run periodically against the MySQL/RedShift clusters, those are:

* [Redshift ETL from MySQL](https://jenkins.getvictorious.com/view/Analytics/job/Redshift%20ETL%20from%20MySQL/) Job that automates the ETL process between MySQL and Redshift (for the "vicky" schema)
* [Redshift Post-ETL Queries](https://jenkins.getvictorious.com/view/Analytics/job/Redshift%20Post-ETL%20Queries/) Executes a set of SQL queries after MySQL->Redshift ETL and before updating the mirror cluster
* [Redshift Mirror Update](https://jenkins.getvictorious.com/view/Analytics/job/Redshift%20Mirror%20Update/) This job creates a fresh snapshot of the main Redshift cluster and uses that snapshot to update the mirror cluster
* [Redshift Queries (Weekly)](https://jenkins.getvictorious.com/view/Analytics/job/Redshift%20Queries%20(Weekly)/) Runs weekly SQL queries against the redshift cluster

The ETL job is started at 12:05am PDT, once that's finished, the Post-ETL queries job is triggered and once those are done, the Mirror update job runs.

The weekly queries job runs only on Monday mornings at 12:05am.

The daily and weekly jobs contain a set of "Execute shell" blocks for each query to be executed which simply execute psql and pass in the filename containing the query to be executed, source code for the SQL queries is kept in the Analytics github repo: [https://github.com/Victorious/analytics/](https://github.com/Victorious/analytics/) the code from (the master branch of) this repo is updated every time the jobs run, so any updates in the github repo will automatically take effect the next time the job runs.

To add another task to the daily/weekly jobs, go to the job in Jenkins,

* Hit "Configure",
* Scroll down to the "Build" section
* At the bottom press "Add build Step"
* Select "Execute shell"
* Copy the code snippet from one of the existing blocks, adjusting the name of the script to execute (which must be checked into the Analytics github repo

Note: You can Drag&Drop the "Execute Shell" steps to adjust the order they will run.

