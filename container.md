# Container 容器

## create

創建一個新容器 (尚未啟動它)
```shell
# docker create -it ubuntu:latest
# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
22200565aa3b        ubuntu:latest       "/bin/bash"         2 minutes ago                                               agitated_swartz
```
## start

啟動一個容器
```shell
# docker start -ai 22200565aa3b
root@22200565aa3b:/# echo "hello world"
hello world
root@22200565aa3b:/# exit
exit
# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
22200565aa3b        ubuntu:latest       "/bin/bash"         5 minutes ago       Exited (0) 3 seconds ago                       agitated_swartz
```

## run

創建一個容器，並在其中執行命令
```shell
# docker run -t -i ubuntu:latest /bin/bash
root@6f765f9fb5c8:/# echo "hello world"
hello world
root@6f765f9fb5c8:/# exit
exit
```

## stop

終止一個正在運行的容器
```shell
# docker stop 22200565aa3b
22200565aa3b
```

## attach

依附到一個正在運行的容器中

## exec

在運行的容器內執行命令

## rm 

刪除給定的若干容器
```shell
# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                          PORTS               NAMES
22200565aa3b        ubuntu:latest       "/bin/bash"         8 minutes ago       Exited (0) About a minute ago                       agitated_swartz

# docker rm 22200565aa3b
22200565aa3b

# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

## export

導出容器內容成為一個 tar file

## import

導入一個 tar file 來創建一個 image
