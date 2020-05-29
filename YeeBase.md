YeeBase is an external repo which extended Yii Framework 1.0 functionality with various customization.

## Helpful Functions

#### getControllersInPath($path, $withPostfixController = true)
To get the list of controllers by specific file path. `$path` is the full path to the directory. e.g.: `/var/www/open_hub/protected/controllers`

`getControllersInPath('/var/www/open_hub/protected/controllers')`
```php
Array
(
    [0] => BackendController
    [1] => CategoryController
    [2] => FrontendController
    [3] => GeofocusController
    [4] => ResourceController
    [5] => TestController
)
```

`getControllersInPath('/var/www/open_hub/protected/controllers', false)`
```php
Array
(
    [0] => backend
    [1] => category
    [2] => frontend
    [3] => geofocus
    [4] => resource
    [5] => test
)
```
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


