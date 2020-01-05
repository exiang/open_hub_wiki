`protected/overrides` directory allows you to override the core code to make changes without touching the original files. This is handy where changes you needs to make are beyond module capability. Having all your customization code in one place in overrides directory enable OpenHub original code to be easily upgrade in future too. 

## File base Overriding
Please note that this is not an object oriented overriding concept, but a file base overriding. 

As example, if you like to extend `protected/models/HUB.php` to add extra function called `myCustomFunction()`, you can not just create a `HUB.php` file in the overrides directory with one function only and hopping all the other existing functions of original files will be included automatically.

```php
<?php
class HUB
{
	public static function myCustomFunction()
	{
		return 'Hello World';
	}
}
```

Instead, you should copy the entire original `HUB.php` file and place it in the overrides directory, and make modification to add in this extra function.

## Files you can override now
Files that can be overridden are those located in:
* `protected/controllers`
* `protected/commands`
* `protected/models` and `protected/models/hub`
* `protected/components`
* `protected/components/widgets`
* `protected/extensions`
* `protected/helpers`

If you have sub directories to override, you will need to explicitly define them in `protected/config/main.php` and  `protected/config/console.php`.
```php
'import' => array(
    // overrides
    'application.overrides.helpers.*',
    'application.overrides.models.*',
    'application.overrides.models.hub.*',
    'application.overrides.components.*',
    'application.overrides.components.widgets.*',
    'application.overrides.extensions.*',

    'application.overrides.components.subDirectory1.*',
),
```

The reason is you not allowed to import files recursively to include all sub directories, you may refer this link to understand why: [https://code.google.com/archive/p/yii/issues/1568](https://code.google.com/archive/p/yii/issues/1568) 

## Override Controller
You can override web controller.

### To create a new controller:
1. Create new file `protected/overrides/controllers/OverrideController.php`:
```php
<?php

class OverrideController extends Controller
{
    public $layout = 'application.views.layouts.frontend';
    
    public function actionIndex()
    {
	echo 'I am a new overridden controller';
    }
}
```
2. You may access this controller thru URL 'https://yourdomain.com/override`

### To create new controller render with view
1. Modify `actionIndex()` above to:
```php
public function actionIndex()
{
    $this->render('index');
}
```
2. Create new file `protected/overrides/views/override/index.php`:
```html
<p>I am a new overridden controller render with view</p>
```

### How it works
Yii framework `CwebApplication` is extended by `protected/components/WebApplication.php` and call in `public_html/index.php`:
```php
require_once(dirname(__FILE__).'/../protected/components/WebApplication.php');
$app = new WebApplication($config);
```
In `WebApplication`, when `createController` is called, OpenHub will first check for controller with exact name found in `protected/overrides/controllers` folder. If found, the overridden version will be use. 
Reference: https://forum.yiiframework.com/t/controller-override/48217


## Override Command (Console)
Command is like web controller, except they are for console interface. You can override an existing command or create new command file here in `protected/overrides/commands`.

### To create new command:
1. Create new file `protected/overrides/commands/OverrideCommand.php`
```php
<?php

class OverrideCommand extends ConsoleCommand
{
    public $verbose = false;

    public function actionIndex()
    {
        echo 'Congratulation, you has successfully created a new override command';
    }

}
2. Execute it thru CLI:
```
php yiic override
```

```
### To override an existing command:
For example, we are modifying existing `TestCommmand`

1. Copy the existing command file from `protected/commands/TestCommmand.php` to `protected/overrides/commands/TestCommmand.php` and modify the `actionIndex()` to output something different than the original.
2. Modify `protected/config/console.php` to point to the overridden file:
```php
'commandMap' => array(
    'test'=>array(
        'class'=>'application.overrides.commands.TestCommand',
    ),
),
```
Notes: This is an extra step required to override existing command
3. Execute it thru CLI:
```
php yiic test
```
