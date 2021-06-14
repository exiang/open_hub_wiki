## Modal Common
This is a bootstrap modal ready for use in UI. Below is an example where by clicking a button will trigger ajax load view into the modal common.

Button to trigger:
```html
<a data-load-url="<?php echo $this->createUrl('cpanel/createExperience') ?>" id="btn-cv-addNewExperience" class="btn btn-primary btn-sm pull-right"><?php echo Html::faIcon('fa-plus') ?> <?php echo Yii::t('cv', 'Add New') ?></a>
```

To show:
```php
<?php Yii::app()->clientScript->registerScript(
'cv-cpanel-experience',
<<<EOD

$('body').on( "click", '#btn-cv-addNewExperience', function(e) {
    var url2Load = $(this).data('loadUrl');
    $('#modal-common').html('<div class="text-center text-white margin-top-3x">'+$('#block-spinner').html()+'</div>').load(url2Load, function(response, status, xhr){}).modal('show');
});				

EOD
); ?>
```

To hide:
```js
$('#modal-common').html('').modal('hide');
$('body').removeClass('modal-open');
$('.modal-backdrop').remove();
```

## Date Time picker
```php
$form->bsDateTextField($model, 'dateStart', array('options' => array(
    'dateFormat' => 'yy-mm-dd',
    'changeMonth' => 'true',
    'changeYear' => 'true',
    'yearRange' => sprintf('1920:%s', date('Y')),
    'constrainInput' => 'true',
    'timeInput' => false,
    'showTime' => false,
    'showHour' => false,
    'showMinute' => false,
    'showTimepicker' => false,
)))
```

```php
<?php $this->widget('application.yeebase.extensions.CJuiDateTimePicker.CJuiDateTimePicker', array(
    'name' => 'dateStart',
    'value' => date('Y-m-d', strtotime('this week monday')),
    // additional javascript options for the date picker plugin
    'options' => array(
        'showAnim' => 'fold',
        'dateFormat' => 'yy-mm-dd',
        'changeMonth' => true,
        'changeYear' => true,
        'timeInput' => false,
        'showTime' => false,
        'showHour' => false,
        'showMinute' => false,
        'showTimepicker' => false,
    ),
    'htmlOptions' => array(
        'v-model' => 'dateStart'
    ),
)); ?>
```

## Html Editor
Html Editor is wrap around an Yii extension called `NHCKEditor` which is base on `CKEditor` (https://ckeditor.com/).

A mini editor with maximum 100 words

![Screenshot 2021-06-14 at 5 21 40 PM](https://user-images.githubusercontent.com/5336690/121869834-05d8ff00-cd35-11eb-86a4-4299d79a6269.png)

```php
<?php echo $form->bsHtmlMiniEditor($model, 'html_solution', array('wordcount' => array('maxWordCount' => 100))); ?>
```

A standard editor
```php
<?php echo $form->bsHtmlEditor($model['form'], 'htmlContent', array('someConfiguration' => 'Foo Bar')); ?>
```

### Notes:
As the Html Editor can take a long time to load, it is wise to disable the submit button before it is fully loaded.

```js
<?php Yii::app()->clientScript->registerScript(
'job-vacancy-process',
<<<EOD
CKEDITOR.on('instanceReady', function(){
	$('#btn-submit').removeClass('disabled');
})
EOD
); ?>
```


## Html::activeThumb()
This is a helper function to display thumbnail image for your active model in yee. 

```php <?php echo Html::activeThumb($data, 'image_cover') ?>```

<img width="423" alt="Screenshot 2021-02-24 at 2 59 47 AM" src="https://user-images.githubusercontent.com/5336690/108894286-0c8c1b00-764d-11eb-942a-7fb20ac2dad7.png">

By default, activeThumb used `adaptiveResize` method to generate the thumbnail, which respect the original image ratio and try to show the entire image for completeness. The only drawback is this come with the TV box padding effect.

### Resize method
```php <?php echo Html::activeThumb($data, 'image_cover', array('method' => 'resize')) ?>```

<img width="441" alt="Screenshot 2021-02-24 at 2 57 47 AM" src="https://user-images.githubusercontent.com/5336690/108894279-0ac25780-764d-11eb-8a41-a096564d9106.png">

`resize` method will scale the image to fill up the output size, ignoring the ratio. 

### How it works
By default, recommended setting of OpenHub is to pre-generate thumbnail images. The alternative is to generate them on the fly.

Thumbnails are pre-generated the moment user uploads images thru form, then upload and save to storage (AWS S3).

`config/thumbnail.php` setting information feeds the application required information to generate into series of designated size thumbnail images.
```php
return array(
	'challenge' => array(
		'cover' => array('32x32', '80x80', '320x320'),
		'header' => array('32x32', '80x80', '320x320'),
	)
);

```

You may pass in the `width` and `height` parameter and `Html::ativeThumb` is smart enough to auto-match with the closest pre-generated thumbnails.
For example, code below will use the 320x320 thumbnail: 

```php <?php echo Html::activeThumb($data, 'image_cover', array('width'=>300, 'height'=>'200)) ?>```
