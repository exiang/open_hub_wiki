> We are still in the process building a web interface installer to make installation easier for our users. Please stay tune with us.

### Manual Installation
1. Download the distributed zip file
    * PHP composer dependency packages are included
2. Setup required servers & services:
    1. Setup Ubuntu Server (18.04 & above)
    1. Setup Apache Web Server (Apache 2)
    1. Setup relational database server (MariaDB or MySQL) and load default SQL to it
    1. Setup graph database server (Neo4j)
    1. Setup AWS S3 storage. You need 2 buckets, one for secure protected storage and one for public accessible storage
    1. Setup AWS ElasticSearch server. 
    1. Acquire Google API key
    1. Setup SMTP mail server. We recommend using transaction mail services from Mailchimp
    1. Setup Redis server for faster caching (optional) or use default file cache
    1. Setup Cloudflare (optional) to use its HTTPS service
3. Upload to web server and unzip
4. Copy `protected/dist.env` to `protected/.env`, and has variable in it modified according to your environment
5. Make sure the following directory and all files in it are writable by server
    * `protected/vendor`
    * `protected/runtime`
    * `protected/messages`
    * `protected/data`
    * `protected/modules`
    * `protected/overrides`
    * `public_html/assets`
    * `public_html/uploads
6. Update composer
```bash
cd ~/docker/magic_hub_docker
./bash.sh
cd /var/www/open_hub/protected
composer update
```
7. Run message command 
```bash 
php yiic message config/message.php 
```
8. Setup crons
9. Login to Backend
    * Update Settings
