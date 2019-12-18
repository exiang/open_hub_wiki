As with Coding standards naming consistency is very important in OpenHub, thus there are conventions that every OpenHub contributor should follow.

## Naming conventions

### Database
#### Table naming
Table should be name in singular form and all lowercase (e.g. organization, individual, event). Use underscore '_' if the table name is form by 2 words (e.g. startup_stage).

If it is a many-to-many relationship table, for example:
Intermediate table startup_stage2intake links startup_stage table with intake table. Ordering of table name in this format can be easily decide by asking this question: "who is linking to who?". In this case, we are linking many startup stages to an intake table. 

#### Column naming
OpenHub enforce column naming to generated CRUD code in backend using Yee module (a customize module from Yii). Hence, naming convention for table columns are strict and must be follow by all developers.

  * `id` - Table must has an id column as primary key, unless it is a many-to-many relationship intermediate table. `int(11)` should be sufficient or use `bigint(20)` if you know it is going to be big.
  * `code` - Unique alphanumeric format column, useful for porting data across systems where auto increment id can not be use. code are normally in `varchar(64)`, its value can be a generated UUID string.
  * `slug` - Table who expose to public user thru URL will required this column. Unlike code, while unique, slug must be short and descriptive to human. (e.g. hello-world-sample). Use `varchar(128)`
  * `date_added` - When is the record created? Use `int(11)` unixtimestamp format.
  * `date_modified` - When is the record last modified? Use `int(11)` unixtimestamp format.
  * `ordering` - When this is sortable table. Use `double` format
##### Foreign Key
##### Different type of data column
###### image
###### file
###### boolean
###### image
###### text
###### html
###### csv
###### date
###### json

### Controllers & actions
OpenHub controllers follow these naming conventions:

Prefix controller with resource name in singular form (e.g. `OrganizationController`, `IndividualController`, `EventController`);

Action name should be clear and concise (e.g. `actionCreate`(), `actionBulkInsert`() are good examples, but `actionForm`() or `actionProcess`() is not and thus should be avoided).

We have some standard action names: - `actionList` : display the listing - `actionCreate` : show language creation form page and handle its submit - `actionUpdate` : show language edit form page and handle its submit - `actionDelete` : delete an item