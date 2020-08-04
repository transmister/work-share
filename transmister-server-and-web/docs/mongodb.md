# MongoDB

## Install docker

https://www.docker.com/get-started

## Get the Docker Image of MongoDB

``` bash
docker pull mongo
```

## Use a mirror in China Mainland (Optional)

Edit `daemon.json` as:

```json
{
  "registry-mirrors" : [
    "http://registry.docker-cn.com",
    "http://docker.mirrors.ustc.edu.cn",
    "http://hub-mirror.c.163.com"
  ],
  "insecure-registries" : [
    "registry.docker-cn.com",
    "docker.mirrors.ustc.edu.cn"
  ]
}
```


# Run MongoDB
``` bash
docker run -itd -v <LocalDirectoryPath>:/data/db --name mongo -p 27017:27017 mongo
```

> [Referance (zh_CN)](https://www.runoob.com/docker/docker-install-mongodb.html)

## Use MongoDB in Node.js

> [Official Documentation (MongoDB Node.js Driver)](https://docs.mongodb.com/drivers/node)
> [mongoose](https://mongoosejs.com/)
