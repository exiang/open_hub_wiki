## Setting

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