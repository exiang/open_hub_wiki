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
```php
The view file will be located at `/protected/views/site/about.php`:
```
<p><?php echo $greeting ?>
```
