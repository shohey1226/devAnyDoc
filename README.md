
### Install huga in Ubuntu

$ sudo dpkg -i * .deb


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