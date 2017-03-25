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
/etc/init.d/php7.0-fpm restart

cd /etc/nginx/sites-available
cp default default.bak
vi default
```
index index.php index.html index.htm index.nginx-debian.html;

location ~ \.php$ {
    include snippets/fastcgi-php.conf;

    # With php7.0-cgi alone:
    fastcgi_pass 127.0.0.1:9000;
    # With php7.0-fpm:
    # fastcgi_pass unix:/run/php/php7.0-fpm.sock;
}
```
/etc/init.d/nginx restart

cd /var/www/html
vi index.php
```
<?php phpinfo(); ?>
```
通过浏览器访问：http://localhost/
![](image/screenshot_1490241080980.png)

vi /etc/apache2/ports.conf
```
Listen 8080
```

cd /etc/apache2/sites-available
cp 000-default.conf 000-default.conf.bak
vi 000-default.conf 将第一行改为
```
<VirtualHost *:8080>
```
/etc/init.d/apache2 restart
通过浏览器访问：http://localhost:8080/
![](image/screenshot_1490242653637.png)

cd /etc/nginx/sites-available
vi laravel-demo.conf
```
server {
       listen 80;
       listen [::]:80;

       server_name laravel-demo.com;

       root /vagrant/laravel/public;
       index index.php index.html;

       location / {
               try_files $uri $uri/ =404;
       }

       location ~ \.php$ {
               include snippets/fastcgi-php.conf;
               fastcgi_pass 127.0.0.1:9000;
       }
}
```
/etc/init.d/nginx restart
通过浏览器访问：http://laravel-demo.com/
![](image/screenshot_1490242933787.png)

vi /etc/hosts
```
127.0.1.1       laravel-demo.com
```
curl -I "http://laravel-demo.com"

cd /etc/apache2/sites-available
cp 000-default.conf laravel-demo.conf
vi laravel-demo.conf  将对应选项更改成如下：
```
ServerName laravel-demo.com
ServerAdmin zhulinjie_cool@126.com
DocumentRoot /vagrant/laravel/public
```
/etc/init.d/apache2 restart
通过浏览器访问：http://laravel-demo.com:8080/
![](image/screenshot_1490243301004.png)
curl -I "http://laravel-demo.com:8080"

搭建weiyuyan环境
vi /etc/hosts
```
127.0.0.1       api.4000669696.com
```
注：这里配置以后其它端请求 api 才能够成功

cd /etc/nginx/sites-available/

vi api.4000669696.com.conf
```
server {
       listen 80;
       listen [::]:80;

       server_name api.4000669696.com;

       root /vagrant/weiyuyan/wyyapi/public;
       index index.php index.html;

       location / {
                try_files $uri $uri/ /index.php$is_args$query_string;
       }

       location ~ \.php$ {
                try_files $uri = 404;
                fastcgi_pass 127.0.0.1:9000;
                fastcgi_index index.php;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                include fastcgi_params;
       }
}
```

vi teacher.dev.4000669696.com.conf
```
server {
       listen 80;
       listen [::]:80;

       server_name teacher.dev.4000669696.com;

       root /vagrant/weiyuyan/teacher/public;
       index index.php index.html;

       location / {
                try_files $uri $uri/ /index.php$is_args$query_string;
       }

       location ~ \.php$ {
                try_files $uri = 404;
                fastcgi_pass 127.0.0.1:9000;
                fastcgi_index index.php;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                include fastcgi_params;
       }
}
```
其它端同上...

apache配置
cd /etc/apache2/

vi ports.conf
```
Listen 80
```

vi apache2.conf 
```
<Directory /vagrant/>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
</Directory>
```

cd mods-enabled/
```
ln -s ../mods-available/rewrite.load
```

cd sites-available/
vi 000-default.conf
```
<VirtualHost *:80>
```

cp 000-default.conf api.4000669696.com.conf
vi api.4000669696.com.conf
```
ServerName api.4000669696.com
ServerAdmin zhulinjie_cool@126.com
DocumentRoot /vagrant/weiyuyan/wyyapi/public
```

vi teacher.dev.4000669696.com.conf
```
ServerName teacher.dev.4000669696.com
ServerAdmin zhulinjie_cool@126.com
DocumentRoot /vagrant/weiyuyan/teacher/public
```
其它端同上...

说明：
weiyuyan为开发环境，之前apache使用的是8080端口，但由于curl在请求非80端口的时候需要增加 `curl_setopt($ch, CURLOPT_PORT, 8080)` 这个选项，因此决定apache与nginx都使用80端口，但默认只开启nginx

编辑Vagrantfile
```
config.vm.provision "shell", inline: <<-SHELL
	/etc/init.d/apache2 stop
SHELL
```
vagrant reload --provision
