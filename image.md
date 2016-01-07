# Image

## search

到docker hub搜尋相關image
```
docker search [--automated[=false]] [--help] [--no-trunc[=false]] [-s|--stars[=0]] TERM
```

範例
```
$ sudo docker search ubuntu
```

## pull

從網路repository下載image
```
docker pull [-a|--all-tags[=false]] [--help] NAME[:TAG]
```

範例
```
$ sudo docker pull ubuntu
```

## images

列出本地主機有哪些image
```
docker images [--help] [-a|--all[=false]] [--digests[=false]] [-f|--filter[=[]]] [--no-trunc[=false]] [-q|--quiet[=false]] [REPOSITORY]
```

範例
```
$ sudo docker images
```

## inspect

獲取image或container詳細訊息
```
docker inspect [--help] [-f|--format[=FORMAT]] CONTAINER|IMAGE [CONTAINER|IMAGE...]
```

example
```
$ sudo docker inspect ubuntu
```

## tag

添加image標籤
```
docker tag [-f|--force[=false]] [--help] IMAGE[:TAG] [REGISTRY_HOST/][USERNAME/]NAME[:TAG]
```

範例
```
$ sudo docker tag ubuntu:latest ubuntu:mytest
```

## rmi

移除image。移除最後一個tag前，不會刪除image
```
docker rmi [-f|--force[=false]] [--help] [--no-prune[=false]] IMAGE [IMAGE...]
```

範例
```
$ sudo docker rmi ubuntu:mytest
```

## run

利用image創建一個container，然後運行
```
docker  run  [-a|--attach[=[]]]  [--add-host[=[]]]  [-c|--cpu-shares[=0]]  [--cap-add[=[]]]  [--cap-drop[=[]]]
       [--cidfile[=CIDFILE]] [--cpuset-cpus[=CPUSET-CPUS]] [-d|--detach[=false]] [--device[=[]]]  [--dns-search[=[]]]
       [--dns[=[]]]   [-e|--env[=[]]]   [--entrypoint[=ENTRYPOINT]]   [--env-file[=[]]]  [--expose[=[]]]  [-h|--host‐
       name[=HOSTNAME]]  [--help]  [-i|--interactive[=false]]  [--ipc[=IPC]]  [-l|--label[=[]]]   [--label-file[=[]]]
       [--link[=[]]]   [--lxc-conf[=[]]]   [--log-driver[=[]]]  [-m|--memory[=MEMORY]]  [--memory-swap[=MEMORY-SWAP]]
       [--mac-address[=MAC-ADDRESS]]   [--name[=NAME]]   [--net[="bridge"]]   [-P|--publish-all[=false]]   [-p|--pub‐
       lish[=[]]]  [--pid[=[]]]  [--privileged[=false]]  [--read-only[=false]]  [--restart[=RESTART]]  [--rm[=false]]
       [--security-opt[=[]]] [--sig-proxy[=true]] [-t|--tty[=false]]  [-u|--user[=USER]]  [-v|--volume[=[]]]  [--vol‐
       umes-from[=[]]] [-w|--workdir[=WORKDIR]] [--cgroup-parent[=CGROUP-PATH]] IMAGE [COMMAND] [ARG...]
```

範例
```
$ sudo docker run -ti ubuntu:latest /bin/bash
root@1acd1462f25e:/# touch test
root@1acd1462f25e:/# exit
```

## commit
基於已有的image的container創建一個新的image
```
docker commit [-a|--author[=AUTHOR]] [--help] [-c|--change[=[]]] [-m|--message[=MESSAGE]]
       [-p|--pause*[=*true]] CONTAINER [REPOSITORY[:TAG]]
```

範例
```
$ sudo docker commit -m "added a new file" -a "docker newbee" 1acd1462f25e test
76f907542ce8af838cdb5d07f872c258036f1839f7cac751e445df0b4d6cddb3

$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
test                latest              76f907542ce8        17 seconds ago      187.9 MB
ubuntu              latest              d55e68e6cc9c        3 weeks ago         187.9 MB
```

## save、load

將image存到本地文件
```
docker save [--help] [-o|--output[=OUTPUT]] IMAGE [IMAGE...]
```

將本地文件導到本地image repository
```
docker load [--help] [-i|--input[=INPUT]]
```

範例
```
$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
test                latest              76f907542ce8        9 minutes ago       187.9 MB
ubuntu              latest              d55e68e6cc9c        3 weeks ago         187.9 MB

$ sudo docker rmi test:latest
Untagged: test:latest
Deleted: 76f907542ce8af838cdb5d07f872c258036f1839f7cac751e445df0b4d6cddb3

$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
ubuntu              latest              d55e68e6cc9c        3 weeks ago         187.9 MB

$ sudo docker load -i test.latest.tar

$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
test                latest              76f907542ce8        12 minutes ago      187.9 MB
ubuntu              latest              d55e68e6cc9c        3 weeks ago         187.9 MB
```
