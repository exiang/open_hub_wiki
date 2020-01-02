Modularization to the underlying system are done phase by phase. The following features are not supported by module yet:

* Migrate - Migration code only works from `protected/migrations` folder
* Test - There’s no modularize unit test yet
* Messages - Multilingual are centralized at `protected/messages/LANGUAGE_CODE/*.php`. It’s recommended to prefix your languages with module name, eg: `Yii::t(‘MODULE_NAME’, ‘Hello World’)`
* Data for Yee - Yee code generator do not read data definition for model generation from your module. The generated model has to be stored in `protected/models` if to use the compare diff feature, as Yee are not able to generate model at module level