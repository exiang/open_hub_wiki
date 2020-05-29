YeeBase is an external repo which extended Yii Framework 1.0 functionality with various customization.

## Helpful Functions
#### getActionsInController($controllerName, $moduleKey = '', $withPrefixAction = true)
This will return a list of actions name inside a controller which belongs to base or module. 

`YeeBase::getActionsInController('TestController', 'boilerplateStart')`

```php
Array
(
    [0] => actionBoilerplateStartOrganizationBehavior
    [1] => actionIndex
)
```

`YeeBase::getActionsInController('TestController', 'boilerplateStart', false)`
```php
Array
(
    [0] => boilerplateStartOrganizationBehavior
    [1] => index
)
```
