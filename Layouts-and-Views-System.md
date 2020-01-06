## View
The V of the M-V-C, a view is a PHP script consisting mainly of user interface elements. It can contain PHP statements, but it is recommended that these statements should not alter data models and should remain relatively simple. In the spirit of separating of logic and presentation, large chunks of logic should be placed in controllers or models rather than in views.

Laravel used Twig, some used Smarty Template, and there are many more template system out there. OpenHub believe in simplicity, so we stick to PHP.

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

## Layouts
Layout is a special view that is used to decorate views. It usually contains parts of a user interface that are common among several views, you may also think of it as a master template for all your views. For example, a layout may contain a header and a footer, and embed the view in between, like this:

```php
<div>Header code here...</div>
<?php echo $content; ?>
<div>Footer code here...</div>
```
where `$content` stores the rendering result of the view.

Layout is automatically applied when calling render(). By default, layouts sits at `protected/views/layouts` but can be overridden at `protected/overrides/views/layouts`.

OpenHub layout system make use of theme. Theme is a superset of layout in OpenHub architecture.

For more information on Yii framework views and layouts system, please visit https://www.yiiframework.com/doc/guide/1.1/en/basics.view.

## Theme
Theming is a systematic way of customizing the outlook of pages in a Web application. By applying a new theme, the overall appearance of a Web application can be changed instantly and dramatically.

In Yii, each theme is represented as a directory consisting of view files, layout files, and relevant resource files such as images, CSS files, JavaScript files, etc. The name of a theme is its directory name. All themes reside under the same directory `public_html/themes`. At any time, only one theme can be active.

OpenHub used a modified inspinia theme, located in `public_html/themes/inspinia`. Inspinia is a commercial Bootstrap theme, you will need to purchase its license at https://wrapbootstrap.com/theme/inspinia-responsive-admin-theme-WB0R5L90S. Demo of inspinia integration to YeeBase is available thru URL `https://yourdomain.com/inspinia`.

For more information on Yii framework theme system, please visit https://www.yiiframework.com/doc/guide/1.1/en/topics.theming
