## sendNotify
You can send a notification by calling `HUB::sendNotify`.

```php 
sendNotify($userType, $username, $message, $title = '', $content = '', $priority = 3, $options = null) 
```
* receiver user type: member, admin or organization
* message can either be a string or a json payload
* priority, default to 3, 1 is least important, 5 is most important
* compose of title, message and content is the job of `NotifyMaker`

```php
$notifyMaker = NotifyMaker::member_user_linkUserEmail($user, $user2email);
$options['email']['receivers'][] = array('method' => 'cc', 'name' => $nameTeam, 'email' => $emailTeam);
HUB::sendNotify('member', $submission->user->username, $notifMaker['message'], $notifMaker['title'], $notifMaker['content'], 3, $options);
```

`NotifyMaker` function follow the following format: `NotifyMaker::receiverUserType_module_verbNouns`

```php
public static function member_user_linkUserEmail($user, $user2email)
{
    $return['title'] = Yii::t('notify', 'Please verify this email address');
    $return['message'] = Yii::t('notify', 'Please verify this email address.');

    $params['email'] = $user2email->user_email;
    $params['username'] = $user->username;
    $params['urlVerify'] = Yii::app()->createAbsoluteUrl('/cpanel/verifyUser2Email', array('email' => $user2email->user_email, 'key' => $user2email->verification_key));
    $params['urlDelete'] = Yii::app()->createAbsoluteUrl('/cpanel/cancelUser2Email', array('email' => $user2email->user_email, 'key' => $user2email->delete_key));

    // always start with views folder
    $return['content'] = HUB::renderPartial('/emails/_user_linkUserEmail', $params, true);

    return $return;
}
```

> We use `HUB::renderPartial` here but not `$this->renderPartial` in order for it to work correctly in CLI environment too