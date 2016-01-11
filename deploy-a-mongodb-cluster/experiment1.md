# 實驗ㄧ：Dockerizing MongoDB on a VM

參考連結
- [mongo Shell Quick Reference](https://docs.mongodb.org/v3.2/reference/mongo-shell/)
- [MongoDB shell命令行的使用](http://www.2cto.com/database/201210/159130.html)

## 準備

啟動 Ubuntu/VM，使用 ssh 登入，切換使用者 ```root```
```
$ vagrant up
$ vagrant ssh
$ sudo su -
#
```

## 下載 MongoDB Docker Image

搜尋適合的 Image
```
# docker search mongodb
NAME                    DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mongo                   MongoDB document databases provide high av...   1251      [OK]
tutum/mongodb           MongoDB Docker image – listens in port 2...     87                   [OK]
frodenas/mongodb        A Docker Image for MongoDB                      6                    [OK]
...
```

下載 Image
```
# docker pull mongo
# docker images mongo
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
mongo               latest              f2a1deabdc69        4 weeks ago         316.9 MB
```

## 操作 Mongodb

啟動 container
```
# docker run --name mongo -d mongo
2581dd980b2f92e9e4ebd041011fccc9314193158d50e33b9b511098594d9802
# docker ps -a
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS               NAMES
2581dd980b2f        mongo:latest        "/entrypoint.sh mong   4 seconds ago       Up 3 seconds        27017/tcp           mongo
# docker exec -ti mongo /bin/bash
```

啟動 mongodb
```
root@2581dd980b2f:/# mongo
MongoDB shell version: 3.2.0
connecting to: test
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
	http://docs.mongodb.org/
Questions? Try the support group
	http://groups.google.com/group/mongodb-user
>
```

操作資料庫
```
> db.students.insert(
... [
... {"id":"001","name":"AAA"},
... {"id":"002","name":"BBB"},
... {"id":"003","name":"CCC"}
... ]
... )
> db.students.count();
3
> db.students.find({"id":"002"});
{ "_id" : ObjectId("56926393571f34687b4620f2"), "id" : "002", "name" : "BBB" }
> exit
```

離開並停止container
```
root@2581dd980b2f:/# exit
#
# docker stop mongo
mongo
```
