# Alert Rules
The following tables contain opinionated useful alert rules by different resource type. Each alert rule targets a specific **scope** and triggers under certain given **condition** and is evaluated based on given **evaluation criteria**. Refer [here](https://docs.microsoft.com/en-us/azure/azure-monitor/alerts/alerts-metric-overview) to learn how alert rule works.

## Resource Types

### Resource Group
> NOTE: Resource group level monitoring is primarily to detect unplanned new deployment.

| Description | Signal Type | Condition | Evaluation | Remarks |
| --- | --- | --- | --- | --- |
| [New deployment](./src/resource-group/new-deployment-alert.json) | Activity Log (Succeeded status) | Whenever the activity log has a new deployment succeeded status event | Triggers when the event occurs | In **Filter by resource type**, select **Deployment (deployments)**. This will set the scope resource as **All deployments**.


### Azure SQL Database
> NOTE: Unlike on-premise SQL server, many key metrics such as **Failed Connections** and **Blocked by Firewall** are available at Azure SQL database resource level instead of Azure SQL server. Therefore, if there multiple Azure SQL databases, each Azure SQL database will have its own set of alert rules. This is the design as of the time of writing.

| Description | Signal Type | Condition | Evaluation | Remarks |
| --- | --- | --- | --- | --- |
| [Request blocked by firewall](./src/sql-database/blocked-by-firewall-alert.json) | Metric (Block by firewall, Sum) | Whenever the total blocked by firewall is greater than 1  | 5 mins worth of data, runs every 1 min
| [Failed SQL connection](./src/sql-database/failed-connections-alert.json) | Metric (Failed connections, Sum) | Whenever the sum failed connections is greater than 4 over an period of 5 mins aggregated data | 5 mins worth of data, runs every 1 min | Adjust threshold accordingly to accommodate for transient network error
| [Data space used](./src/sql-database/data-space-used-alert.json) | Metric (Data space used percent, Max) | Whenever the average data space used percent is greater than 85% | 5 mins worth of data, runs every 1 min
| [Database transaction unit (DTU) used](./src/sql-database/dtu-percentage-used-alert.json) | Metric (DTU percentage, Avg) | Whenever the average DTU percentage is greater than 85% | 5 mins worth of data, runs every 1 min
| [Deadlock](./src/sql-database/dtu-percentage-used-alert.json) | Metric (Deadlock, Sum) | Whenever the sum deadlocks is greater than 4 | 5 mins worth of data, runs every 1 min


### Azure SQL Server
| Description | Signal Type | Condition | Evaluation | Remarks |
| --- | --- | --- | --- | --- |
| [Update firewall rules](./src/sql-server/update-firewall-rules-alert.json) | Activity Log (Succeeded status) | Whenever the activity log has a SQL Server firewall update succeeded status event | Triggers when the event occurs


### App Service
| Description | Signal Type | Condition | Evaluation | Remarks |
| --- | --- | --- | --- | --- |
| [Stop app service](./src/app-service/stop-app-service-alert.json) | Activity Log (Succeeded status) | Whenever the activity log has a stop web app (sites) succeeded status event | Triggers when the event occurs
| [Restart app service](./src/app-service/restart-app-service-alert.json) | Activity Log (Succeeded status) | Whenever the activity log has a restart web app (sites) succeeded status event | Triggers when the event occurs
| *[Delete app service](./src/app-service/delete-app-service-alert.json) | Activity Log (Succeeded status) | Whenever the activity log has a delete web app (sites) succeeded status event | Triggers when the event occurs | *The scope must be either the owner resource group or subscription in order to work.
| *[Availability test](https://docs.microsoft.com/en-us/azure/azure-monitor/app/monitor-web-app-availability) | Metric | Whenever the average failed locations is greater than or equal 5 | Over the last 5 mins, run every 1 min | *Availability test is created in Application Insights.


## Stay tuned...more to come..