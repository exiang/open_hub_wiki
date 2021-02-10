## How it works
Developer should not concern how user will get a notification message. It is handled by system automatically. Email are the default channel for sending a message; where user with verified mobile number will get additional SMS notification; same concept applied to push notification channel then.

Outgoing message will also be recorded in database under `notify` table for references.

## sendNotify
You can send a notification by calling `HUB::sendNotify`.

```php 
sendNotify($userType, $username, $message, $title = '', $content = '', $priority = 3, $options = null) 
```
* `$userType`: supported receiver user type can be either `member`, `admin` or `organization`
* `$username`: `username` (email address) for member and admin; or `code` for organization
* `$message`: Required field, either a string or a json payload, use for short messaging channel like SMS and Push notification (where `$content` will not be use)
* `$title`: a string, mainly use for channel like Email
* `$content`: a long string, HTML is supported and will be automatically adapt to plain text for alternative, mainly use for channel like Email
* `$priority`: default to 3, 1 is least important, 5 is most important
* `$options`: additional parameters can be pass in to control for each channel, like setting a CC in email for examaple

```php
$notifyMaker = NotifyMaker::member_user_linkUserEmail($user, $user2email);
$options['email']['receivers'][] = array('method' => 'cc', 'name' => $nameTeam, 'email' => $emailTeam);
HUB::sendNotify('member', $submission->user->username, $notifMaker['message'], $notifMaker['title'], $notifMaker['content'], 3, $options);
```

As you can see, compose of `title`, `message` and `content` is the job of `NotifyMaker` function and it follows the format of: `NotifyMaker::receiverUserType_module_verbNouns`

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

The purpose of `NotifyMaker` is to keep all messaging compose in one single place for ease of management. However, base on situation, you may go for the shortcut:

```php
$notifMaker['title'] = Yii::t('notify', 'Please verify this email address');
$notifMaker['message'] = Yii::t('notify', 'Please verify this email address.');

$params['email'] = $user2email->user_email;
$params['username'] = $user->username;
$params['urlVerify'] = Yii::app()->createAbsoluteUrl('/cpanel/verifyUser2Email', array('email' => $user2email->user_email, 'key' => $user2email->verification_key));
$params['urlDelete'] = Yii::app()->createAbsoluteUrl('/cpanel/cancelUser2Email', array('email' => $user2email->user_email, 'key' => $user2email->delete_key));

// always start with views folder
$notifMaker['content'] = HUB::renderPartial('/emails/_user_linkUserEmail', $params, true);

$options['email']['receivers'][] = array('method' => 'cc', 'name' => $nameTeam, 'email' => $emailTeam);
HUB::sendNotify('member', $submission->user->username, $notifMaker['message'], $notifMaker['title'], $notifMaker['content'], 3, $options);
```