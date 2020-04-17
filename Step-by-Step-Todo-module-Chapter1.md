## Chapter 1
At the end of this chapter, you will have a basic working TODO module.

### Getting started with boilerplateStart
BoilerplateStart serve as a template for new module creation.
1. Copy `boilerplateStart` module folder and rename to `todo` to create the `todo` module
2. Edit `readme.md`. Replace the text inside to 'My first custom module to manage todo list'
3. Rename `BoilerplateStartModule.php` to `TodoModule.php`
4. Edit `TodoModule.php` file
    - Change the class name from `BoilerplateStartModule` to `TodoModule`
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
	$migration->createIndex('user_id', 'todo', 'user_id', false);
	$migration->addForeignKey("fk_todo-user_id", "todo", "user_id", "user", "id", "RESTRICT", "RESTRICT");

	return true;
}
```
> For more information on BoilerplateStart, please refer to [Quick Start with Boilerplate](Quick-Start-with-Boilerplate)

### Setup configuration
Go to `protected/modules/todo/config` folder
1. Delete `console.php` and `main.php`
2. Edit `console.dist.php`
   - Replace all `boilerplateStart` to `todo`
   - Replace all `BoilerplateStart` to `Todo`
   - Make sure everything in `modules > todo > modelBehaviors` are commented, we will come back to this later in chapter 2. when injecting core model
   - Set `var1` to 'Hello'
   - Set `var2` to 'World'
3. Edit `main.dist.php`
   - Replace all `boilerplateStart` to `todo`
   - Replace all `BoilerplateStart` to `Todo`
   - Make sure everything in `modules > todo > modelBehaviors` are commented, we will come back to this later in chapter 2 when injecting core model
   - Set `var1` to 'Hello'
   - Set `var2` to 'World'
4. Looking at both your `main.dist.php` and `console.dist.php` config files, at `modules > todo`, you now have 2 variables. These variable must reflect in your `TodoModule.php` too.
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

> For more information on module configuration, please refer to [Module Config](Module-Config)

> For more information on module installation, please refer to [Module Install & Upgrade
](Module-Install-&-Upgrade)

### Some clean up
Go to `protected/modules/todo/model` folder
  - Rename `HubBoilerplateStart.php` to `HubTodo.php`
  - Edit `HubTodo.php`
    - Replace all `boilerplateStart` to `todo`
    - Replace all `BoilerplateStart` to `Todo`        

Go to views folder
  - Edit `frontend\index.php`, change the file content to 
```php 
Hi, I am TODO module frontend index '<?php echo $this->module->var1 ?>'
```

### Congratulation, you made it!
You are now ready to install this new module you have just created.
1. Go to `https://mydomain.com/sys/module/admin` 
2. Click on the 'New Modules' tab
3. Click Initialize button on the Todo module to initialize your module
4. Click on the 'Modules' tab
5. Click Install button on the Todo module to install your module

After installing the module from backend, you may access your module frontend with url: `https://mydomain.com/todo`

## Next Chapter
[Chapter 2](Step-by-Step-Todo-module-Chapter2) - Add in CRUD function in backend