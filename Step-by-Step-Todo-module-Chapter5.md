## Chapter 5
Advance use case

## Settings in Module
As what you have learnt from Chapter 1, you can define variable in `protected/modules/todo/TodoModule.php` and use it with syntax `<?php echo $this->module->var1 ?>`. This none hardcoded method allows admin to edit configuration files like `protected/modules/todo/main.php` to change its value.

OpenHub also provide an alternative to module settings, which is by using the functionalities provided by `Setting` default model.

To setup these setting, in `install()` function:

`$this->setupSetting('todo-defaultDisplayLimit', '100', array('datatype' => 'integer'));`
  * Please strictly follow this naming convention, prefix your module setting with the module code, e.g. `todo-`

To use a setting:

`Setting::code2value('todo-defaultDisplayLimit', 50)`
  * 50 is the default value to use when todo-defaultDisplayLimit not found or set



## Inject CSS & JS to application scope
## Multilingual i18n
## Cron Job
## Sub domain for module
