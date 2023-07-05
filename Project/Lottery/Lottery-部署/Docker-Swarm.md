## 1. Ubuntu 装机

注意：

如果在脚本执行过程中出现错误—`Syntax error: "(" unexpected`，是因为上面脚本代码对于标准 bash 是正确的，因为 Ubuntu/Debian 为了加快开机速度，用 dash 代替了传统的 bash，解决方法就是取消dash。执行 `sudo dpkg-reconfigure dash`，在选择项中选 No 即可。

### 1.1 Docker 安装

使用官方脚本自动安装：

```sh
curl -fsSL https://test.docker.com -o test-docker.sh

sudo sh test-docker.sh
```

官网下载步骤：https://docs.docker.com/engine/install/ubuntu/

### 1.2 Git 安装

```sh
sudo apt update
sudo apt install git
```

生成公私钥：

```sh
cd ~/.ssh
ssh-keygen -t rsa -C email
```

拉取 swarm-framework 项目：

```sh
git clone git@github.com:AruNi-Lottery/swarm-framework.git
```


### 1.3 Oh-My-Zsh 安装   

Oh-My-Zsh：*Unleash* your terminal.

进入 `/swarm-framework/scripts/oh-my-zsh`，依次执行：

```sh
sh zsh.sh
# 重启终端
sh oh-my-zsh.sh

zsh omz-plugin.sh
# 重启终端
```


## 2. Swarm 集群

### 2.1 搭建集群

目前有两台服务器：

- 115.227.3.246：天翼云 4C8G，作为 manager（master）

- 175.24.165.212：腾讯云 2C2G，作为 node01

首先修改服务器的 HOSTNAME，便于后续区分 node：

```sh
master 执行：hostnamectl set-hostname master
node01 执行：hostnamectl set-hostname node01
```

设置 hosts 文件相互解析（2 台都要设置），`vim /etc/hosts`：

```sh
115.227.3.246 master
175.24.165.212 node01
```

然后在 manager 这个节点上执行如下操作，表示要将它设置为 manager，并且设置自己的通讯 IP 为 `115.227.3.246`：

```sh
docker swarm init --advertise-addr 115.227.3.246
Swarm initialized: current node (zsqac9gx41s2zo7xrjqt1yuq0) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-444n4xc3pylbenvi5aomndxj79k0v5e6vxhuxxcurzx91ok2ql-7zqxm36xrfsgiaeende9krytu 115.227.3.246:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```

如上就完成了 manager 节点的设置，而且它给出了添加工作 node 和添加 manager 从 node 的命令。

那么在另一台服务器上执行上面的第一条命令，添加为工作节点即可（若加入失败，看看服务器的安全组有没有添加该端口`2377、7946、4789`）：

```sh
docker swarm join --token SWMTKN-1-444n4xc3pylbenvi5aomndxj79k0v5e6vxhuxxcurzx91ok2ql-7zqxm36xrfsgiaeende9krytu 115.227.3.246:2377
This node joined a swarm as a worker.
```

此时 swarm 集群就搭建完毕了。

### 2.2 使用集群

manager 是我们管理集群的入口，我们的 docker 命令都是在 manager 上执行，node 节点上是不能执行 dockr 命令的。

此时可以在 manager 节点查看下 swarm 的 node 信息：

```sh
docker node ls
ID                            HOSTNAME   STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
w3f8mofs0zanykdguyaetz8u8 *   master     Ready     Active         Leader           24.0.2
a7bvpbn1tc30bdb4g540003ue     node01     Ready     Active                          20.10.5
```

## 3. 开始部署

### 3.1 部署基础服务

1、**在 node 中添加 Service 的 label 标签，以指明该 Service 要在哪些匹配的 node 上运行**。基础服务部分如下（注意更改后面的节点名为服务器的 HOSTNAME）：

- Node01 为 work 节点，内存较小，所以就添加了下面这些标签：

  ```sh
  docker node update --label-add basic-business=true node01
  docker node update --label-add basic-volumes=true node01
  
  docker node update --label-add basic-dozzle=true node01
  docker node update --label-add basic-zookeeper=true node01
  docker node update --label-add basic-kafka=true node01
  docker node update --label-add basic-redis=true node01
  ```

- master 为 manager 节点，内存较大，主要部署下面这些基础服务，后面的项目也会部署在这上面：

  ```sh
  docker node update --label-add basic-business=true master
  docker node update --label-add basic-volumes=true master
  
  docker node update --label-add basic-mysql=true master
  docker node update --label-add basic-xxl-job=true master
  ```

查看节点上的标签：

```sh
docker node inspect --format '{{json .Spec.Labels}}' 节点名称
```

2、进入 `swarm-framework/scripts` 目录，执行如下命令，部署所有基础服务到 framework stack 上：

```sh
sh start_basic_service.sh
```



> 如果启动失败，可以查看启动失败的日志，如果还没有日志显示，则直接查看 error 信息：
>
> ```sh
> docker service ps 服务名称 --no-trunc
> ```



3、此时就可以登录 portainer 了查看服务了，地址为  `ip地址:10020`。

![image-20230605000000541](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202306050000728.png)

