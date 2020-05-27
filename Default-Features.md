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

#### Delete an embed:
```php
$success = Embed::deleteEmbed('cpanel-deleteAccountMessage')
```