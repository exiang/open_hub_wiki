In this tutorial, you will learn how to build a TODO module from ground up.

## Chapter 1
At the end of this chapter, you will have a working TODO module where you can CRUD records in backend.

### Getting started with boilerplateStart
BoilerplateStart serve as a template for new module creation.
1. Copy `boilerplateStart` to `todo` module
2. Edit readme.md, replace the text in it to 'My first custom module to manage todo list'
3. Rename `BoilerplateStartModule.php` to `TodoModule.php`
4. Edit `TodoModule.php` file
    - change the class name from BoilerplateStartModule to TodoModule
    - replace all `boilerplateStart` to `todo`
    - replace all `BoilerplateStart` to `Todo`
    - we going to create a new database table structure called `todo`, this has to be done in code thru `installDb` function:
```php
public function installDb($forceReset = false)
{
	$migration = Yii::app()->db->createCommand();
	// when force reset, database will be purge and all data lost permanently
	if ($forceReset) {
		if (Yii::app()->db->schema->getTable('todo', true)) {
			$migration->dropTable('todo');
		}
	}

	$migration->createTable('todo', array(
		'id' => 'pk',
		'user_id' => 'integer NOT NULL',
		'title' => 'string NOT NULL',
		'text_content' => 'text NULL',
		'json_extra' => 'text NULL',
		'date_added' => 'integer',
		'date_modified' => 'integer',
	));

	$migration->alterColumn('todo', 'title', 'varchar(255) NULL');
	$this->createIndex('user_id', 'todo', 'user_id', false);
	$this->addForeignKey("fk_todo-user_id", "todo", "user_id", "user", "id", "RESTRICT", "RESTRICT");

	return true;
}
```
### Setup configuration
Go to `protected/modules/todo/config` folder
1. Delete `console.php` and `main.php`
2. Edit `console.dist.php`
   - replace all `boilerplateStart` to `todo`
   - replace all `BoilerplateStart` to `Todo`
   - make sure everything in `modules > todo > modelBehaviors` are commented, we will come back to this later in chapter 2. when injecting core model
3. edit `main.dist.php`
   - replace all `boilerplateStart` to `todo`
   - replace all `BoilerplateStart` to `Todo`
   - make sure everything in `modules > todo > modelBehaviors` are commented, we will come back to this later in chapter 2 when injecting core model
4. looking at both your `main.dist.php` and `console.dist.php` config files, at `modules > todo`, you have 2 variables:
   - var1
   - var2

These variable must reflect in your `TodoModule.php` too.
```php
<?php
// use camelcase for class name with first character in uppsercase
class TodoModule extends WebModule
{
	public $defaultController = 'frontend';
	private $_assetsUrl;
	public $oauthSecret;

	public $var1;
	public $var2;
}
```
5. Edit about.yaml
   - set `code` value to `todo`
   - give it a version, lets set `1.0`
   - set `title` value to 'My TODO Module`
   - set `oneliner` value to 'My first custom module to manage todo list'. It should be short and descriptive to help other users and developers to easily understand what your module function
   - set `lastUpdate` to current date in `YYYY-MM-DD` format, e.g. `2020-02-08`
   - under `developer`, set the `author`, `organization`, `email` to your detail
6. Do not copy `console.dist.php` to `console.php` and `main.dist.php` to `main.php`. This will be done automatically when you install the module

### Some clean up
Go to `protected/modules/todo/model` folder
  - rename `HubBoilerplateStart.php` to `HubTodo.php`
  - edit `HubTodo.php`
    - replace all `boilerplateStart` to `todo`
    - replace all `BoilerplateStart` to `Todo`        

Go to views folder
  - edit `frontend\index.php`, change the file content to `Hi, I am TODO module frontend index`

### Congratulation, you make it!
You are now ready to install this new module you have just created.
After install the module from backend, you may access your module frontend with url: `https://hubd.mymagic.my/todo`

## Chapter 2
Now, your Todo module is not doing much other than display static page content. Let's build a CRUD (Create-Read-Ulter-Delete) function with database to manage user generated todo list.

