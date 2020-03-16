## Date Time picker
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