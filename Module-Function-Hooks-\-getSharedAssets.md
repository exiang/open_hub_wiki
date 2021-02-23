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
