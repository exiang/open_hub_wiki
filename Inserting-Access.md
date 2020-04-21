### Insert Access
There are some ways to add access into the database
* through the backend menu: Site > Sys > Access/Route > Create Access
![](https://user-images.githubusercontent.com/55473894/79817944-2c154000-83b9-11ea-8bb2-3f403daf01b8.png)
![](https://user-images.githubusercontent.com/55473894/79821176-a8f7e800-83c0-11ea-831a-d4487ca20b41.png)
   > This list will populate the module found in the system. **Known issue** is, if in the module has BackendController, if will not populate action inside it but it will shown action in BackendController located in DefaultController. This issue can be solved by using a namespace that could differentiate the classes. Alternatively, you can use other ways to do it.


* through the backend menu: Site > Sys > Access/Route > Create Custom Access
![](https://user-images.githubusercontent.com/55473894/79819459-9c719080-83bc-11ea-9ed8-36c06de45ecd.png)
   > You need to add manually one by one or you can even create fake action


* through migration file
```php
public function up()
{
	$migration = Yii::app()->db->createCommand();

	$module = 'todo';

	// need to define controller & action to be insert into table access
	// depend on how many controller & action have in the module
	$arrControllerAction = [
		// 'controllerOne' => ['actionOne','actionTwo'],
		// 'controllerTwo' => ['actionOne']
	];

	foreach($arrControllerAction as $controller => $actions){
		foreach($actions as $action){
			$access = [$module, $controller, $action];
			$access = array_filter($access);
			$route = implode('/',$access);

			// need to check before insert in case using forceInstall
			$exist = Yii::app()->db->createCommand()
					->select('code')->from('access')
					->where('code=:code', [':code'=>$route])
					->queryRow();
			if($exist===false){
				$migration->insert('access', [
					'code' => $route,
					'title' => $route,
					'module' => $module,
					'controller' => $controller,
					'action' => $action,
					'is_active' => 1,
					'date_added' => time(),
					'date_modified' => time()
				]);
			}
		}
	}
}
```