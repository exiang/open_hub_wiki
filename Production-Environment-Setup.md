### Pick a Domain
In this case, assume it is `openhub.mymagic.my`.

### Setup a EC2 instance
Make sure you created an elastic IP and map to this instance. Assume your public elastic IP is `64.128.128.128`.

### Using Cloudflare
Cloudflare is free and it helps you protect your server against DDOS. 
First, point your domain name to Cloudflare, then point it to AWS using A records:

```
Type: A
Name: openhub.mymagic.my
IPv4 Address: 64.128.128.128

Type: A
Name: api-openhub.mymagic.my
IPv4 Address: 64.128.128.128
```
> If you ping `openhub.mymagic.my` now, you will find the IP address you get is not `64.128.128.128` but something else, e.g. `100.24.16.200`. This is Cloudflare proxy IP, that's how it protect your real server from attack.

### Map IP in local hosts file
IP address is hard to remember, you will like to map it to a hostname in your local machine for easier access.

In Command Line Interface:
```sudo nano /etc/hosts```

edit and save:
```
64.128.128.128 ip.openhub.mymagic.my
```

> The reason we mapping to ip.openhub.mymagic.my but not openhub.mymagic.my is because we already routed openhub.mymagic.my thru Cloudflare and we do not what to interfere with it

### SSH in Mac
We need to do a little setup on Mac now which will help us easily connect to this instance by just typing `ssh ip.openhub.mymagic.my`.

In Command Line Interface:
```sudo nano ~/.ssh/config```

edit and save:
```
Host ip.openhub.mymagic.my
Hostname ip.openhub.mymagic.my
User ubuntu
IdentityFile "~/private/path/to/aws/magic-secret.pem"
```