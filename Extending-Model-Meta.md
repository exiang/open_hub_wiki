# Meta
Meta allowing you to extends a model without changing database structure. This is helpful when module need to add new attribute to one of the core model such as organization, individual, event...

## Enable Meta for Model
Meta do not automatically enable for model, developers need to do it thru coding.

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