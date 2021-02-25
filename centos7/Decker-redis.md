# Docker 安装 Redis



```shell

mkdir -p /mydata/redis/conf
touch /mydata/redis/conf/redis.conf

# 从镜像市场下载
docker pull redis

#　运行
docker run -p 6379:6379 --name redis \
-v /mydata/redis/data:/data \
-v /mydata/redis/conf/redis.conf:/etc/redis/redis.conf \
-d redis redis-server /etc/redis/redis.conf

# 直接进去redis客户端。
docker exec -it redis redis-cli
```

AOP持久化

```shell
vim /mydata/redis/conf/redis.conf
# 插入下面内容
appendonly yes
保存

docker restart redis
```

设置启动Docker启动时启动Redis

```shell
docker update redis --restart=always
```

