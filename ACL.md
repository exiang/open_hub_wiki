## What is ACL?
It is a list of permissions attached to an action or operation. ACL can specifies which roles are granted access to actions or operations. By doing this, you do not have to hard-coded the role to access any menu or action and you can add dynamic role as per need in the future and allow specific action or operation they can access.

## How it works?
We will supply the role for current user with the action or operation

```
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

## Usage
There are few ways to use the function. It depends on where you want to put it.

 * in accessRule for Controller file using _**expression**_. Provide _**Yii::app()->controller**_ as second parameter.
 ```
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

 * in view to hide or show menu 
	* _**first array**_ - Action is in the _**same Controller**_). Provide _**Yii::app()->controller**_ as second parameter and Action for the third parameter (in this case, _create_ is the action)
	* _**second array**_ - Action is in the _**different Controller**_). Provide _**custom parameter**_ for the second parameter
 ```
 $this->menu = array(
	array(
		'label' => Yii::t('app', 'Create Todo'), 'url' => array('/todo/todo/create'),
		'visible'=>HUB::roleCheckerAction(Yii::app()->user->getState("rolesAssigned"), Yii::app()->controller,'create')
	),
	array(
		'label' => Yii::t('app', 'Create Sample1'), 'url' => array('/module1/controller1/create'),
		'visible'=>HUB::roleCheckerAction(Yii::app()->user->getState("rolesAssigned"), (object)['id'=>'controller1','action'=>(object)['id'=>'create'],'module'=>(object)['id'=>'module1']])
	),
);
```