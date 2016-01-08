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
```shell
# docker run -idt ubuntu
842c8147831f508368054e849e2a65f2f4e17f61982272ed2ba6d8848791f244
# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
842c8147831f        ubuntu:latest       "/bin/bash"         6 seconds ago       Up 5 seconds                            adoring_fermat
# docker attach adoring_fermat
root@842c8147831f:/# echo "hello world"
hello world
root@842c8147831f:/# exit
exit
# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

## exec

在運行的容器內執行命令
```shell
# docker run -idt ubuntu
0e02d5678cdd5bb519daee556378533765d43bbfecf910be84466d8951656db8
# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
0e02d5678cdd        ubuntu:latest       "/bin/bash"         9 seconds ago       Up 9 seconds                            tender_mccarthy
# docker exec -ti 0e02d5678cdd5bb519daee556378533765d43bbfecf910be84466d8951656db8 /bin/bash
root@0e02d5678cdd:/# echo "hello world"
hello world
root@0e02d5678cdd:/# exit
exit
# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
0e02d5678cdd        ubuntu:latest       "/bin/bash"         57 seconds ago      Up 56 seconds                           tender_mccarthy
```

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
