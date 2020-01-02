OpenHub support multiple domains, accommodating 1 instance installation for multisite of different domain names. A common use case is to use different domain on a specific module for branding purposes.

## Domain Specific Configuration
This can be done by setting the `protected/config/domain.php` files. This file hold array of settings to be override and merge into main config array base on detected `$_SERVER['SERVER_NAME']`. 

Inside `protected/config/domain.php` file:
```php
<?php

return array(
    'hubactivate.my'=>array(
        'defaultController'=>'activate/frontend',
        'params'=>array(
            'masterDomain'=>'hubactivate.my',
            'masterUrl'=>'//hubactivate.my',
            'baseDomain'=>'hubactivate.my',
            'baseUrl'=>'//hubactivate.my',
            'connectSecretKey'=>'insert your secret key here...',
            'connectClientId'=>'55',
        )
    )
);
```

  * Use `defaultController` to redirect request to specific `module\controller` instead of the default `app\site` setting.