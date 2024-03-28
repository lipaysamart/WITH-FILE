## Usage

>从上至下，依次执行

```sh
docker network create --subnet "172.31.238.0/24" --gateway "172.31.238.1" cloudcanal-network
docker volume create clougence_mysql_volume
```

```sh
docker-compose up -d
```

>执行时需要确认容器状态为 Running

```sh
docker exec cloudcanal-console bash -c "cd /home/clougence/cloudcanal/console/bin;su clougence -c "./stopConsole.sh""
docker exec cloudcanal-console bash -c "cd /home/clougence/cloudcanal/console/bin;su clougence -c "./startConsoleAndUpdDB.sh""
```

```sh
ln -s $(docker volume inspect $(docker volume ls | grep cloudcanal_console | awk '{print $2}') | grep Mountpoint | grep -o /.*_data) console_data
ln -s $(docker volume inspect $(docker volume ls | grep cloudcanal_sidecar | awk '{print $2}') | grep Mountpoint | grep -o /.*_data) sidecar_data
```

>查看日志

```sh
tail -f ls console_data/cloudcanal/console/logs/console.log
```