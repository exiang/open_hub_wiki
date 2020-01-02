## Directory Structure
Here is how the files are organized in module:

  * Commands - `protected/yiic.php` has been modified to read all commands action from `protected/modules/YOUR_MODULE/commands` folder
  * Config - `protected/config/main.php` has been modified to include `protected/config/module.php` that read all configuration from `protected/modules/YOUR_MODULE/config` folder. Same goes to the  `protected/config/console.php`
  * Components
  * Controllers
  * Models
  * Views

## Inject functions to core Model
Behavior allow developer to inject custom functions into existing core model (e.g. Organization, Individual, Event, etc) without modify the code code (e.g. `protected/model/Organization.php`), all within the context of module.

For example, `protected/modules/boilerplateStart/components/BoilerplateStartOrganizationBehavior.php` add customize functions to `Organization` model:

```
<?php

Yii::import('modules.boilerplateStart.models.*');

// this allow you to inject method into organization object
class BoilerplateStartOrganizationBehavior extends Behavior
{
	// the organization model
	public $model;

	//
	// items
	public function countAllBoilerplateStartItems()
	{
		return HubBoilerplateStart::countAllOrganizationBoilerplateStarts($this->model);
	}

	public function getActiveBoilerplateStartItems($limit = 100)
	{
		return HubBoilerplateStart::getActiveOrganizationBoilerplateStarts($this->model, $limit);
	}
}
```

This allow me to call `$organization->getActiveBoilerplateStartItems()` in my module controller.

### Conflicting behavior
According to Yii documentation:

> When there can be name conflicts among different behaviors attached to the same component, the conflicts are automatically resolved by prioritizing the behavior attached to the component first. Name conflicts caused by different traits requires manual resolution by renaming the affected properties or methods.

Hence, it's important to name your behavior function uniquely to prevent conflict with other modules. Follow the naming convention of `[verb][Adjective][ModuleName][Subject]()`.

## Main function hooks
Implement these function hooks in `protected/modules/YOUR_MODULE/YourModule.php` to change how OpenHub core code works.

#### getOrganizationViewTabs
#### getOrganizationActions
Called by core Organization feature to list out action buttons at both cpanel and backend view. 
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
Pass in Organization object and year (e.g. `getOrganizationActFeed($organization, '2020')`), this function is called by activity feed to list module records that associate with this organization for this year.

#### getIndividualViewTabs

#### getIndividuaActions
Called by core Individual feature to list out action buttons at both cpanel and backend view. 

#### getIndividualActFeed
Pass in Individual object and year (e.g. `getIndividualActFeed($individual, '2020')`), this function is called by activity feed to list module records that associate with this individual for this year.

#### getUserActFeed
Pass in User object and year (e.g. `getUserActFeed($user, '2020')`), this function is called by activity feed to list module records that associate with this user for this year.

#### getNavItems
```php
function getNavItems($controller, $forInterface){}
```
This function is called to all modules by initBackendMenu() in `protected/components/Controller.php`, to acquired navigation items. 


Available `$forInterface` code:
  * Backend
    * backendNavService
    * backendNavDev
    * backendNavUserService
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
#### getSharedAssets
Sometimes, a module needs to inject `CSS` or `Javascript` files across the OpenHub application. This function allow this to be done by passing in the interface layout code. 

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
Individual feature allows admin to merge duplicate from source to target records in backend. When this happens, module' records associate with the source individual needs to be transfer to the target individual. You will need to code the merging logic here. 

## Dealing with Database
#### install
#### installDb

## Web API
Wapi support modularization architecture and able to read `protected/modules/YOUR_MODULE/data/api/*.yaml` for swagger definition. `protected/modules/wapi/V1Controller.php` has been modified to auto load `protected/modules/YOUR_MODULE/actions/wapi/V1Controller/*.php` for api action