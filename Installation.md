> We are still in the process building a web interface installer to make installation easier for our users. Please stay tune with us.


### Manual Installation
1. Download the distributed zip file
    * PHP composer dependency packages are included
2. Setup required servers & services:
    1. Setup Ubuntu Server (18.04 & above)
    1. Setup Apache Web Server (Apache 2)
    1. Setup MySQL database server and load default SQL to it
    1. Setup AWS S3 storage. You need 2 buckets, one for secure protected storage and one for normal public storage
    1. Setup AWS ElasticSearch server. 
    1. Acquire Google API key
    1. Setup SMTP mail server. We recommend using transaction mail services from Mailchimp
    1. Setup Redis server for faster caching (optional) or use default file cache
    1. Setup Cloudflare (optional) to use its HTTPS service
3. Upload to web server and unzip
4. Modify configuration files under `protected/config`, all `*.dist.php` need to copy to `*.php` and has variable in it modified according to your environment
5. Make sure the following directory and all files in it are writable by server
    * `protected/runtime`
    * `protected/messages`
    * `protected/data`
    * `protected/modules`
    * `protected/overrides`
    * `public_html/assets`
    * `public_html/uploads`
6. Setup crons
7. Login to Backend
    * Update Settings


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
#### console.php
This is the `main.php` version of console environment for command interface.

#### params.php
This is the application params that shared among web controller and console command interfaces.
* **maintenance** - true|false, set to true if you need to temporary disable the site to enable maintenance mode 
* **dev** - true|false, set true if it is a development environment
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
