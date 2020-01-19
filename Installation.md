> We are still in the process building a web interface installer to make installation easier for our users. Please stay tune with us.


### Manual Installation
1. Download the distributed zip file
    * PHP composer dependency packages are included
2. Setup required servers & services:
    1. Setup Ubuntu Server (18.04 & above)
    1. Setup Apache Web Server (Apache 2)
    1. Setup MySQL database server and load default data to it
    1. Setup AWS S3 storage. You need 2 buckets, one for secure protected storage and one for normal public storage
    1. Setup AWS ElasticSearch server. 
    1. Acquire Google API key
    1. Setup SMTP mail server. We recommend using transaction mail services from Mailchimp
    1. Setup Redis server for faster caching (optional) or use default file cache
    1. Setup Cloudflare (optional) to use its HTTPS service
3. Upload to web server and unzip
4. Modify configuration files under `protected/config`, all `*.dist.php` need to copy to `*.php` and has variable in it modified according to your environment
5. Make sure the following directory and all files in it are writable by server
    * `protected/runtime`
    * `protected/messages`
    * `protected/data`
    * `protected/modules`
    * `protected/overrides`
    * `public_html/assets`
    * `public_html/uploads`
6. Setup crons
7. Login to Backend
    * Update Settings