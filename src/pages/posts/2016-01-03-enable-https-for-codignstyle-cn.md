---
date: 2016-01-03 15:33
status: public
title: '为codingstyle.cn启用 HTTPS'
---

[软件匠艺社区](https://codingstyle.cn) 上线后，很多用户反馈没有 HTTPS。
于是趁着元旦假期研究了一下，花了半天时间，踩了一些坑，已全面开启并强制 HTTPS。
## Why HTTPS？
所有网站开启 HTTPS 可以说是必然的，只是早晚的问题。
主要是**安全**，对用户负责，其次是据说 Google 为了推进 HTTPS 普及，在显示结果中提高了启用 HTTPS 网站的优先级。

## 知识储备
我在动手之前，先学习了一下 HTTPS 的基本原理：
[HTTPS工作原理](https://cattail.me/tech/2015/11/30/how-https-works.html?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)
[全站 HTTPS 来了](http://segmentfault.com/a/1190000004199917)

## 选择证书
![](https://ww2.sinaimg.cn/large/61412e43gw1ezmd3e72fej21gw0beae0.jpg)
选择 Let's Encrypt 能切实感受的好处就是：免费和自动化。
不过目前**有效期只有三个月**，建议在两个月的时候更新证书。不过更新也是很方便的。

## 生成证书
我在配置过程中主要参考这篇文章：
[Let’s Encrypt网站启用https以及nginx配置安全优化](https://www.embbnux.com/2015/12/29/letsencrypt_with_nginx_config_for_wordpress/)。

ssh 到服务器上，随便找一个地方执行如下命令：
```
git clone https://github.com/letsencrypt/letsencrypt
cd letsencrypt
./letsencrypt-auto certonly -d codingstyle.cn -d www.codingstyle.cn
```
Let's encrypt 提供的工具非常棒，基本可以自动化地完成验证过程。执行过程中如果你的 80 端口已被占用，需要先停用一下。
命令执行成功后会生成证书和私钥：
```
/etc/letsencrypt/live/codingstyle.cn/fullchain.pem
/etc/letsencrypt/live/codingstyle.cn/privkey.pem
```
这两个文件在稍后的 nginx 配置中会用到。

## nginx 配置
### 启用 ssl
```
server {
    listen 443 ssl;
    ssl_certificate /etc/letsencrypt/live/codingstyle.cn/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/codingstyle.cn/privkey.pem;
}
```
### 优化安全性
通过评测网站：[www.ssllabs.com](http://www.ssllabs.com)检测结果为 C。
按以下步骤优化后可得到 A+。
![](https://ww4.sinaimg.cn/large/61412e43gw1ezmdjo6qwnj21bm0pyn37.jpg)
```
mkdir -p /opt/dhparam/keys
openssl dhparam -out /opt/dhparam/keys/dhparams.pem 2048
sudo chmod 700 /opt/dhparam/keys
```
nginx.conf
```
server {
    listen 443 ssl;
    ...
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers 'ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA';
    ssl_prefer_server_ciphers on;
    ssl_session_cache shared:SSL:10m;
    ssl_dhparam /opt/dhparam/keys/dhparams.pem;
}
```
### 重定向
用户访问网站的时候可能会有 non-WWW 和 WWW，`http://` 和 `https://` 四种情况：

* http://codingstyle.cn
* http://www.codingstyle.cn
* https://codingstyle.cn
* https://www.codingstyle.cn

为了把另外三个地址重定向到 `https://codingstyle.cn`，需要做一些配置：
```
server {
    listen 80;
    server_name codingstyle.cn;
    rewrite ^ https://$server_name$request_uri? permanent;
}

server {
    listen 443 ssl;
    server_name codingstyle.cn;
    if ($host = www.$server_name) {
      rewrite ^(.*) https://$server_name$request_uri? permanent;
    }
}
```
最后，完整的配置是这样：
```
http {
  server {
    listen 80;
    server_name codingstyle.cn;
    rewrite ^ https://$server_name$request_uri? permanent;
  }

  server {
    listen 443 ssl;
    server_name codingstyle.cn;
    ssl_certificate /etc/letsencrypt/live/codingstyle.cn/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/codingstyle.cn/privkey.pem;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers 'ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA';
    ssl_prefer_server_ciphers on;
    ssl_session_cache shared:SSL:10m;
    ssl_dhparam /opt/dhparam/keys/dhparams.pem;

    if ($host = www.$server_name) {
      rewrite ^(.*) https://$server_name$request_uri? permanent;
    }
}
```

## 其它配置
最后，因为我使用 又拍云 做 CDN，还需要配置使其开启 HTTPS：
![](https://ww3.sinaimg.cn/large/61412e43gw1ezmdxpihffj21e60pc773.jpg)

我的程序是 Fork 的 Ruby China，还需要在应用配置中做一些修改，过程中也遇到一些问题，请看[「这里」](https://ruby-china.org/topics/28593)。