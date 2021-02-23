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

### Navigation

### Dashboard
* [getDashboardNotices](Module-Function-Hooks-%5C-getDashboardNotices)
* [getDashboardViewTabs](Module-Function-Hooks-%5C-getDashboardViewTabs)

### Backend only
* [getBackendAdvanceSearch](Module-Function-Hooks-%5C-getBackendAdvanceSearch)







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
