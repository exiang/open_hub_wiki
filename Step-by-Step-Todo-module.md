In this tutorial, you will learn how to build a TODO module from ground up.

  * [Chapter 1](Step-by-Step-Todo-module-Chapter1)
## Chapter 1
At the end of this chapter, you will have a working TODO module where you can CRUD records in backend.

### Getting started with boilerplateStart
BoilerplateStart serve as a template for new module creation.
1. Copy `boilerplateStart` to `todo` module
2. Edit readme.md, replace the text in it to 'My first custom module to manage todo list'
3. Rename `BoilerplateStartModule.php` to `TodoModule.php`
4. Edit `TodoModule.php` file
    - Change the class name from BoilerplateStartModule to TodoModule
    - Replace all `boilerplateStart` to `todo`
    - Replace all `BoilerplateStart` to `Todo`
    - We going to create a new database table structure called `todo`, this has to be done in code thru `installDb` function:
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
   - Replace all `boilerplateStart` to `todo`
   - Replace all `BoilerplateStart` to `Todo`
   - Make sure everything in `modules > todo > modelBehaviors` are commented, we will come back to this later in chapter 2. when injecting core model
3. Edit `main.dist.php`
   - Replace all `boilerplateStart` to `todo`
   - Replace all `BoilerplateStart` to `Todo`
   - Make sure everything in `modules > todo > modelBehaviors` are commented, we will come back to this later in chapter 2 when injecting core model
4. Looking at both your `main.dist.php` and `console.dist.php` config files, at `modules > todo`, you have 2 variables:
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
   - Set `code` value to `todo`
   - Give it a version, lets set `1.0`
   - Set `title` value to 'My TODO Module`
   - Set `oneliner` value to 'My first custom module to manage todo list'. It should be short and descriptive to help other users and developers to easily understand what your module function
   - Set `lastUpdate` to current date in `YYYY-MM-DD` format, e.g. `2020-02-08`
   - Under `developer`, set the `author`, `organization`, `email` to your detail
6. Do not copy `console.dist.php` to `console.php` and `main.dist.php` to `main.php`. This will be done automatically when you install the module

### Some clean up
Go to `protected/modules/todo/model` folder
  - Rename `HubBoilerplateStart.php` to `HubTodo.php`
  - Edit `HubTodo.php`
    - Replace all `boilerplateStart` to `todo`
    - Replace all `BoilerplateStart` to `Todo`        

Go to views folder
  - Edit `frontend\index.php`, change the file content to `Hi, I am TODO module frontend index`

### Congratulation, you make it!
You are now ready to install this new module you have just created.
After install the module from backend, you may access your module frontend with url: `https://hubd.mymagic.my/todo`

## Chapter 2
Now, your Todo module is not doing much other than display static page content. Let's build a CRUD (Create-Read-Ulter-Delete) function with database to manage user generated todo list.

### Create a model


1. Create a new file `todo.build.php` in `/protected/data` folder, with content:
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
2. Generate the model with yee at `https://hubd.mymagic.my/yee/model/index`
   - Set `model path` value to `application.modules.todo.models`
   - Set `module name` value to `todo`
   - Make sure `build relations` and `build extend class` are checked

### Create a CRUD controller and views
1. Generate controller with yee at `https://hubd.mymagic.my/yee/model/index`
   - Set `model class` to `application.modules.todo.models.Todo`
   - Make sure `Controller ID` is `todo/todo`
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
In this chapter, we will add in an extra feature. Imagine here is a new request from your boss: As admin, I would like to see any pending TODO tasks in associate to a specific startup or company.

Analyzing this requirement, we know these are the action plans, translated in developer language:
1. TODO can be associate to organization
2. TODO has status (new, pending, in progress, done)
3. I am able to see list of TODO associated to a specific organization in the organization view page.

### Using upgrades to make changes
We will need to modify the database structure of `todo` table to add in the following new columns:
   * `organization_id` - foriegn key to link to organization table
   * `status` - an enum to keep track of the TODO current status

1. OpenHub module architecture comes with built-in versioning control. As this is an upgraded version of Todo module, we would like to change it's version numbering in `protected/modules/todo/config/about.yaml`
   * Set `version` from `1.0` to `1.1`

