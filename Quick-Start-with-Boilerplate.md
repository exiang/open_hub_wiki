Modules must follow some guidelines to work on OpenHub. If you want to get started quickly, with a standard module templates, you can use the `Boilerplate` Module.

### Boilerplate Module
Please refer to `protected/modules/boilerplateStart` that serve as a starting base for your new created module.
Its core file is at `protected/modules/boilerplateStartModule/BoilerplateStartModule.php`. All functions are equipped with self describe comment. These functions allow your module to inject code or interface into existing model, layout base on predefined design. 

### My first module
Let’s start; this will enable us to better describe its structure. We will name it "My Plugin".

#### Creating module thru CLI
```
cd protected
php yiic boilerplate create --name="My Plugin"
```

When successful, you will get this ouput message:
`New directory created at: /Users/yeesiang/docker/magic_hub_docker/magic_hub/protected/modules/myPlugin`

#### Creating module manually
1. First, copy the `boilerplateStart` folder located in the module's folder at `/protected/modules` and rename it to your new module. It should have the same name as the module, with no space, no hyphen and underscore, only alphanumerical characters, all in lower camel case format: `myPlugin`.
1. This folder must contain the main file, a PHP file of the same name with postfix of Module as the folder, which will handle most of the processing: MyPluginModule.php. Rename it from `BoilerplateStartModule.php`. Notice the filename is in upper camel case format.
1. Next, you need to rename everything of `boilerplateStart` to `myPlugin`, and `BoilerplateStart` to `MyPlugin` (in case sensitive manner). This including the filename as well as content in those files.
1. Modify `/protected/modules/boiletplateStart/config/about.yaml`:
    * change code from `boilerplatStart` to `myPlugin`
    * change title to 'My Plugin'
    * change oneliner to 'My first plugin module for OpenHub'

Once completed, your new module is now ready to be install. That is enough for a basic module but obviously more files and folders can be added later.

### Install a module
  * Login to backend
  * Click Site -> System -> Module
  * Click `New Module` tab, your new module is in the list
  * Click the `initialize` button
  * Once installation completed, your module is now moved to `Module` tab