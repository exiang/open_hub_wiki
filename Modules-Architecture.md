Modules Architecture
Module is the basic building block in Central. To achieve greater modularization, all your changes should be contained within your module. In order to do this, you need to understand Central modules architecture.

Module vs Service
All services are module, but not all modules are services.

Supported Features
The following are features in Yii that is supported by Central:

Commands - protected\yiic.php has been modified to read all commands action from modules\YOUR_MODULE\commands folder
Config - protected\config\main.php has been modified to include protected\config\module.php that read all configuration from modules\YOUR_MODULE\config folder. Same goes to the protected\config\console.php
Components
Controllers
Models
Views
API - Wapi support modularization architecture and able to read modules\YOUR_MODULE\data\api*.yaml for swagger definition as well as protected\modules\wapi\V1Controller.php has been modified to auto load modules\YOUR_MODULE\actions\wapi\V1Controller*.php for api action
Boilerplate Module
Please refer to protected\modules\boilerplateStart that serve as a starting base for your new created module.

Central Functions
Refer to protected\modules\boilerplateStartModule\BoilerplateStartModule.php. All functions are equipped with self describe comment.

These functions allow your module to inject code or interface into existing model, layout base on pre define design.

Todo
Modularization to the underlying system are done phase by phase. The following features are not supported by module yet:

Migrate - Migration code only works from protected\migrations folder
Test - There’s no modularize unittest yet
Messages - Multilingual are centralized at protected\messages\LANGUAGE_CODE*.php. It’s recommended to prefix your languages with module name, eg: Yii::t(‘MODULE_NAME’, ‘Hello World’)
Data for Yee - Yee do not read data definition for model generation from your module. The generated model has to be stored in protected/models if to use the compare diff feature, as Yee are not able to generate model at module level