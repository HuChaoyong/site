---
title: Basic Of Nginx
catalog: true
date: 2019-04-23 20:53:12
subtitle:
header-img:
tags:
- Network,Linux
catagories:
- Network,Linux
---

# Install
* open the [Download Page](http://nginx.org/en/download.html)
* Download last Stable version.
* unzip file and open dir. (Like  nginx-1.14.2 )
* set configure for it. 
```bash
# after compile, nginx will install on this dir
 ./configure --prefix=/etc/nginx
 ```
 * compile
 ```bash
 sudo make && make install
 ```
 * open dir '/etc/nginx/sbin', run nginx by command
 ```bash
 ./nginx
 ```
 * open [localhost](http://localhost), nginx will show .

 # Use Nginx Build a Resource site.
 * use config like flow
 ```bash
 server {
	listen 80;
	index index.html index.htm;
	location / {
		root /home/resource;
        autoindex   on;
        autoindex_exact_size off;
        autoindex_localtime on;
	}
}
 ```
 * save and reload config file
 ```bash
 sudo nginx -s reload
 ```
 * open [localhost](http://localhost), a resource site will show.
 > if you want to build a nice site by some css. look flow.
 ## build a beautiful resource site.(Here use ngx-fancyindex module)
 * [Download](https://github.com/aperezdc/ngx-fancyindex) ngx-fancyindex module
 * unzip ngx-fancyindex on your nginx dir.then, your dir tree will like this
 ```
├── auto
├── CHANGES
├── CHANGES.ru
├── conf
├── configure
├── contrib
├── html
├── LICENSE
├── Makefile
├── man
├── ngx-fancyindex
├── objs
├── README
└── src
 ```
 * set configure, add ngx-fancyindex module
 ```bash
 ./configure --prefix=/home/nginx --add-module=./ngx-fancyindex-master
 ```
 * compiler
 ```
 sudo make && make install
 ```
* set config file
```bash
server {
	listen 8073;
	index index.html index.htm;
	location / {
		root /home/resource;
	        if ($request_filename ~* ^.*?\.(txt|doc|pdf|rar|png|jpg|mp3|mp4|xz|json|gz|zip|docx|exe|xlsx|ppt|pptx)$){
        	    add_header Content-Disposition attachment;
        	}
		fancyindex on;
		fancyindex_exact_size off;
		fancyindex_localtime on;

		fancyindex_header "/theme/header.html";
		fancyindex_footer "/theme/footer.html";
		fancyindex_ignore "theme";
		fancyindex_name_length 255;
	}
}
```
* download style file [here](/download/theme.zip), unzip for on config root dir. (there is /home/resource),file tree will like this.
```
.
└── theme
```
* run nginx ,you will see a nice html for your resource site.
![avator](/img/resource-site.png)
