# Dockerizing an SSH daemon service

從[Docker 技术入门与实战](https://book.douban.com/subject/26284823/)第10章「創建支持SSH服務的鏡像」無法透過commit創建出能跑sshd的服務，不知為何執行```run -p 10022:22 -d sshd:ubuntu /run.sh```指令，總是得到狀態為```Exited (0)```的container。

找到[Dockerizing an SSH daemon service](https://docs.docker.com/engine/examples/running_ssh_service/)這篇，透過Dockerfile建立image，就能順利讓sshd在container裡面跑起來。步驟如下：

```
# docker build -t eg_sshd .
# docker run -d -P --name test_sshd eg_sshd
# docker ps -a
CONTAINER ID        IMAGE               COMMAND               CREATED             STATUS              PORTS                   NAMES
80c17e6a4c42        sshd:test           "/usr/sbin/sshd -D"   3 seconds ago       Up 3 seconds        0.0.0.0:32768->22/tcp   test_sshd
# ssh root@localhost -p 32768
root@localhost's password:
root@80c17e6a4c42:~#
root@80c17e6a4c42:~# exit
```
