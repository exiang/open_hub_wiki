## Module Installation
#### install
```php
public function install($forceReset = false){}
```

#### installDb
```php
public function installDb($forceReset = false){}
```

## Module Upgrade

> Please note that unlike `Yii migrate` which run on CLI, module upgrade runs thru Web Server (e.g. Apache) and this could break your upgrade process if it took too long beyond the limit of PHP `max_execution_time` setting or depends on it is running as `mod_php` or `mod_fastcgi`

The modular architecture of OpenHub comes with built-in versioning control. Update version numbering in `protected/modules/[moduleCode]/config/about.yaml`

Migration instruction file is located at `protected/modules/[moduleCode]/upgrades/upgrade-1.1.php`:
```php
<?php
function upgrade_module_1_1($about)
{
	$migration = Yii::app()->db->createCommand();
	$migration->addColumn('todo', 'organization_id', 'integer NULL');
	$migration->addColumn('todo', 'status', "enum('new','pending','processing','done','cancel','fail') DEFAULT 'new'");

	// addForeignKey ( $name, $table, $columns, $refTable, $refColumns, $delete = null, $update = null )
	$migration->addForeignKey('fk_todo-organization_id', 'todo', 'organization_id', 'organization', 'id', 'SET NULL', 'CASCADE');

	return "Upgraded to version 1.1\n";
}
```

Instructions in this file will run automatically when you upgrade your module.

Click on Upgrade button to upgrade your module from backend.
![](https://user-images.githubusercontent.com/5336690/74012856-2a48fc80-49c6-11ea-880e-34d017d80647.png)

You will see this when Upgrade migration completed successfully.
![](https://user-images.githubusercontent.com/5336690/74012902-45b40780-49c6-11ea-84d7-849a43d41c82.png)
