Every operation on the trimble system needs to be tracked in a specific operations_log table in the database. This tracking can only be done when accessing the system through the webapp.
### operations_log

| id  | user   | user_trimble | operation   | operation_description | date |
| --- | ------ | ------------ | ----------- | --------------------- | ---- |
| int | string | string       | string/enum | string                | date |
This table needs to be filled by every supported operation towards the trimble system by the appropriate endpoints.

## Endpoints
### GET /log/operation/list?user=&usertrimble=&operation=&date=
Get the list of all the operation logs filtered by the query parameters passed