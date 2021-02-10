## How it works
Module assets are store in path like `/protected/moudles/boilerplateStart/assets`. It will be auto copied to path like `/public_html/assets/933ea96f` after deployment.

In the development environment, you will need to set `DEV = true` in `/protected/.env` for the copy mechanism to auto kick in. `BoilerplateStartModule` has this piece of code that ensures this working.
```php
public function beforeControllerAction($controller, $action)
{
    if (parent::beforeControllerAction($controller, $action)) {
        if (true == Yii::app()->params['dev']) {
            Yii::app()->assetManager->forceCopy = true;
        }

        return true;
    } else {
        return false;
    }
}
```

## Images
```php
<?php echo Html::image($this->module->assetsUrl.'/images/photo1.png'); ?>
```

or
```php
public function getAssetsUrl()
{
    if (null === $this->_assetsUrl) {
        $this->_assetsUrl = Yii::app()->getAssetManager()->publish(Yii::getPathOfAlias('ekyc.assets'));
    }

    return $this->_assetsUrl;
}
```
so you can do this in view:
```php
<?php echo Html::image($this->module->getAssetUrl().'/images/photo1.png'); ?>
```