### Insert Access
There are some ways to add access into the database
1.  through the backend menu: `Site > Sys > Access/Route > Create Access`
![](https://user-images.githubusercontent.com/55473894/79817944-2c154000-83b9-11ea-8bb2-3f403daf01b8.png)
![](https://user-images.githubusercontent.com/55473894/79821176-a8f7e800-83c0-11ea-831a-d4487ca20b41.png)
   > The `Action` will populated from the `Controller` based on the module selected

2.  through the backend menu: `Site > Sys > Access/Route > Create Custom Access`
![](https://user-images.githubusercontent.com/55473894/79819459-9c719080-83bc-11ea-9ed8-36c06de45ecd.png)
   > You may adding `Access` manually one by one or you can even create fake action

3.  alternatively, you might want to do it through migration file
```php
public function up()
{
	// Access::setAccessRole('module_name','controller_file',['action1','action2'],['role1','role2']);
	Access::setAccessRole('todo','TodoController',['index','admin'],['admin','developer']);
}
```

## Assign Access to Role
* after done the above step (#1 or #2) then you should go to `Site > Sys > Role > Manage Role` to assign the `Access` to the `Role`
![](https://user-images.githubusercontent.com/55473894/83234192-795bad00-a1c2-11ea-8b88-df24156151ef.png)

* choose & select the action/route you want grant access to that `Role`
![](https://user-images.githubusercontent.com/55473894/83234198-7c569d80-a1c2-11ea-90e4-5ef9322f1995.png)
