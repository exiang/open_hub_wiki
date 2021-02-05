Yii Framework support event driven application out of the box by default. 

```php  
class TestModel extends ActiveRecordBase
{
    // ...
    public function setApprove()
    {
        if ($this->save()) {
            $this->raiseEvent('onApproveSuccess', new CEvent($this, array('model' => $this)));
        } else {
            $this->raiseEvent('onApproveFailed', new CEvent($this, array('model' => $this)));
        }
    }

    // this is required event is empty
    public function onApproveSuccess($event){}

    // this is required event is empty
    public function onApproveFailed($event){}
}
```

To trigger it:
```php
$model = new TestModel();

$model->attachEventHandler('onApproveSuccess', function ($event) {echo "Test model #".$event->params['model']->id." is updated successfully");});
$model->attachEventHandler('onApproveFailed', function ($event) {echo "Test model #".$event->params['model']->id." failed to update");});
```