上一章中我们的 Redis 使用 Docker 部署非常方便，所以这里我们要把 Kafka 和 MySQL 都部署到 Docker 上来，方便管理和后续项目部署。

## 1. Docker 部署 Kafka

由于 Kafka 依赖于 Zookeeper，所以需要先部署 Zookeeper。

这里只部署单机的 Kafka，不涉及集群的内容。

### 1.1 部署 Zookeeper

拉取 Zookeeper3.6 镜像：

```sh
docker pull zookeeper:3.6
```

run 一个容器，命令很简单，没什么需要解释的：

```sh
docker run -d --name zookeeper -p 2181:2181 \
-v /home/zookeeper/data:/data \
-v /home/zookeeper/log:/datalog \
-v /etc/localtime:/etc/localtime \
zookeeper:3.6
```

docker -ps：

```sh
☁  ~  docker ps
CONTAINER ID   IMAGE           COMMAND                  CREATED          STATUS          PORTS                                                  NAMES
3315dc258eed   zookeeper:3.6   "/docker-entrypoint.…"   14 minutes ago   Up 14 minutes   2888/tcp, 3888/tcp, 0.0.0.0:2181->2181/tcp, 8080/tcp   zookeeper
```

### 1.2 部署 Kafka

拉取 Kafka2.8.0 镜像：

```sh
docker pull bitnami/kafka:2.8.0
```

run 一个容器：

```sh
docker run -d --name kafka --publish 9092:9092 \
--link zookeeper \
--env KAFKA_ZOOKEEPER_CONNECT=175.24.165.212:2181 \
--env KAFKA_ADVERTISED_HOST_NAME=175.24.165.212 \
--env KAFKA_ADVERTISED_PORT=9092 \
--env ALLOW_PLAINTEXT_LISTENER=yes \
--env listeners=PLAINTEXT://175.24.165.212:9092 \
--env advertised.listeners=PLAINTEXT://175.24.165.212:9092 \
-v /home/kafka:/bitnami/kafka \
-v /etc/localtime:/etc/localtime \
bitnami/kafka:2.8.0
```

注：要加 `ALLOW_PLAINTEXT_LISTENER=yes`，否则启动会报错。

如果遇到 mkdir 无权限，这是因为在容器内创建目录 `/bitnami/kafka/config` 时，由于权限问题无法创建。这是由于默认情况下，Docker 容器中的进程以非特权用户身份运行，无法在容器内创建目录或文件。所以需要在主机上更改 `/home/kafka` 目录的权限，以便容器中的进程可以访问它：

```sh
sudo chmod 777 /home/kafka
```

docker ps：

```sh
☁  /  docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED             STATUS             PORTS                                                  NAMES
d511feeb45f7   bitnami/kafka:2.8.0   "/opt/bitnami/script…"   2 minutes ago       Up 2 minutes       0.0.0.0:9092->9092/tcp                                 kafka
3315dc258eed   zookeeper:3.6         "/docker-entrypoint.…"   59 minutes ago      Up 59 minutes      2888/tcp, 3888/tcp, 0.0.0.0:2181->2181/tcp, 8080/tcp   zookeeper
```

## 2. 创建 Topic

在我们的 Kafka 容器内创建项目需要的两个 topic，这里就不进入容器执行了：

```sh
# lottery_invoice
docker exec kafka kafka-topics.sh --create --zookeeper 175.24.165.212:2181 --replication-factor 1 --partitions 1 --topic lottery_invoice

# lottery_activity_partake topic：
docker exec kafka kafka-topics.sh --create --zookeeper 175.24.165.212:2181 --replication-factor 1 --partitions 1 --topic lottery_activity_partake
```

查看 topics：

```sh
docker exec kafka kafka-topics.sh --list --zookeeper 175.24.165.212:2181
lottery_activity_partake
lottery_invoiceoice
```

> 待续。。。本地连接失败：Error connecting to node fa42bb7573c1:9092









