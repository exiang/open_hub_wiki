`protected/overrides` directory allow you to override the core code without touching the original files. This is handy where changes you need to make is beyond a module capability.

Files that can be overridden are those located in:
* `protected/models*` and `protected/models/hub`
* `protected/components`
* `protected/extensions`
* `protected/helpers`

To understand why you not allowed to import files recursively including all the subdirectory, please refer to issue: [https://code.google.com/archive/p/yii/issues/1568](https://code.google.com/archive/p/yii/issues/1568) 

## Todo
However, you can't override a modules and controller now. We are working on this base on https://forum.yiiframework.com/t/controller-override/48217