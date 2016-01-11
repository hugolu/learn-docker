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

## 設定 Replica Set

登入 node1
```
# docker exec -ti node1 /bin/bash
root@node1:/#
```

登入 mongo shell
```
root@node1:/# mongo
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

定義 replica set (for copy & paste)
```
var demoConfig = { 
    _id: "demo", 
    members: [
        { _id: 0, 
          host: '172.17.0.1:27017', 
          priority: 10
        },
        { _id: 1, 
          host: '172.17.0.2:27017'
        }, 
        { _id: 2, 
          host: '172.17.0.3:27017', 
          arbiterOnly: true
        }
    ] 
 };
 ```
 
 在mongo shell執行以下操作
 ```
 > var demoConfig = {
...     _id: "demo",
...     members: [
...         { _id: 0,
...           host: '172.17.0.1:27017',
...           priority: 10
...         },
...         { _id: 1,
...           host: '172.17.0.2:27017'
...         },
...         { _id: 2,
...           host: '172.17.0.3:27017',
...           arbiterOnly: true
...         }
...     ]
...  };
>
> rs.initiate(demoConfig)
{ "ok" : 1 }
demo:OTHER> rs.conf()
{
	"_id" : "demo",
	"version" : 1,
	"protocolVersion" : NumberLong(1),
	"members" : [
		{
			"_id" : 0,
			"host" : "172.17.0.1:27017",
			"arbiterOnly" : false,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 10,
			"tags" : {

			},
			"slaveDelay" : NumberLong(0),
			"votes" : 1
		},
		{
			"_id" : 1,
			"host" : "172.17.0.2:27017",
			"arbiterOnly" : false,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 1,
			"tags" : {

			},
			"slaveDelay" : NumberLong(0),
			"votes" : 1
		},
		{
			"_id" : 2,
			"host" : "172.17.0.3:27017",
			"arbiterOnly" : true,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 1,
			"tags" : {

			},
			"slaveDelay" : NumberLong(0),
			"votes" : 1
		}
	],
	"settings" : {
		"chainingAllowed" : true,
		"heartbeatIntervalMillis" : 2000,
		"heartbeatTimeoutSecs" : 10,
		"electionTimeoutMillis" : 10000,
		"getLastErrorModes" : {

		},
		"getLastErrorDefaults" : {
			"w" : 1,
			"wtimeout" : 0
		}
	}
}
demo:SECONDARY>
```

等候數秒，觀察 relica set 狀態。
- node1 成為 PRIMARY
- node2 成為 SECONDARY
- node3 成為 ARBITER
```
demo:SECONDARY>
demo:PRIMARY>
demo:PRIMARY> rs.status()
{
	"set" : "demo",
	"date" : ISODate("2016-01-11T07:14:58.834Z"),
	"myState" : 1,
	"term" : NumberLong(1),
	"heartbeatIntervalMillis" : NumberLong(2000),
	"members" : [
		{
			"_id" : 0,
			"name" : "172.17.0.1:27017",
			"health" : 1,
			"state" : 1,
			"stateStr" : "PRIMARY",
			"uptime" : 1463,
			"optime" : {
				"ts" : Timestamp(1452496398, 1),
				"t" : NumberLong(1)
			},
			"optimeDate" : ISODate("2016-01-11T07:13:18Z"),
			"infoMessage" : "could not find member to sync from",
			"electionTime" : Timestamp(1452496397, 1),
			"electionDate" : ISODate("2016-01-11T07:13:17Z"),
			"configVersion" : 1,
			"self" : true
		},
		{
			"_id" : 1,
			"name" : "172.17.0.2:27017",
			"health" : 1,
			"state" : 2,
			"stateStr" : "SECONDARY",
			"uptime" : 111,
			"optime" : {
				"ts" : Timestamp(1452496398, 1),
				"t" : NumberLong(1)
			},
			"optimeDate" : ISODate("2016-01-11T07:13:18Z"),
			"lastHeartbeat" : ISODate("2016-01-11T07:14:57.341Z"),
			"lastHeartbeatRecv" : ISODate("2016-01-11T07:14:57.312Z"),
			"pingMs" : NumberLong(0),
			"syncingTo" : "172.17.0.1:27017",
			"configVersion" : 1
		},
		{
			"_id" : 2,
			"name" : "172.17.0.3:27017",
			"health" : 1,
			"state" : 7,
			"stateStr" : "ARBITER",
			"uptime" : 111,
			"lastHeartbeat" : ISODate("2016-01-11T07:14:57.341Z"),
			"lastHeartbeatRecv" : ISODate("2016-01-11T07:14:54.280Z"),
			"pingMs" : NumberLong(0),
			"configVersion" : 1
		}
	],
	"ok" : 1
}
```

## 插入資料 @node1

在node1的mongo shell內，插入資料
```
demo:PRIMARY> db.students.insert([
... {"id":"001","name":"AAA"},
... {"id":"002","name":"BBB"},
... {"id":"003","name":"CCC"}
... ])
BulkWriteResult({
	"writeErrors" : [ ],
	"writeConcernErrors" : [ ],
	"nInserted" : 3,
	"nUpserted" : 0,
	"nMatched" : 0,
	"nModified" : 0,
	"nRemoved" : 0,
	"upserted" : [ ]
})
```

檢索插入結果
```
demo:PRIMARY> db.students.find()
{ "_id" : ObjectId("569356c68afe17c2b2c1dbb3"), "id" : "001", "name" : "AAA" }
{ "_id" : ObjectId("569356c68afe17c2b2c1dbb4"), "id" : "002", "name" : "BBB" }
{ "_id" : ObjectId("569356c68afe17c2b2c1dbb5"), "id" : "003", "name" : "CCC" }
```

退出 mongo shell，離開 node1
```
demo:PRIMARY> exit
bye
root@node1:/# exit
exit
#
```

## 檢索資料 @node2

登入 node2，進入 mongo shell
```
# docker exec -ti node2 /bin/bash
root@node2:/# mongo
demo:SECONDARY>
```

設定 Secondary 可讀
```
demo:SECONDARY> db.getMongo().setSlaveOk();
```

讀取資料，確定 replication 機制成功。
```
demo:SECONDARY> db.students.find();
{ "_id" : ObjectId("569356c68afe17c2b2c1dbb3"), "id" : "001", "name" : "AAA" }
{ "_id" : ObjectId("569356c68afe17c2b2c1dbb4"), "id" : "002", "name" : "BBB" }
{ "_id" : ObjectId("569356c68afe17c2b2c1dbb5"), "id" : "003", "name" : "CCC" }
```

退出 mongo shell，離開 node2
```
demo:SECONDARY> exit
bye
root@node2:/# exit
exit
#
```
