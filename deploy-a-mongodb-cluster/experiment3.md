# 實驗三：Dockerizing MongoDB cluster on three VMs

參考連結
- [Deploy a Replica Set](https://docs.mongodb.org/manual/tutorial/deploy-replica-set/)
- [Deploy a MongoDB Cluster in 9 steps Using Docker](https://medium.com/@gargar454/deploy-a-mongodb-cluster-in-steps-9-using-docker-49205e231319#.tuztjshnt)
- [MongoDB - Setting up a Replica Set](https://gist.github.com/leommoore/6742850)
- [MongoDb的“not master and slaveok=false”错误及解决方法](http://www.blogdaren.com/m/?post=2158)

基本程序
- 在各台虛擬機上啟動 mongod 實體，設定 replica set 並把那些 mongod 實體加入其中。

話說前頭
- 我對 mongodb replication 機制不熟，這個實驗只是透過他人的教學網頁拼湊出結果。
- 這個實驗目的在建構一個跨網路的 docker cluster，不會對 mongodb 深入研究。

## 準備

安裝三台VM，Vagrantfile 設定如下：

### node1
```
Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "private_network", ip: "192.168.33.11"
end
```

### node2
```
Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "private_network", ip: "192.168.33.12"
end
```

### node3
```
Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "private_network", ip: "192.168.33.13"
end
```

設定好後，個別啟動 Ubuntu/VM，使用 ssh 登入，切換使用者 ```root```
```
$ vagrant up
$ vagrant ssh
$ sudo su -
#
```

## 安裝套件

在每台虛擬機執行以下指令，安裝相關套件
```
# apt-get update
# apt-get install -y docker.io
# docker pull mongo
```

## 設定網路

在每台虛擬機執行以下指令，設定各節點資訊
```
# export node1=192.168.33.11
# export node2=192.168.33.12
# export node3=192.168.33.13
```

如果想要重開機後還能維持設定，請執行以下動作
```
# cat >> /etc/hosts << EOF
192.168.33.11 node1
192.168.33.12 node2
192.168.33.13 node3
EOF
```


<< 未完待續 >>
