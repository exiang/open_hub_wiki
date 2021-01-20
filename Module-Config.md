<img width="170" alt="Screenshot 2021-01-20 at 3 16 19 PM" src="https://user-images.githubusercontent.com/5336690/105140715-10e78480-5b33-11eb-999f-2c5975e5692b.png">

A standard module has the following configuration files inside `config` folder.

A module is not initialise to activate until the below configuration files are created. These are installation environment specification configurations:
  * `module/config/main.php` - setting specific for web applications and it override value in `module/config/main.base.php`
  * `module/config/console.php` - setting specific for command line console applications and it override value in `module/config/console.base.php`

### Exposing a module variable to config
Your `BoilerplateStartModule` has a new variable called `var1` where its value can be access anywhere in code with `Yii::app()->getModule('boilerplateStart')->var1`.
```
<?php

// use camelcase for class name with first character in uppercase
class BoilerplateStartModule extends WebModule
{
	public $var1;

        // more code...
}
```

You like to set a default value to it. By this we means the value will be commit to the code repository and available to the public when they download and install your module. If this value are the same for both `web` and `console` environments, you put it at `/protected/boilerplateStart/modules/config/base.php`:
```
<?php

return array(
	'modules' => array(
		'boilerplateStart' => array(
                 'var1' => 'from config/base.php',
        )
    )

    // other configurations ....
);
```

But if you need `var1` value to be different at both environment, for example, `apple` for `web` and `orange` for `console` environment, you will need to modify these files respectively:

for `/protected/boilerplateStart/modules/config/main.base.php`:
```
<?php
return array(
	'modules' => array(
		'boilerplateStart' => array(
			'var1' => 'apple',
		)
	)
);
```
`/protected/boilerplateStart/modules/config/console.base.php`
```
<?php
return array(
	'modules' => array(
		'boilerplateStart' => array(
			'var1' => 'orange',
		)
	)
);
```

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