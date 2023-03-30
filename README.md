# Restaurant-system
The Restaurant Monitoring System is a backend API that helps restaurant owners track the online and offline status of their stores during business hours. The system polls each store roughly every hour and records whether the store was active or not in a CSV file. The system also has data on the business hours of all the stores and the timezone for each store.

The system provides two APIs:

/trigger_report endpoint that triggers the generation of a report from the data provided (stored in the database). The API has no input and returns a report ID (a random string). The report ID is used to poll the status of report completion.

/get_report endpoint that returns the status of the report or the CSV. The API takes a report ID as input and returns the following:

If report generation is not complete, return "Running" as the output
If report generation is complete, return "Complete" along with the CSV file with the following schema: store_id, uptime_last_hour(in minutes), uptime_last_day(in hours), update_last_week(in hours), downtime_last_hour(in minutes), downtime_last_day(in hours), downtime_last_week(in hours) The uptime and downtime reported in the CSV only include observations within business hours. The system extrapolates uptime and downtime based on the periodic polls we have ingested to the entire time interval.

## /trigger_report
This endpoint triggers the generation of a report from the data provided (stored in the database).
### Request
```
POST /api/trigger_report HTTP/1.1
```
### Response
```
HTTP/1.1 200 OK
Content-Type: application/json

{
    'message': 'Success', 
    'error_code':200,
    "report_id": "random_string"
}
```
## /get_report
This endpoint returns the status of the report or the CSV.
### Request
```
GET /api/get_report?report_id=random_string HTTP/1.1
```
### Response
```
    HTTP/1.1 200 OK
    Content-Type: text/csv

    store_id, uptime_last_hour(in minutes), uptime_last_day(in hours), update_last_week(in hours), downtime_last_hour(in minutes), downtime_last_day(in hours), downtime_last_week(in hours)
    1, 60, 

```
