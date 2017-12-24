---
date: 2016-03-09T00:11:02+01:00
title: Getting started
weight: 10
---

## Installation on Server

*<span style="color: red">The connection of this setup is <b>NOT</b> encrypted. Secure connection is highly recommended. Please follow [secure connection](/secure-connection/)</span>*


We provide docker(\*1) image so that all processes for devAny is up and runing.
Please install docker by following [docker installation](https://docs.docker.com/engine/installation/)

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



## Installation on Client

Download app and open the setup. Add server IP/hostname, port, username and password that you specify in server setup.

![Setup](/images/devAny_mobile_setup.jpg)


Then turn your mobile to landscape, you should have development environment!

![First View](/images/devAny_first_view.jpg)


OK, then we move onto how to use.


