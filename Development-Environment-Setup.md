To get started with OpenHub, you will need to setup a local development environment where you can modify code and test it live on browser.

## Requirements
### System
  * Apache 2.0 and above
  * PHP version: 7.2
    * [YAML](https://www.php.net/manual/en/book.yaml.php)
    * [CURL](https://www.php.net/manual/en/book.curl.php)
    * [GD]
    * [REDIS]
    * [ZLIB]
  * [MySQL 5.5 (must be able to support json query)]()
  * [AWS S3](https://aws.amazon.com/s3/)
  * [Elastic Search](https://www.elastic.co/)
  * [Redis Cache (optional, do NOT support cluster)](https://redis.io/)
  * [Neo4j](https://neo4j.com/)
  * Google API
  * [PHP Composer](https://getcomposer.org/)

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
To setup Docker on local development server, we highly recommend using docker than the manual way. Use docker from repo: [hub_docker](https://github.com/mymagic/hub_docker). Refer to the setup documents there

## Preparation
### Setup AWS S3 buckets
We needs 2 buckets here, one for public access and another to store secure files
#### Public Bucket
1. Create a bucket and name it `openhub-staging`
2. Make sure `Block all public access` is `off`
3. Go to `Properties` and `Static Website Hosting`, select `Use this bucket to host a website`
3. Here is the bucket policy for your reference:
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AddPerm",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::openhub-staging/*"
        }
    ]
}
```
#### Secure Bucket
1. Create a bucket and name it `openhub-staging-secure`
2. Make sure `Block all public access` is `on`
### Setup AWS Elastic Cache - REDIS
### Setup AWS Elastic Search
### Setup Neo4J
### Setup Mandrill
### Get a Google API Account
### Get an Open Exchange Rate Account
### Get a MaGIC Connect Account

## Application Setups
1. Make sure repo is loaded correctly.
Open Hub used submodule at `protected/yeebase` and `public_html/themes`, make sure codes are loaded properly too.

``` git submodule update --init --recursive --force```
2. Create all the required dist folders:
  * `protected/messages`
  * `protected/vendor`
  * `protected/overrides`
  * `protected/runtime`
  * `public_html/assets`
  * `public_html/uploads`

3. Update composer
```bash
cd ~/docker/magic_hub_docker
./bash.sh
cd /var/www/open_hub/protected
composer update
```
4. Complete `protected/.env` files 
5. Run message command 
```bash 
php yiic message config/message.php 
```
6. Test from url `https://openhub.mymagic.my/test`