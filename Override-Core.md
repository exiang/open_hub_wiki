`protected/overrides` directory allows you to override the core code to make changes without touching the original files. This is handy where changes you needs to make are beyond module capability. Having all your customization code in one place in overrides directory enable OpenHub original code to be easily upgrade in future too. 

## File base Overriding
Please note that this is not an object oriented overriding concept, but a file base overriding. 

As example, if you like to extend `protected/models/HUB.php` to add extra function called `myCustomFunction()`, you can not just create a `HUB.php` file in the overrides directory with one function only and hopping all the other existing functions of original files will be included automatically.

```
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
* `protected/commands`
* `protected/models` and `protected/models/hub`
* `protected/components`
* `protected/extensions`
* `protected/helpers`

To understand why you not allowed to import files recursively including all the subdirectory, please refer to issue: [https://code.google.com/archive/p/yii/issues/1568](https://code.google.com/archive/p/yii/issues/1568) 

## Override Command (Console)
Command is like web controller, except they are for console interface. You can override an existing command or create new command file here in `protected/overrides/commands`.

To override an existing command:
1. Copy the existing command file from `protected/commands` to `protected/overrides/commands`
2. Modify `protected/config/console.php` to point to the overridden file (e.g. we are modifying existing `TestCommmand`):
```
'commandMap' => array(
    'test'=>array(
        'class'=>'application.overrides.commands.TestCommand',
    ),
),
```

## Todo
However, you can't override a modules, controllers and commands (cli) now. We are working on this base on https://forum.yiiframework.com/t/controller-override/48217