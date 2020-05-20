## Advance Query in Yii

To select all matching individual records associated with email 'erlich@piedpiper.com' in simple SQL:

``` sql
SELECT t.* FROM individual as t 
LEFT JOIN `individual2email` as i2e ON t.id=i2e.individual_id 
WHERE i2e.user_email='erlich@piedpiper.com'
```

Method 1 - Doing this in Yii ActiveRecord:

``` php
$userEmail = 'erlich@piedpiper.com';

$criteria = new CDbCriteria;
$criteria->select = 't.*';
$criteria->join = 'LEFT JOIN `individual2email` AS `i2e` ON t.id = i2e.individual_id';
$criteria->addCondition('i2e.user_email LIKE :userEmail');
$criteria->params[':userEmail'] = $userEmail;

$result = Individual::model()->findAll($criteria);
```

or, if relations is defined in `Individual` model like this:
``` php
public function relations()
{
    return array(
        'individual2Emails' => array(self::HAS_MANY, 'Individual2Email', 'individual_id'),
         ...
```

Method 2 - then, you can also query it with:
```php
$criteria = new CDbCriteria;
$criteria->with = 'individual2Emails';
$criteria->addCondition('individual2Emails.user_email LIKE :userEmail');
$criteria->params[':userEmail'] = $userEmail;

$result = Individual::model()->findAll($criteria);
```
