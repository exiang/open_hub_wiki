## Method 1: Web Backend
After login to backend dashboard, OpenHub module automatically notify you on available upgrade.
![](https://user-images.githubusercontent.com/5336690/82883167-aa46a280-9f74-11ea-8ff4-4a05c7c72dee.png)

Todo:
- Upgrade page will show you changes of this version. Click confirm to proceed (one way ticket)
- Wait for the upgrade to complete

## Method 2: OpenHub Command
```
cd /var/www/protected
php yiic openhub upgrade
```

use parameter `--force=1` to force upgrade when latest release version is lower than current installation. Use this carefully as you are command for a downgrade.

## Method 3: Manually
SSH into your web server:

1. Download latest release package, unzip and replace files
PHP composer dependency packages and SQL database structure are included.
```
cd /home/ubuntu
wget https://openhub-main.s3-ap-southeast-1.amazonaws.com/github/release/openhub-latest.zip
```

When completed, unzip to `/var/www` (you may wanto make a backup before this):
```
unzip openhub-latest.zip -d /var/www
```

2. Apply migration update, including new database changes.
```
cd /var/www/protected
php yiic migrate up
```

3. Refresh translation messages, generate the message php file across the system.
```
cd /var/www/protected
php yiic message config/message.php
```

4. Check if there's any changes required at `protected/.env` file

5. Check all modules configs
