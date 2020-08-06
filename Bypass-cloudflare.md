### Overview
Although we strongly recommend putting cloudflare in front of your installation, but there's situation where you need to bypass cloudflare. For example, Cloudflare security:
* prevent sending code in POST data. This will affect OpenHub's module configuration editing feature.
* impose a maximum limit to number of POST items. This will affect i18n module translation feature.

### The solution
The solution is to create a subdomain and have it bypass by cloudflare.

1) Create an A record DNS entry in cloudflare, e.g: `bypass-openhub.mymagic.my`
<img width="1054" alt="Screenshot 2020-08-06 at 9 25 05 AM" src="https://user-images.githubusercontent.com/5336690/89480049-f1e47880-d7c6-11ea-9652-6fdb2c4bb84f.png">

2) Edit `.env` to include the following entries:
```
NOPROXY_DOMAIN=bypass-hub.mymagic.my
NOPROXY_URL=//bypass-hub.mymagic.my
```

3) Edit `/protected/overrides/domain.php` to add an entry:
Note: as `bypass-hub.mymagic.my` is a new subdomain, you will need to obtain new connect id and secret key and update to the code block below.
```
'bypass-hub.mymagic.my.com' => array(
  'params' => array(
    'connectSecretKey' => '',
    'connectClientId' => '21',
  )
),
```

