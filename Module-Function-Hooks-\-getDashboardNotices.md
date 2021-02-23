For both cpanel and backend, display flash message on top of page.
```php
public function getDashboardNotices($model, $realm = 'backend')
{
    if($realm == 'cpanel'){
        $notices[] = array('message' => 'Hello World', 'type' => Notice_WARNING);
        return $notices;
    }
}
```