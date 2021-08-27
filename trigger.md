Yii Framework support event driven application out of the box by default. To avoid confuse with `Event` model of OpenHub, we called it `Trigger`.

## Custom Trigger
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

To trigger it (usually from controller):
```php
$model = new TestModel();

$model->attachEventHandler('onApproveSuccess', function ($event) {echo "Test model #".$event->params['model']->id." is updated successfully");});
$model->attachEventHandler('onApproveFailed', function ($event) {echo "Test model #".$event->params['model']->id." failed to update");});
```

## Built-in Trigger
Developers may built-in trigger to their module at model and controller level, for other modules to add on features to it.

For example, `job` module cpanel `publishJob()` trigger this event upon job publishing successfully.
```php
HUB::triggerEvent($this->module, 'publishJob', array('jobVacancy' => $model));
```

`MagicOps` module can implement this event with the following code in `MagicOpsModule`
```php
public function triggerJobPublishJob($params)
{
    $msg = sprintf(Yii::t('magicOps', "'{organizationTitle}' published new job vacancy '{jobTitle}' {link}", array(
        '{organizationTitle}' => $params['jobVacancy']->ownerOrganization->title,
        '{jobTitle}' => $params['jobVacancy']->title,
        '{link}' => $params['jobVacancy']->getPublicUrl(true),
    )));
    if (YeeModule::hasActiveModule('discord')) {
        HubDiscord::broadcast($msg, Yii::app()->getModule('job')->discordChannelPublishedJob);
    }
}
```

### Common building blocks

#### Organization
* triggerHubOrganizationCreateOrganization(array('organization'=>$organization))
* triggerHubOrganizationUpdateOrganization(array('organization'=>$organization))

#### Individual
* triggerHubIndividualCreateIndividual(array('individual'=>$individual))
* triggerHubIndividualUpdateIndividual(array('individual'=>$individual))

### Event
* triggerHubEventCreateEvent(array('event'=>$event))
* triggerHubEventUpdateEvent(array('event'=>$event))

### Default Modules
#### Registry
* triggerHubRegistrySet(array('registry'=>$registry))

#### CV
* triggerCVEnablePortfolio(array('cvPortfolio'=>$cvPortfolio))
* triggerCVDisablePortfolio(array('cvPortfolio'=>$cvPortfolio))

#### Job
* triggerJobSubmit(array('jobVacancy'=>$jobVacancy))
* triggerJobPublishJob(array('jobVacancy'=>$jobVacancy))
* triggerJobUnpublishJob(array('jobVacancy'=>$jobVacancy))
* triggerJobApproveJob(array('jobVacancy'=>$jobVacancy))