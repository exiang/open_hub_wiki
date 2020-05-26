## Manually
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
