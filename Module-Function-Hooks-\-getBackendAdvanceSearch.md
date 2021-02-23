
```php
public function getBackendAdvanceSearch($controller, $searchFormModel){}
```
You may like to list module records in backend search when admin search for a keyword. 



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
