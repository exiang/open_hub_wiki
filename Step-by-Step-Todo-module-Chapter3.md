## Chapter 3
Imagine: after published the module, you got a new request from your boss: 
> As admin, I would like to see any pending TODO tasks in associate to a specific startup or company.

Analyzing the requirement, these are the action plans (translated to developer language):
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

> For more information on Module Upgrade, please refer to [Module Install & Upgrade
](Module-Install-&-Upgrade)

### Regenerate MVC
Next, we will need to regenerate the model, controller and view to reflect this database changes.

Update `protected/modules/todo/data/todo.build.php` to
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

Remember one of the requirements is to allow admin to click into an organization and get a list of associated TODO?
Luckily, OpenHub support [Model Behavior Injection](Inject-Functions-to-Core-Models), where module developer can attach custom functions to existing core model, e.g. Organization in this case.

1. Rename `protected/modules/todo/components/BoilerplateStartOrganizationBehavior.php` to `protected/modules/todo/components/TodoOrganizationBehavior.php` and replace its content with:
```php
<?php

Yii::import('modules.todo.models.*');

// this allow you to inject method into organization object
class TodoOrganizationBehavior extends Behavior
{
	// the organization model
	public $model;

	//
	// items
	public function countAllTodoItems()
	{
		return HubTodo::countAllTodos($this->model);
	}

	public function getTodoItems($limit = 100)
	{
		return HubTodo::getTodos($this->model, $limit);
	}

	public function shout()
	{
		return 'I am from todo module!';
	}
}
```

2. Modify `protected/modules/todo/models/HubTodo.php`, replace with content:
```php
<?php

class HubTodo
{
	public function countAllTodos($organization)
	{
		return Todo::model()->countByAttributes(array(
            'organization_id'=> $organization->id
        ));
	}

	public function getTodos($organization, $limit = 100)
	{
		return Todo::model()->findAll(array(
			'condition' => 'organization_id=:organizationId',
			'params' => array(':organizationId'=> $organization->id),
			'limit' => $limit,
			'order' => 'id DESC'
		));
	}
}

```

3. Modify `protected/modules/todo/config/console.php` and `protected/modules/todo/config/main.php`, also their dist files, to enable `modules > todo > modelBehaviors > Organization`
```php
'modelBehaviors' => array(
    'Organization' => array(
        'class' => 'application.modules.todo.components.TodoOrganizationBehavior',
    ),
```

4. Modify `protected/modules/todo/controllers/TestController.php`, replace `actionBoilerplateStartOrganizationBehavior()` to `actionTodoOrganizationBehavior()` and with content:
```php
public function actionTodoOrganizationBehavior($id)
{
    $organization = Organization::model()->findByPk($id);
    foreach($organization->getTodoItems() as $todo)
    {
        echo sprintf('<li>#%d - %s</li>', $todo->id, $todo->title);
    }
    
}
```

Next, make sure you have created few todo items from backend and linked it to a specific organization. Acquired its organization id, append that to the end of URL, e.g. `https://hubd.mymagic.my/todo/test/todoOrganizationBehavior?id=999`


You should now see list of todos associated to this organization in display.

5. It will be convenience for us to see the list of TODOs in the organization detail page. Let's add a new tab beneath the page.

Modify `protected/modules/todo/TodoModule.php`, change `getOrganizationViewTabs()` function to:
```php
public function getOrganizationViewTabs($model, $realm = 'backend')
{
    $tabs = array();
    if ($realm == 'backend') {
        if (Yii::app()->user->accessBackend) {
            $tabs['todo'][] = array(
                'key' => 'todo',
                'title' => 'Todo',
                'viewPath' => 'modules.todo.views.backend._view-organization-todo',
            );
        }
    }

    return $tabs;
}
```

Create a new partial view at `protected/modules/todo/views/backend/_view-organization-todo.php`:

```php
<?php $todos = $model->getTodoItems() ?>
<ol>
<?php foreach($todos as $todo): ?>
    <li><span class="label label-default"><?php echo $todo->formatEnumStatus($todo->status) ?></span> <a href="<?php echo $this->createUrl('todo/todo/view', array('id'=>$todo->id)) ?>"><?php echo $todo->title ?></a> by <?php echo $todo->user->username ?> created on <?php echo Html::formatDateTime($todo->date_added) ?></li>
<?php endforeach; ?>
</ol>
```
 
> For more information on view tab injection, please refer to [Module Function Hooks \ getOrganizationViewTabs
](Module-Function-Hooks#getorganizationviewtabs)

## Create an action button
Next, we like to add an extra button to easily create TODO in the above tab.

1. Modify `getOrganizationActions()` in `protected/modules/todo/TodoModule.php`

```php
public function getOrganizationActions($model, $realm = 'backend')
{
    if ($realm == 'backend') {
        if (!Yii::app()->user->accessBackend) {
            return;
        }

        $actions['todo'][] = array(
            'visual' => 'primary',
            'label' => 'Create TODO',
            'title' => 'Create TODO for this organization',
            'url' => Yii::app()->controller->createUrl('/todo/todo/create', array('organizationId' => $model->id)),
        );
    } elseif ($realm == 'cpanel') {
    }

    return $actions;
}
```

2. It will be better if we can auto select the specific organization in create page, saving user's time of selecting from the list. Edit `actionCreate()` in `protected/modules/todo/controllers/TodoController.php`

```php
public function actionCreate($organizationId='')
{
    $model=new Todo;
    $model->organization_id = $organizationId;

```

## Next Chapter
[Chapter 4](Step-by-Step-Todo-module-Chapter4) - Implement Web API