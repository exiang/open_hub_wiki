> Here is our setup guide using most of AWS services. This is the recommended way but not the only way. 

## Preparation

### Pick a Domain
In this case, assume the setup is at `openhub.mymagic.my`. Please take note OpenHub must run on a SSL enabled server thru https.

### Setup EC2 instance
Make sure you created an elastic IP and map to this instance. Assume your public elastic IP is `64.128.128.128`.

1. Setup Ubuntu Server (18.04 & above)
2. Setup Apache Web Server (Apache 2)
3. Setup PHP 7.2 and all the required modules

#### PHP.ini setting
```
max_execution_time = 120
max_input_time = 60
memory_limit = 1024M
max_input_vars = 10000
post_max_size = 48M
upload_max_filesize = 20M
output_buffering = 20480
mbstring.language = 'UTF-8'
mbstring.internal_encoding = 'UTF-8'
mbstring.http_output = 'UTF-8'
mbstring.encoding_translation = 1
```

### Setup RDS
1. Select `Easy Create`
2. Select `MariaDB` in configuration
3. Select `Free tier`
4. Wait for it to be created. When done, make sure you set `Public accessibility` to on.
5. Test remote connectivity with mysqladmin
6. Login and create database: 
   * Charset: `utf8mb4`
   * Collation: `uft8mb4_unicode_ci`
7. Last, copy the created database name, database hostname, database username and database user password.

### Setup AWS S3 buckets
We needs 2 buckets here, one for public access and another to store secure files
#### Public Bucket
1. Create a bucket and name it `openhub-staging`
2. Make sure `Block all public access` is `off`
3. Go to `Properties` and `Static Website Hosting`, select `Use this bucket to host a website`
3. Here is the bucket policy for your reference:
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AddPerm",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::openhub-staging/*"
        }
    ]
}
```
#### Secure Bucket
1. Create a bucket and name it `openhub-staging-secure`
2. Make sure `Block all public access` is `on`

### Setup AWS Elastic Cache - REDIS
Setup Redis server for faster caching (optional) or use default file cache. Here, we use REDIS.
* One replica will do
* `cache.t3.micro` will do for the start
* Uncheck `Multi-AZ with Auto-Failover`
* Note down the `Primary Endpoint`


### Setup AWS Elastic Search
  * Domain name: Just insert any name like `esearch`
  * Note down the `Endpoint`

### Setup Neo4J
Setup graph database server (Neo4j) following [instruction here](Neo4J-Database).

### Setup Mandrill
Setup SMTP mail server. We recommend using transaction mail services from Mailchimp

### Get a Google API Account
Google API account is required, use to translate address to lat long location etc. 

### Get an Open Exchange Rate Account
Required for currency conversion to works. All monetary value stored in database in USD. Get a free account here [https://openexchangerates.org/signup/free](https://openexchangerates.org/signup/free).

### Acquire MaGIC Account
OpenHub still reply on MaGIC Account for user authorization and authentication. This step has to be done by MaGICian with admin right.

1. Goto `https://account.mymagic.my/api`
2. Click `Create New Client`
3. Insert:
   * Name: Anything that help you easily identify
   * Redirect URL: https://square.sarawak.digital/connectCallback
4. Copy the Secret and Client ID.

### Using Cloudflare
Cloudflare is free and it helps you protect your server against DDOS. We also use Cloudflare for it easy setup HTTPS.

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

You may get complaint on 'Permission for xxx Key are too open.' This command will help solving this: `chmod 600 ~/private/path/to/aws/magic-secret.pem`

## Setup OpenHub
### Download a release
PHP composer dependency packages and SQL database structure are included.
```
cd /home/ubuntu
wget https://openhub-main.s3-ap-southeast-1.amazonaws.com/github/release/openhub-latest.zip
```

When completed, unzip to `/var/www`:

```
unzip openhub-latest.zip -d /var/www
```

### Run Web Installer
Open browser and go to `https://openhub.mymagic.my/installer`. Follow the instruction there and fill up everything needed.