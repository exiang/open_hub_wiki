Model Behavior allows developer to inject custom functions into existing core model (e.g. Organization, Individual, Event, etc) without modify the code code (e.g. `protected/model/Organization.php`), all within the context of module.

For example, `protected/modules/boilerplateStart/components/BoilerplateStartOrganizationBehavior.php` add customize functions to `Organization` model:

```
<?php

Yii::import('modules.boilerplateStart.models.*');

// this allow you to inject method into organization object
class BoilerplateStartOrganizationBehavior extends Behavior
{
	// the organization model
	public $model;

	//
	// items
	public function countAllBoilerplateStartItems()
	{
		return HubBoilerplateStart::countAllOrganizationBoilerplateStarts($this->model);
	}

	public function getActiveBoilerplateStartItems($limit = 100)
	{
		return HubBoilerplateStart::getActiveOrganizationBoilerplateStarts($this->model, $limit);
	}
}
```

This allow me to call `$organization->getActiveBoilerplateStartItems()` in my module controller.

### Remember to define in configuration
Your model behaviors will works only if you define them in `YOUR_MODULE/config/main.php` and `YOUR_MODULE/config/console.php`

```php
'modelBehaviors' => array(
    'Organization' => array(
        'class' => 'application.modules.boilerplateStart.components.BoilerplateStartOrganizationBehavior',
    ),
    'Individual'=>array(
        'class'=>'application.modules.boilerplateStart.components.BoilerplateStartIndividualBehavior',
    ),
    'Event'=>array(
        'class'=>'application.modules.boilerplateStart.components.BoilerplateStartEventBehavior',
    ),
    'Resource'=>array(
        'class'=>'application.modules.boilerplateStart.components.BoilerplateStartResourceBehavior',
    ),
),
```
### Conflicting behavior
According to Yii documentation:

> When there can be name conflicts among different behaviors attached to the same component, the conflicts are automatically resolved by prioritizing the behavior attached to the component first. Name conflicts caused by different traits requires manual resolution by renaming the affected properties or methods.

Hence, it's important to name your behavior function uniquely to prevent conflict with other modules. Follow the naming convention of `[verb][Adjective][ModuleName][Subject]()`.