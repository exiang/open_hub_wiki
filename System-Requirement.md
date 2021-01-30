### Server System
  * [Apache 2.0](https://httpd.apache.org/) and above
  * PHP version: 7.2
    * [YAML](https://www.php.net/manual/en/book.yaml.php)
    * [CURL](https://www.php.net/manual/en/book.curl.php)
    * [MBSTRING](https://www.php.net/manual/en/book.mbstring.php)
    * [GD](https://www.php.net/manual/en/book.image.php)
    * [ZLIB](https://www.php.net/manual/en/book.zlib.php)
    * [REDIS](https://github.com/phpredis/phpredis)
    * [BCMATH] (required by Neo4J Composer package)
  * [PHP Composer](https://getcomposer.org/)
  * [MariaDb](https://mariadb.org/) (must be able to support json query)
  * [AWS S3](https://aws.amazon.com/s3/)
  * [Elastic Search](https://www.elastic.co/)
  * [Redis Cache (optional, do NOT support cluster)](https://redis.io/)
  * [Neo4j](https://neo4j.com/)
  * [Google API](https://console.developers.google.com/)

### Others Development Tools
  * [Docker](https://www.docker.com/)
  * IDE - [Visual Studio Code](https://code.visualstudio.com/)
  * GIT Client - [Fork](https://git-fork.com/)
  * API Gateway - [TYK.io](https://tyk.io/)
  * OS - Apple Mac

### Licenses
  * [Inspinia](https://wrapbootstrap.com/theme/inspinia-responsive-admin-theme-WB0R5L90S) - You will need to purchase Inspinia (single license at USD44) if you are using the default theme shipped with OpenHub
  * [CKeditor](https://ckeditor.com/) - Optional, you will need to purchase this only if you are using the WYSIWYG HTML Editor with image that need to store at S3.

### Notes
  * PHP mbstring module is required to store json_x data ensuring array is in utf8 format