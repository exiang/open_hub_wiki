```php
public function getBackendAdvanceSearch($controller, $searchFormModel){}
```
You may like to list module records in backend search when admin search for a keyword. 

<img width="1263" alt="Screenshot 2021-02-24 at 2 51 34 AM" src="https://user-images.githubusercontent.com/5336690/108892791-43613180-764b-11eb-8414-ad32da171376.png">


```php
public function getBackendAdvanceSearch($controller, $searchFormModel)
{
    $searchModel = new BoilerplateModel('search');
    $result['boilerplateStart'] = $searchModel->searchAdvance($searchFormModel->keyword);

    return array(
        'boilerplateStart' => array(
            'tabLabel' => Yii::t('backend', 'Boilerplate'),
            'itemViewPath' => 'application.modules.boilerplateStart.views.backend._view-boilerplateStart-advanceSearch',
            'result' => $result['boilerplateStart'],
        ),
    );
}
```

### Modify model to support searchAdvance()
> :information_source: This required modification to your model to implement `searchAdvance()` function

For example, edit `/protected/modules/sample/models/Sample.php`.

Add `searchAdvance()` function:
```php
public function searchAdvance($keyword)
{
    $this->unsetAttributes();

    $this->title = $keyword;
    $this->text_short_description = $keyword;
    // others table column to search base on this keyword

    $tmp = $this->search(array('compareOperator' => 'OR'));
    $tmp->sort->defaultOrder = 't.is_active DESC, t.date_added DESC';

    return $tmp;
}
```

### Implement Partial View
You may implement your advanceSearch partial view `/protected/modules/boilerplateStart/views/backend/_view-boilerplateStart-advanceSearch.php` using the following code as template:

```php
<div class="viewCard col-sm-6 item-flex">
    <div class="media margin-md">
    <div class="media-left">
       Logo Image
    </div>
    <div class="media-body">
        <a href="<?php echo $this->createUrl('/boilerplateStart/sample/view', array('id' => $data->id)); ?>">
        <h4 class="media-heading">
            Title
            <small><span class="text-muted">Sub Title</span></small>
        </h4></a>
        <div>Short description</div>
    </div>
    </div>
</div>
```
