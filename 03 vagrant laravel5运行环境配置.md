#vagrant laravel5运行环境配置

vi Vagrantfile
```
config.vm.box = "ubuntu"
config.vm.network "forwarded_port", guest:80, host:80
config.vm.network "forwarded_port", guest:8080, host:8080
config.vm.synced_folder "E:/demo", '/demo', :smb => true
config.vm.network "private_network", ip: "192.168.100.126"
```

vi /etc/php/7.0/fpm/pool.d/www.conf
```
listen = 127.0.0.1:9000
```

vi /etc/apache2/ports.conf
```
Listen 8080
```

cp /etc/apache2/sites-available/000-default.conf 000-default.conf.bak
vi 000-default.conf 将第一行改为
```
<VirtualHost *:8080>
```



