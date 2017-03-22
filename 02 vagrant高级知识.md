#vagrant高级知识

端口转发
vi Vagrantfile
```
config.vm.network "forwarded_port", guest:80, host:80
config.vm.network "forwarded_port", guest:9000, host:9000
```
上一节，通过浏览器就可以像这样访问


添加共享目录（默认已有一个vagrant的共享目录）
```
config.vm.synced_folder "E:/demo", '/demo', :smb => true
config.vm.network "private_network", ip: "192.168.100.126"
```
注：设置共享目录，必须设置私有网络。

