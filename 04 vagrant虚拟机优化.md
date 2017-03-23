#vagrant虚拟机优化

虚拟机名称
```
config.vm.hostname = "test"
```

虚拟机主机名
```
vb.name = "ubuntu_test"
```

配置虚拟机内存
```
vb.memory = "512"
```

配置虚拟机CPU
```
vb.cpus = 1
```

完整示例
先复制一份Vagrantfile为Vagrantfile.bak作为备份
Vagrantfile
```
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu"
  config.vm.network "forwarded_port", guest:80, host:80
  config.vm.network "forwarded_port", guest:8080, host:8080
  config.vm.synced_folder "E:/demo", '/demo', :smb => true
  config.vm.network "private_network", ip: "192.168.100.126"
  config.vm.hostname = "test"

  config.vm.provider "virtualbox" do |vb|
    vb.name = "ubuntu_test"
    vb.memory = "512"
    vb.cpus = 1
  end
end
```

优化vagrant同步目录功能
官方文档说明：[https://www.vagrantup.com/docs/synced-folders/virtualbox.html](https://www.vagrantup.com/docs/synced-folders/virtualbox.html)  
Nginx优化
cd /etc/nginx/
vi nginx.conf
```
sendfile off
```


Apache优化
注：apache2这个版本无需优化