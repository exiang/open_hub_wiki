A OpenHub module consists of a main PHP file with as many other PHP files as needed, as well as the necessary assets (images, JavaScript, CSS, etc.) to display the module’s interface, whether to the user (on the front office) or to the provider (on the back office).

Any OpenHub module, once installed on an online system, can interact with one or more "hooks". Hooks enable you to hook/attach your code to the current View at the time of the code parsing (i.e., when displaying the an organization etc). Specifically, a hook is a shortcut to the various methods available from the Module object, as assigned to that hook.

As a module developer, you make changes within the context of module. You must not modify the core files. 

## Modules’ operating principles
Modules are the ideal way to let your talent and imagination as a developer express themselves, as the creative possibilities are many and you can do pretty much anything with OpenHub’s module API.

Any module:
  * can display a variety of content (blocks, text, etc.), perform many tasks (cron, batch update, import, export, etc.), interface with other tools, and much much more.
  * can be made as configurable as necessary; the more configurable it is, the easier it will be to use, and thus will be able to address the needs of a wider range of users.
  * can add functionalities to OpenHub without having to edit its core files, thus making it easier to perform an update of OpenHub without having the transpose all core changes. Indeed, you should always strive to stay away from core files when building a module, even though this may seem necessary in some situations.
  * can use functions of other modules or share its own functionality to others
  * can be seen as one or part of a service; however, not all modules are services as some do not have frontend interface for user to interact with.

## Different type of modules
* 3rd-party Module
* [Default Module](Default-Modules)
* [YeeBase Module](YeeBase-Modules)

## Modules folder
OpenHub modules are found in the `protected/modules` folder. This is true for both default modules (provided with OpenHub) and 3rd-party modules that are subsequently installed.

### Directory Structure
Inside each module, files are organized as:

  * Commands - `protected/yiic.php` has been modified to read all commands action from `protected/modules/YOUR_MODULE/commands` folder
  * Config - `protected/config/main.php` has been modified to include `protected/config/module.php` that read all configuration from `protected/modules/YOUR_MODULE/config` folder. Same goes to the  `protected/config/console.php`
  * Components
  * Controllers
  * Models - Neo4J OGM stores under `protected/modules/YOUR_MODULE/models/neo4j`
  * Views

## Helpful Functions
#### YeeModule::getCoreModules()
Return a list of system defined core modules' code

#### YeeModule::getNewModules()
Return a list of new modules' code, including those who are not installed yet

#### YeeModule::getAllModules()
Just a short hand of `return Yii::app()->modules;`

#### YeeModule::getActiveParsableModules()
Return list of activated parsable modules
```php
Array
(
    [collection] => Array
        (
            [var1] => 
            [var2] => 
            [modelBehaviors] => Array
                (
                    [Organization] => Array
                        (
                            [class] => application.modules.collection.components.CollectionOrganizationBehavior
                        )

                    [User] => Array
                        (
                            [class] => application.modules.collection.components.CollectionUserBehavior
                        )

                )

            [class] => collection.CollectionModule
        )

    [openHub] => Array
        (
            [var1] => 
            [var2] => 
            [githubOrganization] => mymagic
            [githubRepoName] => open_hub
            [githubReleaseUrl] => https://openhub-main.s3-ap-southeast-1.amazonaws.com/github
            [isMockUpgrade] => 1
            [modelBehaviors] => Array
                (
                )

            [class] => openHub.OpenHubModule
        )
...
```