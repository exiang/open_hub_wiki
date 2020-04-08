### Pick a Domain
In this case, assume it is openhub.mymagic.my.

### SSH in Mac
```sudo nano ~/.ssh/config`
and edit:
Host openhub.mymagic.my
Hostname openhub.mymagic.my
User ubuntu
IdentityFile "~/private/path/to/aws/magic-secret.pem"
```