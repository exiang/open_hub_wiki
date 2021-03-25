Render as card in the dashboard. Currently support only cpanel (backend card coming soon...)

![Screenshot 2021-03-25 at 7 05 22 PM](https://user-images.githubusercontent.com/5336690/112463364-18c6dd80-8d9d-11eb-9f96-a38da42c099e.png)

```php
public function getDashboardViewCards($model, $realm = 'backend')
{
    $cards = array();
    if ($realm == 'backend') {
        if (Yii::app()->user->accessBackend) {
        }
    } elseif ($realm == 'cpanel') {
        // announce talentbank if user have not created one
        if (!Yii::app()->controller->user->hasCvPortfolio()) {
            $cards['cv'][] = array(
                'key' => 'dashboard-card-cv-newUser',
                'title' => 'Create Portfolio',
                'viewPath' => 'modules.cv.views.cpanel._view-dashboard-card-newUser',
                'ordering' => -100,
            );
        }
    }

    return $cards;
}
```

And for the view, is simply:

```php
<p>Showcase your portfolio on <?php echo Yii::app()->getModule('cv')->appName ?>, a digital platform where most our startups scout for co-founders and talents.</p>

<p><a class="btn btn-white btn-outline btn-xs" href="<?php echo $this->createUrl('cv/cpanel') ?>">Start your adventure here!</a></p>
```