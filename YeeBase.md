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

`YeeBase::getActionsInController('SiteController')`
```php
Array
(
    [0] => actionIndex
    [1] => actionWelcome
    [2] => actionAbout
    ...
)
```

#### getActionsInControllerPath($controllerFilePath, $withPrefixAction = true)
If you need to get the list of actions by specific controller file path, use this instead. `$controllerFilePath` is the full path to the controller. e.g.: `/var/www/open_hub/protected/controllers/SiteController.php`
