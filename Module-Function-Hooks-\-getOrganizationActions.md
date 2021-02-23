```php
public function getOrganizationActions($model, $realm = 'backend'){}
```
This function hook allows you to define list of action buttons in injected interface block to view organization page in both cpanel and backend for your module (thru `getOrganizationViewTabs()`).

```php
if ($realm == 'backend') {
    if (!Yii::app()->user->accessBackend) {
        return;
    }

    $actions['boilerplatStart'][] = array(
        'visual' => 'primary',
        'label' => 'Backend Action 1',
        'title' => 'Backend Action 1 short description',
        'url' => Yii::app()->controller->createUrl('/boilerplatStart/backend/action1', array('id' => $model->id)),
    );
} elseif ($realm == 'cpanel') {
    $actions['boilerplatStart'][] = array(
        'visual' => 'primary',
        'label' => 'Frontend Action 1',
        'title' => 'Frontend Action 1 short description',
        'url' => Yii::app()->controller->createUrl('/boilerplatStart/frontend/action1', array('id' => $model->id)),
    );
}

return $actions;
```

Next, not to forget you will also has to implement the rendering part of these action buttons in your view.

```php
<?php if(!empty($actions['boilerplatStart'])): ?>
<div class="row">
    <div class="col col-md-12">
    <div class="well text-center">
    <?php foreach($actions['boilerplatStart'] as $action): ?>
        <a class="margin-bottom-sm btn btn-<?php echo $action['visual']?>" href="<?php echo $action['url'] ?>" title="<?php echo $action['title'] ?>"><?php echo $action['label'] ?></a>
    <?php endforeach; ?>
    </div>
    </div>
</div>
<?php endif; ?>
```
_Screenshot of how your module injected action buttons will display in view organization page_:

![image](https://user-images.githubusercontent.com/5336690/71715673-3254d000-2e4d-11ea-947d-7c9975e3e7a8.png)
