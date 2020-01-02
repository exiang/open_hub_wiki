OpenHub’s extensibility revolves around modules, which are small programs that make use of OpenHub’s functionalities and changes them or add to them in order to make OpenHub’s easier to use or more tailored to the provider’s needs.

## Introduction
A OpenHub module consists of a main PHP file with as many other PHP files as needed, as well as the necessary assets (images, JavaScript, CSS, etc.) to display the module’s interface, whether to the user (on the front office) or to the provider (on the back office).

Any OpenHub module, once installed on an online system, can interact with one or more "hooks". Hooks enable you to hook/attach your code to the current View at the time of the code parsing (i.e., when displaying the an organization etc). Specifically, a hook is a shortcut to the various methods available from the Module object, as assigned to that hook.

As a module developer, you make changes within the context of module. You must not modify the core files. 

## Modules’ operating principles
Modules are the ideal way to let your talent and imagination as a developer express themselves, as the creative possibilities are many and you can do pretty much anything with OpenHub’s module API.

Any module:
  * can display a variety of content (blocks, text, etc.), perform many tasks (cron, batch update, import, export, etc.), interface with other tools, and much much more.
  * can be made as configurable as necessary; the more configurable it is, the easier it will be to use, and thus will be able to address the needs of a wider range of users.
  * can add functionalities to OpenHub without having to edit its core files, thus making it easier to perform an update of OpenHub without having the transpose all core changes. Indeed, you should always strive to stay away from core files when building a module, even though this may seem necessary in some situations.
  * can use functions of other modules or share its own functionality to others
  * can be seen as one or part of a service; however, not all modules are services as some do not have frontend interface for user to interact with.

## Modules folder
OpenHub modules are found in the `protected/modules` folder. This is true for both default modules (provided with OpenHub) and 3rd-party modules that are subsequently installed.

## Quick start
Modules must follow some guidelines to work on OpenHub. If you want to get started quickly, with a standard module templates, you can use the `Boilerplate` Module.

### Boilerplate Module
Please refer to `protected/modules/boilerplateStart` that serve as a starting base for your new created module.
Its core file is at `protected/modules/boilerplateStartModule/BoilerplateStartModule.php`. All functions are equipped with self describe comment. These functions allow your module to inject code or interface into existing model, layout base on predefined design. 

### My first module
Let’s start; this will enable us to better describe its structure. We will name it "My Plugin".

1. First, copy the `boilerplateStart` folder located in the module's folder at `/protected/modules` and rename it to your new module. It should have the same name as the module, with no space, no hyphen and underscore, only alphanumerical characters, all in lower camel case format: `myPlugin`.
1. This folder must contain the main file, a PHP file of the same name with postfix of Module as the folder, which will handle most of the processing: MyPluginModule.php. Rename it from `BoilerplateStartModule.php`. Notice the filename is in upper camel case format.
1. Next, you need to rename everything of `boilerplateStart` to `myPlugin`, and `BoilerplateStart` to `MyPlugin` (in case sensitive manner). This including the filename as well as content in those files.
1. Modify `/protected/modules/boiletplateStart/config/about.yaml`:
    * change code from `boilerplatStart` to `myPlugin`
    * change title to 'My Plugin'
    * change oneliner to 'My first plugin module for OpenHub'
5. Once completed, your new module is now ready to be install. That is enough for a basic module but obviously more files and folders can be added later.

### Install a module
  * Login to backend
  * Click Site -> System -> Module
  * Click `New Module` tab, your new module is in the list
  * Click the `initialize` button
  * Once installation completed, your module is now moved to `Module` tab

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

## Todo
Modularization to the underlying system are done phase by phase. The following features are not supported by module yet:

* Migrate - Migration code only works from `protected/migrations` folder
* Test - There’s no modularize unittest yet
* Messages - Multilingual are centralized at `protected/messages/LANGUAGE_CODE/*.php`. It’s recommended to prefix your languages with module name, eg: `Yii::t(‘MODULE_NAME’, ‘Hello World’)`
* Data for Yee - Yee do not read data definition for model generation from your module. The generated model has to be stored in `protected/models` if to use the compare diff feature, as Yee are not able to generate model at module level