2. Create a new migration instruction file at `protected/modules/todo/upgrades/upgrade-1.1.php`:
```php
<?php

function upgrade_module_1_1($about)
{
	$migration = Yii::app()->db->createCommand();
	$migration->addColumn('todo', 'organization_id', 'integer NULL');
	$migration->addColumn('todo', 'status', "enum('new','pending','processing','done','cancel','fail')") DEFAULT 'new';

	// addForeignKey ( $name, $table, $columns, $refTable, $refColumns, $delete = null, $update = null )
	$migration->addForeignKey('fk_todo-organization_id', 'todo', 'organization_id', 'organization', 'id', 'SET NULL', 'CASCADE');

	return "Upgraded to version 1.1\n";
}
```

Instructions in this file will automatically run when you upgrade your module.

Upgrade Todo module from backend.
![](https://user-images.githubusercontent.com/5336690/74012856-2a48fc80-49c6-11ea-880e-34d017d80647.png)

Upgrade migration completed successfully.
![](https://user-images.githubusercontent.com/5336690/74012902-45b40780-49c6-11ea-84d7-849a43d41c82.png)

Table changes reflected in database.
![table database changes](https://user-images.githubusercontent.com/5336690/74012943-5a909b00-49c6-11ea-8b96-7e34d26185ca.png)

Next, we will need to regenerate the model, controller and view to reflect this database changes.

Update `protected/data/todo.build.php` to
```php
<?php

return array(
	'layout' => '//layouts/backend',
	'menuTemplate' => array(
		'index' => 'admin, create',
		'admin' => 'create',
		'create' => 'admin',
		'update' => 'admin, create, view',
		'view' => 'admin, create, update, delete',
	),
	'admin' => array(
		// version 1.1
		'list' => array('id', 'organization_id', 'title', 'user_id', 'status', 'date_added', 'date_modified'),
	),
	'structure' => array(
		'json_extra' => array('isJson' => true),
		// version 1.1
		'status' => array(
			'isEnum' => true, 'enumSelections' => array(
				'new' => 'New',
				'pending' => 'Pending',
				'processing' => 'Processing',
				'done' => 'Done',
				'cancel' => 'Cancel',
				'fail' => 'Fail',
			),
		),
	),
	// this foreignKey is mainly for crud view generation. model relationship will not use this at the moment
	'foreignKey' => array(
		'user_id' => array('relationName' => 'user', 'model' => 'User', 'foreignReferAttribute' => 'username'),
		// version 1.1
		'organization_id' => array('relationName' => 'organization', 'model' => 'Organization', 'foreignReferAttribute' => 'title'),
	),
	'json' => array(
		'extra' => array(
		)
	),
);

```

Go to Yee Model Generator `https://hubd.mymagic.my/yee/model`
1. Set `Table Name` to `todo`
1. Set `Model Path` to `application.modules.todo.models`
1. [!important] Uncheck `Build Extend Class`
1. Click Preview
1. `modules/todo/models/TodoBase.php` is in red, click `diff` link to compare the different between new and current model code
1. You should also takes the opportunity to compare any changes to those functions you had overrided in `odules/todo/models/Todo.php` in this case, and update accordingly manually
1. Checked [overwrite] and click the Generate button when everythign is good to go

Go to Yee Crud Generator `https://hubd.mymagic.my/yee/crud/index`
1. Set `Model Class` to `application.modules.todo.models.Todo`
2. Set `Controller ID` to `todo/todo`
3. Dont straightaway jump into regenerate the code files to replace existing. Remember we had done quite some changes here on the view `_form.php`? It's best to use the `[diff]` to carefully check what changes will be done to each of the files and apply them manually.
4. Edit `protected/modules/todo/views/todo/_form.php`, **add in** the following blocks:
```php
<div class="form-group <?php echo $model->hasErrors('organization_id') ? 'has-error':'' ?>">
	<?php echo $form->bsLabelEx2($model,'organization_id'); ?>
	<div class="col-sm-10">
		<?php echo $form->bsForeignKeyDropDownList($model, 'organization_id'); ?>
		<?php echo $form->bsError($model,'organization_id'); ?>
	</div>
</div>

<div class="form-group <?php echo $model->hasErrors('status') ? 'has-error':'' ?>">
	<?php echo $form->bsLabelEx2($model,'status'); ?>
	<div class="col-sm-10">
		<?php echo $form->bsEnumDropDownList($model, 'status'); ?>
		<?php echo $form->bsError($model,'status'); ?>
	</div>
</div>
```
5. Back to the Yee Crud Generator, check and regenerate for all except `modules/todo/views/todo/_form.php `.

Now, we can create a TODO record and link it to a specific company.

### Inject to Organization Core Model

## Chapter 4
In this chapter, we explore how to build web API interface for TODO module

## Chapter 5
Advance use case
## Inject CSS & JS to application scope
## Multilingual i18n
## Gave your module a specific subdomain

