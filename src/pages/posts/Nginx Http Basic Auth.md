---
date: '2017/11/20'
---

<img src='https://img2.baidu.com/it/u=4004905786,133766336&fm=253&fmt=auto&app=138&f=PNG?w=800&h=492' />

用如下命令：

`yum install -y htpasswd`

`htpasswd -c pass.db username`

Nginx 配置中添加：
```
auth_basic '用户登录';
auth_basic_user_file pass.db
```

重启 Nginx
