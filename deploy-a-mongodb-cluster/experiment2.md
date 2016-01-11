# 實驗二：Dockerizing MongoDB cluster on a VM

參考連結
- [Deploy a Replica Set](https://docs.mongodb.org/manual/tutorial/deploy-replica-set/)
- [Deploy a MongoDB Cluster in 9 steps Using Docker](https://medium.com/@gargar454/deploy-a-mongodb-cluster-in-steps-9-using-docker-49205e231319#.tuztjshnt)
- [MongoDB - Setting up a Replica Set](https://gist.github.com/leommoore/6742850)
- [MongoDb的“not master and slaveok=false”错误及解决方法](http://www.blogdaren.com/m/?post=2158)

基本程序
- 啟動要成為 replica set 的多個 mongod 實體，設定 replica set 並把那些 mongod 實體加入其中。

## 準備

啟動 Ubuntu/VM，使用 ssh 登入，切換使用者 ```root```
```
$ vagrant up
$ vagrant ssh
$ sudo su -
#
```

## 啟動 mongo 節點

啟動三個 mongo container
```
# docker run --name node1 --hostname node1 -p 27017:27017 -d mongo --replSet "demo"
9845f7d63a9ea564c1102e6a514421e7bd7d1da84c9246975e8c9d8e0186ae21
# docker run --name node2 --hostname node2 -p 27018:27017 -d mongo --replSet "demo"
c15faa77f9d349784c12781172628c139ebd5a2a0db0d8839a9d89aafe30c0a0
# docker run --name node3 --hostname node3 -p 27019:27017 -d mongo --replSet "demo"
6ed1aea17f30dd778549b244b96e0c576abb675576dd2eebe693c3bd5f173cc6
```
- ```--name node#``` docker container 名稱
- ```--hostname node#``` login container 看到的主機名稱
- ```-p <host port>:<container port>``` container port = 27017，但 port-mapping 到主機不同 port 上
- ```-d mongo --replSet "demo"``` mongo背景執行，replic set name 設為 "demo"

觀察 container 狀態
```
# docker ps -a
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                      NAMES
6ed1aea17f30        mongo:latest        "/entrypoint.sh --re   24 seconds ago      Up 23 seconds       0.0.0.0:27019->27017/tcp   node3
c15faa77f9d3        mongo:latest        "/entrypoint.sh --re   26 seconds ago      Up 25 seconds       0.0.0.0:27018->27017/tcp   node2
9845f7d63a9e        mongo:latest        "/entrypoint.sh --re   26 seconds ago      Up 25 seconds       0.0.0.0:27017->27017/tcp   node1
```

查詢 container IP
```
# docker inspect --format '{{ .NetworkSettings.IPAddress }}' node1
172.17.0.1
# docker inspect --format '{{ .NetworkSettings.IPAddress }}' node2
172.17.0.2
# docker inspect --format '{{ .NetworkSettings.IPAddress }}' node3
172.17.0.3
```


<< 未完待續 >>
