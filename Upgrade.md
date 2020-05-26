## Method 1: Thru Web Backend
## Method 2: Thru OpenHub Command
```
cd /var/www/protected
php yiic openhub upgrade
```

use parameter `--force=1` to force upgrade when latest release version is lower than current installation. Use this carefully as you are command for a downgrade.

## Method 3: Manually
SSH into your web server:

1. Download latest release package, unzip and replace files

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
