# linux nginx安装

1.下载nginx,[nginx下载地址](http://nginx.org/en/download.html)
```
cd /usr/local

//安装c++编译环境
yum install gcc-c++

//安装nginx及相关组件

//openssl安装
wget http://www.openssl.org/source/openssl-fips-2.0.10.tar.gz
tar zxvf openssl-fips-2.0.10.tar.gz
cd openssl-fips-2.0.10
./config && make && make install

//pcre安装
wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.40.tar.gz
tar zxvf pcre-8.40.tar.gz
mv pcre-8.40 pcre
cd pcre
./configure && make && make install

//zlib安装
wget http://zlib.net/zlib-1.2.11.tar.gz
tar zxvf zlib-1.2.11.tar.gz
mv zlib-1.2.11 zlib
cd zlib
./configure && make && make install

//nginx安装
wget http://nginx.org/download/nginx-1.19.9.tar.gz
tar zxvf nginx-1.10.2.tar.gz
mv nginx-1.10.2 nginx
cd nginx
./configure && make && make install

```

2.nginx启动
```
cd /usr/local/nginx
./sbin/nginx -t //测试配置文件
./sbin/nginx start //启动
./sbin/nginx -s reload //重启
./sbin/nginx stop //停止
./sbin/nginx -c /usr/local/nginx/conf/nginx.conf //指定配置文件启动
```

3.查看nginx启动状态
```
ps -aux | grep nginx
```

4.nginx配置文件，文件地址 /usr/local/nginx/conf/nginx.conf，一个server代表一个站点配置
```
{
    server {
        listen       8000;
        listen       somename:8080;
        server_name  somename  alias  another.alias;

        location / {
            root   html;
            index  index.html index.htm;
        }
    }
}
```

5.多域名配置
```
//nginx.conf底部添加
include vhost/*.conf;

cd /usr/local/nginx/conf/vhost


//示例
server {
    listen 80;
    server_name 你的域名;
    
    keepalive_timeout    3000;
    
    #一个location代表一个重定向
    location / {
        proxy_set_header Host $host;
        proxy_set_header x-real-ip $remote_addr;
        proxy_pass http://服务器ip:3000; #指向项目端口
    }
    access_log  /data0/wwwlogs/xxx.access.log;
    error_log /data0/wwwlogs/xxx.error.log;
}

server {
    listen 80;
    server_name 你的域名;
   
    root  /data0/wwwroot/;
    index index.html index.htm
    keepalive_timeout    300000;

    access_log  /data0/wwwlogs/xxx.access.log;
    error_log /data0/wwwlogs/xxx.error.log;
}
```

