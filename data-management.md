# Data Management

## 掛載本地目錄作為 data volumn
```
# docker run -it -v /opt/data:/opt/data ubuntu /bin/bash
root@3caed146561a:/# ls /
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
root@3caed146561a:/# echo "hello world" > /opt/data/hello.txt
root@3caed146561a:/# exit
# cat /opt/data/hello.txt
hello world
```

## container 之間共用數據，使用 data volumn container
```
# docker run -it -v /opt/data --name node1 ubuntu
root@9e37cd90c168:/# echo "hell from node1" > /opt/data/hello.txt
root@9e37cd90c168:/# exit
#
# docker run -it --volumes-from node1 --name node2 ubuntu
root@e6e08b16751b:/# cat /opt/data/hello.txt
hell from node1
root@e6e08b16751b:/# echo "hello from node2" >> /opt/data/hello.txt
root@e6e08b16751b:/# exit
#
# docker start 9e37cd90c168586c10d52790a9d5fbdf49acb394cc914bafe41400654c48758b
9e37cd90c168586c10d52790a9d5fbdf49acb394cc914bafe41400654c48758b
# docker exec -it 9e37cd90c168586c10d52790a9d5fbdf49acb394cc914bafe41400654c48758b /bin/bash
root@9e37cd90c168:/# cat /opt/data/hello.txt
hell from node1
hello from node2
root@9e37cd90c168:/# exit
#
```

## 備份、還原
```
# docker run --volumes-from node1 -v $(pwd):/backup ubuntu tar vcf /backup/data.tar /opt/data/
tar: Removing leading `/' from member names
/opt/data/
/opt/data/hello.txt
# ls
data.tar
# tar -tf data.tar
opt/data/
opt/data/hello.txt
```
```
# docker exec -it node1 /bin/bash
root@9e37cd90c168:/# rm -f /opt/data/hello.txt
root@9e37cd90c168:/# exit
#
# docker run --volumes-from node1 -v $(pwd):/backup ubuntu tar vxf /backup/data.tar
opt/data/
opt/data/hello.txt
#
# docker exec -it node1 /bin/bash
root@9e37cd90c168:/# cat /opt/data/hello.txt
hell from node1
hello from node2
root@9e37cd90c168:/# exit
#
```
