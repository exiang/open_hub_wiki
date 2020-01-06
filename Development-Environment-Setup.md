To get started with OpenHub, you will need to setup a local development environment where you can modify code and test it live on browser.

## Requirements
### System
  * PHP version: 7.2
  * MySql 5.5 (must be able to support json query)
  * AWS S3
  * Elastic Search 
  * Elastic Cache (optional, do NOT support cluster)
  * Noe4j
  * Google API
  * PHP Composer

### Others Development Tools
  * Docker
  * OS - Apple Mac
  * IDE - [Visual Studio Code](https://code.visualstudio.com/)
  * GIT Client - [Fork](https://git-fork.com/)
  * API Gateway - [TYK.io](https://tyk.io/)

### Licenses
  * [Inspinia](https://wrapbootstrap.com/theme/inspinia-responsive-admin-theme-WB0R5L90S) - You will need to purchase Inspinia (single license at USD44) if you are using the default theme shipped with OpenHub

## Environment
  * Dev - your local machine will be use for the development (e.g. https://hubd.mymagic.my)
  * Staging - this is the staging site for beta testing (e.g. https://hub.mymagic.my)
  * Production - this is your live site (e.g. https://central.mymagic.my)

## Setup with Docker
To setup Docker on local development server, we highly recommend using docker than the manual way. Use docker from repo: magic_docker7. Refer to the setup documents there