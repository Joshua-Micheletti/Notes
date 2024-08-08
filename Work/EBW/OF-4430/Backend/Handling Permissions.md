The API needs to store the available permissions and be able to retrieve them.
This requires a database table to store all the permissions and at least a database table to store the users coming from different applications.
This table will store where the user comes from or not, depending on if it's gonna be necessary. By default, it will not store this information.

One of the permissions that are gonna be necessary is the ability to retrieve the access log list.

## Tables
### permission

| id  | Permission | Active |
| --- | ---------- | ------ |
| int | string     | int    |
| 256 | PERMISSION | 1      |
This table will store the list of the available permissions for the webapp.

### user_permission
Relation table that connects permissions to the user

| id  | Permission | User         | Active |
| --- | ---------- | ------------ | ------ |
| int | string     | string       | int    |
| 128 | PERMISSION | j.micheletti | 1      |
## Endpoints
### GET /permission/{permission}
Retrieve the list of all active permissions
### GET /permission/user/{user}?permission=
Retrieve the list of all active permissions associated with a user. Passing a permission query parameter 
### POST /permission
Adds a new permission to the list of permissions
### PATCH /permission
