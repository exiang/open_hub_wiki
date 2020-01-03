Implement these function hooks in `protected/modules/YOUR_MODULE/YourModule.php` to change how OpenHub core code works.

#### getOrganizationViewTabs

```php
public function getOrganizationViewTabs($model, $realm = 'backend'){}
```
This function hook allows you to inject an interface tab & content block to view Organization page in both cpanel and backend for your module.

_Screenshot of how your module injected content will display in view organization page_

![image](https://user-images.githubusercontent.com/5336690/71715430-42b87b00-2e4c-11ea-8308-4ca243d41984.png)

#### getOrganizationActions
```php
public function getOrganizationActions($model, $realm = 'backend'){}
```
This function hook allows you to define list of action buttons in injected interface block to view organization page in both cpanel and backend for your module (thru `getOrganizationViewTabs()`).

```php
if ($realm == 'backend') {
    if (!Yii::app()->user->accessBackend) {
        return;
    }

    $actions['boilerplatStart'][] = array(
        'visual' => 'primary',
        'label' => 'Backend Action 1',
        'title' => 'Backend Action 1 short description',
        'url' => Yii::app()->controller->createUrl('/boilerplatStart/backend/action1', array('id' => $model->id)),
    );
} elseif ($realm == 'cpanel') {
    $actions['boilerplatStart'][] = array(
        'visual' => 'primary',
        'label' => 'Frontend Action 1',
        'title' => 'Frontend Action 1 short description',
        'url' => Yii::app()->controller->createUrl('/boilerplatStart/frontend/action1', array('id' => $model->id)),
    );
}

return $actions;
```

Next, not to forget you will also has to implement the rendering part of these action buttons in your view.

```php
<?php if(!empty($actions['boilerplatStart'])): ?>
<div class="row">
    <div class="col col-md-12">
    <div class="well text-center">
    <?php foreach($actions['boilerplatStart'] as $action): ?>
        <a class="margin-bottom-sm btn btn-<?php echo $action['visual']?>" href="<?php echo $action[url] ?>" title="<?php echo $action['title'] ?>"><?php echo $action[label] ?></a>
    <?php endforeach; ?>
    </div>
    </div>
</div>
<?php endif; ?>
```

#### getOrganizationActFeed
```php
public function getOrganizationActFeed($organization, $year){}
```

Pass in Organization object and year (e.g. `getOrganizationActFeed($organization, '2020')`), this function is called by activity feed to list module records that associate with this organization for this year.

#### getIndividualViewTabs
```php
public function getIndividualViewTabs($model, $realm = 'backend'){}
```
This function hook allows you to inject an interface block to view Individual page in both cpanel and backend for your module.

#### getIndividuaActions
```php
public function getIndividuaActions($model, $realm = 'backend'){}
```
This function hook allows you to define list of action buttons in injected interface block to view Individual page in both cpanel and backend for your module (thru `getIndividualViewTabs()`).

#### getIndividualActFeed
```php
public function getIndividualActFeed($individual, $year){}
```
Pass in Individual object and year (e.g. `getIndividualActFeed($individual, '2020')`), this function is called by activity feed to list module records that associate with this individual for this year.

#### getUserActFeed
```php
public function getUserActFeed($user, $year){}
```
Pass in User object and year (e.g. `getUserActFeed($user, '2020')`), this function is called by activity feed to list module records that associate with this user for this year.

#### getNavItems
```php
function getNavItems($controller, $forInterface){}
```
This function is called to all modules by initBackendMenu() in `protected/components/Controller.php`, to acquired navigation items. 

Available `$forInterface` code:
  * Backend
    * backendNavService![image](https://user-images.githubusercontent.com/5336690/71714577-63330600-2e49-11ea-921f-673471b66050.png)
    * backendNavDev![image](https://user-images.githubusercontent.com/5336690/71714606-8067d480-2e49-11ea-971d-9680ea2b2f27.png)
    * backendNavUserService![image](https://user-images.githubusercontent.com/5336690/71714621-8b226980-2e49-11ea-85ee-3c84f4719248.png)

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

#### getAsService
```php
public function getAsService($interface){}
```

#### getSharedAssets
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
#### getBackendAdvanceSearch
```php
public function getBackendAdvanceSearch($controller, $searchFormModel){}
```
You may like to list module records in backend search when admin search for a keyword. 

```php
public function getBackendAdvanceSearch($controller, $searchFormModel)
{
    $searchModel = new BoilerplateModel('search');
    $result['boilerplateStart'] = $searchModel->searchAdvance($searchFormModel->keyword);

    return array(
        'boilerplateStart' => array(
            'tabLabel' => Yii::t('backend', 'Boilerplate'),
            'itemViewPath' => 'application.modules.boilerplateStart.views.backend._view-boilerplateStart-advanceSearch',
            'result' => $result['boilerplateStart'],
        ),
    );
}
```

#### doOrganizationsMerge
```php
public function doOrganizationsMerge($source, $target){}
```

Organization feature allows admin to merge duplicate from source to target records in backend. When this happens, module' records associate with the source organization needs to be transfer to the target organization. You will need to code the merging logic here. 

Examples:
```php
public function doOrganizationsMerge($source, $target)
{
    $transaction = Yii::app()->db->beginTransaction();

    // process challenge
    $sql = sprintf("UPDATE challenge SET owner_organization_id='%s' WHERE owner_organization_id='%s'", $target->id, $source->id);
    Yii::app()->db->createCommand($sql)->execute();

    $transaction->commit();

    return array($source, $target);
}
```

#### doIndividualsMerge
```php
public function doIndividualsMerge($source, $target){}
```
Individual feature allows admin to merge duplicate from source to target records in backend. When this happens, module' records associate with the source individual needs to be transfer to the target individual. You will need to code the merging logic here. 