> 接着可以到 portainer 的 Environments 中添加一下 Devcloud 的 ip，以便在 portainer 中点击 Service 的 Published Ports 就能直接进入到对应的页面。
>
> 具体操作如下：
>
> （1）点击 Environment —> primary，在 Public IP 中填写 Devcloud 的 IP：
>
> ![image-20230604235542270](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202306042355929.png)
>
> （2）点击 Published Ports，即可快速进入界面：
>
> ![image-20230604235503082](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202306042355502.png)



### 3.2 服务启动失败问题

（1）image 拉取失败问题：

- 若由于 image 拉取失败而启动失败，可以手动拉取一下失败的 image，拉取完后选择启将该服务 Update 一下即可。

（2）显示宿主机的文件不存在：

- 去 yml 文件中看挂载了哪些路径，如果服务器上没有则创建即可，注意，**两台服务器上都要同步文件**。

（3）Service 显示 pending 状态，错误信息为 `no suitable node (scheduling constraints not satisfied on 1 node)`：

- 这是因为 **没有在任何节点给该服务打上匹配的 Label 标签，导致它找不到合适的节点运行**。所以到 portainer 的 Swarm 中，选择一台节点，加上该 Service 的 Label 即可，然后 update 一下这个 Service 即可。

（4）xxl-job 启动失败：
- 需要先将 resource 下的 xxl_job.sql 导入 MySQL，xxl-job 才能正常启动。

（5）Dozzle 无法监控另一台（或多台）主机的日志：
- 在 `deploy_services/10030.dozzle.yml` 文件中，DOZZLE_REMOTE_HOST 指定了其他主机的 IP 和端口，注意该端口需要是 Docker 引擎对外提供 API 接口（远程连接）的端口，默认是 2375（我这里用的 2378）。如果 Docker 没有开启，则需要在 `/usr/lib/systemd/system/docker.service` 的 ExecStart 参数后面追加 `-H tcp://0.0.0.0:2378 -H unix:///var/run/docker.sock`，然后执行 `systemctl daemon-reload`、`systemctl restart docker`。重新部署 Dozzle 后，在 Dozzle 界面就可以选择主机了：

  ![image-20230605215258238](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202306052152874.png)

### 3.3 Nacos 启动

> 目前问题：使用官方镜像的 compose 文件不起来，提示：stat bin/docker-startup.sh: no such file or directory: unknown"

拉取镜像：

```sh
docker pull nacos/nacos-server:v2.2.3
```

部署服务：

```sh
docker service create -d -p 8848:8848 --env MODE=standalone --network framework_swarm_backend --hostname nacos --name nacos nacos/nacos-server:v2.2.3
```

### 3.4 xxl-job 启动

> 目前问题：compose 文件通过 PARAMS 指定的端口不生效，xxl-job 在启动时还是监听者 8080 端口。而且在浏览器通过暴露的端口访问不了。

拉去镜像：

```sh
docker pull xuxueli/xxl-job-admin:2.3.0
```

部署服务：

```sh
docker service create \
-e PARAMS=" --server.port=7397 --spring.datasource.url=jdbc:mysql://115.227.3.246:10060/xxl_job?useUnicode=true&characterEncoding=UTF-8&serverTimezone=GMT%2B8 --spring.datasource.username=root --spring.datasource.password=123456 --xxl.job.accessToken=84de7125-8e06-4d97-9f34-8e3d4f6b2e0f" \
--network framework_swarm_backend \
--hostname xxl-job-admin  \
-p 7397:7397 \
--name xxl-job-admin  \
-d xuxueli/xxl-job-admin:2.3.0
```



### 3.5 Kafka 创建 Topic

进入 kafka 容器，在 kafka 目录下 `/opt/kafka_2.13-2.8.1` 执行脚本命令，添加两个 topic：

> 由于 Docker 在容器内部是通过 service name 来定位服务的（类似域名解析），所以在容器内使用服务名来连接 zookeeper 即可。

```sh
bin/kafka-topics.sh --create --zookeeper zookeeper:2181 --replication-factor 1 --partitions 1 --topic lottery_activity_partake

bin/kafka-topics.sh --create --zookeeper zookeeper:2181 --replication-factor 1 --partitions 1 --topic lottery_invoice
```




## 4. 服务启动流程

### 4.1 基础服务启动流程

1、基础服务的启动入口是 `/swarm-framework/scripts` 目录下的 `start_basic_service.sh` 脚本，该脚本的主要内容如下：

（1）进入 `/swarm-framework/deploy_services` 目录，该目录下包含所有基础服务的 Docker-Compose 文件，这些 yml 文件的文件名都是以 10xxx 开头，标识这是 basic 层的服务。

（2）定义 stack 的名称：`STACK_NAME=framework`（默认）。然后将当前目录下所有 basic 层的 Docker Compose 文件名（文件名以 10xxx 开头）获取到，存储到 `EXECUTE_YML` 数组中。

（3）遍历 `EXECUTE_YML` 数组中的文件名（DOCKER-COMPOSE_FILENAME），执行 `docker stack deploy -c DOCKER-COMPOSE_FILENAME STACK_NAME` 将该服务部署到该 stack（默认 stack_name = framework）中。

到此，所有基础服务就启动起来了。



### 4.2 业务服务启动流程

业务服务的启动流程与上面的基础服务类似，所以就不再赘述了。
