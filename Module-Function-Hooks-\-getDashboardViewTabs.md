Rendered as tabs in backend dashboard.
```php
public function getDashboardViewTabs($model, $realm = 'backend')
{
    $tabs = array();
    if ($realm == 'backend') {
        if (Yii::app()->user->accessBackend) {
            $tabs['esLog'][] = array(
                'key' => 'esLog',
                'title' => 'Log',
                'viewPath' => 'modules.esLog.views.backend._view-dashboard-esLog'
            );
        }
    }

    return $tabs;
}
```

Rendered as block (flowing down) at cpanel.
```php
public function getDashboardViewTabs($model, $realm = 'backend')
{
    $tabs = array();
    if ($realm == 'cpanel') {
        $tabs['magicOps'][] = array(
            'key' => 'magicOps',
            'title' => 'Welcome',
            'viewPath' => 'modules.magicOps.views.cpanel._view-dashboard-welcome',
            'ordering' => -100,
        );
    }

    return $tabs;
}
```