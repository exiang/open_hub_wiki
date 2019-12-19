As OpenHub is built on top of Yii Framework version 1, our developers have done their best to clearly and intuitively separate the various parts of the software on top of the framework structure.

Here is how the files are organized:

#### /public_html
  * `/css`: contains all application level CSS files that are not attached to themes
  * `/images`: contains all of OpenHub default images, icons and picture files – that is, those that do not belong to the theme. This is where you can find the country flags (`/flags`), languages (`/languages`) sub folders.
  * `/javascript`: contains all application level JavaScript files that are not attached to themes
  * `/static`: this is where you store your static media file like HTML, Video, etc.
  * `/uploads`: this is where the user uploaded files will be store at. Even you are using AWS S3 for storage (`/protected/config/params.php` parameter `storageMode`), this folder is use to generate temporary thumbnails of images before uploads to cloud. _Notes:_ It is distributed as `/uploads.dist` and should be rename to `/uploads`.
  * `/vendor`: contains all application level's vendor package, each in its own folder where inside are mostly are a mix of javascript, css, images.
  * `/themes`: contains all the currently-installed themes, each in its own folder.
  * `index.php`: entry point to the entire application. It is modified from the default Yii framework version.

#### /protected
  * `/commands`: each of the files is an executable command in CLI, which can be use by cron to perform scheduling task
  * `/components`: 
  * `/config`: contains all of OpenHub configuration files. Unless asked to, you should never edit them, as they are directly handled by OpenHub's installer and back office (#TODO).
  * `/controllers`: contains all the application level controller, as in Model-View-Controller (or MVC), the software architecture used by OpenHub. Each file controls a specific part of OpenHub.
  * `/data`: contains none code meta data files, likes web API definition in YAML file format and instructruction for Yee code generator in `table_name.build.php` format.
  * `/extensions`:
  * `/i18n`:
  * `/messages`: contains translation files
  * `/migrations`:
  * `/models`:
  * `/modules`:
  * `/runtime`:
  * `/tests`:
  * `/vendor`:
  * `/views`:
  * `/yeebase`:
  * `composer.json`:
  * `yiic`:
  * `yiic.bat`:
  * `yiic.php`:

#### /framework
#### /.vscode