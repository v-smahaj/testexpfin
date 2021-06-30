# Telemetry
The Expert Finder Bot web app logs telemetry to [Azure Application Insights](https://azure.microsoft.com/en-us/services/monitor/). You can go to the respective Application Insights blade of the Azure App Services to view basic telemetry about your services, such as requests, failures, and dependency errors, custom events, traces etc. .

The app integrates with Application Insights to gather bot activity analytics, as described [here](https://blog.botframework.com/2019/03/21/bot-analytics-behind-the-scenes/).

The app uses user tenant id for tracing logs.Developer should ensure that the solution meets their privacy/data retention requirements, and can choose to remove it if they wish.

The app logs a few kinds of events:

`Trace` logs keeps the track of application events and also logs the user activities. 

`Exceptions` logs keeps the records of exceptions tracked in the application.

 *Application Insights queries:*
 
- This query gives total number of times my profile command triggered.
```
traces 
| where message contains "my profile command"

```
- This query gives total number of times my profile is updated.
```
traces 
| where message contains "profile updated"

```
- This query gives total number of times search command triggered.
```
traces 
| where message contains "Search command triggered"