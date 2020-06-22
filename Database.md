## Create a new table
Please follow this [naming convention](Extending-Model-Meta) strictly.

## Flexible Schema
OpenHub used traditional relationship database as the core data storage method. The development team has been creative to  achieve flexible schema with the following 2 methods:
  * `json_extra` column
  * `meta_structure` table, please [refer here](Extending-Model-Meta).

### json_extra method
First, create variable name in camelCase in the model class.
```php
<?php

class MentorProgram extends MentorProgramBase
{
	public $aFlexibleVariable = 'abc';
}
```

Secondly, make sure this variable is safe to write thru `$_POST`
```php
public function rules()
{
    // NOTE: you should only define rules for those attributes that
    // will receive user inputs.
    return array(
        // json_extra
        array('brandCode', 'safe'),
    );
}
```

Third, use `beforeSave` to automatically save this variable into `json_extra` column in database table.
```php
protected function beforeSave()
{
    // custom code here
    // ...
    $this->jsonArray_extra->aFlexibleVariable = $this->aFlexibleVariable;
    return parent::beforeSave();
}
```

Last, use `afterFind` to automatically load this variable into model from database.
```php
protected function afterFind()
{
    // custom code here
    // ...

    parent::afterFind();
    $this->aFlexibleVariable = $this->jsonArray_extra->aFlexibleVariable;

    // return void
}
```

## Yii ActiveRecord
### Advance Query

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

Method 2 - or, if relations is defined in `Individual` model like this:
``` php
public function relations()
{
    return array(
        'individual2Emails' => array(self::HAS_MANY, 'Individual2Email', 'individual_id'),
         ...
```

then, you can have shorter query:
```php
$criteria = new CDbCriteria;
$criteria->with = 'individual2Emails';
$criteria->addCondition('individual2Emails.user_email LIKE :userEmail');
$criteria->params[':userEmail'] = $userEmail;

$result = Individual::model()->findAll($criteria);
```

## Updating Database structure
> Todo: Automated build should auto generate `public_html/installer/protected/data/base.sql` removing manual work in step 2 below.
Please follow this rule strictly:
1. All updates to core database structure must be done thru migration code
2. For best installation experience (without the need to run migration in ssh), update `public_html/installer/protected/data/base.sql` corresponding to your changes. Do not forget insert entry to `tbl_migration`  too.
3. All modules update, regardless default or 3rd party modules, must be done in respective [module upgrade function](Module-Install-&-Upgrade).