## What is ACL?
Access Control List is a list of permissions attached to an action or operation. ACL can specifies which roles are granted access to actions or operations. By doing this, you do not have to hard-coded the role to access any menu or action and you can add dynamic role as per need in the future and allowed specific action or operation they can access.

## Why ACL?
You can change preset Role to limit/grant the access to any action. You can add new Role to match with the new requirements without opening the source code.

## How it works?
We will supply the role for current user with the action or operation

```php
/**
 * to check whether role is allowed to access the route/action
 *
 * @param string $role
 * @param object $controller
 * @param string $action this param could be use for custom part to be accessed by the role
 *
 * @return boolean
 **/
public function roleCheckerAction($role, $controller, $action = '') {
	...
	...
}
```

## How to add the access?
[Inserting Access](https://github.com/mymagic/open_hub/wiki/Inserting-Access)

## Usage
There are few ways to use the function. It depends on where you want to put it.

 * example in accessRules function in Controller file using `expression`. Provide `Yii::app()->controller` as second parameter.
 ```php
 public function accessRules(){
	return array(
 	array('allow',
			'actions' => array('admin', 'delete'),
			'users' => array('@'),
			'expression' => 'HUB::roleCheckerAction(Yii::app()->user->getState("rolesAssigned"), Yii::app()->controller)'
		),
	);
 }
 ```

 * example in view to hide or show menu 
	* _**first array**_ - Action is in the _**same Controller**_. Provide `Yii::app()->controller` as second parameter and Action for the third parameter (in this case, _admin_ is the action)
	* _**second array**_ - Action is in the _**different Controller**_. Provide _**custom parameter**_ for the second parameter
 ```php
 $this->menu = array(
	array(
		'label' => Yii::t('app', 'Manage Todo'), 'url' => array('/todo/todo/admin'),
		'visible'=>HUB::roleCheckerAction(Yii::app()->user->getState("rolesAssigned"), Yii::app()->controller,'admin')
	),
	array(
		'label' => Yii::t('app', 'Manage Sample1'), 'url' => array('/module1/controller1/admin'),
		'visible'=>HUB::roleCheckerAction(Yii::app()->user->getState("rolesAssigned"), (object)['id'=>'controller1','action'=>(object)['id'=>'admin'],'module'=>(object)['id'=>'module1']])
	),
);
```

* example in gridview to hide or show icon 
	* _**view icon**_ - Action is in the _**different Controller**_. Provide _**custom parameter**_ for the second parameter
	* _**update icon**_ - Action is in the _**same Controller**_. Provide `Yii::app()->controller` as second parameter and Action for the third parameter (in this case, _update_ is the action)
```php
array(
	'class' => 'application.components.widgets.ButtonColumn',
	'template' => '{view}{update}',
	'buttons' => array(
		'view' => array(
			'url' => 'Yii::app()->controller->createUrl("/organization/view", array("id"=>$data->id))'),
			'visible' => function(){ return HUB::roleCheckerAction(Yii::app()->user->getState("rolesAssigned"), (object)['id'=>'organization','action'=>(object)['id'=>'view']]); }
		'update' => array(
			'visible' => function(){ return HUB::roleCheckerAction(Yii::app()->user->getState("rolesAssigned"), Yii::app()->controller,'update'); }
		),
	),
),
```


## For Sensitive Data
Some data or information, it can be only visible/accessible to specific role. You may use this to limit the accessibility `Yii::app()->user->getState('accessSensitiveData')` 
```php
$this->menu = array(
   array(
   	'label' => Yii::t('app', 'Manage Todo'), 'url' => array('/todo/todo/admin'),
   	'visible'=>Yii::app()->user->getState('accessSensitiveData')
   ),
   array(
   	'label' => Yii::t('app', 'Manage Sample1'), 'url' => array('/module1/controller1/admin'),
   	'visible'=>Yii::app()->user->getState('accessSensitiveData')
   ),
);
```

## Action that is limited to Developer but not SuperAdmin role
For action that have real route, you need to allow `Developer` role to access it. Refer to this [step](https://github.com/mymagic/open_hub/wiki/Inserting-Access#assign-access-to-role). Certain cases, you need custom/fake access to allow certain role to have that accessibility. For this case the role is `Developer`.
```php
$this->menu = array(
   array(
   	'label' => Yii::t('app', 'Manage Todo'), 'url' => array('/todo/todo/admin'),
   	'visible'=>HUB::roleCheckerAction(Yii::app()->user->getState("rolesAssigned"), (object)['id'=>'todo','action'=>(object)['id'=>'admin'],'module'=>(object)['id'=>'todo']])
   ),
   array(
   	'label' => Yii::t('app', 'Manage Sample1'), 'url' => array('/module1/controller1/admin'),
   	'visible'=>HUB::roleCheckerAction(Yii::app()->user->getState('rolesAssigned'), (object)['id' => 'custom', 'action' => (object)['id' => 'developer']])
   ),
);
```