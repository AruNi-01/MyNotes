## 1. 部署 Portainer

下载 Docker，然后部署 Portainer，由于最新版的 portainer-ce 界面是社区版，会有很多广告让你升级，所以这里下 2.14.2 版，用起来比较舒服：

```sh
docker run -d  --name portainer -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v /app/portainer_data:/data --restart always --privileged=true portainer/portainer-ce:2.14.2
```

访问 ip:9000 即可。

## 2. 部署 Redis

### 2.1 拉取 Redis 镜像

我们这里选择的是 6.2.6 版本：

```sh
☁  ~  docker pull redis:6.2.6
# ......

☁  ~  docker images
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
redis        6.2.6     3c3da61c4be0   13 months ago   113MB
```

### 2.2 创建挂载的文件

我们现在只需要把镜像 run 起来，就能得到一个运行的 redis 容器了。

不过我们需要提前将文件挂载好，比如配置文件，持久化文件，保存到我们的宿主机上来，否则容器挂了我们的数据也丢了。（**挂载**：即将宿主的文件和容器内部目录相关联，相互绑定，在宿主机内修改文件的话也随之修改容器内部文件）

- 配置文件 `redis.conf`：在宿主机自己创建文件，将配置文件的内容粘贴进去。用于 redis 实例的启动；

  > 在 Redis 官网可找到配置文件的内容：https://redis.io/docs/management/config/

- 持久化文件夹 `data`：自己创建即可。

我创建的文件如下：

```sh
☁  redis-lottery-volume  pwd
/home/redis-lottery-volume
☁  redis-lottery-volume  ls
data  redis.conf
```

### 2.3 启动 Redis 容器

现在只需要把 redis 镜像 run 起来，就可得到 redis 容器了，在 run 镜像的时候，把文件给挂载上去即可。

启动命令：

```sh
docker run -p 6379:6379 --name redis-lottery -v /home/redis-lottery-volume/redis.conf:/etc/redis/redis.conf -v /home/redis-lottery-volume/data:/data -d redis:6.2.6 redis-server /etc/redis/redis.conf
```

- `-p 6379:6379` 将 6379 端口映射出去
- `--name` 给这个容器取一个名字
- `-v` 数据卷挂载
  - `/home/redis-lottery-volume/redis.conf:/etc/redis/redis.conf` 将 liunx 路径下的redis.conf 和容器内 redis 下的 redis.conf 挂载在一起；
  - `/home/redis-lottery-volume/data:/data` 这个同上。
- `-d` 表示后台启动 redis
- `redis-server /etc/redis/redis.conf` 以配置文件启动 redis，加载容器内的 conf 文件，最终找到的是挂载的目录，也就是 liunx下的 `/home/redis-lottery-volume/redis.conf`

> 注意：`redis.conf` 中的 daemonize 不要设置为 yes，因为 docker 是以后台进程启动的 redis，redis 不能再以后台进程启动了，否则会出错。

启动成功：

![image-20230520115646124](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305201156616.png)

查看容器运行日志：`docker logs --since 30m redis-lottery`

进入容器：

```sh
☁  redis-lottery-volume  docker exec -it redis-lottery /bin/bash
root@fdeed99aefbd:/data# redis-cli

127.0.0.1:6379> ping
(error) NOAUTH Authentication required.
127.0.0.1:6379> auth 123456
OK
127.0.0.1:6379> ping
PONG
```



## 3. 部署 Kafka

拉取镜像：

```sh
docker pull wurstmeister/kafka
docker pull wurstmeister/zookeeper
```

启动 zookeeper：

```sh
docker run -d --name zookeeper -p 2181:2181 -v /home/zookeeper-lottery-volume:/data -t wurstmeister/zookeeper
```

启动 kafka：

```sh
docker run -d --name kafka \
-v /home/kafka-lottery-volume:/bitnami/kafka \
-p 9092:9092 \
-e KAFKA_BROKER_ID=0 \
-e KAFKA_ZOOKEEPER_CONNECT=175.24.165.212:2181 \
-e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://175.24.165.212:9092 \
-e KAFKA_LISTENERS=PLAINTEXT://0.0.0.0:9092 wurstmeister/kafka
```

