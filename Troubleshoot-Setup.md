### Upgrade Composer 1.x to 2.0
The older version of OpenHub used composer 1.x for its dependency on [Wikimedia](https://github.com/wikimedia/composer-merge-plugin). You may want to update to composer 2.0 as wikimedia start to support it. Here's how you can do it:

Edit `/protected/composer.json` to update wikimedia version `"wikimedia/composer-merge-plugin": "^2.0",`

In CLI

```
cd /var/www/protected
sudo composer update
sudo composer self-update --2
```

You may get runtime error 'SHA384 is not supported by your openssl extension, could not verify the phar file integrity'

```
sudo rm -f /usr/local/bin/composer
sudo curl -s https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
```

Now, do `composer update` again and should be working fine