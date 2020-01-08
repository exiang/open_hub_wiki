## Which layout to use

You can define which layout to use in OpenHub's module controller in one of the following methods:

```php
public $layout = 'backend';
```

Or

```php
public function init()
{
    parent::init();
    $this->layout = 'backend';
    ....
}
```

  * `layouts.backend` - using the layout named `backend` found in base layout directory (be it default or overridden)
  * `backend` - using the layout named `backend` found in layout directory inside module
  * `application.views.layouts.backend` - explicitly using the layout name `backend` in from the default layout directory (Note: this is dangerous, as it will bypass the layout override and caused inconsistency)

## Sample of layout view file in a module
In `protected/modules/boilerplateStart/views/layouts/backend.php`:

```php
<?php Yii::app()->getClientScript()->registerCssFile($this->module->getAssetsUrl() . '/css/backend.css'); ?>
<?php Yii::app()->getClientScript()->registerScriptFile($this->module->getAssetsUrl() . '/javascript/backend.js', CClientScript::POS_END); ?>

<?php $this->beginContent('layouts.backend'); ?>
	<?php echo $content; ?>
<?php $this->endContent(); ?>
```

Notice the use of `layouts.backend`, it will automatically using the backend layout view file from base layout directory, be it the default or overridden.