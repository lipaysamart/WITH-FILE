## 简介

Redis 6是Redis 有史以来最大的一个版本：对客户端缓存某方面的功能进行了重新设计，主要是放弃了 "缓存插槽"（caching slot）改为使用键名（key name）。

## 使用

```sh
docker-compose up -d
```
or
```sh
docker run --name redis \
-p 6379:6379 \
-v ./redis/redis.conf:/etc/redis/redis.conf \
-v ./data:/data \
-d registry.cn-guangzhou.aliyuncs.com/kubernetes-default/redis:6.0.20 redis-server /etc/redis/redis.conf
```

### 官方配置文件

```bash
wget https://raw.githubusercontent.com/redis/redis/6.0/redis.conf
```