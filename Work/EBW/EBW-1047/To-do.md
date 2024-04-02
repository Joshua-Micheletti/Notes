Make loading layer be controlled by privacy policies defined with context_id and user_id.
Every layer should have its context_id set up correctly, and the elements with a user_id set, should only be visible by the user corresponding to that ID.
Only the user who owns the layer should be able to modify it (only for private layers)
## Check if Fede's solution for loading config works
More or less, it only loads the configuration with the User_id
## Adapt the Database accordingly
- [x] Adapt shape_layer table
	- add the user_id and context_id fields in the table
	- **remove "unique_shape_layer_label" constraint (no entries with same label)**
	- **add "unique_label_context_user" constraint (no entries with same label, context and user)**
- [x] Adapt layer content tables
	- the tables now are called: "layer_\<id\>" where id is an increasing number stored in the table "layer_lookup".
	- the table "layer_lookup" stores the layers with 4 columns:
		- id: integer auto-increment number
		- label: layer name
		- context_id: context id of the specified environment
		- user_id: user id of the specified user
- [x] Adapt GeoServer layers
	- layers are now named "layer_\<id\>" where id is stored inside the "layer_lookup" table
- [ ] Adapt configuration table
	- every entry checks only in its environment (context_id + user_id) depending on if it's private or not
	- if it's private and there's no entry yet, a new one is created
	- you can't have multiple layers with the same name inside the same environment (label, context, user)
## Adapt the Backend
### Insert
- [x] Adapt insertion in shape_layer
- [ ] Adapt insertion in database (table creation)
- [ ] Adapt GeoServer insertion
- [ ] Adapt configuration table insertion
### Get
### Queries


## Adapt the Frontend

## Adapt the permissions