# nginx-conf

1. 使用 [certbot](https://github.com/certbot/certbot) 认证 [letsencrypt](https://letsencrypt.org/) 证书
1. 使用基于 [jenkins](https://jenkins.io/) 搭建的 [上线CI服务](https://ci.xuexb.com/view/%E7%AE%80%E5%8D%95%E6%90%9C%E7%B4%A2/job/jiandansousuo-nginx/)

### 编译nginx参数

```
configure arguments:
    --prefix=nginx-${version}
    --pid-path=var/run/nginx.pid
    --lock-path=var/run/nginx.lock
    --conf-path=nginx-conf/conf/nginx.conf 
    --with-openssl=src/openssl-OpenSSL_1_0_2k
    --user=xiaowu
    --group=xiaowu
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
~/var/log/nginx/last/{access,error}.{type}.log
```

### 命令

```
# 获取证书, 更新后把证书ln到./ssl中
./bin/certbot-auto certonly \
    --no-bootstrap \
    --webroot -w ./acme-challenge \
    -d amp.jiandansousuo.com \
    -d mip.jiandansousuo.com \
    -d www.jiandansousuo.com \
    -d jiandansousuo.com \
    -d api.jiandansousuo.com \
    -d static.jiandansousuo.com \
    -d jiandansousuo.org \
    -d www.jiandansousuo.org \
    -d jiandansousuo.cn \
    -d www.jiandansousuo.cn \
    -d demo.jiandansousuo.com

# 自动更新
./bin/certbot-auto renew
```
