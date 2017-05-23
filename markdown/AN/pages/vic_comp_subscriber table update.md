## Subscription Related Analytics Events

Before reviewing how the vic_comp_subscriber table is created, it is important to review the various subscription events used to populate these metrics: 

* backend_receipt_validated_from_store / success - The first instance of this event marks that beginning of the subscription for that user id. Subsequent instances of this call can represent when a user restores a purchase, when they successfully renew, or when the reactivate after churning from the previous subscription. 
* client_receipt_sent_to_store (where species is not null) - This event should occur immediately before the above event and contains the sku in the species column for the subscription that was purchased or restored. In instances where the above event is triggered by the backend renewal check, this client_receipt_sent_to_store is not sent
* subscription / renewal / success or fail - This event is triggered by the backend renewal check that currently runs every day at 5 PM pacific time and checks all receipts / users where the subscription end date stored in dynamo occurred within the last 24 hours. Each of these events corresponds with a check completed by the renewal event and sends along the subscription end date that caused this user to fall within the 24 hour, the original sku, and the original transaction id. In the case where the renewal was a success, the event also sends along the future subscription end date and the related sku

## vic_comp_subscriber Contents

The vic_comp_subscriber table contains data related to subscription metrics. The dimensional information is the same as the other vic_comp ETL tables with the notable addition of a sku column. The metrics included in this table are as follows: 

* new_sub
* reactivated_sub
* sku_change_sub
* internal_sub
* churn

## Logic for Calculating above Metrics

New_sub represents the users who subscribed for very first time on that user ID on any sku. This is the number used as the gross subscribers metric and uses the same logic as the initial iteration of the subscriber query (pulls the very first subscription event for any user). 

reactivated_sub represents the users who had previously churned but started a new subscription on either the same sku or a different sku. Must be after a previous churn event. Current calculation performed each day takes the listed of users marked as "churned" on the user_summary_master table and compares them against users who have had a successful validation event in the past day as reactivated users. 

sku_change_sub represents the users who were initially subscribed on one sku and during the course of that subscription, prior to the subscription end date and the renewal check, changed to a different sku. On the renewal check, these users are marked as successfully renewing but their old and new sku are not the same. 

internal_sub are all users sign up for an account whose email ends in "@victorious.com", "@getvictorious.com", "@example.com", or "@something.net". There are additional users who fall into this categorization how was identified as a victorious user and separated out using their user id. 

churn are all the users that had a failed renewal event following a previous validated subscription event and users that changed their sku during the subscription captured during the successful renewal event. This calculation does NOT capture any users that would be categorized as an internal_sub including those separated out by their user id. 

## Cases Not Captured by the above Logic

* Duplicate users - In the case where users purchase a subscription, and restore purchase under that same receipt but as a different victorious user, these will be counted as two separate users.
* Users who churned in the hours between 5 PM and midnight PST on a particular day (ex. day 1), they will not be immediately captured as a churn when the job runs at midnight (ex. day 2). They will be counted on the next day (ex. day 3). If the user reactivates between when they churned and before the users is counted as a churn on day 3, they will not be counted as a reactivated sub but will be marked one in the user_summary_table.  

The possibilities of the last case occurring reduces with the movement of the renewal check to occur at midnight PST which is currently in development. 

