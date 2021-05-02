```php
public function doOrganizationsMerge($source, $target){}
```

Organization feature allows admin to merge duplicate from source to target records in backend. When this happens, module' records associate with the source organization needs to be transfer to the target organization. You will need to code the merging logic here. 

Examples:
```php
public function doOrganizationsMerge($source, $target)
{
    $transaction = Yii::app()->db->beginTransaction();

    // process challenge
    $sql = sprintf("UPDATE challenge SET owner_organization_id='%s' WHERE owner_organization_id='%s'", $target->id, $source->id);
    Yii::app()->db->createCommand($sql)->execute();

    $transaction->commit();

    return array($source, $target);
}
```

### Merge Preview
To help admin performing the merge action understand which associate records will be affected, edit to populate the list at view file `/protected/modules/challenge/views/_organization/_getOrganizationNodes.php`.