The API needs to store the available permissions and be able to retrieve them.

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
### GET /permission
Retrieve the list of all active permissions
### GET /permission/{permission}
Retrieve the list of all the users with that permission
### GET /permission/user/{user}?permission=
Retrieve the list of all active permissions associated with a user. Passing a permission query parameter returns whether the user has that permission enabled.
### POST /permission/{permission}
Adds a new permission to the list of permissions
### PATCH /permission/{permission}?name=&active=
Allows to change the name of the permission and whether it's active or not
### PATCH /permission/user/{user}?permission=&active=
Assigns a permission to a user and sets whether it's active or not. If the permission is already assigned, update the 'active' value accordingly.


