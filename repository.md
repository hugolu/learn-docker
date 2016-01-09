# Repository

# search
搜尋 docker hub 上面的 image
```
# docker search centos
```

# pull
下載 images 到本地
```
# docker pull centos
```

# 使用 registry 創建本地 private repository
```
# mkdir -p /opt/data/registry
# docker run -d -p 5000:5000 -v /opt/data/registry/:/tmp/registry registry
2d69060209fa8bd41bfbfb9f84c880c425dafc51b8ce4fb80fdc9d43e87c05b7
# docker tag test:latest 127.0.0.1:5000/test
# docker images
REPOSITORY            TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
test                  latest              b1a09f7b1d92        25 minutes ago      187.9 MB
127.0.0.1:5000/test   latest              b1a09f7b1d92        25 minutes ago      187.9 MB
registry              latest              f7dc621663e4        4 days ago          422.8 MB
ubuntu                latest              d55e68e6cc9c        4 weeks ago         187.9 MB
# docker push 127.0.0.1:5000/test
The push refers to a repository [127.0.0.1:5000/test] (len: 1)
Sending image list
Pushing repository 127.0.0.1:5000/test (1 tags)
9377ad319b00: Image successfully pushed
a82f81f25750: Image successfully pushed
b207c06aba70: Image successfully pushed
d55e68e6cc9c: Image successfully pushed
b1a09f7b1d92: Image successfully pushed
Pushing tag for rev [b1a09f7b1d92] on {http://127.0.0.1:5000/v1/repositories/test/tags/latest}
# curl http://127.0.0.1:5000/v1/search
{"num_results": 1, "query": "", "results": [{"description": "", "name": "library/test"}]}
# docker rmi test
Untagged: test:latest
# docker rmi 127.0.0.1:5000/test
Untagged: 127.0.0.1:5000/test:latest
Deleted: b1a09f7b1d92f95f6d9d561a2c2f49415d61bf2d9263a74df43ab80662d8af2d
# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
registry            latest              f7dc621663e4        4 days ago          422.8 MB
ubuntu              latest              d55e68e6cc9c        4 weeks ago         187.9 MB
# docker pull 127.0.0.1:5000/test
Pulling repository 127.0.0.1:5000/test
b1a09f7b1d92: Download complete
9377ad319b00: Download complete
a82f81f25750: Download complete
b207c06aba70: Download complete
d55e68e6cc9c: Download complete
Status: Downloaded newer image for 127.0.0.1:5000/test:latest
# docker images
REPOSITORY            TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
127.0.0.1:5000/test   latest              b1a09f7b1d92        30 minutes ago      187.9 MB
registry              latest              f7dc621663e4        4 days ago          422.8 MB
ubuntu                latest              d55e68e6cc9c        4 weeks ago         187.9 MB
```
