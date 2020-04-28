## Chapter 5
Advanced use cases

## Settings in Module
There are 2 methods to define a setting in module.

First method is introduced in Chapter 1, where you can define variable in `protected/modules/todo/TodoModule.php` and use it with syntax `<?php echo $this->module->var1 ?>`. This non-hardcoded method allows admin to edit configuration files like `protected/modules/todo/main.php` to change its value.

The 2nd method, an alternative to module settings which is by using the functionalities provided by `Setting` default model. Unlike method 1 which is stored in file, method 2 is stored in database and accessible from `Backend > Site > Setting`.

To setup a setting, add the code inside `install()` function in file `protected/modules/todo/TodoModule.php`:

`$this->setupSetting('todo-defaultDisplayLimit', '100', array('datatype' => 'integer'));`
  * This will create a setting entry with key `todo-defaultDisplayLimit` of `integer` data type and has default value of `100`
  * Please strictly follow this naming convention, prefix your module setting with the module code, e.g. `todo-`

To use a setting:

`Setting::code2value('todo-defaultDisplayLimit', 50)`
  * 50 is the default error code value to use when todo-defaultDisplayLimit not found or set

## Inject CSS & JS to application scope
There are scenarios where your module need to apply CSS and Javascript to the entire application beyond module scope. e.g. a bookmark module will needs to add an 'Add to bookmark' button across all pages. 

OpenHub module support this thru `protected/modules/todo/TodoModule.php`:
```php
public function getSharedAssets($forInterface='*')
{
    switch($forInterface)
    {
        case 'layout-backend':
        {
            $return['css'][] = array('src' => self::getAssetsUrl() . '/css/backend.shared.css');
            $return['js'][] = array('src' => self::getAssetsUrl() . '/javascript/backend.shared.js', 'position'=>CClientScript::POS_END);
            break;
        }
        default:
        {
            break;
        }

    }

    return $return;
}
```

In `protected/views/layouts/backend.php`, this following code will scan all modules for assets to include.

```php
<?php
$modules = YeeModule::getParsableModules();
foreach ($modules as $moduleKey => $moduleParams) {
	if (method_exists(Yii::app()->getModule($moduleKey), 'getSharedAssets')) {
		$assets = Yii::app()->getModule($moduleKey)->getSharedAssets('layout-backend');
		foreach ($assets['js'] as $jsAsset) {
			Yii::app()->getClientScript()->registerScriptFile($jsAsset['src'], !empty($jsAsset['position']) ? $jsAsset['position'] : CClientScript::POS_END, !empty($jsAsset['htmlOptions']) ? $jsAsset['htmlOptions'] : array());
		}
		foreach ($assets['css'] as $cssAsset) {
			Yii::app()->getClientScript()->registerCssFile($cssAsset['src'], !empty($cssAsset['media']) ? $cssAsset['media'] : '');
		}
	}
}
?>
```

All you need to do next is to code your logic in the following list of files:
  * `protected/modules/todo/assets/css/backend.shared.css`
  * `protected/modules/todo/assets/css/frontend.shared.css`
  * `protected/modules/todo/assets/javascript/backend.shared.js`
  * `protected/modules/todo/assets/javascript/frontend.shared.js`

## Command
Your module can contain Command Line functions to run back office functionality without worry about web server timeout limitation. A CLI command can also be escalate into cron job to run scheduled tasks automatically.

Create file `protected/modules/todo/commands/TodoCommand.php` and fill with content:
```php
<?php

class TodoCommand extends ConsoleCommand
{
    public $verbose=false;
    public function actionIndex()
    {
	echo "Available command:\n";
        echo "  * Shout --param1=hello --param2=world\n";
        echo "\n"; 
    }
	
    public function actionShout($param1, $param2)
    {
	echo sprintf("%s %s\n", $param1, $param2);
        
        Yii::app()->esLog->log(sprintf("called todo\should"), 'command', array('trigger'=>'TodoCommand::actionShout', 'model'=>'', 'action'=>'', 'id'=>''), '', array('param1'=>$param1, 'param2'=>$param2));
    }
    
}
```

To run this command, at your docker project folder:
```
./bash.sh
cd magic_hub/protected
php yiic todo Shout --param1=Hello --param2=World
```

To turn it into a cron command, please refer to this [documentation](Cron-Commands).

## Multilingual i18n
Your module might need to support multilingual and internationalization (i18n). Please refer to this [documentation](i18n) for more details.

In short: 
1. Change all your interface text to i18n format. e.g. from `Hello World` to `<?php echo Yii:t('todo', 'Hello World')?>`
  * Remember to use module code `todo` as the translation context
1. For big chunk of text, separate them into partial view file.
1. For database, follow the documentation above to learn how to structure it.

## Change module name
You are accessing todo module from url `https://mydomain.com/todo` and your marketing department suddenly needs to change this to `https://mydomain.com/newtodo`. 

**You SHOULD NOT rename this module**.

What you can do is edit `protected/modules/todo/config/main.php` and change this part of code:
```php
'urlManager' => array(
    'rules' => array(
        'newtodo'=>'todo/frontend/index',
        'newtodo/<controller>/<action>'=>'todo/<controller>/<action>/*',
    ),
),
```

Now, you are good to go with url `https://mydomain.com/newtodo` and the old one `https://mydomain.com/todo` is still functioning for backward compatibility.

## Using composer in module
OpenHub comes with default composer packages install at `protected/vendor`. However, your module may required specific package that not included in this default bundle. You can have your module specific packages located at `protected/modules/todo/vendor`.

Example:
```
cd protected/modules/todo
composer require knplabs/github-api php-http/guzzle6-adapter "^1.1"
```

* `vendor` directory will be created, ignore by repository in default
* `composer.json` & `composer.lock` files created

## Sub domain for module
Todo...