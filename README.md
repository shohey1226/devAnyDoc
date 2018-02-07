This repository is the content of [https://www.devany.net](https://www.devany.net).
[HUGO](https://gohugo.io/) is being used to build the site. 

*PR is always welcome to keep the website clean.*



## Notes

This is what I did to setup the site.
Added here so that I don't forget the steps :-)

### Install hugo on Ubuntu

```
# Download the deb file from https://github.com/gohugoio/hugo/releases , then
$ sudo dpkg -i * .deb
```

### Install theme

When you clone the hugo repo, you don't have the theme. You need to install separately.

```
$ cd <dir to root of hugo>
$ git clone https://github.com/digitalcraftsman/hugo-material-docs.git themes/hugo-material-docs
```

### How to start local server and access from outdide of the host 

```
$ hugo serve -p 5000 --bind=0.0.0.0 --baseUrl=secure.devany.net
```
