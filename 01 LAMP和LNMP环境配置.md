#LAMP和LNMP环境配置

参考资料：[https://github.com/apanly/mooc/tree/master/vagrant](https://github.com/apanly/mooc/tree/master/vagrant)  

查看nginx软件是否存在
```
apt-cache search nginx
```

安装nginx
```
// 安装nginx
apt-get install nginx
// 查看nginx版本
nginx -v
// 测试nginx
curl -I 'http://127.0.0.1'
```

查看nginx是否安装成功
```
nginx -v
```

测试nginx
```
curl -I 'http://127.0.0.1'
```

安装apache
```
apt-get install apache2
```