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

## Html::activeThumb()
This is a helper function to display thumbnail image for your active model in yee. 

```php <?php echo Html::activeThumb($data, 'image_cover') ?>```

<img width="423" alt="Screenshot 2021-02-24 at 2 59 47 AM" src="https://user-images.githubusercontent.com/5336690/108894286-0c8c1b00-764d-11eb-942a-7fb20ac2dad7.png">

By default, activeThumb used `adaptiveResize` method to generate the thumbnail, which respect the original image ratio and try to show the entire image for completeness. The only drawback is this come with the TV box padding effect.

### Resize method
```php <?php echo Html::activeThumb($data, 'image_cover', array('method' => 'resize')) ?>```

<img width="441" alt="Screenshot 2021-02-24 at 2 57 47 AM" src="https://user-images.githubusercontent.com/5336690/108894279-0ac25780-764d-11eb-8a41-a096564d9106.png">

`resize` method will scale the image to fill up the output size, ignoring the ratio. 