OpenHub’s extensibility revolves around modules, which are small programs that make use of OpenHub’s functionalities and changes them or add to them in order to make OpenHub’s easier to use or more tailored to the provider’s needs.

## Introduction
A OpenHub module consists of a main PHP file with as many other PHP files as needed, as well as the necessary assets (images, JavaScript, CSS, etc.) to display the module’s interface, whether to the user (on the front office) or to the provider (on the back office).

Any OpenHub module, once installed on an online system, can interact with one or more “hooks”. Hooks enable you to hook/attach your code to the current View at the time of the code parsing (i.e., when displaying the an organization etc). Specifically, a hook is a shortcut to the various methods available from the Module object, as assigned to that hook.

## Modules’ operating principles
Modules are the ideal way to let your talent and imagination as a developer express themselves, as the creative possibilities are many and you can do pretty much anything with OpenHub’s module API.

Any module:
  * can display a variety of content (blocks, text, etc.), perform many tasks (cron, batch update, import, export, etc.), interface with other tools, and much much more.
  * can be made as configurable as necessary; the more configurable it is, the easier it will be to use, and thus will be able to address the needs of a wider range of users.
  * can add functionalities to OpenHub without having to edit its core files, thus making it easier to perform an update of OpenHub without having the transpose all core changes. Indeed, you should always strive to stay away from core files when building a module, even though this may seem necessary in some situations.
  * can use functions of other modules or share its own functionality to others
  * can be seen as one or part of a service; however, not all modules are services as some do not have frontend interface for user to interact with.

## Modules folder
OpenHub modules are found in the `protected/modules` folder. This is true for both default modules (provided with OpenHub) and 3rd-party modules that are subsequently installed.

## Quick start
Modules must follow some guidelines to work on OpenHub.
If you want to get started quickly, with a standard module templates, you can use the `Boilerplate` Module.

### Boilerplate Module
Please refer to `protected/modules/boilerplateStart` that serve as a starting base for your new created module.
Its core file is at `protected/modules/boilerplateStartModule/BoilerplateStartModule.php`. All functions are equipped with self describe comment. These functions allow your module to inject code or interface into existing model, layout base on predefined design. 

### My first module
Let’s start; this will enable us to better describe its structure. We will name it "My Plugin".

1. First, copy the `boilerplateStart` folder located in the module's folder at `/protected/modules` and rename it to your new module. It should have the same name as the module, with no space, no hyphen and underscore, only alphanumerical characters, all in lower camel case format: `myPlugin`.
1. This folder must contain the main file, a PHP file of the same name with postfix of Module as the folder, which will handle most of the processing: MyPluginModule.php. Rename it from `BoilerplateStartModule.php`. Notice the filename is in upper camel case format.
1. Next, you need to rename everything of `boilerplateStart` to `myPlugin`, and `BoilerplateStart` to `MyPlugin` (in case sensitive manner). This including the filename as well as content in those files.
1. Modify `/protected/modules/boiletplateStart/config/about.yaml`:
    * change code from `boilerplatStart` to `myPlugin`
    * change title to 'My Plugin'
    * change oneliner to 'My first plugin module for OpenHub'
5. Once completed, your new module is now ready to be install. That is enough for a basic module but obviously more files and folders can be added later.

### Install a module
  * Login to backend
  * Click Site -> System -> Module
  * Click `New Module` tab, your new module is in the list
  * Click the `initialize` button
  * Once installation completed, your module is now moved to `Module` tab

## Supported Features
The following are features in Yii that is supported by OpenHub:

  * Commands - `protected/yiic.php` has been modified to read all commands action from `protected/modules/YOUR_MODULE/commands` folder
  * Config - `protected/config/main.php` has been modified to include `protected/config/module.php` that read all configuration from `protected/modules/YOUR_MODULE/config` folder. Same goes to the  `protected/config/console.php`
  * Components
  * Controllers
  * Models
  * Views
  * API - Wapi support modularization architecture and able to read `protected/modules/YOUR_MODULE/data/api/*.yaml` for swagger definition as well as `protected/modules/wapi/V1Controller.php` has been modified to auto load `protected/modules/YOUR_MODULE/actions/wapi/V1Controller/*.php` for api action



## Todo
Modularization to the underlying system are done phase by phase. The following features are not supported by module yet:

* Migrate - Migration code only works from `protected/migrations` folder
* Test - There’s no modularize unittest yet
* Messages - Multilingual are centralized at `protected/messages/LANGUAGE_CODE/*.php`. It’s recommended to prefix your languages with module name, eg: Yii::t(‘MODULE_NAME’, ‘Hello World’)
* Data for Yee - Yee do not read data definition for model generation from your module. The generated model has to be stored in `protected/models` if to use the compare diff feature, as Yee are not able to generate model at module level