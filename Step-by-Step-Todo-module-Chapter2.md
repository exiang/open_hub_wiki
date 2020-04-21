## Chapter 2
Currently your Todo module is not doing much other than displaying static page content. Let's build a CRUD (Create-Read-Update-Delete) function with database to manage user generated todo list.

### Create a model


1. Create a new file `todo.build.php` in `/protected/modules/todo/data` folder, with content:
```php
<?php
return array(
	'layout' => 'backend',
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
        'neo4j' => array (
                'attributes' => array(
                    'id' => 'string',
                    'title' => 'string'
        )
    )
); 
```
Please refer [Neo4J Data Structure](Neo4j-Data-Structure) to get a glimpse on the data type available.

2. Generate the model with yee at `https://mydomain.com/yee/model/index`
   - Set `Table Name` value to `todo`. `Model Class` value will be automatically populated.
   - Set `model path` value to `application.modules.todo.models`
   - Set `module name` value to `todo`
   - Make sure `build relations` and `build extend class` are checked
   - Make sure `Build Neo4J OGM Class` and `Use Column Comments as Attribute Labels` are left unchecked
3. Click `Preview` and then click `Generate`

### Create a CRUD controller and views
1. Click on `CRUD Generator` on the left side of the screen(or access it at `https://mydomain.com/yee/crud/index`)
   - Set `Model Class` to `application.modules.todo.models.Todo`
   - Set Make sure `Controller ID` is `todo/todo`
2. Click Preview and then click Generate
3. Now, you may access backend to manage todo records from `https://mydomain.com/todo/todo/admin`

### Insert Access
* [Inserting Access](Inserting-Access) - Insert Access to limit an access to the Action

### Enhance it
#### A. Automatically track creator
1.Click `Create Todo` on the side menu bar on the right. You will see a list of users in the dropdown field of form which doesn't looks right and should be removed. We would keep track of who created the record when a new Todo record is created and saved to database. This should be done automatically in model `beforeSave` using data from user's browser session to prevent being the record from being tampered with. 

1. Edit `protected/modules/todo/views/todo/_form.php` and remove this section:
```php
<div class="form-group <?php echo $model->hasErrors('user_id') ? 'has-error':'' ?>">
	<?php echo $form->bsLabelEx2($model,'user_id'); ?>
	<div class="col-sm-10">
		<?php echo $form->bsForeignKeyDropDownList($model, 'user_id'); ?>
		<?php echo $form->bsError($model,'user_id'); ?>
	</div>
</div>
```
2. Refresh your browser and the User dropdown field should be removed from the form
3. Open `protected/modules/todo/models/Todo.php` and insert the following codes into `beforeValidate()` function.:
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
EXPLANATION: The code gets the user's id from the browser session and inserts it into user_id variable before the form is validated to be saved later.


#### B. Add to navigation
You may realised that there is no easy way to access this page other than from the accessing URL directly. It will be more user friendly if we can browse for it from the main navigation system in backend.

1. Edit `protected/modules/todo/TodoModule.php`, under `getNavItems()` and insert the following case:

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
EXPLANATION: This code adds a new nav item to the dropdown on the main backend nav bar.
2. You should now be able to access the todo module page easily from the main navbar at the top via Services>TODO>Todo

## Next Chapter
[Chapter 3](Step-by-Step-Todo-module-Chapter3) - Upgrade and link to Organization model