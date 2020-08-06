### Overview
Although we strongly recommend putting cloudflare in front of your installation, but there's situation where you need to bypass cloudflare. For example, Cloudflare security:
* prevent sending code in POST data. This will affect OpenHub's module configuration editing feature.
* impose a maximum limit to number of POST items. This will affect i18n module translation feature.

As result, this error page will be displayed.
<img width="699" alt="Screenshot 2020-08-06 at 9 32 17 AM" src="https://user-images.githubusercontent.com/5336690/89480375-bdbd8780-d7c7-11ea-93cc-55b82d9c7aad.png">


### The solution
The solution is to create a subdomain and have it bypass by cloudflare.

1) Create an A record DNS entry in cloudflare, e.g: `noproxy-openhub.mymagic.my` pointing to the same IP as your main domain.

2) Edit `.env` to include the following entries:
```
NOPROXY_DOMAIN=noproxy-hub.mymagic.my
NOPROXY_URL=//noproxy-hub.mymagic.my
```

3) Edit `/protected/overrides/domain.php` to add an entry:
Note: as `noproxy-hub.mymagic.my` is a new subdomain, you will need to obtain new connect id and secret key and update to the code block below.
```
'noproxy-hub.mymagic.my' => array(
  'params' => array(
    'connectSecretKey' => '',
    'connectClientId' => '99',
  )
),
```