### Create a model

1. Create a new folder `data` inside `todo` module folder.
2. Create a new file `todo.build.php` in `data` folder, with content:
```php
<?php
return array(
	'layout' => '//layouts/backend',
	'menuTemplate' => array(
		'index'=>'admin, create',
		'admin'=>'create',
		'create'=>'admin',
		'update'=>'admin, create, view',
		'view'=>'admin, create, update, delete',
	),
	'admin' => array(
		'list' => array('id', 'title', 'user_id', 'date_added', 'date_modified'),
	),
	'structure' => array(
		'json_extra'=>array('isJson'=>true),
	),
	// this foreignKey is mainly for crud view generation. model relationship will not use this at the moment
	'foreignKey' => array(
		'user_id'=>array( 'relationName'=>'user', 'model'=>'User', 'foreignReferAttribute'=>'username'),
	),
	'json'=>array(
		'extra'=>array(
		)
	),
); 
```
3. Generate the model with yee at `https://hubd.mymagic.my/yee/model/index`
   - set `model path` value to `application.modules.todo.models`
   - set `module name` value to `todo`
   - make sure `build relations` and `build extend class` are checked

### Create a CRUD controller and views
1. Generate controller with yee at `https://hubd.mymagic.my/yee/model/index`
   - set `model class` to `application.modules.todo.models.Todo`
   - make sure `Controller ID` is `todo/todo`
2. Now, you may access backend to manage todo records from `https://hubd.mymagic.my/todo/todo/admin`


### Enhance it
#### Automatically track creator
Click `Create Todo`, you will find a list of user in the form which doesn't looks right and should be remove. When a new Todo record saved to database, we will like to know who created it. This should be done automatically in model `beforeSave` using data from user session to prevent being tempered. 

1. Edit `protected/modules/todo/views/todo/_form.php` and remove this chuck:
```php
<div class="form-group <?php echo $model->hasErrors('user_id') ? 'has-error':'' ?>">
	<?php echo $form->bsLabelEx2($model,'user_id'); ?>
	<div class="col-sm-10">
		<?php echo $form->bsForeignKeyDropDownList($model, 'user_id'); ?>
		<?php echo $form->bsError($model,'user_id'); ?>
	</div>
</div>
```
2. Edit `protected/modules/todo/models/Todo.php` and replace `beforeValidate()` function with:
```php
public function beforeValidate() 
{
	// custom code here
	// ...
	if($this->isNewRecord) {
		$this->user_id = Yii::app()->user->id;
	} 
	return parent::beforeValidate();
}
```


#### Add to navigation
You may realised there is no easy way to access this page other than from the URL. It will be more user friendly if we can browse for it from the main navigation system in backend.

1. Edit `protected/modules/todo/TodoModule.php`, under `getNavItems()`, add:

```php
case 'backendNavService':{
	return array(
		array(
			'label' => Yii::t('backend', 'TODO'), 'url' => '#',
			'visible' => Yii::app()->user->getState('accessBackend') == true,
			'active' => $controller->activeMenuMain == 'todo' ? true : false,
			'itemOptions' => array('class' => 'dropdown-submenu'), 'submenuOptions' => array('class' => 'dropdown-menu'),
			'linkOptions' => array('class' => 'dropdown-toggle', 'data-toggle' => 'dropdown'),
			'items' => array(
				array('label' => Yii::t('app', 'Todo'), 'url' => array('/todo/todo'), 'visible' => Yii::app()->user->getState('accessBackend') == true),
			),
		),
	);

	break;
}
```

## Chapter 3
In this chapter, we will add in extra function to link a todo record to an organization

### Using upgrades to make changes
### Linking TODO record to organization
### Inject to Organization Core Model

## Chapter 4
In this chapter, we explore how to build web API interface for TODO module

## Chapter 5
Advance use case
## Inject CSS & JS to application scope
## Multilingual i18n
## Gave your module a specific subdomain