进入 kafka 容器，在 kafka 目录下 `/opt/kafka_2.13-2.8.1` 执行脚本命令，添加两个 topic：

```sh
bin/kafka-topics.sh --create --zookeeper 175.24.165.212:2181 --replication-factor 1 --partitions 1 --topic lottery_activity_partake

bin/kafka-topics.sh --create --zookeeper 175.24.165.212:2181 --replication-factor 1 --partitions 1 --topic lottery_invoice
```

使用 portainer 进入容器操作会比较方便：

![image-20230603143728374](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202306031437996.png)

## 4. 部署 MySQL

拉取镜像：

```sh
docker pull mysql
```

运行：

```sh
docker run -itd --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql
```

到 Navicat 中导出本地的数据库文件，然后连接服务器的 MySQL，导入库表数据。

## 5. 部署 xxl-job

xxl-job 需要一个 MySQL 的表，直接在连接上新建查询，执行下面的 SQL 语句即可：

```sql
/*
 Navicat Premium Data Transfer

 Source Server         : localmysql
 Source Server Type    : MySQL
 Source Server Version : 80033 (8.0.33)
 Source Host           : localhost:3306
 Source Schema         : xxl_job

 Target Server Type    : MySQL
 Target Server Version : 80033 (8.0.33)
 File Encoding         : 65001

 Date: 03/06/2023 20:20:42
*/

CREATE DATABASE xxl_job;

use xxl_job;

SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for xxl_job_group
-- ----------------------------
DROP TABLE IF EXISTS `xxl_job_group`;
CREATE TABLE `xxl_job_group` (
  `id` int NOT NULL AUTO_INCREMENT,
  `app_name` varchar(64) NOT NULL COMMENT '执行器AppName',
  `title` varchar(12) NOT NULL COMMENT '执行器名称',
  `address_type` tinyint NOT NULL DEFAULT '0' COMMENT '执行器地址类型：0=自动注册、1=手动录入',
  `address_list` text COMMENT '执行器地址列表，多地址逗号分隔',
  `update_time` datetime DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

-- ----------------------------
-- Records of xxl_job_group
-- ----------------------------
BEGIN;
INSERT INTO `xxl_job_group` (`id`, `app_name`, `title`, `address_type`, `address_list`, `update_time`) VALUES (1, 'xxl-job-executor-sample', '示例执行器', 0, NULL, '2023-06-03 20:20:14');
INSERT INTO `xxl_job_group` (`id`, `app_name`, `title`, `address_type`, `address_list`, `update_time`) VALUES (4, 'lottery', 'MyLottery', 0, 'http://10.0.0.25:9001/', '2023-06-03 20:20:14');
COMMIT;

-- ----------------------------
-- Table structure for xxl_job_info
-- ----------------------------
DROP TABLE IF EXISTS `xxl_job_info`;
CREATE TABLE `xxl_job_info` (
  `id` int NOT NULL AUTO_INCREMENT,
  `job_group` int NOT NULL COMMENT '执行器主键ID',
  `job_desc` varchar(255) NOT NULL,
  `add_time` datetime DEFAULT NULL,
  `update_time` datetime DEFAULT NULL,
  `author` varchar(64) DEFAULT NULL COMMENT '作者',
  `alarm_email` varchar(255) DEFAULT NULL COMMENT '报警邮件',
  `schedule_type` varchar(50) NOT NULL DEFAULT 'NONE' COMMENT '调度类型',
  `schedule_conf` varchar(128) DEFAULT NULL COMMENT '调度配置，值含义取决于调度类型',
  `misfire_strategy` varchar(50) NOT NULL DEFAULT 'DO_NOTHING' COMMENT '调度过期策略',
  `executor_route_strategy` varchar(50) DEFAULT NULL COMMENT '执行器路由策略',
  `executor_handler` varchar(255) DEFAULT NULL COMMENT '执行器任务handler',
  `executor_param` varchar(512) DEFAULT NULL COMMENT '执行器任务参数',
  `executor_block_strategy` varchar(50) DEFAULT NULL COMMENT '阻塞处理策略',
  `executor_timeout` int NOT NULL DEFAULT '0' COMMENT '任务执行超时时间，单位秒',
  `executor_fail_retry_count` int NOT NULL DEFAULT '0' COMMENT '失败重试次数',
  `glue_type` varchar(50) NOT NULL COMMENT 'GLUE类型',
  `glue_source` mediumtext COMMENT 'GLUE源代码',
  `glue_remark` varchar(128) DEFAULT NULL COMMENT 'GLUE备注',
  `glue_updatetime` datetime DEFAULT NULL COMMENT 'GLUE更新时间',
  `child_jobid` varchar(255) DEFAULT NULL COMMENT '子任务ID，多个逗号分隔',
  `trigger_status` tinyint NOT NULL DEFAULT '0' COMMENT '调度状态：0-停止，1-运行',
  `trigger_last_time` bigint NOT NULL DEFAULT '0' COMMENT '上次调度时间',
  `trigger_next_time` bigint NOT NULL DEFAULT '0' COMMENT '下次调度时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

-- ----------------------------
-- Records of xxl_job_info
-- ----------------------------
BEGIN;
INSERT INTO `xxl_job_info` (`id`, `job_group`, `job_desc`, `add_time`, `update_time`, `author`, `alarm_email`, `schedule_type`, `schedule_conf`, `misfire_strategy`, `executor_route_strategy`, `executor_handler`, `executor_param`, `executor_block_strategy`, `executor_timeout`, `executor_fail_retry_count`, `glue_type`, `glue_source`, `glue_remark`, `glue_updatetime`, `child_jobid`, `trigger_status`, `trigger_last_time`, `trigger_next_time`) VALUES (1, 1, '测试任务1', '2018-11-03 22:21:31', '2023-05-09 22:42:31', 'XXL', '', 'CRON', '0 0 0 * * ? *', 'DO_NOTHING', 'FIRST', 'demoJobHandler', '', 'SERIAL_EXECUTION', 0, 0, 'BEAN', '', 'GLUE代码初始化', '2018-11-03 22:21:31', '', 0, 0, 0);
INSERT INTO `xxl_job_info` (`id`, `job_group`, `job_desc`, `add_time`, `update_time`, `author`, `alarm_email`, `schedule_type`, `schedule_conf`, `misfire_strategy`, `executor_route_strategy`, `executor_handler`, `executor_param`, `executor_block_strategy`, `executor_timeout`, `executor_fail_retry_count`, `glue_type`, `glue_source`, `glue_remark`, `glue_updatetime`, `child_jobid`, `trigger_status`, `trigger_last_time`, `trigger_next_time`) VALUES (4, 4, '抽奖系统任务，扫描活动状态为审核通过、已过期，做对应的活动状态更变', '2023-05-11 21:31:18', '2023-05-11 21:50:03', 'AarynLu', '', 'FIX_RATE', '10', 'DO_NOTHING', 'FIRST', 'lotteryActivityStateJobHandler', '', 'SERIAL_EXECUTION', 0, 0, 'BEAN', '', 'GLUE代码初始化', '2023-05-11 21:31:18', '', 0, 0, 0);
INSERT INTO `xxl_job_info` (`id`, `job_group`, `job_desc`, `add_time`, `update_time`, `author`, `alarm_email`, `schedule_type`, `schedule_conf`, `misfire_strategy`, `executor_route_strategy`, `executor_handler`, `executor_param`, `executor_block_strategy`, `executor_timeout`, `executor_fail_retry_count`, `glue_type`, `glue_source`, `glue_remark`, `glue_updatetime`, `child_jobid`, `trigger_status`, `trigger_last_time`, `trigger_next_time`) VALUES (5, 4, '抽奖系统任务', '2023-05-12 00:11:10', '2023-05-12 00:11:35', 'AarynLu', '', 'FIX_RATE', '10', 'DO_NOTHING', 'FIRST', 'lotteryOrderMQStateJobHandler', '1', 'SERIAL_EXECUTION', 0, 0, 'BEAN', '', 'GLUE代码初始化', '2023-05-12 00:11:10', '', 0, 0, 0);
COMMIT;

-- ----------------------------
-- Table structure for xxl_job_lock
-- ----------------------------
DROP TABLE IF EXISTS `xxl_job_lock`;
CREATE TABLE `xxl_job_lock` (
  `lock_name` varchar(50) NOT NULL COMMENT '锁名称',
  PRIMARY KEY (`lock_name`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

-- ----------------------------
-- Records of xxl_job_lock
-- ----------------------------
BEGIN;
INSERT INTO `xxl_job_lock` (`lock_name`) VALUES ('schedule_lock');
COMMIT;

-- ----------------------------
-- Table structure for xxl_job_log
-- ----------------------------
DROP TABLE IF EXISTS `xxl_job_log`;
CREATE TABLE `xxl_job_log` (
  `id` bigint NOT NULL AUTO_INCREMENT,
  `job_group` int NOT NULL COMMENT '执行器主键ID',
  `job_id` int NOT NULL COMMENT '任务，主键ID',
  `executor_address` varchar(255) DEFAULT NULL COMMENT '执行器地址，本次执行的地址',
  `executor_handler` varchar(255) DEFAULT NULL COMMENT '执行器任务handler',
  `executor_param` varchar(512) DEFAULT NULL COMMENT '执行器任务参数',
  `executor_sharding_param` varchar(20) DEFAULT NULL COMMENT '执行器任务分片参数，格式如 1/2',
  `executor_fail_retry_count` int NOT NULL DEFAULT '0' COMMENT '失败重试次数',
  `trigger_time` datetime DEFAULT NULL COMMENT '调度-时间',
  `trigger_code` int NOT NULL COMMENT '调度-结果',
  `trigger_msg` text COMMENT '调度-日志',
  `handle_time` datetime DEFAULT NULL COMMENT '执行-时间',
  `handle_code` int NOT NULL COMMENT '执行-状态',
  `handle_msg` text COMMENT '执行-日志',
  `alarm_status` tinyint NOT NULL DEFAULT '0' COMMENT '告警状态：0-默认、1-无需告警、2-告警成功、3-告警失败',
  PRIMARY KEY (`id`),
  KEY `I_trigger_time` (`trigger_time`),
  KEY `I_handle_code` (`handle_code`)
) ENGINE=InnoDB AUTO_INCREMENT=610 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

-- ----------------------------
-- Records of xxl_job_log
-- ----------------------------
BEGIN;
COMMIT;

-- ----------------------------
-- Table structure for xxl_job_log_report
-- ----------------------------
DROP TABLE IF EXISTS `xxl_job_log_report`;
CREATE TABLE `xxl_job_log_report` (
  `id` int NOT NULL AUTO_INCREMENT,
  `trigger_day` datetime DEFAULT NULL COMMENT '调度-时间',
  `running_count` int NOT NULL DEFAULT '0' COMMENT '运行中-日志数量',
  `suc_count` int NOT NULL DEFAULT '0' COMMENT '执行成功-日志数量',
  `fail_count` int NOT NULL DEFAULT '0' COMMENT '执行失败-日志数量',
  `update_time` datetime DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `i_trigger_day` (`trigger_day`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=22 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

-- ----------------------------
-- Records of xxl_job_log_report
-- ----------------------------
BEGIN;
INSERT INTO `xxl_job_log_report` (`id`, `trigger_day`, `running_count`, `suc_count`, `fail_count`, `update_time`) VALUES (1, '2023-05-09 00:00:00', 0, 0, 0, NULL);
INSERT INTO `xxl_job_log_report` (`id`, `trigger_day`, `running_count`, `suc_count`, `fail_count`, `update_time`) VALUES (2, '2023-05-08 00:00:00', 0, 0, 0, NULL);
INSERT INTO `xxl_job_log_report` (`id`, `trigger_day`, `running_count`, `suc_count`, `fail_count`, `update_time`) VALUES (3, '2023-05-07 00:00:00', 0, 0, 0, NULL);
INSERT INTO `xxl_job_log_report` (`id`, `trigger_day`, `running_count`, `suc_count`, `fail_count`, `update_time`) VALUES (4, '2023-05-10 00:00:00', 0, 0, 0, NULL);
INSERT INTO `xxl_job_log_report` (`id`, `trigger_day`, `running_count`, `suc_count`, `fail_count`, `update_time`) VALUES (5, '2023-05-11 00:00:00', 0, 1, 12, NULL);
INSERT INTO `xxl_job_log_report` (`id`, `trigger_day`, `running_count`, `suc_count`, `fail_count`, `update_time`) VALUES (6, '2023-05-12 00:00:00', 0, 9, 1, NULL);
INSERT INTO `xxl_job_log_report` (`id`, `trigger_day`, `running_count`, `suc_count`, `fail_count`, `update_time`) VALUES (7, '2023-05-20 00:00:00', 0, 0, 0, NULL);
INSERT INTO `xxl_job_log_report` (`id`, `trigger_day`, `running_count`, `suc_count`, `fail_count`, `update_time`) VALUES (8, '2023-05-19 00:00:00', 0, 0, 0, NULL);
INSERT INTO `xxl_job_log_report` (`id`, `trigger_day`, `running_count`, `suc_count`, `fail_count`, `update_time`) VALUES (9, '2023-05-18 00:00:00', 0, 0, 0, NULL);
INSERT INTO `xxl_job_log_report` (`id`, `trigger_day`, `running_count`, `suc_count`, `fail_count`, `update_time`) VALUES (10, '2023-05-21 00:00:00', 0, 0, 0, NULL);
INSERT INTO `xxl_job_log_report` (`id`, `trigger_day`, `running_count`, `suc_count`, `fail_count`, `update_time`) VALUES (11, '2023-05-26 00:00:00', 0, 0, 0, NULL);
INSERT INTO `xxl_job_log_report` (`id`, `trigger_day`, `running_count`, `suc_count`, `fail_count`, `update_time`) VALUES (12, '2023-05-25 00:00:00', 0, 0, 0, NULL);
INSERT INTO `xxl_job_log_report` (`id`, `trigger_day`, `running_count`, `suc_count`, `fail_count`, `update_time`) VALUES (13, '2023-05-24 00:00:00', 0, 0, 0, NULL);
INSERT INTO `xxl_job_log_report` (`id`, `trigger_day`, `running_count`, `suc_count`, `fail_count`, `update_time`) VALUES (14, '2023-05-27 00:00:00', 0, 0, 0, NULL);
INSERT INTO `xxl_job_log_report` (`id`, `trigger_day`, `running_count`, `suc_count`, `fail_count`, `update_time`) VALUES (15, '2023-05-28 00:00:00', 0, 0, 0, NULL);
INSERT INTO `xxl_job_log_report` (`id`, `trigger_day`, `running_count`, `suc_count`, `fail_count`, `update_time`) VALUES (16, '2023-05-29 00:00:00', 0, 0, 0, NULL);
INSERT INTO `xxl_job_log_report` (`id`, `trigger_day`, `running_count`, `suc_count`, `fail_count`, `update_time`) VALUES (17, '2023-06-01 00:00:00', 0, 0, 0, NULL);
INSERT INTO `xxl_job_log_report` (`id`, `trigger_day`, `running_count`, `suc_count`, `fail_count`, `update_time`) VALUES (18, '2023-05-31 00:00:00', 0, 0, 0, NULL);
INSERT INTO `xxl_job_log_report` (`id`, `trigger_day`, `running_count`, `suc_count`, `fail_count`, `update_time`) VALUES (19, '2023-05-30 00:00:00', 0, 0, 0, NULL);
INSERT INTO `xxl_job_log_report` (`id`, `trigger_day`, `running_count`, `suc_count`, `fail_count`, `update_time`) VALUES (20, '2023-06-03 00:00:00', 0, 0, 0, NULL);
INSERT INTO `xxl_job_log_report` (`id`, `trigger_day`, `running_count`, `suc_count`, `fail_count`, `update_time`) VALUES (21, '2023-06-02 00:00:00', 0, 0, 0, NULL);
COMMIT;

-- ----------------------------
-- Table structure for xxl_job_logglue
-- ----------------------------
DROP TABLE IF EXISTS `xxl_job_logglue`;
CREATE TABLE `xxl_job_logglue` (
  `id` int NOT NULL AUTO_INCREMENT,
  `job_id` int NOT NULL COMMENT '任务，主键ID',
  `glue_type` varchar(50) DEFAULT NULL COMMENT 'GLUE类型',
  `glue_source` mediumtext COMMENT 'GLUE源代码',
  `glue_remark` varchar(128) NOT NULL COMMENT 'GLUE备注',
  `add_time` datetime DEFAULT NULL,
  `update_time` datetime DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

-- ----------------------------
-- Records of xxl_job_logglue
-- ----------------------------
BEGIN;
COMMIT;

-- ----------------------------
-- Table structure for xxl_job_registry
-- ----------------------------
DROP TABLE IF EXISTS `xxl_job_registry`;
CREATE TABLE `xxl_job_registry` (
  `id` int NOT NULL AUTO_INCREMENT,
  `registry_group` varchar(50) NOT NULL,
  `registry_key` varchar(255) NOT NULL,
  `registry_value` varchar(255) NOT NULL,
  `update_time` datetime DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `i_g_k_v` (`registry_group`,`registry_key`,`registry_value`)
) ENGINE=InnoDB AUTO_INCREMENT=67 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

-- ----------------------------
-- Records of xxl_job_registry
-- ----------------------------
BEGIN;
COMMIT;

-- ----------------------------
-- Table structure for xxl_job_user
-- ----------------------------
DROP TABLE IF EXISTS `xxl_job_user`;
CREATE TABLE `xxl_job_user` (
  `id` int NOT NULL AUTO_INCREMENT,
  `username` varchar(50) NOT NULL COMMENT '账号',
  `password` varchar(50) NOT NULL COMMENT '密码',
  `role` tinyint NOT NULL COMMENT '角色：0-普通用户、1-管理员',
  `permission` varchar(255) DEFAULT NULL COMMENT '权限：执行器ID列表，多个逗号分割',
  PRIMARY KEY (`id`),
  UNIQUE KEY `i_username` (`username`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

-- ----------------------------
-- Records of xxl_job_user
-- ----------------------------
BEGIN;
INSERT INTO `xxl_job_user` (`id`, `username`, `password`, `role`, `permission`) VALUES (1, 'admin', 'e10adc3949ba59abbe56e057f20f883e', 1, NULL);
COMMIT;

SET FOREIGN_KEY_CHECKS = 1;
```

安装 xxl-job 镜像：

```sh
docker pull xuxueli/xxl-job-admin:2.3.0
```

运行：

```sh
docker run -e PARAMS=" --server.port=7397 --spring.datasource.url=jdbc:mysql://115.227.3.246/:3306/xxl_job?useUnicode=true&characterEncoding=UTF-8&serverTimezone=GMT%2B8 --spring.datasource.username=root --spring.datasource.password=123456 --xxl.job.accessToken=84de7125-8e06-4d97-9f34-8e3d4f6b2e0f" -p 7397:7397 -v /logs/xxl-job:/data/applogs --name xxl-job-admin  -d xuxueli/xxl-job-admin:2.3.0
```

进入后台：ip:7397/xxl-job-admin/



## 6. 部署 Nacos

拉取镜像：

```sh
docker pull nacos/nacos-server
```

安装部署：

```sh
docker run -d -p 8848:8848 --env MODE=standalone  --name nacos  nacos/nacos-server
```









