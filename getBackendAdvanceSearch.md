
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

> :information_source: This required modification to your model to implement `searchAdvance()` function