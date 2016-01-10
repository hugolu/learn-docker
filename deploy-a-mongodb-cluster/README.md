# 用 Docker 實現 Mongodb cluster

這是一邊閱讀[Deploy a MongoDB Cluster in 9 steps Using Docker](https://medium.com/@gargar454/deploy-a-mongodb-cluster-in-steps-9-using-docker-49205e231319#.tuztjshnt)一邊練習的筆記。完整說明建議參考原出處。

實驗環境
- Host OS: OS X 10.10.5
- Guest OS: ubuntu/trusty64 on VirtualBox/Vagrant

實驗目標
- 階段一：在一台vm上跑一個 docker container，執行一個 mongodb
- 階段二：在一台vm上跑三個 docker container，組成一個 monbodb cluster 
- 階段三：在三台vm上面各跑一個 docker container，組成一個 mongodb cluster
