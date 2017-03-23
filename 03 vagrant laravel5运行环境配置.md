#vagrant laravel5运行环境配置

vi Vagrantfile
```
config.vm.box = "ubuntu"
config.vm.network "forwarded_port", guest:80, host:80
config.vm.network "forwarded_port", guest:8080, host:8080
config.vm.synced_folder "E:/demo", '/demo', :smb => true
config.vm.network "private_network", ip: "192.168.100.126"
```

