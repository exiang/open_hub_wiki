OpenHub’s extensibility revolves around modules, which are small programs that make use of OpenHub’s functionalities and changes them or add to them in order to make OpenHub’s easier to use or more tailored to the provider’s needs.

* [Modules Architecture](Modules-Architecture)
* [Quick Start with Boilerplate](Quick-Start-with-Boilerplate)
* [Inject Functions to Core Models](Inject-Functions-to-Core-Models)
* [Function Hooks](Module-Function-Hooks)
* [Module Install & Upgrade](Module-Install-&-Upgrade)
* [Module Configuration](Module-Config)
* [Module Layout](Module-Layout)
* [Module Web Assets](Module-Web-Assets)
* [Module Web API](Module-Web-API)

### Tutorial
* [Step by Step Guide building a Todo Module](Step-by-step-Todo-module)

### Notes
Modularization to the underlying system are done phase by phase. The following features are not supported by module yet:

  * Test - There’s no modularize unit test yet
  * Messages - Multilingual are centralized at protected/messages/LANGUAGE_CODE/*.php. It’s recommended to prefix your languages with module name, eg: Yii::t(‘MODULE_NAME’, ‘Hello World’)
  * Data for Yee - Yee code generator do not read data definition for model generation from your module. The generated model has to be stored in protected/models if to use the compare diff feature, as Yee are not able to generate model at module level