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