To get started with OpenHub development, you will need to setup a local development environment where you can modify code and test it live on browser easily.

## Environment
  * Dev - your local machine will be use for the development (e.g. https://hubd.mymagic.my)
  * Staging - this is the staging site for beta testing (e.g. https://hub.mymagic.my)
  * Production - this is your live site (e.g. https://central.mymagic.my)

## Setup local Docker for development
We highly recommend using this over the manual method. Use docker from repo: [hub_docker](https://github.com/mymagic/hub_docker). Refer to the setup documents there

## Setup Remote Staging
Edit `sudo nano /home/ubuntu/repo/web-main.git/hooks/post-receive` and insert:

```
rm -rf /var/www/public_html/assets/*
cd /var/www/protected
sudo mkdir /var/www/protected/vendor
sudo chmod -R 777 /var/www/protected/vendor
sudo chown -R ubuntu:www-data /var/www/protected/vendor
sudo composer update
cd /var/www/
sudo git --git-dir=/home/ubuntu/repo/web-main.git --work-tree=/var/www submodule update --init --recursive --force
sudo mkdir /var/www/protected/messages
sudo chmod -R 777 /var/www/protected/messages
sudo chown -R ubuntu:www-data /var/www/protected/messages
sudo mkdir /var/www/public_html/uploads
sudo chmod -R 777 /var/www/public_html/uploads
sudo chown -R ubuntu:www-data /var/www/public_html/uploads
sudo mkdir /var/www/protected/runtime
sudo chmod -R 777 /var/www/protected/runtime
sudo chown -R ubuntu:www-data /var/www/protected/runtime
sudo mkdir /var/www/public_html/assets
sudo chmod -R 777 /var/www/public_html/assets
sudo chown -R ubuntu:www-data /var/www/public_html/assets
sudo mkdir /var/www/protected/data
sudo chmod -R 777 /var/www/protected/data
sudo chown -R ubuntu:www-data /var/www/protected/data
sudo mkdir /var/www/protected/modules
sudo chmod -R 777 /var/www/protected/modules
sudo chown -R ubuntu:www-data /var/www/protected/modules
cd /var/www/protected
php yiic migrate --interactive=0
php yiic message config/message.php
sudo chmod u+x /var/www/_cron -R
```

Script above basically do the following task for you automatically:

1. Reset and clear assets

2. Make sure repo is loaded correctly.
Open Hub used submodule at `protected/yeebase` and `public_html/themes`, make sure codes are loaded properly too.

``` git submodule update --init --recursive --force```

3. Update composer

4. Create all the required dist folders and make sure they all have write permission:
  * `protected/messages`
  * `protected/vendor`
  * `protected/overrides`
  * `protected/runtime`
  * `public_html/assets`
  * `public_html/uploads`

5. Run Yii migration to update changes

Last, Complete the `protected/.env` files 
