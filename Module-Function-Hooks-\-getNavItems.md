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