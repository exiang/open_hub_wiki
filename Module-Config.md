<img width="170" alt="Screenshot 2021-01-20 at 3 16 19 PM" src="https://user-images.githubusercontent.com/5336690/105140715-10e78480-5b33-11eb-999f-2c5975e5692b.png">

A standard module has the following configuration files inside `config` folder.

A module is not initialise to activate until the below configuration files are created. These are installation environment specification configurations:
  * `module/config/main.php` - setting specific for web applications and it override value in `module/config/main.base.php`
  * `module/config/console.php` - setting specific for command line console applications and it override value in `module/config/console.base.php`

OpenHub module configuration system follow hierarchical design to allow greatest flexiblity for super admin and developers overriding setting at each layer.


<img width="790" alt="Screenshot 2021-01-20 at 4 21 40 PM" src="https://user-images.githubusercontent.com/5336690/105147118-b141a700-5b3b-11eb-9546-50f3a2cef565.png">



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

But if you need `var1` value to be different at both environment, for example, `apple` for `web` and `orange` for `console` environment, you will need to modify these environmental-base-configuration files respectively:

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

for `/protected/boilerplateStart/modules/config/console.base.php`
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

In these config files, your will have some other settings that you do not encourage your super admin to modify. But for those you like to expose, hint of their existent is recorded at `/protected/boilerplateStart/modules/config/main.dist.php` and `/protected/boilerplateStart/modules/config/console.dist.php`:

```
<?php
return array(
	'modules' => array(
		'boilerplateStart' => array(
			'var1' => 'anything you like to change to',
		)
	)
);
```

In backend, through module interface, system admin will be able to take these `*.dist.php` file for reference to set their own `/protected/boilerplateStart/modules/config/main.php` and `/protected/boilerplateStart/modules/config/console.php` respectively.

<img width="1271" alt="Screenshot 2021-01-20 at 3 51 19 PM" src="https://user-images.githubusercontent.com/5336690/105143955-835a6380-5b37-11eb-9735-a0c5f0f1d194.png">



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