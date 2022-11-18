用
yum install -y htpasswd

htpasswd -c pass.db username

Nginx 配置中添加：
auth_basic '用户登录';
auth_basic_user_file pass.db

[[重启 Nginx]]