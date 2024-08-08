The API needs to store the available permissions and be able to retrieve them.
This requires a database table to store all the permissions and at least a database table to store the users coming from different applications.
This table will store where the user comes from or not, depending on if it's gonna be necessary. By default, it will not store this information.

One of the permissions that are gonna be necessary is the ability to retrieve 