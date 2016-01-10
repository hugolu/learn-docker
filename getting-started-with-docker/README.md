# Docker 從頭開始

這是一邊閱讀[Getting Started with Docker](https://serversforhackers.com/getting-started-with-docker)一邊練習的筆記。完整說明建議參考原出處。


實驗環境
- Host OS: OS X 10.10.5
- Guest OS: ubuntu/trusty64 on VirtualBox/Vagrant

實驗目標
- 是在guest os內製作一個執行nginx web server的docker container，最後能在host os透過瀏覽器連接web server

## Vagrantfile

修改```Vagrantfile```，設定guest os對外的網路。
```
config.vm.box = "ubuntu/trusty64"
config.vm.network "private_network", ip: "192.168.33.70"
```

設定好後，使用vagrant指令啟動虛擬機
```
$ vagrant up
```

虛擬機啟動後，透過ssh連線登入，確定private_network interface設定成功
```
$ vagrant ssh
Welcome to Ubuntu 14.04.3 LTS (GNU/Linux 3.13.0-66-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Sun Jan 10 03:27:44 UTC 2016

  System load:  0.39              Users logged in:        0
  Usage of /:   4.9% of 39.34GB   IP address for eth0:    10.0.2.15
  Memory usage: 28%               IP address for eth1:    192.168.33.70
  Swap usage:   0%                IP address for docker0: 172.17.42.1
  ...
```

登入guest os後切換```root```使用者身份
```
$ sudo su -
```

## 第一個 docker container

使用```docker run```啟動一個container。
- 指令：```docker run <Image> [Command]```
```
# docker run ubuntu /bin/bash
# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
09997635bafe        ubuntu:latest       "/bin/bash"         3 seconds ago       Exited (0) 2 seconds ago                       loving_heisenberg
```

觀察到container```09997635bafe```狀態是exited。因為container裡面沒有執行的process，所以container處於停止的狀態。

再```run```一次，但這次加上```-t```與```-i```選項 (分配tty並保持stdin開啟)。
```
# docker run -ti ubuntu /bin/bash
root@b6171817af95:/# echo "hello world" > hello.txt
root@b6171817af95:/# exit
```

觀察剛剛在container內的操作，對原先images做了什麼改變。
- 指令：```docker diff <Container ID>```
```
# docker diff b6171817af95
C /root
A /root/.bash_history
A /hello.txt
```

## 追蹤修改

docker 會追蹤任何對container造成的改變。docker 允許改變 images 內容，提交這些改變，然後push這些改變到其他地方。這就是用docker進行部署的基礎。

再一次，我們執行一個container，並用安裝一些套件。
```
# docker run -ti ubuntu /bin/bash
root@054f7b9e9f77:/# apt-get update
root@054f7b9e9f77:/# apt-get install -y git ack-grep vim curl wget tmux build-essential python-software-properties
root@054f7b9e9f77:/# exit
```

再使用```docker diff```會看到一堆安裝過的東西。
- 指令：```docker commit <Container ID> <Name>:<Tag>```
```
# docker commit 054f7b9e9f77 fideloper/docker-example:0.1
```

下一步，把剛剛做的修改打包成一個 image，以便將來使用。
- 指令：```docker commit <Container ID> <Name>:<Tag>```
```
# docker commit 054f7b9e9f77 fideloper/docker-example:0.1
30059fbd9d2a0a4f2c74f895757f96479bc3d66030b1990269d163f830dd674b
```

看看目前保存的image ID是否包含```30059fbd9d2a0a4f2c74f895757f96479bc3d66030b1990269d163f830dd674b```
- 指令：```docker images```
```
# docker images
REPOSITORY                 TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
fideloper/docker-example   0.1                 30059fbd9d2a        5 minutes ago       429.1 MB
ubuntu                     latest              d55e68e6cc9c        4 weeks ago         187.9 MB
```

找到一個Image，id=```30059fbd9d2a```，repository=```fideloper/docker-example```，這就是剛剛打包的image。

<< 未完待續 >>
