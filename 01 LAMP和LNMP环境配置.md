#LAMP和LNMP环境配置

参考资料：[https://github.com/apanly/mooc/tree/master/vagrant](https://github.com/apanly/mooc/tree/master/vagrant)  

查看nginx软件是否存在
```
apt-cache search nginx
```

安装nginx
```
// 搜索是否存在nginx这款软件
apt-cache search nginx
// 安装nginx
apt-get install nginx
// 查看nginx版本
nginx -v
// 测试nginx
curl -I 'http://127.0.0.1'
```

安装apache
```
apt-get install apache2
apache2 -v
/etc/init.d/nginx stop
/etc/init.d/apache2 start
curl -I 'http://127.0.0.1'
```

安装mysql
```
apt-get install mysql-server mysql-client
mysql -V
```

安装php
```
apt-get install php7.0-cli
php -v
```

安装php扩展
```
apt-get install php7.0-curl php7.0-gd php7.0-json php7.0-mysql php7.0-mcrypt php7.0-mbstring
```

安装apache2的php模块
```
apt-get install libapache2-mod-php7.0
```

安装nginx fastcgi
```
apt-get install php7.0-cgi php7.0-fpm
```

查看apache是否正在运行
```
ps -ef | grep apache
ps -ef | grep nginx
```

为了让apache和nginx可以同时运行，因此将apache的端口修改成9000
```
vi /etc/apache2/ports.conf
/etc/init.d/apache2 restart
```

启动apache/nginx
```
/etc/init.d/apache2 start
/etc/init.d/nginx start
ps -ef | grep apache
ps -ef | grep nginx
curl -I 'http://127.0.0.1'
curl -I 'http://127.0.0.1:9000'
```

通过浏览器访问
vi /var/www/html/nginx.html
```
nginx
```

vi /var/www/html/apache.html
```
apache
```
http://192.168.100.126/nginx.html












