## Chapter 2
Now, your Todo module is not doing much other than display static page content. Let's build a CRUD (Create-Read-Update-Delete) function with database to manage user generated todo list.

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

## Next Chapter
[Chapter 3](Step-by-Step-Todo-module-Chapter3) - Upgrade and link to Organization model