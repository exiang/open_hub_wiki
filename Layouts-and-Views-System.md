## View
The V of the M-V-C, a view is a PHP script consisting mainly of user interface elements. It can contain PHP statements, but it is recommended that these statements should not alter data models and should remain relatively simple. In the spirit of separating of logic and presentation, large chunks of logic should be placed in controllers or models rather than in views.

Laravel used Twig, some frameworks used Smarty, and there are many more template system out there. OpenHub believe in simplicity, so we stick to PHP.

As example, a view can be directly use by controller's action:
```php
class SiteController extends Controller
{
    public function actionAbout()
    {
        $this->render('about', array('greeting'=>'Hello World'))
    }
}
```
The view file will be located at `/protected/views/site/about.php`:
```html
<p><?php echo $greeting ?></p>
```
Open the page in browser, you will not seeing a plain one line Hello World text only. You will also see other layout elements display in this page. We will talk about Layout in next section.

There's also another type of view where its whole purpose is to be included into other view file. The naming convention for this private view file is to have an underscore prefix (e.g.: `_viewProductItem.php`).

## Layout
Layout is a special view that is used to decorate views. It usually contains parts of a user interface that are common among several views, you may also think of it as a master template for all your views. For example, a layout may contain a header and a footer, and embed the view in between, like this:

```php
<div>Header code here...</div>
<?php echo $content; ?>
<div>Footer code here...</div>
```
where `$content` stores the rendering result of the view.

Layout is automatically applied when calling render(). By default, layouts sit at `protected/views/layouts` but can be overridden by custom code in `protected/overrides/views/layouts`.

OpenHub layout system make use of theme. Theme is a superset of layout in OpenHub architecture.

For more information on Yii framework views and layouts system, please visit https://www.yiiframework.com/doc/guide/1.1/en/basics.view.

## Brand
Brand is a small hack around layout system to display different branding (e.g. custom header and footer) for different 'owner' of the system. Please note this is just a gimmick than actual multi tenant system which OpenHub is never aspired to be. 

Brand is implemented thru variable `$this->layoutParams['brand']` and applied system wide. Dedicated brand layout is display according to the calling domain name utilizing [Multi Domains feature](Multi-Domains) and [Core Overrides feature](Override-Core).

In `protected/overrides/config/domain.php`
```php
<?php
return array(
	'hubpsk.gov.my' => array(
		'defaultController' => 'resource/frontend',
		'params' => array(
			'masterDomain' => 'hubpsk.gov.my',
			'masterUrl' => '//hubpsk.gov.my',
			'baseDomain' => 'hubpsk.gov.my',
			'baseUrl' => '//hubpsk.gov.my',
			'connectSecretKey' => '',
			'connectClientId' => '',
			'brand' => 'psk',
		),
		'modules' => array(
			'resource' => array(
				'allowUserAddResource' => false
			)
		)
	)
);

```

In `protected/overrides/views/layouts/frontend.php`, we implemented a hardcoded selection.
``` php
<?php if ($this->layoutParams['brand'] == 'kkip'): ?>
    <?php $this->beginContent('layouts.brands.kkip.frontend'); ?>
<?php elseif ($this->layoutParams['brand'] == 'psk'): ?>
    <?php $this->beginContent('layouts.brands.psk.frontend'); ?>
<?php else: ?>
    <?php $this->beginContent('layouts.brands.default.frontend'); ?>
<?php endif; ?>
    <?php echo $content; ?>
<?php $this->endContent(); ?>
```

<img width="273" alt="Screenshot 2020-06-25 at 11 11 26 AM" src="https://user-images.githubusercontent.com/5336690/85649017-a83b4500-b6d4-11ea-8ca9-3cec060d0145.png">


Brand aware module has higher override priority than system wide brand setting. Current module who are brand aware:
- f7: display branding according to brand value set in intake
- mentor: display branding according to brand value set in mentor program

## Theme
Theming is a systematic way of customizing the outlook of pages in a Web application. By applying a new theme, the overall appearance of a Web application can be changed instantly and dramatically.

In Yii, each theme is represented as a directory consisting of view files, layout files, and relevant resource files such as images, CSS files, JavaScript files, etc. The name of a theme is its directory name. All themes reside under the same directory `public_html/themes`. At any time, only one theme can be active and it is set in `protected/config/main.php`:

```php
$return = array(
    'basePath' => dirname(__FILE__) . DIRECTORY_SEPARATOR . '..',
    'name' => 'MaGIC (Dev)',
    'theme' => 'inspinia',
    ...
)
```

This is how a view layout can point to theme layout:
```php
<?php $this->beginContent(sprintf('webroot.themes.%s.views.layouts._backend', Yii::app()->theme->name)); ?>
    <?php echo $content; ?>
<?php $this->endContent(); ?>
```

OpenHub used a modified inspinia theme, located in `public_html/themes/inspinia`. Inspinia is a commercial Bootstrap theme, you will need to purchase its license at https://wrapbootstrap.com/theme/inspinia-responsive-admin-theme-WB0R5L90S. 

Demo of inspinia integration to YeeBase is available thru URL `https://yourdomain.com/inspinia`.

For more information on Yii framework theme system, please visit https://www.yiiframework.com/doc/guide/1.1/en/topics.theming


## Javascript
Javascript can be embedded inside view:

``
<?php Yii::app()->clientScript->registerScript(
'module-controller-action',
<<<EOD

EOD
); ?>

```