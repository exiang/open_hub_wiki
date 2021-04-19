As with Coding standards naming consistency is very important in OpenHub, thus there are conventions that every OpenHub contributor should follow.

## Naming conventions

### Controllers & actions
OpenHub controllers follow these naming conventions:

Prefix controller with resource name in singular form (e.g. `OrganizationController`, `IndividualController`, `EventController`);

Action name should be clear and concise (e.g. `actionCreate()`, `actionBulkInsert()` are good examples, but `actionForm()` or `actionProcess()` is not and thus should be avoided).

We have some standard action names: 
- `actionList` : display the listing 
- `actionCreate` : show language creation form page and handle its submit 
- `actionUpdate` : show language edit form page and handle its submit 
- `actionDelete` : delete an item

### Functions
> [verb][Adjective][ModuleName][Noun][Preposition][Pronoun][Adjective]

Some examples:
- `getAllModuleXRecords4AdminQuickly()`
- `getOrganization2EmailUserID()`
- `getOrCreateOrganization()`
- `getOrganizationByCode()`
- `getActiveOrganizations()`
- `getOrganization2Email()`

Use plural nouns if you are expecting multiple records in return.

Preposition is written in short form numeric:
  * for -> `4`
  * to -> `2`
  * on -> `On`
  * by -> `By`
  * With -> `With`
  * From -> `From`

Function name from modules that use in global scope with potential to clash with others (e.g. Model Behavior) should have module name as part of the function name.

For example, function `countAllBoilerplateStartItems()` in `/protected/modules/boilerplateStart/components/BoilerplateStartMemberBehavior.php` follow the naming convention of `[verb][Adjective][ModuleName][Noun]`:  

``` php
class BoilerplateStartMemberBehavior extends Behavior
{
	// the member model
	public $model;

	//
	// items
	public function countAllBoilerplateStartItems()
	{
		return HubBoilerplateStart::countAllMemberBoilerplateStarts($this->model);
	}
}
```

### Variable
Variable in PHP and javascript should follow the lowercase first camel case format. e.g: `$var1`, `$formSubmission`. However, variable in Model which map to database table column should follow database column naming convention. e.g: `$full_name`, `$date_added`, `$is_active`.

## Database
### Table naming
Table should be name in singular form and all lowercase (e.g. organization, individual, event). Use underscore '_' if the table name is form by 2 words (e.g. startup_stage).

If it is a many-to-many relationship table, for example:
Intermediate table startup_stage2intake links startup_stage table with intake table. Ordering of table name in this format can be easily decide by asking this question: "who is linking to who?". In this case, we are linking many startup stages to an intake table. 

### Column naming
OpenHub enforce column naming to generated CRUD code in backend using Yee module (a customize module from Yii). Hence, naming convention for table columns are strict and must be follow by all developers.

  * `id` - Table must has an id column as primary key, unless it is a many-to-many relationship intermediate table. `int(11)` should be sufficient or use `bigint(20)` if you know it is going to be big.
  * `code` - Unique alphanumeric format column, useful for porting data across systems where auto increment id can not be use. code are normally in `varchar(64)`, its value can be a generated UUID string.
  * `slug` - Table who expose to public user thru URL will required this column. Unlike code, while unique, slug must be short and descriptive to human. (e.g. hello-world-sample). Use `varchar(128)`
  * `date_added` - When is the record created? Use `int(11)` unixtimestamp format.
  * `date_modified` - When is the record last modified? Use `int(11)` unixtimestamp format.
  * `ordering` - When this is sortable table. Use `double` format
#### Foreign Key
Columns link to foreign table are name in convention of `xyz_id` or `xyz_code`.

#### Different type of data column
##### image
Image data column are name in convention of `image_xyz`. Use `varchar(255)` format.
##### file
File data column are name in convention of `file_xyz`. Use `tinyint(255)` format.
##### boolean
True/False data column are name in convention of `is_xyz`. Use `tinyint(1)` format.
##### text 
Plain text data column are name in convention of `text_xyz`. Use either `varchar(255)` or `longtext` format.
##### html
Rich text (HTML) data column are name in convention of `html_xyz`. Use `longtext` format.
##### url
To store full URL value in convention of `url_website`. Use `varchar(255)` format.
##### latlong
To store spatial data value in convention of `latlong_residential_address`. Use `point` format.
##### ordering
Column to mark ordering of the record. As it only supports 1 level ordering, the column name is just `ordering`. Use  `double` format.
##### csv
CSV (Comma separated Value) data column are name in convention of `csv_xyz`. Use `longtext` format.
##### date
Date time data column are name in convention of `date_xyz(ed)` (e.g. date_posted). Use `int(11)` format to store this unix timestamp value.
##### json
Json data column are name in convention of `json_xyz` (e.g. `json_extra`, `json_original_copy`). Use `longtext` format. This is a special method to allow unstructured value store for a specific records for developer flexibility. 

#### multilingual (i18n)
OpenHub store multilingual value in the same table at different columns (e.g. `title_en`, `title_ms`, `title_zh`). There are pro and cons for this method compared to storing at a different table like some of the other system does. 
  * Pro: SQL are much simpler to build, no complicated join
  * Cons: Enabling extra languages required modification to table structure in database

Multilingual data column are name in convention of `xyz_(en|ms|zh_cn|...)` (e.g. `xyz_en`, `xyz_zh`, `image_xyz_en`, `text_xyz_ms`). Language code are added as postfix to the column name.

#### meta data
Developer can easily add 'virtual column' from PHP code to any table in the database without changing the table structure. It is especially useful for module developer to extend the function of existing common block table (e.g. organization, individual, event). Think of it likes Wordpress database meta structure.

##### meta_structure
In this example, module `sample` would like to add a virtual column called `extraColumn1` to table `organization`.

  * code: `varchar(64)` column. Naming convention for the value is `[OriginalTableName]-[moduleName]-[attributeName]`. (e.g. `Organization-sample-extraColumn1`)
  * datatype: Data type of this virtual column is: 'boolean','integer','float','string','text','html','date','array','enum','timestamp','image','json'
  * ref_table: Reference table.

##### meta_item
  * meta_struture_id:
  * ref_id:
  * value: `longtext`
