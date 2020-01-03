> We are still in the process building a web interface installer to make installation easier for our users. Please stay tune with us.


### Manual Installation
1. Download the distributed zip file
    * PHP composer dependency packages are included
2. Setup required servers & services:
    * Setup MySQL database server and load default data to it
    * Setup AWS S3 storage. You need 2 buckets, one for secure protected storage and one for normal public storage
    * Setup AWS ElasticSearch server. 
    * Acquire Google API key
    * Setup SMTP mail server. We recommend using transaction mail services from Mailchimp
    * Setup Redis server for faster caching (optional) or use default file cache
3. Upload to web server and unzip
4. Modify configuration files under `protected/config`, all `*.dist.php` need to copy to `*.php` and has variable in it modified according to your environment
5. Make sure the following directory are writable by server
    * `protected/runtime`
    * `public_html/assets`
    * `public_html/uploads`
6. Setup crons