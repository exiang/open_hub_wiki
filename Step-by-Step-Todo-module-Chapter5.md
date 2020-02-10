## Chapter 5
Advance use case

## Settings in Module
There are 2 methods to define a setting in module.

First method is introduced in Chapter 1, where you can define variable in `protected/modules/todo/TodoModule.php` and use it with syntax `<?php echo $this->module->var1 ?>`. This none hardcoded method allows admin to edit configuration files like `protected/modules/todo/main.php` to change its value.

The method 2, an alternative to module settings, which is by using the functionalities provided by `Setting` default model. Unlike method 1 which is stored in file, method 2 is stored in database and accessible from `Backend > Site > Setting`.

To setup a setting, add inside `install()` function in file `protected/modules/todo/TodoModule.php`:

`$this->setupSetting('todo-defaultDisplayLimit', '100', array('datatype' => 'integer'));`
  * This will create a setting entry with key `todo-defaultDisplayLimit` of `integer` data type and has default value of `100`
  * Please strictly follow this naming convention, prefix your module setting with the module code, e.g. `todo-`

To use a setting:

`Setting::code2value('todo-defaultDisplayLimit', 50)`
  * 50 is the default value to use when todo-defaultDisplayLimit not found or set

## Inject CSS & JS to application scope
There are scenario where your module need to apply CSS and Javascript to the entire application beyond module scope. e.g. a bookmark module will needs to add an 'Add to bookmark' button across all pages. 

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

## Multilingual i18n
## Cron Job
## Sub domain for module
