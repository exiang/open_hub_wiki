### Insert Access
There are some ways to add access into the database
* through the backend menu: `Site > Sys > Access/Route > Create Access`
![](https://user-images.githubusercontent.com/55473894/79817944-2c154000-83b9-11ea-8bb2-3f403daf01b8.png)
![](https://user-images.githubusercontent.com/55473894/79821176-a8f7e800-83c0-11ea-831a-d4487ca20b41.png)
   > This list will populate the module found in the system. **Known issue** is, if in the module has `BackendController`, if will not populate action inside it but it will shown action in `BackendController` located in `DefaultController`. This issue can be solved by using a namespace that could differentiate the classes. Alternatively, you can use other ways to do it.


* through the backend menu: `Site > Sys > Access/Route > Create Custom Access`
![](https://user-images.githubusercontent.com/55473894/79819459-9c719080-83bc-11ea-9ed8-36c06de45ecd.png)
   > You need to add manually one by one or you can even create fake action


* through migration file
```php
public function up()
{
	// Access::setAccessRole('module_name','controller_file',['action1','action2'],['role1','role2']);
	Access::setAccessRole('todo','TodoController',['index','admin'],['admin','developer']);
}
```