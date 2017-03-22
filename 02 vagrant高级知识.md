#vagrant高级知识

端口转发
vi Vagrantfile
```
config.vm.network "forwarded_port", guest:80, host:80
config.vm.network "forwarded_port", guest:9000, host:9000
```


