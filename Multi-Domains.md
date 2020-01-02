OpenHub support multiple domains, accommodating 1 instance of Central for multisite of different domain names.

## Domain Specific Configuration
This can be done by setting the `protected/config/domain.php` files. This file hold array of settings to be override and merge into main config array base on detected `$_SERVER['SERVER_NAME']`. 

Content of `domain.php` file:
``
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
``

  * Use `defaultController` to redirect request to specific module\controller instead of the default app\site setting.