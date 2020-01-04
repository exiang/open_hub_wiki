A module is not initialize to activate until configuration files are created:
  * `module/config/main.php`
  * `module/config/console.php`

### Changing Module URL
Module with name `mentor` will be access thru url `https://hub.mymagic.my/mentor` by Yii framework rule. If you like to change the URL to `https://hub.mymagic.my/mentorship` and still correctly route to `mentor` module, you should change it in the `modules/mentor/config/main.php` route section.

You should never rename the module folder directly to achieve this. 

```
'components' => array(
    'urlManager' => array(
        'rules' => array(
            'mentorship'=>'mentor/frontend/index',
            'mentorship/<controller>/<action>'=>'mentor/<controller>/<action>/*',
        ),
    ),
),
```