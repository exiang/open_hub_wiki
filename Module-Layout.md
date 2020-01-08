## Which layout to use

In controller:
```php
public $layout = 'layouts.backend';
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