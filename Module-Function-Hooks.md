Implement these function hooks in `protected/modules/YOUR_MODULE/YourModule.php` to change how OpenHub core code works.

### Organization
* [getOrganizationViewTabs](Module-Function-Hooks-%5C-getOrganizationViewTabs)
* [getOrganizationActions](Module-Function-Hooks-%5C-getOrganizationActions)
* [getOrganizationActFeed](Module-Function-Hooks-%5C-getOrganizationActFeed)
* [doOrganizationsMerge](Module-Function-Hooks-%5C-doOrganizationsMerge)

### Individual
* [getIndividualViewTabs](Module-Function-Hooks-%5C-getIndividualViewTabs)
* [getIndividuaActions](Module-Function-Hooks-%5C-getIndividuaActions)
* [getIndividualActFeed](Module-Function-Hooks-%5C-getIndividualActFeed)
* [doIndividualsMerge](Module-Function-Hooks-%5C-doIndividualsMerge)

### User
* [getUserActFeed](Module-Function-Hooks-%5C-getUserActFeed)

### Event
* [getEventActions](Module-Function-Hooks-%5C-getEventViewTabs)
* [getEventViewTabs](Module-Function-Hooks-%5C-getEventActions)

### Member Control Panel

### Backend
* [getBackendAdvanceSearch](Module-Function-Hooks-%5C-getBackendAdvanceSearch)






### getNavItems
```php
function getNavItems($controller, $forInterface){}
```
This function is called to all modules by initBackendMenu() in `protected/components/Controller.php`, to acquired navigation items. 

Available `$forInterface` code:
  * Backend
    * **backendNavService**

      add link under service menu
      ![image](https://user-images.githubusercontent.com/5336690/71714621-8b226980-2e49-11ea-85ee-3c84f4719248.png)
    * **backendNavDev**

      add link under Dev (development) menu, only visible to developer role
      ![image](https://user-images.githubusercontent.com/5336690/71714606-8067d480-2e49-11ea-971d-9680ea2b2f27.png)
    * **backendNavUserService**

      add link under user avatar menu
      ![image](https://user-images.githubusercontent.com/5336690/71714577-63330600-2e49-11ea-921f-673471b66050.png)

  * Event
    * eventAdminSideNav
  * Cpanel
    * cpanelNavDashboard 
    * cpanelNavSetting
    * cpanelNavCompany
    * cpanelNavCompanyInformation
  * External modules
    * MdecMSC
      * mdecMscAdminSideNav

``` php
public function getNavItems($controller, $forInterface)
{
    switch ($forInterface) {
        case 'backendNavService':

            return array(
                array(
                    'label' => Yii::t('backend', 'Open Innovation Challenge'), 'url' => '#',
                    'visible' => Yii::app()->user->getState('accessBackend') == true,
                    'active' => $controller->activeMenuMain == 'challenge' ? true : false,
                    'itemOptions' => array('class' => 'dropdown-submenu'), 'submenuOptions' => array('class' => 'dropdown-menu'),
                    'linkOptions' => array('class' => 'dropdown-toggle', 'data-toggle' => 'dropdown'),
                    'items' => array(
                        array('label' => Yii::t('app', 'Challenge Overview'), 'url' => array('/challenge/backend'), 'visible' => Yii::app()->user->getState('accessBackend') == true),
                    ),
                ),
            );

            break;
    }
}
```

### getAsService
```php
public function getAsService($interface){}
```

### getSharedAssets
```php
public function getSharedAssets($forInterface = '*'){}
```

Sometimes, a module needs to inject `CSS` or `Javascript` files across the OpenHub application. This function allow this to be done by passing in the interface layout code. 

Available layout codes are:
* layout-backend
* layout-frontend

```php
public function getSharedAssets($forInterface = '*')
{
    switch ($forInterface) {
        case 'layout-backend': {
                $return['css'][] = array('src' => self::getAssetsUrl() . '/css/backend.shared.css');
                $return['js'][] = array('src' => self::getAssetsUrl() . '/javascript/backend.shared.js', 'position' => CClientScript::POS_END);
                break;
            }
        case 'layout-frontend': {
                $return['css'][] = array('src' => self::getAssetsUrl() . '/css/frontend.shared.css');
                $return['js'][] = array('src' => self::getAssetsUrl() . '/javascript/frontend.shared.js', 'position' => CClientScript::POS_END);
                break;
            }
        default: {
                break;
            }
    }

    return $return;
}
```




### getDashboardNotices
For both cpanel and backend, display flash message on top of page.
```php
public function getDashboardNotices($model, $realm = 'backend')
{
    if($realm == 'cpanel'){
        $notices[] = array('message' => 'Hello World', 'type' => Notice_WARNING);
        return $notices;
    }
}
```

### getDashboardViewTabs
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

### 