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

<< 未完待續 >>
