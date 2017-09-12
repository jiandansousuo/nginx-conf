# nginx-conf

### 编译nginx参数

```
configure arguments:
    --prefix=/home/centos/local/nginx-${version}
    --pid-path=/home/centos/var/run/nginx.pid
    --lock-path=/home/centos/var/run/nginx.lock
    --conf-path=/home/centos/github/nginx-conf/conf/nginx.conf 
    --with-openssl=../src/openssl-OpenSSL_1_0_2k
    --user=nginx
    --group=nginx
    --with-http_ssl_module
    --with-http_realip_module
    --with-http_dav_module
    --with-http_gzip_static_module
    --with-http_v2_module
    --with-http_sub_module
```

### 目录结构

```
# 可执行文件
./bin/

# nginx配置
./conf/

# 扩展文件, 包括通用favicon.ico、通用robots.txt、ssl证书验证
./conf/inc/

# 站点配置
./conf/vhost/

# 公用静态文件
./html/

# ssl证书 - 忽略提交
./ssl/

# 验证文件 - 忽略提交
./acme-challenge/

# 日志源文件
/home/centos/var/log/nginx/last/{access,error}.{type}.log
```

### 命令

```
# 获取证书, 更新后把证书ln到./ssl中
./bin/certbot-auto certonly \
    --no-bootstrap \
    --webroot -w /home/centos/github/nginx-conf/acme-challenge \
    -d www.jiandansousuo.com \
    -d jiandansousuo.com \
    -d api.jiandansousuo.com \
    -d static.jiandansousuo.com \
    -d jiandansousuo.org \
    -d www.jiandansousuo.org \
    -d jiandansousuo.cn \
    -d www.jiandansousuo.cn

# 生成dhparam
openssl dhparam -out ./ssl/dhparam.pem 2048

# 生成root_ca_cert_plus_intermediates
wget https://letsencrypt.org/certs/isrgrootx1.pem
mv isrgrootx1.pem root.pem
cat root.pem chain.pem > root_ca_cert_plus_intermediates

# 自动更新
/home/centos/nginx-conf/bin/certbot-auto renew
```

### 感谢

- [certbot](https://github.com/certbot/certbot)
- [letsencrypt](https://letsencrypt.org/)