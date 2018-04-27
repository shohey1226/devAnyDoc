---
date: 2016-03-09T00:11:02+01:00
title: Getting started
weight: 10
---

## Installation on Server

![devAny Diagram](/images/devAny_diagram.jpg)

*<span style="color: red">The connection of this setup is <b>NOT</b> encrypted. Secure connection is highly recommended. Please follow [secure connection](/getting-started/#secure-connection) after the below instruction is completed.</span>*

There are a couple of processes need to run on server to use devAny. In order to avoid this tedious setup, I provide images that includes all required processes and the way to create the image on your end using [Packer ](https://www.packer.io).


### 1. Using Docker

Docker is well-known container and the easiest way to launch devAny process on server. One drawbacks is that only mounted volume is persistent after relaunch docker container. For example, if you install packages with apt-get to /user/local/bin, those are gone after restarting the container process. Therefore, installing all packages in your mounted volume with linuxbrew, rbenv, nvm or others are recommended. I put dockerfile in [devany-docker](https://github.com/shohey1226/devany-docker). Please feel free to fork and add your packages and build it by yourself.

Okay, moving on the how to. First of all, please install docker by following [docker installation](https://docs.docker.com/engine/installation/)

After docker is installed, you just run the following command.

```
$ sudo docker run -d -h devany \
-v /home/devany:/home/<USERNAME>
-e HOME=/home/<USERNAME> -e USERNAME=<USERNAME> -e PASSWORD=<YOUR PASSWORD> \
-p 8888:8888 -p 3000:5000 shohey1226/devany:latest
```

Please set your `<USERNAME>` and `<YOUR PASSWORD>`.  For example, 

```
$ sudo docker run -d -h devany \
-v /home/devany:/home/shohey1226 \
-e HOME=/home/shohey1226 -e USERNAME=shohey1226 -e PASSWORD=devanyisawesome \
-p 8888:8888 -p 3000:5000 shohey1226/devany:latest  
```


*\*1. If you are not familiar with docker. Please go through [their documentation](https://docs.docker.com/).*

### 2. Using IaaS

Here I only explain AWS and Google Could but [Packer](https://www.packer.io) has more capability to build images.
Feel free to fork [devany-packer](https://github.com/shohey1226/devany-packer) and create image. (PR is welcome by the way)

#### 2-1 Prepare the image

**Amazon EC2 image(AMI)**

There are public images so you can use. You can find these AMI ID in the region, and copy it if needed.

* ami-b2dc9ed4 (region: ap-northeast-1)
* ami-32f7fd52 (region: us-west-1)

**Google Cloud image**

I found it is difficult to share google images to others. I explain how to make the image on your end.
Before running below command, 

1. Go to [Download Packer](https://www.packer.io/downloads.html) and install Packer.
2. Prepare account file with [Running Without a Compute Engine Service Account](https://www.packer.io/docs/builders/googlecompute.html#running-without-a-compute-engine-service-account) 
3. Find out which zone you want to buid on with [Region and Zone](https://cloud.google.com/compute/docs/regions-zones/regions-zones)

```text
$ git clone https://github.com/shohey1226/devany-packer.git
$ cd devany-packer
$ REGION=asia-northeast1-a GOOGLE_ACCOUNT_FILE=<account file> packer build -only=google devany.json  
```

#### 2-2 Set up user

Bring up the instance with image and login to the instance. Make sure the port 8888 is opened on the firewall.

```text
$ sudo su
ROOT# USERNAME=<USERNAME> PASSWORD=<PASSWORD> /run.sh
```
Make sure that the processes are runnging.

```bash
$ sudo supervisorctrl status
apache                           RUNNING   pid 23, uptime 22 days, 12:38:14
ttyd                             RUNNING   pid 24, uptime 22 days, 12:38:14
```


## Setup on mobile

Download app from [iTunes App Store](http://itunes.apple.com/app/id1315254200) and open the setup(portrait view). Add server IP/hostname, port, username and password that you specify in server setup.

![Setup](/images/devAny_mobile_setup.jpg)


Then turn your mobile to landscape, you should have development environment!

![First View](/images/devAny_first_view.jpg)

## Secure connection

The connection between mobile app and server should be encrypted. Otherwise raw data is transmitted between them and there is possibility to be sneaked.
(Please follow the below after you completed "Getting started" and confirmed that the app is working)

![devAny Diagram secured connection](/images/devAny_diagram_secured.jpg)

There are many ways to accomplish this but I introduce a cost-saving way, which is [Let's encrypt](https://letsencrypt.org/).
In order to do that, you need to have your own [domain](https://en.wikipedia.org/wiki/Domain_name). 

As example, we use the below info to setup this.

* IP address of server - 123.123.123.123
* Hostname - secure.devany.net 



### 1. Set up "A record" on DNS. 

Domain Buy Service usually provide the console to configure DNS records. Go to yours and add the following.

```txt
secure  A    123.123.123.123
```

Confirm that you can resolve the hostname

```bash
$ host secure.devany.net
secure.devany.net has address 123.123.123.123
```


### 2. Install applications for let's encrypt

*\*OS/distro is Linux/ubuntu*

```bash
$ sudo add-apt-repository ppa:certbot/certbot
$ sudo apt-get update
$ sudo apt-get install python-certbot-nginx
```

### 3. Configure nginx

Change `server_name` to your hostname.

```bash
$ sudo vi /etc/nginx/sites-available/default

server_name secure.devany.net # change this line
```


Test your configuration.

```shell
ubuntu@:~$ sudo nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

And reload the configuraion.

```
ubuntu@:~$ sudo systemctl reload nginx
```


### 4. Set up certbot


```bash
ubuntu@:~$ sudo certbot --nginx -d secure.devany.net
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator nginx, Installer nginx
Obtaining a new certificate
Performing the following challenges:
tls-sni-01 challenge for secure.devany.net
Waiting for verification...
Cleaning up challenges
Deployed Certificate to VirtualHost /etc/nginx/sites-enabled/default for set(['secure.devany.net'])

Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
-------------------------------------------------------------------------------
1: No redirect - Make no further changes to the webserver configuration.
2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
new sites, or if you're confident your site works on HTTPS. You can undo this
change by editing your web server's configuration.
-------------------------------------------------------------------------------
Select the appropriate number [1-2] then [enter] (press 'c' to cancel): 1

-------------------------------------------------------------------------------
Congratulations! You have successfully enabled https://secure.devany.net

You should test your configuration at:
https://www.ssllabs.com/ssltest/analyze.html?d=secure.devany.net
-------------------------------------------------------------------------------

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/secure.devany.net/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/secure.devany.net/privkey.pem
   Your cert will expire on 2018-03-17. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot again
   with the "certonly" option. To non-interactively renew *all* of
   your certificates, run "certbot renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```


### 5. Test renewing the certificate and set the crontab

```bash
ubuntu@:~$ sudo certbot renew --dry-run
Saving debug log to /var/log/letsencrypt/letsencrypt.log

-------------------------------------------------------------------------------
Processing /etc/letsencrypt/renewal/secure.devany.net.conf
-------------------------------------------------------------------------------
Cert not due for renewal, but simulating renewal for dry run
Plugins selected: Authenticator nginx, Installer nginx
Renewing an existing certificate
Performing the following challenges:
tls-sni-01 challenge for secure.devany.net
Waiting for verification...
Cleaning up challenges

-------------------------------------------------------------------------------
new certificate deployed with reload of nginx server; fullchain is
/etc/letsencrypt/live/secure.devany.net/fullchain.pem
-------------------------------------------------------------------------------

-------------------------------------------------------------------------------
** DRY RUN: simulating 'certbot renew' close to cert expiry
**          (The test certificates below have not been saved.)

Congratulations, all renewals succeeded. The following certs have been renewed:
  /etc/letsencrypt/live/secure.devany.net/fullchain.pem (success)
** DRY RUN: simulating 'certbot renew' close to cert expiry
**          (The test certificates above have not been saved.)
-------------------------------------------------------------------------------

IMPORTANT NOTES:
 - Your account credentials have been saved in your Certbot
   configuration directory at /etc/letsencrypt. You should make a
   secure backup of this folder now. This configuration directory will
   also contain certificates and private keys obtained by Certbot so
   making regular backups of this folder is ideal.
```


Once it's confirmed that renewal is working, set the job in cron so periodically checks the certification.

```bash
$ sudo su 
$ crontab -e
# add the below
32 0 * * * certbot renew --post-hook "systemctl reload nginx" >> /tmp/certbot.log
```

### 6. Set up reverse proxy on nginx to connect to docker container

Open `/etc/nginx/sites-available/default` and change it to the below.

```bash
$ sudo vi /etc/nginx/sites-available/default
...
        server_name secure.devany.net;

        location / {
          proxy_pass http://localhost:8888/;
          proxy_redirect off;
          proxy_set_header Host $host;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Ssl on;

          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection "upgrade";
        }

        location ~ /.well-known {
          allow all;
        }
...        
```


then, restart nginx.

```bash
$ sudo systemctl restart nginx
```


All done!  Now you can feel safe when you use devAny:)


### References

* https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-16-04
* https://gist.github.com/robbiet480/b1e9a2a22501b8304547
* https://serverfault.com/questions/790772/cron-job-for-lets-encrypt-renewal


OK, then we move onto [how to use](/how-to-use/).


