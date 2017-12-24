---
date: 2016-03-08T21:07:13+01:00
title: devAny - Write code with your mobile
type: index
weight: 0
---

## What is devAny?

**devAny** is a mobile application that provides a development environment.
The programer needs three things, 1) Terminal 2) Editor and 3) Browser 
and devAny provides them. This is not replacing current develpment envrinment like desktop or laptop but
more like support remote work without PCs or educational purposes that people don't have own PCs.

![Material Screenshot](/images/devAny_top_image.jpg)

## Prerequisites

* **DevAny app** - Install app from [App Store](https://xxx);
* **Server** -  devAny doesn't provide Unix envrionment. You need to have a server for devaAny to connect.
* **Bluetooth Keyboard** -  We all konow that virtual keyboard is hard to type. Find your favorite keyboard.
* **The way to connect to monitor** - physical cable or AirPlay
* **Domain** -  This is not mandatoruy but TSL/SSL connection is highly recommened.


## Installation on Server

*<span style="color: red">This setup is <b>NOT</b> secure. Secure connection is highly recommended. Please follow [this](/secure)</span>*

devAny [Docker](https://docker.io) image helps to set up processes.
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

## Installation on Client

Download app and open the setup. Add server IP/hostname, port, username and password that you specify in server setup.

![Setup](/images/devAny_mobile_setup.jpg)


Then turn your mobile to landscape, you should have development environment!

![First View](/images/devAny_first_view.jpg)


OK, then we move onto how to use.


