## Setting
Think setting like file config except it is store in database and can be easily update thru backend. 

#### Create/Update a setting:
> Use `Embed` if the value is i18n sensitive. e.g.: chunk of instruction text that need to be display in different language according to user selection

```php
$value = '1,2,3';
$dataType = 'array';
Setting::setSetting('moduleCode-settingVariableName', $value, $dataType, $dataTypeValue = '');
```
* will auto initialize module setting, auto create if not exists, else just ignore 
* code should come with prefix with moduleKey infront, eg: `boilerplateStart-var1`
* supported `dataType`: `image`, `integer`, `boolean`, `float`, `string`, `text`, `html`, `array`, `enum`, `date`, `file`

#### Retrieve a setting value
```php
Setting::code2value('moduleCode-settingVariableName', $defaultValue = null);
```
* pass in `defaultValue` to return as default value if setting record not found

## Embed
Think of Embed as snippet of content to include in your website which is i18n sensitive. A standard snippet has the following structure:

* Title (`title_en`, `title_ms`...)
* Description Text (`text_description_en`, `text_description_ms`...)
* HTML Content (`html_content_en`, `html_content_ms`...)
* Image (`image_main_en`, `image_main_ms`...)

#### Create/Update an embed:
```php
$embed = Embed::setEmbed('cpanel-deleteAccountMessage', array(
    'is_title_enabled' => true,
    'is_text_description_enabled' => false,
    'is_html_content_enabled' => true,
    'is_image_main_enabled' => false,
    'is_default' => true,
    'title_en' => 'Deactivate Account',
    'html_content_en' => '<b>Hello</b> World',
));
```

Then, you can check is the embed successfully saved or not using:
```
$embed->validate();
print_r($embed->getErrors());
```

#### Using an embed in view:
> Notice that you do not need to insert the specific language suffix as it has already taken care of internally

Display the title:
```php
<?php echo Embed::code2value('cpanel-deleteAccountMessage', 'title') ?>
```
Display the Html Content:
```php
<?php echo Embed::code2value('cpanel-deleteAccountMessage', 'html_content') ?>
```

Getting the entire object can be done without specified any attribute name:
```php
<?php echo Embed::code2value('cpanel-deleteAccountMessage') ?>
```

When a code not found, `Embed::code2value()` will return empty string '' if the `attribute` is specified; or null if `attribute` is not specified. If you need it to throw exception instead of this default mute behaviour, set the 3rd parameter `exceptionIfNotFound` to `true`:

```php
<?php echo Embed::code2value('cpanel-deleteAccountMessage', 'html_content', true) ?>
<?php echo Embed::code2value('cpanel-deleteAccountMessage', '', true) ?>
```

#### Caching
Do not panic if your update do not apply immediately. Request is cache for 30 seconds internally.

#### Delete an embed:
```php
$success = Embed::deleteEmbed('cpanel-deleteAccountMessage')
```