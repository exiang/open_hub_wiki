### Pick a Domain
In this case, assume it is openhub.mymagic.my.

### Setup a EC2 instance
### Map IP in local hosts file
IP address is hard to remember, you will like to map it to a hostname in your local machine for easier access.

```sudo nano /etc/hosts```

edit and save:
```
64.128.128.128 ip.openhub.mymagic.my
```

### SSH in Mac
```sudo nano ~/.ssh/config```

edit and save:
```
Host ip.openhub.mymagic.my
Hostname ip.openhub.mymagic.my
User ubuntu
IdentityFile "~/private/path/to/aws/magic-secret.pem"
```