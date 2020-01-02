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

## Modules folder
OpenHub modules are found in the `protected/modules` folder. This is true for both default modules (provided with OpenHub) and 3rd-party modules that are subsequently installed.