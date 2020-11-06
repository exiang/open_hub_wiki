# Meta
Meta allowing you to extends a model without changing database structure. As one of the method of [flexible schema](Database#flexible-schema), this is helpful when module needs to add new attribute to one of the core model such as organization, individual, event...

## Enable Meta for Model
Meta is not automatically enabled for all models, it has to be done thru coding by developer.

[Refer here](Naming-Convention#meta-data) on how to name your meta variable.

```php
public function init()
{
    // meta
    $this->initMetaStructure($this->tableName());
    parent::init();
}
```

```php
public function rules()
{
    // NOTE: you should only define rules for those attributes that
    // will receive user inputs.
    $return = array(
        array('code, title', 'required'),
        ...
    );
    
    // meta
    $return[] = array('_dynamicData', 'safe');

    return $return;
}
```

```php
public function relations()
{
    return array(
        ...
        // meta
        'metaStructures' => array(self::HAS_MANY, 'MetaStructure', '', 'on' => sprintf('metaStructures.ref_table=\'%s\'', $this->tableName())),
        'metaItems' => array(self::HAS_MANY, 'MetaItem', '', 'on' => 'metaItems.ref_id=t.id AND metaItems.meta_structure_id=metaStructures.id', 'through' => 'metaStructures'),
    );
}
```

OpenHub comes with built in function `MetaStructure::initMeta` to help you create the structure:

``` php
MetaStructure::initMeta('organization', 'sample', 'extraColumn1', 'boolean', 'Highlight in Sample', 'Is this organization a lighted sample?', '');
```

This will create a `meta_structure` record with code `Organization-sample-extraColumn1` where `sample` is the module code name and `extraColumn1` is the variable name.

## Get a meta structure
As simple as:
```php
<?php echo $model->_dynamicData['Organization-sample-extraColumn1']
```

## Query a meta structure
SQL join is required to query value from meta data.

```sql
SELECT o.id FROM organization as `o` 

INNER JOIN `meta_structure` as ms1 on ms1.ref_table='organization'
INNER JOIN `meta_item` as mi1 on mi1.meta_structure_id=ms1.id

INNER JOIN `meta_structure` as ms2 on ms2.ref_table='organization'
INNER JOIN `meta_item` as mi2 on mi2.meta_structure_id=ms2.id

WHERE  
(ms1.code='Organization-idea-membershipType'  AND mi1.ref_id=o.id) AND 
(ms2.code='Organization-idea-isEnterprise' AND mi2.value='1' AND mi2.ref_id=o.id) 

GROUP BY o.id ORDER BY o.title ASC
```
## Known Issues
### Meta Items value cant set due to bad code from modules' behavior
You tried to set value `$org->_dynamicData[organization-abc'] = 'Hello World'` but it is not working, meta value is not saved. 
But when you commented the following line from model `Organization` for example, the problem no longer exists. 
```php
public function behaviors()
{
    /*foreach (Yii::app()->modules as $moduleKey => $moduleParams) {
        if (isset($moduleParams['modelBehaviors']) && !empty($moduleParams['modelBehaviors']['Organization'])) {
            $return[$moduleKey] = Yii::app()->getModule($moduleKey)->modelBehaviors['Organization'];
            $return[$moduleKey]['model'] = $this;
        }
    }*/
}
```
This show the sign where one of your module (as long as it is initiated) is causing this issue.