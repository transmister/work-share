# MongoDB

## 安装docker

https://www.docker.com/get-started

## 获取mongo镜像

``` bash
docker pull mongo
```

## 运行mongo

``` bash
docker run -itd -v <LocalDirectoryPath>:/data/db --name mongo -p 27017:27017 mongo
```

> [参考](https://www.runoob.com/docker/docker-install-mongodb.html)

## 在Node.js中使用mongoDB
> [官方参考文档 (MongoDB Node.js Driver)](https://docs.mongodb.com/drivers/node)
