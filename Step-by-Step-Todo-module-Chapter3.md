## Chapter 3
SCENARIO: after publishing the module, you receive a new request from your boss: 
> As admin, I would like to see any pending TODO tasks associated with a specific startup or company.

After analyzing the requirement, these are the action plans (translated to developer language):
1. A TODO to be associated to an organization
2. A TODO to have statuses (new, pending, in progress, done)
3. Users to be able to see list of TODO associated to a specific organization in the organization view page.

### Step 1: Using UPGRADES to make changes
We will need to modify the database structure of `todo` table to add in the following new columns:
   * `organization_id` - foreign key to link to organization table
   * `status` - an enum to keep track of the TODO current status

1. The modular architecture of OpenHub comes with built-in versioning control. Since we are upgrading Todo module, we should change its version numbering in `protected/modules/todo/config/about.yaml`
   * Set `version` from `1.0` to `1.1`

2. Create a new `migration` instruction file at `protected/modules/todo/upgrades/upgrade-1.1.php`:
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

Click on Upgrade button to upgrade Todo module from backend.
![](https://user-images.githubusercontent.com/5336690/74012856-2a48fc80-49c6-11ea-880e-34d017d80647.png)

You will see this when Upgrade migration completed successfully.
![](https://user-images.githubusercontent.com/5336690/74012902-45b40780-49c6-11ea-84d7-849a43d41c82.png)

Table changes will be reflected in database.
![table database changes](https://user-images.githubusercontent.com/5336690/74012943-5a909b00-49c6-11ea-8b96-7e34d26185ca.png)

> For more information on Module Upgrade, please refer to [Module Install & Upgrade
](Module-Install-&-Upgrade)

### Step 2: Regenerate MVC
Next, we will need to regenerate the model, controller and view to reflect changes in the database.

Update `protected/modules/todo/data/todo.build.php` to
```php
<?php

return array(
	'layout' => 'backend',
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

Go to Yee Model Generator at `https://mydomain.com/yee/model`
1. Set `Table Name` to `todo`
1. Set `Model Path` to `application.modules.todo.models`
1. Set `Module Name` to `todo`
1. [!important] Uncheck `Build Extend Class`
1. Click Preview
1. If `modules/todo/models/TodoBase.php` is in red, click `diff` link to compare the difference between new and current model codes
1. You should also takes the opportunity to compare any changes to those functions you had overwritten in `modules/todo/models/Todo.php` in this case, and update accordingly manually
1. Check [overwrite] and click the Generate button when everything is good to go


Go to Yee CRUD Generator `https://mydomain.com/yee/crud/index`
1. Set `Model Class` to `application.modules.todo.models.Todo`
2. Set `Controller ID` to `todo/todo`
3. Do not jump straightaway into regenerate the code files to replace existing files. Remember we had done quite some changes here on the view `_form.php`? It's best to use the `[diff]` to carefully check what changes will be done to each of the files and apply them manually.
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
5. Go back to the Yee CRUD Generator, check and regenerate for all files except `modules/todo/views/todo/_form.php `.

Now, we can create a TODO record and link it to a specific company.

### Step 3: Inject to Organization Core Model

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

3. Replace the `modelBehaviors` section of `protected/modules/todo/config/console.php` and `protected/modules/todo/config/main.php`, also their respective `dist` files with the following codes to enable `modules > todo > modelBehaviors > Organization`. Ensure brackets are enclosed.
```php
'modelBehaviors' => array(
    'Organization' => array(
        'class' => 'application.modules.todo.components.TodoOrganizationBehavior',
    ),
```

4. Modify `protected/modules/todo/controllers/TestController.php`, replace `actionBoilerplateStartOrganizationBehavior()` with `actionTodoOrganizationBehavior()` and with content:
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

Next, make sure you have created few todo items from backend and linked it to a specific organization. Get its organization id, append that to the end of URL, e.g. `https://mydomain.com/todo/test/todoOrganizationBehavior?id=999`


You should now be able to see list of todos associated to this organization on the page.

5. It will be convenient for us to see the list of TODOs in the organization details page. Let's add a new tab beneath the page.

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
Now, we view list of TODO in the tab of the organization details page at https://yourdomain.com/organization/view?id=999.

> For more information on view tab injection, please refer to [Module Function Hooks \ getOrganizationViewTabs
](Module-Function-Hooks#getorganizationviewtabs)

### Step 4: Create an action button
Next, we would like to add an extra button to easily create new TODO in the tab.

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
    ...
```
3. Replace the content of _view-organization-todo.php with this code:
```php
<?php $todos = $model->getTodoItems() ?>
<?php if(!empty($actions['todo'])): ?>
<div class="row">
    <div class="col col-md-12">
    <div class="well text-center">
    <?php foreach($actions['todo'] as $action): ?>
        <a class="margin-bottom-sm btn btn-<?php echo $action['visual']?>" href="<?php echo $action[url] ?>" title="<?php echo $action['title'] ?>"><?php echo $action[label] ?></a>
    <?php endforeach; ?>
    </div>
    </div>
</div>
<?php endif; ?>
<ol>
<?php foreach($todos as $todo): ?>
    <li><span class="label label-default"><?php echo $todo->formatEnumStatus($todo->status) ?></span> <a href="<?php echo $this->createUrl('todo/todo/view', array('id'=>$todo->id)) ?>"><?php echo $todo->title ?></a> by <?php echo $todo->user->username ?> created on <?php echo Html::formatDateTime($todo->date_added) ?></li>
<?php endforeach; ?>
</ol>
```

### Handling Organization Merging 
#todo...

## Next Chapter
[Chapter 4](Step-by-Step-Todo-module-Chapter4) - Implement Web API