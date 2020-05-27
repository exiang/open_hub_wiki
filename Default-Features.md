## Setting

## Embed
Think of Embed as snippet of content to include in your website which is i18n sensitive. A standard snippet has the following structure:

* Title
* Description Text
* HTML Content
* Image

#### Create/Update an embed:
```php
$embed = Embed::setEmbed('cpanel-deactivateAccountMessage', array(
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


#### Delete an embed:
```php
$success = Embed::deleteEmbed('cpanel-deactivateAccountMessage')
```