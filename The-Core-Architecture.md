As OpenHub is built on top of Yii Framework version 1, our developers have done their best to clearly and intuitively separate the various parts of the software on top of the framework structure.

Here is how the files are organized:

#### /public_html
  * `/css`: contains all application level CSS files that are not attached to themes
  * `/images`: contains all of OpenHub default images, icons and picture files – that is, those that do not belong to the theme. This is where you can find the country flags (`/flags`), language icons (`/languages`) sub folders.
  * `/javascript`: contains all application level JavaScript files that are not attached to themes
  * `/static`: this is where you store your static media file like HTML, Video, etc.
  * `/uploads`: this is where the user uploaded files will be store at. Even you are using AWS S3 for storage (`/protected/config/params.php` parameter `storageMode`), this folder is use to generate temporary thumbnails of images before uploads to cloud. _Notes:_ It is distributed as `/uploads.dist` and should be rename to `/uploads` during setup.
  * `/vendor`: contains all application level's vendor package, each in its own folder where inside are mix of javascript, css & images files.
  * `/themes`: contains all the currently-installed themes, each in its own folder.
  * `index.php`: entry point to the entire application. It is modified from the default Yii framework version.

#### /protected
  * `/commands`: contains all console application, each of the files is an executable command in CLI, which can be use by cron to perform scheduling task
  * `/components`: contains components (e.g. helpers, widgets) that are only used by this application. One important file here is `Controller.php`
  * `/config`: contains all of OpenHub configuration files. Unless asked to, you should never edit them, as they are directly handled by OpenHub's installer and back office (#TODO).
  * `/controllers`: contains all the application level controller, as in Model-View-**Controller** (or MVC), the software architecture used by OpenHub. Each file controls a specific part of OpenHub.
  * `/data`: contains none code meta data files, likes web API definition in YAML file format and instructruction for Yee code generator in `table_name.build.php` format.
  * `/extensions`: contains third-party libraries that are only used by this application. Extensions is one of the way to extend Yii Framework functionality.
  * `/i18n`: contains the customized locale data which is not the translation message
  * `/messages`: contains translation message files
  * `/migrations`: contains database migration instructions created thru `yiic migrate create migration_instruction_name`
  * `/models`: contains model classes that are specific for the application, as in **Model**-View-Controller (or MVC)
  * `/modules`: a module is a self-contained mini program that consists of models, views, controllers and other supporting components. In many aspects, a module resembles to an application who extends the functionality of OpenHub.
  * `/runtime`: stores dynamically generated files, likes cache and log
  * `/tests`: contains all test related code 
  * `/vendor`: composer installed package are stored here
  * `/views`: stores controller actions view scripts, as in Model-**View**-Controller (or MVC)
  * `/yeebase`: OpenHub sit on top of an extended version of Yii Framework version 1. This is where the extended code sit in and shared across other applications.

#### /framework
#### /.vscode