---
date: 2016-03-09T00:11:02+01:00
title: Getting started
weight: 10
---

## Setup on Server

![diagram](/images/diagram1.svg)

*<span style="color: red">The connection of this setup is <b>NOT</b> encrypted. Secure connection is highly recommended. </span>*
Images of devAny server are provided so you can set up instantly. We use [Packer](https://www.packer.io) to build image. 
Currently, docker, ami, Google Cloud vm and LXD can be built. PR is always welcome to have another image builder.

### 1. With Docker

Install docker. Then,

```
$ sudo docker run -d -h devany \
-v /home/devany:/home/<USERNAME>
-e HOME=/home/<USERNAME> -e USERNAME=<USERNAME> -e PASSWORD=<YOUR PASSWORD> \
-p 8888:8888 shohey1226/devany:latest
```

Please set your `<USERNAME>` and `<YOUR PASSWORD>`.  For example, 

```
$ sudo docker run -d -h devany \
-v /home/devany:/home/shohey1226 \
-e HOME=/home/shohey1226 -e USERNAME=shohey1226 -e PASSWORD=devanyisawesome \
-p 8888:8888 shohey1226/devany:latest  
```

### 2. On AWS EC2

Install [Packer](https://www.packer.io). Then,

```
$ git clone https://github.com/shohey1226/devany-packer.git
$ cd devany-packer
$ REGION=ap-northeast-1 AWS_ACCESS_KEY=xxx AWS_SECRET_KEY=yyy packer build -only=ami devany.json
```

Start VM with the image that has just created and login. Then,

```
$ sudo su
ROOT# USERNAME=<USERNAME> PASSWORD=<PASSWORD> /run.sh
```

### 3. On Google Cloud VM

Install [Packer](https://www.packer.io). Then,

```
$ git clone https://github.com/shohey1226/devany-packer.git
$ cd devany-packer
$ REGION=asia-northeast1-a GOOGLE_ACCOUNT_FILE=<account file> packer build -only=google devany.json 
```

Start VM with the image that has just created and login. Then,

```
$ sudo su
ROOT# USERNAME=<USERNAME> PASSWORD=<PASSWORD> /run.sh
```


## Setup on mobile

Download app from [iTunes App Store](http://itunes.apple.com/app/id1315254200) and open the setup(portrait view). Add server IP/hostname, port, username and password that you specify in server setup.

![Setup](/images/devAny_mobile_setup.jpg)


Then turn your mobile to landscape, you should have development environment!

![First View](/images/devAny_first_view.jpg)


