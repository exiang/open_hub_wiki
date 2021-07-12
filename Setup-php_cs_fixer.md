1. Go to your /magic_hub directory.

2. Install php-cs-fixer like below:
```
   $ mkdir --parents tools/php-cs-fixer
   $ composer require --working-dir=tools/php-cs-fixer friendsofphp/php-cs-fixer
```

3. In your /magic_hub directory, create two files:
   .php-cs-fixer.php
   .php_cs.dist

4. Cut, paste and save the following to .php-cs-fixer.php:
   [https://github.com/FriendsOfPHP/PHP-CS-Fixer/blob/3.0/.php-cs-fixer.dist.php](https://github.com/FriendsOfPHP/PHP-CS-Fixer/blob/3.0/.php-cs-fixer.dist.php)

5. Cut, paste and save the following to .php_cs.dist
   [https://github.com/FriendsOfPHP/PHP-CS-Fixer/blob/3.0/.php_cs.dist](https://github.com/FriendsOfPHP/PHP-CS-Fixer/blob/3.0/.php_cs.dist)

6. Create your own workspace settings /magic_hub/edmond.code-workspace. You can name it according to your preference. eg anything.code.work-space. Paste in the following:
```
   # /magic_hub/vs.code-workspace
   {
	"folders": [
		{
			"path": "."
		},
		{
			"path": "../open_hub"
		},
		{
			"path": "../magic_hub_modules"
		},
		{
			"path": "../universal_asset"
		}
	],
	"settings": {
		"editor.formatOnSave": true,
		"php-cs-fixer.executablePath": "/home/edmondtm/magic_docker_for_linux/magic_hub/tools/php-cs-fixer/vendor/bin/php-cs-fixer",
		"php-cs-fixer.config": "/home/edmondtm/magic_docker_for_linux/magic_hub/.php-cs-fixer.php",
		"php-cs-fixer.onsave": true,
		"php-cs-fixer.rules": "@PSR2",
		"php-cs-fixer.pathMode": "override",
		"php-cs-fixer.documentFormattingProvider": true,
		"php-cs-fixer.autoFixBySemicolon": true,
		"php-cs-fixer.allowRisky": false,
		"php-cs-fixer.autoFixByBracket": true,
		"files.exclude": {
			"**/._*": true
		},
		"php-cs-fixer.exclude": [],
	}
   }
```

   Replace "php-cs-fixer.executablePath" and "php-cs-fixer.config" with the absolute link inside your development environment.

7. Restart your vscode using your newly created workspace and test. eg. "code edmond.work-space"

8. Set .gitignore for the files so you don't commit your personal settings to the repo.

   



