<img width="170" alt="Screenshot 2021-01-20 at 3 16 19 PM" src="https://user-images.githubusercontent.com/5336690/105140715-10e78480-5b33-11eb-999f-2c5975e5692b.png">

A standard module has the following configuration files inside `config` folder.

A module is not initialise to activate until the below configuration files are created. These are installation environment specification configurations:
  * `module/config/main.php` - setting specific for for web applications and it override value in `module/config/main.base.php`
  * `module/config/console.php` - setting specific for command line console applications and it override value in `module/config/console.base.php`




### Changing Module URL
Module with name `mentor` will be access thru url likes `https://hub.mymagic.my/mentor` by Yii framework rule. If you needs to change the URL to `https://hub.mymagic.my/mentorship` and still correctly point to `mentor` module, you should change it in the `modules/mentor/config/main.php` route section.

You should never rename the module folder directly to achieve this. 

``` php
'components' => array(
    'urlManager' => array(
        'rules' => array(
            'mentorship'=>'mentor/frontend/index',
            'mentorship/<controller>/<action>'=>'mentor/<controller>/<action>/*',
        ),
    ),
),
```