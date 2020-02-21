### Config Files
#### main.php
Located at `protected/config/main.php`

1. change the application name to yours, e.g.
```
'name' => 'MaGIC (Dev)',
```

2. under `components\requests\csrfCookie`, set to your domain
```
'domain' => '.mymagic.my',
```

3. under `components\db`, set your database connection credential
```
'db' => array(
    'connectionString' => 'mysql:host=localhost;dbname=magic_hub',
    'username' => 'root',
    'password' => 'mypassword',
    ...
),
```

4. under `components\cache`, you may choose to use the default file cache
```
'cache' => array(
    'class' => 'CFileCache',
    ...
),
```
 or using a redis cache server for scalability. Please take note that Cluster Redis Server is not supported by Yii1.
```
'cache' => array(
    //'class' => 'CFileCache', // aws redis only available for local vpn
    'class' => 'CRedisCache',
    'hostname' => 'myRedisServerHost.com:6379',
    'port'=>6379,
),
```

5. under `components\esLog`,  you may choose to either enable or disable it. esLog used AWS Elastic Search to log users activities. 
```
'esLog' => array(
    'class' => 'application.yeebase.components.EsLog',
    'esLogRegion' => '',
    'enableEsLog' => true,
    'esLogIndexCode' => 'log-default',
    'esLogEndpoint' => '',
    'esLogKey' => '',
    'esLogSecret' => '',
    'esTestVar' => '123',
),
``` 

6. under `components\s3`, you has to set the `aKey`(api key) and `sKey`(secret key) to your AWS S3 bucket.
```
's3' => array(
    'class' => 'application.yeebase.extensions.s3.ES3',
    'aKey' => '',
    'sKey' => '',
),
```

7. under `components\neo4j`, you may choose to either enable or disable it and set your neo4j database connection credential

```
'neo4j' => array(
	'enable' => true, // enable or disable neo4j database
	'class'=>'application.extensions.neo4j.NeoEntity',
	'neoConnectionString' => '', // protocol://username:password@neo4jurl:porttype  -- type port (http)7474, (bolt)7687 ex. bolt://neo4j:password@localhost:7687
),
```
#### console.php
This is the `main.php` version of console environment for command interface.

#### params.php
This is the application params that shared among web controller and console command interfaces.
* **maintenance** - true|false, set `true` to temporary disable the site to enable maintenance mode 
* **dev** - true|false, set `true` to mark this is a development environment
* **cache** - true|false, to enable application wide cache or not
* **environment** - development|staging|production, specify the environment mode of this installation
* **sourceCurrency** - MYR
* **languages** - Array, specified languages supported by this system which is align with your database. e.g.
```
'languages'=>array('en' => 'English', 'ms' => 'Bahasa', 'zh' => '中文'),
```
* **frontendLanguages** - Array, subset of your supported languages that you like to make it available to frontend. e.g.
```
'frontendLanguages'=>array('en' => 'English', 'ms' => 'Bahasa'),
```
* **backendLanguages** - Array, subset of your supported languages that you like to make it available in backend. e.g.
```
'backendLanguages'=>array('en' => 'English', 'ms' => 'Bahasa', 'zh' => '中文'),
```
* **adminEmail** - email address of administrator
* **webmasterEmail** - published email for this website
* **routineEmails** - Array, a list of emails to be notify by cron activities. e.g.
```
'routineEmails'=>array('exiang83@gmail.com'),
```

#### path.php
Path configuration should be automatically detected.

```php
Yii::setPathOfAlias('wwwroot', dirname(__DIR__, 2) . '/public_html');
Yii::setPathOfAlias('uploads', dirname(__DIR__, 2) . '/public_html/uploads');
Yii::setPathOfAlias('static', dirname(__DIR__, 2) . '/public_html/static');
Yii::setPathOfAlias('runtime', dirname(__DIR__) . '/runtime');
Yii::setPathOfAlias('cronRoutine', dirname(__DIR__) . '/runtime/routine');
Yii::setPathOfAlias('cronLog', dirname(__DIR__, 2) . '/_cron/runtime/log');
Yii::setPathOfAlias('data',dirname(__DIR__) . '/data');

Yii::setPathOfAlias('components', dirname(__DIR__) . '/components');
Yii::setPathOfAlias('controllers', dirname(__DIR__) . '/controllers');
Yii::setPathOfAlias('models', dirname(__DIR__) . '/models');
Yii::setPathOfAlias('modules', dirname(__DIR__) . '/modules');
Yii::setPathOfAlias('views', dirname(__DIR__) . '/views');
Yii::setPathOfAlias('messages', dirname(__DIR__) . '/messages');
Yii::setPathOfAlias('overrides', dirname(__DIR__) . '/overrides');
```