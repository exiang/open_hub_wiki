
#### install
```php
public function install($forceReset = false){}
```

#### installDb
```php
public function installDb($forceReset = false){}
```

#### upgrade
Migration instruction file is located at `protected/modules/todo/upgrades/upgrade-1.1.php`:
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