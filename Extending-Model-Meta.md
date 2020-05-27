# Meta
Meta allowing you to extends a model without changing database structure. This is helpful when module need to add new attribute to one of the core model such as organization, individual, event...

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

``` php
MetaStructure::initMeta('organization', 'sample', 'extraColumn1', 'boolean', 'Highlight in Sample', 'Is this organization a lighted sample?', '');
```

## Known Issues
### Meta Items value cant set due to bad code from modules' behavior
You tried to set value `$org->_dynamicData[organization-abc'] = 'Hello World' but it is not working, meta value is not saved. 
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