# Docker 入门

## Docker 概述

### 为什么需要 Docker

> 环境切换/配置麻烦

我们在写程序时，会接触到好几个环境：

- 开发环境：自己开发时写代码的环境
- 测试环境：给测试去跑的环境
- 生产环境：测试完可以对外发布使用的环境。

在编写项目的时候，我们很多时间都花在了部署 “环境” 上：

- 如果现在重装了系统，我想要跑 `war/jar` 包，得去安装一下JDK、Tomcat、MySQL等配置各种的环境变量才能跑起来。
- 好不容易在测试环境下跑起来了，在生产环境就各种出错。
- 跟着教学视频做分布式/集群的项目，跑一堆的虚拟机，每个虚拟机都要安装对应的环境。



> 应用之间需要隔离

例如，服务器上部署了两个服务：

- Node 服务，用来启动 Web 服务。
- Java 服务，用来提供前后端分离的API接口。

将两个服务部署在同一个服务器可能会引发的问题：

- 若 Java 服务出现了异常，导致 CPU 占用达到100%，那另一个 Node 服务也会收到关联。
- 两个服务所依赖的环境各不相同，可能照成各种冲突或者无法兼容，导致调试非常麻烦。



### Docker 解决问题

为了解决上述问题，Docker 闪亮登场！！！

![image-20220520182016348](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/011973965f6795c2334ef1cf2711ff0b.png)
Docker 的思想就来自于集装箱！

Docker 可以将我们想要的环境**构建**（打包）成一个镜像，然后我们可以**推送**（发布）到网上去。在需要用这个环境的时候，在网上**拉取**一份就好了。

有了 Docker，我们在搭环境的时候，跟以前的方式就不一样了：

- **之前**：在开发环境构建出了一个war包，想跑到Linux下运行。我们得先在Linux下载好Java、Tomcat、MySQL，配置好对应的环境变量，将war包丢到Tomcat的webapps文件夹下，才能跑起来。
- **现在**：在Linux下直接拉取一份镜像(各种环境都配好了)，将镜像运行起来，把war包丢进去就好了。



隔离：Docker 核心思想，打包装箱，每个箱子是互相隔离的。

Docker通过隔离机制，可以将服务器利用到极致！



在容器技术出来之前，我们都是使用虚拟机技术！

虚拟机：在 Window 中装一个 VMware，通过这个软件我们可以虚拟出来一台或者多台电脑，但是特别笨重！

虚拟机也属于虚拟化技术，Docker 容器技术，也是一种虚拟化技术。

虚拟机和容器技术的区别：

- VMware：Linux Centos 原生镜像（一个电脑） 隔离、需要开启多个虚拟机！ 内存占用几个G，需要几分钟才能启动！
- Docker 容器技术：隔离，镜像（最核心的环境 4m + jdk + mysql）十分的小巧，运行镜像就可以了！小巧！ 内存占用只有几个M，秒级启动！

## Docker 能干嘛

### 虚拟机技术

![image-20200515153852954](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/a38007024084daea5854b4b84b470101.png)
**缺点**：

1. 资源占用十分多
2. 冗余步骤多
3. 启动很慢



### 容器化技术

容器化技术不是模拟一个完整的操作系统，每个应用都有自己的Lib库：

![image-20200515094336846](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/cc015968bdc9616b1f754bfdf6afb222.png)

比较Docker和虚拟机技术的不同：

- 传统虚拟机，虚拟出一条硬件，运行一个完整的操作系统，然后在这个系统上安装和运行软件
- 容器内的应用直接运行在宿主机的内容，容器是没有自己的内核的，也没有虚拟我们的硬件，所以就轻便了
- 每个容器间是互相隔离，每个容器内都有一个属于自己的文件系统，互不影响



> DevOps（开发、运维）

**应用更快速的交付和部署**：

传统：一对帮助文档，安装程序。

Docker：打包镜像发布测试一键运行。

**更便捷的升级和扩缩容**：

使用了 Docker之后，我们部署应用就和搭积木一样
项目打包为一个镜像，扩展服务器A！服务器B

**更简单的系统运维**：
在容器化之后，我们的开发，测试环境都是高度一致的

**更高效的计算资源利用**：

Docker是内核级别的虚拟化，可以在一个物理机上可以运行很多的容器实例！服务器的性能可以被压榨到极致。

## Docker 安装

### Docker 的基本组成

![查看源图像](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/f15c6e60c568af17742bf035ede241ce.png)

**镜像（Image)**：

Docker 镜像就好比是一个目标，可以通过这个目标来创建容器服务，tomcat镜像 --> run --> 容器（提供服务器），通过这个镜像可以创建多个容器（最终服务运行或者项目运行就是在容器中的）。

**容器(Container)**：

Docker 利用容器技术，独立运行一个或者一组应用，通过镜像来创建的。对容器有启动，停止，删除，基本命令。目前就可以把这个容器理解为就是一个简易的 Linux 系统。

**仓库(Repository)**：

Docker Hub 仓库就是存放镜像的地方，仓库分为公有仓库和私有仓库。



### 安装 Docker

> 环境准备

Linux要求内核3.0以上、CentOS 7

```shell
[root@VM-16-12-centos ~]# uname -r
3.10.0-1160.45.1.el7.x86_64		# 要求3.0以上
[root@VM-16-12-centos ~]# cat /etc/os-release 
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"
```



> 安装

帮助文档：https://docs.docker.com/engine/install/
卸载与安装

```shell
# 1.卸载旧版本
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
# 2.需要的安装包
yum install -y yum-utils

# 3.设置镜像的仓库
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
# 上述方法默认是从国外的，不推荐

# 推荐使用国内的
yum-config-manager \
    --add-repo \
    https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
    
# 更新yum软件包索引
yum makecache fast

# 4.安装docker相关的 docker-ce 社区版 而ee是企业版
yum install docker-ce docker-ce-cli containerd.io # 这里我们使用社区版即可

# 5.启动docker
systemctl start docker

# 6. 使用docker version查看是否按照成功
docker version

# 7. 测试
docker run hello-world

# 8. 查看已经下载的镜像(从这里可以查看已有镜像的id)
[root@VM-16-12-centos ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    feb5d9fea6a5   7 months ago   13.3kB
```

卸载docker

```shell
#1. 卸载依赖
yum remove docker-ce docker-ce-cli containerd.io
#2. 删除资源
rm -rf /var/lib/docker
# /var/lib/docker 是docker的默认工作路径！
```

## 阿里云镜像加速

### 1、登录阿里云找到容器服务

![image-20220516212449873](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/1fb7b0fc55eef20edc42f1e9d022e64a.png)

进入控制台管理。

### 2、找到镜像加速器

![image-20220516212605016](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/9e1b1c6bfe37f1fc4dbfb028875a05fb.png)

### 3、配置使用

```shell
#1.创建一个目录
sudo mkdir -p /etc/docker

#2.编写配置文件
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://zg0mtssz.mirror.aliyuncs.com"]
}
EOF

#3.重启服务
sudo systemctl daemon-reload

sudo systemctl restart docker
```



## 回顾 hello-world 流程

![image-20220516212045633](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/2e55af09edbe4e013ff2b88008580de4.png)

### docker run

`docker run xxx` 具体流程如下：

![image-20200515102637246](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/8fe2a3909db235251423bec36ea8aa52.png)

### 底层原理

**Docker 是怎么工作的**？

Docker 是一个CS (Client-Server) 结构的系统，Docker 的守护进程运行在主机上。通过 Socket 从客户端访问！

Docker-Server 接收到 Docker-Client 的指令，就会执行这个命令！

![image-20200515102949558](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/ae5e2c05efeb1f4c1c806d927ffd4232.png)

**为什么 Docker 比 Vm 快**：

1. Docker 有着比虚拟机更少的抽象层。由于 Docker 不需要 Hypervisor 实现硬件资源虚拟化，运行在 Docker 容器上的程序直接使用的都是实际物理机的硬件资源。因此在 CPU、内存利用率上 Docker 将会在效率上有明显优势。
2. Docker 利用的是宿主机的内核，而不需要 Guest OS。



Guest OS： VM（虚拟机）里的的系统

Host OS：物理机里的系统

![image-20200515104117329](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/296764d655b7d11ebc20ba779a33d4f7.png)

因此，当新建一个容器时，Docker 不需要和虚拟机一样重新加载一个操作系统内核。从而避免引导、加载操作系统内核返个比较费时费资源的过程。

当新建一个虚拟机时，虚拟机软件需要加载 GuestOS，返个新建过程是**分钟级别**的。而docker由于直接利用宿主机的操作系统，则省略了这个复杂的过程，因此新建一个 Docker 容器只需要**几秒钟**。

# Docker 常用命令

## 1. 帮助命令

```shell
docker version    #显示docker的版本信息。
docker info         #显示docker的系统信息，包括镜像和容器的数量
docker --help     #帮助命令
```

帮助文档的地址：https://docs.docker.com/engine/reference/commandline/build/

## 2. 镜像命令

```shell
docker images 	#查看所有本地主机上的镜像 可以使用docker image ls代替

docker search xxx	#搜索镜像

docker pull xxx 	#下载镜像 docker image pull

docker rmi xxx  	#删除镜像 docker image rm
```

### 查看镜像

```shell
[root@VM-16-12-centos ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    feb5d9fea6a5   7 months ago   13.3kB

# 解释
#REPOSITORY			# 镜像的仓库源
#TAG				# 镜像的标签(版本)		---lastest 表示最新版本
#IMAGE ID			# 镜像的id
#CREATED			# 镜像的创建时间
#SIZE				# 镜像的大小

# 可选项
Options:
  -a, --all         Show all images (default hides intermediate images) 	#列出所有镜像
  -q, --quiet       Only show numeric IDs 	# 只显示镜像的id
  
[root@VM-16-12-centos ~]# docker images -a  	#列出所有镜像详细信息	
[root@VM-16-12-centos ~]# docker images -aq		#列出所有镜像的id
feb5d9fea6a5
```



### 搜索镜像

```shell
[root@VM-16-12-centos ~]# docker search mysql

# 可选参数：
# --filter=STARS=3000 	#过滤，搜索出来的镜像收藏STARS数量大于3000的
Options:
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print search using a Go template
      --limit int       Max number of search results (default 25)
      --no-trunc        Don't truncate output


[root@VM-16-12-centos ~]# docker search mysql --filter=STARS=3000
NAME      DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql     MySQL is a widely used, open-source relation…   12578     [OK]       
mariadb   MariaDB Server is a high performing open sou…   4834      [OK]
```

![image-20220516214047156](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/9f55ff2d5d8ba848e7396601aba7e050.png)

### 下载镜像

```shell
# 下载镜像 docker pull 镜像名[:tag]
[root@VM-16-12-centos ~]# docker pull tomcat:8
8: Pulling from library/tomcat 		# 如果不写tag，默认就是latest版本
90fe46dd8199: Already exists   	       # 分层下载： docker image 的核心 联合文件系统
35a4f1977689: Already exists 
bbc37f14aded: Already exists 
74e27dc593d4: Already exists 
93a01fbfad7f: Already exists 
1478df405869: Pull complete 
64f0dd11682b: Pull complete 
68ff4e050d11: Pull complete 
f576086003cf: Pull complete 
3b72593ce10e: Pull complete 
Digest: sha256:0c6234e7ec9d10ab32c06423ab829b32e3183ba5bf2620ee66de866df # 签名防伪
Status: Downloaded newer image for tomcat:8
docker.io/library/tomcat:8 		# 真实地址


docker pull tomcat:8	# 等价于下面指令
docker pull docker.io/library/tomcat:8
```



### 删除镜像

```shell
# docker rmi -f 镜像id 		# 删除指定id的镜像
[root@VM-16-12-centos ~]# docker rmi -f f19c56ce92a8

# docker rmi -f $(docker images -aq) 	# 删除全部的镜像(用Linux命令$()列出所有镜像id，递归删除)
# or
[root@VM-16-12-centos ~]# docker rmi $(docker ps -a -q)
```

## 3. 容器命令

说明：我们有了镜像才可以创建容器，Linux，下载 Centos 镜像来学习。

### 镜像下载

```shell
# docker 中下载 centos
docker pull centos

docker run 镜像id 	# 新建容器并启动

docker ps 		# 列出所有运行的容器 
docker container list	# 与上面一样，列出所有运行的容器

docker rm 容器id 		# 删除指定容器

docker start 容器id		# 启动容器
docker restart 容器id		# 重启容器
docker stop 容器id		# 停止当前正在运行的容器
docker kill 容器id		# 强制停止当前容器
```



### 新建容器并启动

```shell
docker run [可选参数] image | docker container run [可选参数] image 
# 参书说明：
--name="Name"		# 容器名字 tomcat01 tomcat02 用来区分容器
-d					# 后台方式运行
-it 				# 使用交互方式运行，进入容器查看内容
-p					# 指定容器的端口 -p 8080(宿主机):8080(容器)
		-p ip:主机端口:容器端口
		-p 主机端口:容器端口(常用)
		-p 容器端口
		容器端口
-P(大写) 				随机指定端口


# 测试、启动并进入容器
[root@VM-16-12-centos ~]# docker run -it centos /bin/bash	# /bin/bash：在container中启动一个bash shell
[root@8f0314a47907 /]# ls
bin  etc   lib	  lost+found  mnt  proc  run   srv  tmp  var
dev  home  lib64  media       opt  root  sbin  sys  usr
[root@8f0314a47907 /]# exit 	#从容器退回主机
exit
[root@VM-16-12-centos ~]# 

# 从容器返回主机，容器仍然在运行：快捷键 Control+P+Q

```



### 列出所有运行的容器

```shell
docker ps  		# 列出当前正在运行的容器

# 可选参数：
  -a, --all     	 # 列出当前正在运行的容器 + 带出历史运行过的容器
  -n=?, --last int   # 列出最近创建的?个容器 。?为1则只列出最近创建的一个容器, 为2则列出2个
  -q, --quiet          # 只列出容器的编号
```


![image-20220516220926574](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/532edf833717dc70299fe54a549b4230.png)



### 退出容器

```shell
exit 		# 容器直接退出
快捷键 ctrl +P +Q  	# 容器不停止退出 	---注意：这个很有用的操作
```



### 删除容器

```shell
docker rm 容器id   		# 删除指定的容器，不能删除正在运行的容器
docker rm -rf 容器id		# 强制删除 

docker rm -f $(docker ps -aq)  	 # 删除所有的容器（使用Linux命令$()列出所有容器id，递归删除）
docker ps -a -q|xargs docker rm  # 删除所有的容器（一种Linux写法）
```



### 启动和停止容器的操作

```shell
docker start 容器id	  # 启动容器
docker restart 容器id	   # 重启容器
docker stop 容器id	# 停止当前正在运行的容器
docker kill 容器id	  # 强制停止当前容器
```

## 4. 常用其他命令

### 后台启动命令

```shell
# 命令 docker run -d 镜像名
[root@VM-16-12-centos ~]# docker run -d centos
a0c8f5672c12f4c5afcb101399ea67f6c441dd129f2d7ba170f60bd6297e5d42

[root@VM-16-12-centos ~]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
# 问题：docker ps          发现 centos 停止了
# 常见的坑，docker 容器使用后台运行，就必须要有要一个前台进程，docker发现没有应用，就会自动停止

# nginx，容器启动后，发现自己没有提供服务，就会立刻停止，就是没有程序了
```

每个 docker 容器需要一个前台进程一直运行，因为 docker 容器并不是虚拟机，是利用 linux 的 cgroup 和 namespace 隔离的，在宿主机上本质是个隔离的进程。

例如一个容器的主进程是启动 nginx，`service nginx  start`，这句 shell 充当了主进程，fork了一个子进程启动了 nginx 后这个 shell 就退出了，nginx 是 shell 的子进程，主进程消亡子进程也就停止了，容器也就会一运行就 exited 了。执行完成 `service nginx start` 这个命令也就几秒钟，容器就退出了。

### 查看日志

```shell
[root@VM-16-12-centos ~]# docker logs --help

Usage:  docker logs [OPTIONS] CONTAINER

Fetch the logs of a container

Options:
        --details            Show extra details provided to logs
  -f,  --follow             Follow log output
        --since string    Show logs since timestamp (e.g. 2013-01-02T13:23:37Z) or relative
                                 (e.g. 42m for 42 minutes)
  -n,  --tail string         Number of lines to show from the end of the logs (default "all")
  -t,   --timestamps     Show timestamps
        --until string       Show logs before a timestamp (e.g. 2013-01-02T13:23:37Z) or relative
                      	         (e.g. 42m for 42 minutes)



# 模拟日志      用 shell脚本执行死循环打印 docker6666
[root@VM-16-12-centos ~]# docker run -d centos /bin/sh -c "while true;do echo docker6666;sleep 1;done"		
3ebc061085ed79c9bf890caf1edff40146b510245cac39de7d68b8e3f1edf5ff

# 查看日志
-tf			# 显示日志信息（一直更新）
--tail number 		# 需要显示日志条数
docker logs -t --tail n 容器id 		# 查看n行日志
docker logs -ft 容器id 		# 跟着日志

[root@VM-16-12-centos ~]# docker logs -t --tail 5 进程id	# 查看5条日志信息
```



### 查看容器中进程信息 ps

```shell
# 命令 docker top 容器id
```

![image-20220516224705621](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/bdc78ec308096829ce1eb6e85412c2b1.png)

UID：当前用户id

PID：process id 进程ID

PPID：parent process id 父进程ID

### 查看镜像的元数据

```shell
# 命令
docker inspect 容器id

#测试
➜  ~ docker inspect 55321bcae33d
[
    {
        "Id": "55321bcae33d15da8280bcac1d2bc1141d213bcc8f8e792edfd832ff61ae5066",
        "Created": "2022-05-17T05:22:05.515909071Z",
        "Path": "/bin/sh",
        "Args": [
            "-c",
            "while true;do echo 6666;sleep 1;done"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 22973,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2020-05-15T05:22:06.165904633Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:470671670cac686c7cf0081e0b37da2e9f4f768ddc5f6a26102ccd1c6954c1ee",
        "ResolvConfPath": "/var/lib/docker/containers/55321bcae33d15da8280bcac1d2bc1141d213bcc8f8e792edfd832ff61ae5066/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/55321bcae33d15da8280bcac1d2bc1141d213bcc8f8e792edfd832ff61ae5066/hostname",
        "HostsPath": "/var/lib/docker/containers/55321bcae33d15da8280bcac1d2bc1141d213bcc8f8e792edfd832ff61ae5066/hosts",
        "LogPath": "/var/lib/docker/containers/55321bcae33d15da8280bcac1d2bc1141d213bcc8f8e792edfd832ff61ae5066/55321bcae33d15da8280bcac1d2bc1141d213bcc8f8e792edfd832ff61ae5066-json.log",
        "Name": "/bold_bell",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "docker-default",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "Capabilities": null,
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/1f347949ba49c4dbee70cea9ff3af39a14e602bc8fac8331c46347bf6708757a-init/diff:/var/lib/docker/overlay2/5afcd8220c51854a847a36f52775b4ed0acb16fe6cfaec3bd2e5df59863835ba/diff",
                "MergedDir": "/var/lib/docker/overlay2/1f347949ba49c4dbee70cea9ff3af39a14e602bc8fac8331c46347bf6708757a/merged",
                "UpperDir": "/var/lib/docker/overlay2/1f347949ba49c4dbee70cea9ff3af39a14e602bc8fac8331c46347bf6708757a/diff",
                "WorkDir": "/var/lib/docker/overlay2/1f347949ba49c4dbee70cea9ff3af39a14e602bc8fac8331c46347bf6708757a/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "55321bcae33d",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "while true;do echo 6666;sleep 1;done"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20200114",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS",
                "org.opencontainers.image.created": "2020-01-14 00:00:00-08:00",
                "org.opencontainers.image.licenses": "GPL-2.0-only",
                "org.opencontainers.image.title": "CentOS Base Image",
                "org.opencontainers.image.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "63ed0c837f35c12453bae9661859f37a08541a0749afb86e881869bf6fd9031b",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/63ed0c837f35",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "b129d9a5a2cbb92722a2101244bd81a9e3d8af034e83f338c13790a1a94552a1",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.4",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:04",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "ad5ada6a106f5ba3dda9ce4bc1475a4bb593bf5f7fbead72196e66515e8ac36a",
                    "EndpointID": "b129d9a5a2cbb92722a2101244bd81a9e3d8af034e83f338c13790a1a94552a1",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.4",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:04",
                    "DriverOpts": null
                }
            }
        }
    }
]
```



### 进入当前正在运行的容器

```shell
# 我们通常容器都是使用后台方式运行的，需要进入容器，修改一些配置

# 方式一：
docker exec -it 容器id bash/shell

#测试
[root@VM-16-12-centos ~]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED        STATUS        PORTS     NAMES
6b62e29dd4d8   centos    "/bin/bash"   16 hours ago   Up 16 hours             thirsty_pasteur
[root@VM-16-12-centos ~]# docker exec -it 6b62e29dd4d8 /bin/bash
[root@6b62e29dd4d8 /]# 

# 方式二：
docker attach 容器id
#测试
[root@VM-16-12-centos ~]# docker attach 6b62e29dd4d8
[root@6b62e29dd4d8 /]# 
正在执行当前的代码...

# 两种方式的区别：
#docker exec ：进入当前容器后开启一个新的终端，可以在里面操作。（常用）
#docker attach ：进入容器正在执行的终端，不会启动新的进程。
```



###  从容器内拷贝到主机上

```shell
# 命令：
docker cp 容器id:容器内路径  主机目的路径

# 测试：
[root@VM-16-12-centos ~]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES
b4a0e1eb36ac   centos    "/bin/bash"   18 seconds ago   Up 18 seconds             vigorous_pike

# 1. 进入docker容器内部
[root@VM-16-12-centos ~]# docker exec -it b4a0e1eb36ac /bin/bash
[root@b4a0e1eb36ac /]# ls
bin  etc  	 lib       lost+found   mnt   proc  run   srv  tmp  var
dev  home  lib64   media           opt    root  sbin  sys  usr

# 2. 在容器的home目录新建test.java文件
[root@b4a0e1eb36ac /]# cd /home
[root@b4a0e1eb36ac home]# touch test.java
[root@b4a0e1eb36ac home]# ls
test.java
[root@55321bcae33d /]# exit
exit

# 3. 将容器中的test.java拷贝到当前主机的home文件加下
[root@VM-16-12-centos ~]# docker cp b4a0e1eb36ac:/home/test.java /home
[root@VM-16-12-centos ~]# cd /home
[root@VM-16-12-centos home]# ls
jdk-8u202-linux-x64.rpm  lighthouse  test.java
```

### 命令大全


![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/14fb5b50e7f8953bd305dd26deaee877.jpeg)

```shell
  attach      Attach local standard input, output, and error streams to a running container
  #当前shell下 attach连接指定运行的镜像
  build       Build an image from a Dockerfile # 通过Dockerfile定制镜像
  commit      Create a new image from a container's changes #提交当前容器为新的镜像
  cp          Copy files/folders between a container and the local filesystem #拷贝文件
  create      Create a new container #创建一个新的容器
  diff        Inspect changes to files or directories on a container's filesystem #查看docker容器的变化
  events      Get real time events from the server # 从服务获取容器实时时间
  exec        Run a command in a running container # 在运行中的容器上运行命令
  export      Export a container's filesystem as a tar archive #导出容器文件系统作为一个tar归档文件[对应import]
  history     Show the history of an image # 展示一个镜像形成历史
  images      List images #列出系统当前的镜像
  import      Import the contents from a tarball to create a filesystem image #从tar包中导入内容创建一个文件系统镜像
  info        Display system-wide information # 显示全系统信息
  inspect     Return low-level information on Docker objects #查看容器详细信息
  kill        Kill one or more running containers # kill指定docker容器
  load        Load an image from a tar archive or STDIN #从一个tar包或标准输入中加载一个镜像[对应save]
  login       Log in to a Docker registry #
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes
```

## 5. Docker 练习

### 部署 Nginx

```shell
# 1. 搜索镜像 search 建议去DockerHub搜索，可以看到帮助文档
[root@VM-16-12-centos ~]# docker search nginx

# 2. 拉取下载镜像 pull
[root@VM-16-12-centos ~]# docker pull nginx

# 3. 列出镜像 查看是否下载成功镜像
[root@VM-16-12-centos ~]# docker images

# 4. 运行测试
# 参数：
# -d 后台运行
# --name 给容器命名
# -p 宿主机端口：容器内部端口
[root@VM-16-12-centos ~]# docker run -d --name nginx01 -p 3344:80 nginx
0e024fcbdc58e20617fd77997eef9fb3fc9d71661c9056b8b952d4c7391abee5

# 5. 查看正在启动的镜像
[root@VM-16-12-centos ~]# docker ps

# 测试访问有没有问题
curl localhost:8080
```

此时我们的 Nginx 就在 Docker 中部署成功了，在服务器防火墙中添加3344端口：

![image-20220517185251001](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/912bdb432c4f824803a2b5df841e9481.png)

现在就可以通过服务器的公网IP，访问3344端口，就成功的访问了 Nginx：

![image-20220517204414135](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/88efaf9debd2fdf252aa6d11eb7dfcc2.png)

**宿主机端口** 和 **容器内部端口** 以及端口暴露：

![image-20220517185923124](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/0d0c9ebd76cc685d1ed3dec4df1641a3.png)

```shell
# 6. 进入容器
[root@VM-16-12-centos ~]# docker exec -it nginx01 /bin/bash
root@0e024fcbdc58:/# whereis nginx	# 找到Nginx的位置
nginx: /usr/sbin/nginx /usr/lib/nginx /etc/nginx /usr/share/nginx
root@0e024fcbdc58:/# cd /etc/nginx
root@0e024fcbdc58:/etc/nginx# ls
conf.d	fastcgi_params	mime.types  modules  nginx.conf  scgi_params  uwsgi_params


# 7. 退出容器
root@0e024fcbdc58:/etc/nginx# exit
exit
[root@VM-16-12-centos ~]# 


[root@VM-16-12-centos ~]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                   NAMES
0e024fcbdc58   nginx     "/docker-entrypoint.…"   35 minutes ago   Up 35 minutes   0.0.0.0:3344->80/tcp, :::3344->80/tcp   nginx01

#7. 停止容器
[root@VM-16-12-centos ~]# docker stop 0e024fcbdc58
0e024fcbdc58
[root@VM-16-12-centos ~]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[root@VM-16-12-centos ~]# 
```

**问题：**我们每次改动nginx配置文件，都需要进入容器内部？十分麻烦，要是可以在容器外部提供一个映射路径，达到在容器外部修改文件名，容器内部就可以自动修改就好了。

这种技术我们后面会学习：`-v 数据卷` 技术！



### 部署 Tomcat

```shell
# 之前的启动都是后台，停止了容器，容器还是可以查到， docker run -it --rm 镜像名 一般是用来测试，用完就删除（删除的是容器，镜像还在）
# --rm       Automatically remove the container when it exits 用完即删
# 先不pull到docker中，直接运行 tomcat9.0
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker run -it --rm tomcat:9.0
# Control+c 终止运行


# 1. 下载 tomcat
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker pull tomcat

# 2. 查看下载的镜像
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker images

# 3. 以后台方式，暴露端口方式，启动运行
[root@VM-16-12-centos ~]# docker run -d -p 3355:8080 --name tomcat01 tomcat
706630d66e6fa44432f44bec09f2114b0b7ad303a80b7a4bd2ff189dc7866874

# 4. 测试访问有没有问题
curl localhost:8080

# 5. 根据容器id进入tomcat容器
[root@VM-16-12-centos ~]# docker exec -it 706630d66e6f /bin/bash
root@706630d66e6f:/usr/local/tomcat# 

# 6. 查看tomcat容器内部内容：
root@706630d66e6f:/usr/local/tomcat# ls -l
total 160
-rw-r--r-- 1 root root 18994 Dec  2 22:01 BUILDING.txt
-rw-r--r-- 1 root root  6210 Dec  2 22:01 CONTRIBUTING.md
-rw-r--r-- 1 root root 60269 Dec  2 22:01 LICENSE
-rw-r--r-- 1 root root  2333 Dec  2 22:01 NOTICE
-rw-r--r-- 1 root root  3378 Dec  2 22:01 README.md
-rw-r--r-- 1 root root  6905 Dec  2 22:01 RELEASE-NOTES
-rw-r--r-- 1 root root 16517 Dec  2 22:01 RUNNING.txt
drwxr-xr-x 2 root root  4096 Dec 22 17:07 bin
drwxr-xr-x 1 root root  4096 May 17 11:23 conf
drwxr-xr-x 2 root root  4096 Dec 22 17:06 lib
drwxrwxrwx 1 root root  4096 May 17 11:23 logs
drwxr-xr-x 2 root root  4096 Dec 22 17:07 native-jni-lib
drwxrwxrwx 2 root root  4096 Dec 22 17:06 temp
drwxr-xr-x 2 root root  4096 Dec 22 17:06 webapps
drwxr-xr-x 7 root root  4096 Dec  2 22:01 webapps.dist
drwxrwxrwx 2 root root  4096 Dec  2 22:01 work

# 进入webapps目录
root@706630d66e6f:/usr/local/tomcat# cd webapps
root@706630d66e6f:/usr/local/tomcat/webapps# ls
root@706630d66e6f:/usr/local/tomcat/webapps# 

# 发现问题：1. linux命令少了； 2. webapps目录为空 
# 原因：阿里云镜像的原因，阿里云默认是最小的镜像，所以不必要的都剔除掉，只保证最小可运行的环境。
# 解决方案：将webapps.dist下的文件都拷贝到webapps下即可。

root@706630d66e6f:/usr/local/tomcat# cd webapps.dist
root@706630d66e6f:/usr/local/tomcat/webapps.dist# ls
ROOT  docs  examples  host-manager  manager
root@706630d66e6f:/usr/local/tomcat/webapps.dist# cd ..
# cp -r ：递归持续复制，用于目录的复制行为
root@706630d66e6f:/usr/local/tomcat# cp -r webapps.dist/* webapps
root@706630d66e6f:/usr/local/tomcat# cd webapps
root@706630d66e6f:/usr/local/tomcat/webapps# ls
ROOT  docs  examples  host-manager  manager
root@706630d66e6f:/usr/local/tomcat/webapps# 
```

这样 docker 部署 tomcat 就可以访问了，同样需要在服务器的防火墙中添加3355端口：

![image-20220517204344873](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/a2dc319b2a44a9364aef1b6d8e23e695.png)



 **问题**：我们以后要部署项目，如果每次都要进入容器是不是十分麻烦？要是可以在容器外部提供一个映射路径，比如webapps，我们在外部放置项目，就自动同步内部就好了！

这个技术后面也会学习。



### 部署 ES + Kibana

```shell
# es 暴露的端口很多！十分耗内存
# es 的数据一般需要放置到安全目录！挂载
# --net somenetwork ? 网络配置

# 直接启动 elasticsearch
[root@VM-16-12-centos ~]# docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.6.2

# 测试一下es是否成功启动
[root@VM-16-12-centos ~]# curl localhost:9200
{
  "name" : "2e3c1b286f3c",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "OMu3NguDRF6qhzRm3yQfpA",
  "version" : {
    "number" : "7.6.2",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "ef48eb35cf30adf4db14086e8aabd07ef6fb113f",
    "build_date" : "2020-03-26T06:34:37.794943Z",
    "build_snapshot" : false,
    "lucene_version" : "8.4.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}

[root@VM-16-12-centos ~]# docker stats  # 查看docker容器使用内存情况

CONTAINER ID   NAME            CPU %     MEM USAGE / LIMIT     MEM %     NET I/O         BLOCK I/O       PIDS
2e3c1b286f3c   elasticsearch   0.00%     1.233GiB / 1.795GiB   68.72%    1.18kB / 942B   209MB / 729kB   43
# 可以看到服务器总共1.795G，ES就占了1.233G，占用了68.72%


# 测试成功就关掉elasticSearch，防止耗内存
[root@VM-16-12-centos ~]# docker stop 2e3c1b286f3c
2e3c1b286f3c

# 可以添加内存的限制，修改配置文件 -e 环境配置修改
# 先移除刚刚的容器，不然名字会冲突
[root@VM-16-12-centos ~]# docker rm -f 2e3c1b286f3c
2e3c1b286f3c

# 最大给ES  512M
[root@VM-16-12-centos ~]# docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:7.6.2

[root@VM-16-12-centos ~]# docker stats

CONTAINER ID   NAME            CPU %     MEM USAGE / LIMIT     MEM %     NET I/O     BLOCK I/O       PIDS
c2e6ff669c5a   elasticsearch   0.00%     376.9MiB / 1.795GiB   20.51%    656B / 0B   106MB / 729kB   44
# 此时可以看到，ES的内存占用较少
```



要使用 Kibana 连接 ES ？思考网络如何才能连接？
![image-20220517200340052](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/59e99aa1d0f1cbf41ba5f02f59fc638e.png)

## 6. Portainer 可视化面板

- portainer(先用这个)

```shell
docker run -d -p 8080:9000 \
--restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
```

- Rancher(CI/CD再用)



Portainer 是 Docker 的图形化界面管理工具，提供一个后台面板供我们操作。

```shell
# 安装命令
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker run -d -p 8080:9000 \
> --restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
```

测试访问8080端口：
 ![image-20220517204633140](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/3a81e7fd6721983759ee289280f54ceb.png)
创建账户后选择本地仓库：

![image-20220517205225228](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/2e589a85cd1f321ddffe4fc8494f93dc.png)

此时就可以在 Portainer 中看见我们的 Docker 了：

![image-20220517205414572](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/c8de41680fff874bdd4169e3863d1eb7.png)

![image-20220517205429404](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/3fb16b26fd837091d0bcbe308201a26d.png)

# Docker 镜像

## 镜像是什么

镜像是一种轻量级、可执行的独立软件保，用来打包软件运行环境和基于运行环境开发的软件，他包含运行某个软件所需的所有内容，包括代码、运行时库、环境变量和配置文件。

所有应用，直接打包docker镜像，就可以直接跑起来！

**如何得到镜像**

- 从远程仓库下载
- 别人拷贝给你
- 自己制作一个镜像—DockerFile

## Docker 镜像加载原理

### UnionFs（联合文件系统）

UnionFs（联合文件系统）：Union文件系统（UnionFs）是一种**分层、轻量级并且高性能**的文件系统，他支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下（ unite several directories into a single virtual filesystem)。

Union文件系统是 Docker 镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

**特性**：

一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录。



> Docker镜像加载原理

docker 的镜像实际上是由一层一层的文件系统组成，这种层级的文件系统为 UnionFS。

`boots (boot file  system)`，主要包含 bootloader 和 Kernel。bootloader 主要是引导加载 kernel,  Linux 刚启动时会加bootfs文件系统，在 Docker镜像的最底层是  boots。这一层与我们典型的 Linux/Unix 系统是一样的，包含 boot 加载器和内核。当 boot 加载完成之后整个内核就都在内存中了，此时内存的使用权已由 bootfs 转交给内核，此时系统也会卸载 bootfs。

`rootfs（root file system)`，在 bootfs 之上。包含的就是典型 Linux 系统中的 `/dev`, `/proc`, `/bin`, `/etc` 等标准目录和文件。 rootfs 就是各种不同的操作系统发行版，比如 Ubuntu, Centos等等。

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/356d07dcbf285795a400653a367606a0.png)

平时我们安装进虚拟机的CentOS都是好几个G，为什么Docker这里才200M？

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/1d187c35efd83dbafb32982cc05f6c3f.png)

对于个精简的OS，rootfs 可以很小，只需要包合最基本的命令，工具和程序库就可以了，因为底层直接用 Host 的 kernel，自己只需要提供 rootfs 就可以了。由此可见对于不同的 Linux 发行版， boots 基本是一致的， rootfs 会有差別，因此不同的发行版可以公用 bootfs。

所以虚拟机是分钟级别，而容器是秒级！

## 分层理解

### 分层的镜像

我们可以去下载一个镜像，注意观察下载的日志输出，可以看到是一层层的在下载

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNTE2MzgzOTE4MC5wbmc?x-oss-process=image/format,png)

**思考：为什么Docker镜像要采用这种分层的结构呢**？

最大的好处莫过于资源共享了！比如有多个镜像都从相同的Base镜像构建而来，那么宿主机只需在磁盘上保留一份base镜像，同时内存中也只需要加载一份base镜像，这样就可以为所有的容器服务了，而且镜像的每一层都可以被共享。

查看镜像分层的方式可以通过 `docker image inspect 镜像名`  命令：

```shell
➜  / docker image inspect redis          
[
    {
        "Id": "sha256:f9b9909726890b00d2098081642edf32e5211b7ab53563929a47f250bcdc1d7c",
        "RepoTags": [
            "redis:latest"
        ],
        "RepoDigests": [
            "redis@sha256:399a9b17b8522e24fbe2fd3b42474d4bb668d3994153c4b5d38c3dafd5903e32"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2020-05-02T01:40:19.112130797Z",
        "Container": "d30c0bcea88561bc5139821227d2199bb027eeba9083f90c701891b4affce3bc",
        "ContainerConfig": {
            "Hostname": "d30c0bcea885",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "6379/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.12",
                "REDIS_VERSION=6.0.1",
                "REDIS_DOWNLOAD_URL=http://download.redis.io/releases/redis-6.0.1.tar.gz",
                "REDIS_DOWNLOAD_SHA=b8756e430479edc162ba9c44dc89ac394316cd482f2dc6b91bcd5fe12593f273"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"redis-server\"]"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:704c602fa36f41a6d2d08e49bd2319ccd6915418f545c838416318b3c29811e0",
            "Volumes": {
                "/data": {}
            },
            "WorkingDir": "/data",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {}
        },
        "DockerVersion": "18.09.7",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "6379/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.12",
                "REDIS_VERSION=6.0.1",
                "REDIS_DOWNLOAD_URL=http://download.redis.io/releases/redis-6.0.1.tar.gz",
                "REDIS_DOWNLOAD_SHA=b8756e430479edc162ba9c44dc89ac394316cd482f2dc6b91bcd5fe12593f273"
            ],
            "Cmd": [
                "redis-server"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:704c602fa36f41a6d2d08e49bd2319ccd6915418f545c838416318b3c29811e0",
            "Volumes": {
                "/data": {}
            },
            "WorkingDir": "/data",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": null
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 104101893,
        "VirtualSize": 104101893,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/adea96bbe6518657dc2d4c6331a807eea70567144abda686588ef6c3bb0d778a/diff:/var/lib/docker/overlay2/66abd822d34dc6446e6bebe73721dfd1dc497c2c8063c43ffb8cf8140e2caeb6/diff:/var/lib/docker/overlay2/d19d24fb6a24801c5fa639c1d979d19f3f17196b3c6dde96d3b69cd2ad07ba8a/diff:/var/lib/docker/overlay2/a1e95aae5e09ca6df4f71b542c86c677b884f5280c1d3e3a1111b13644b221f9/diff:/var/lib/docker/overlay2/cd90f7a9cd0227c1db29ea992e889e4e6af057d9ab2835dd18a67a019c18bab4/diff",
                "MergedDir": "/var/lib/docker/overlay2/afa1de233453b60686a3847854624ef191d7bc317fb01e015b4f06671139fb11/merged",
                "UpperDir": "/var/lib/docker/overlay2/afa1de233453b60686a3847854624ef191d7bc317fb01e015b4f06671139fb11/diff",
                "WorkDir": "/var/lib/docker/overlay2/afa1de233453b60686a3847854624ef191d7bc317fb01e015b4f06671139fb11/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:c2adabaecedbda0af72b153c6499a0555f3a769d52370469d8f6bd6328af9b13",
                "sha256:744315296a49be711c312dfa1b3a80516116f78c437367ff0bc678da1123e990",
                "sha256:379ef5d5cb402a5538413d7285b21aa58a560882d15f1f553f7868dc4b66afa8",
                "sha256:d00fd460effb7b066760f97447c071492d471c5176d05b8af1751806a1f905f8",
                "sha256:4d0c196331523cfed7bf5bafd616ecb3855256838d850b6f3d5fba911f6c4123",
                "sha256:98b4a6242af2536383425ba2d6de033a510e049d9ca07ff501b95052da76e894"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]
```



**理解**：

所有的 Docker 镜像都起始于一个基础镜像层，当进行修改或培加新的内容时，就会在当前镜像层之上，创建新的镜像层。

举一个简单的例子，假如基于 Ubuntu Linux16.04 创建一个新的镜像，这就是新镜像的第一层；如果在该镜像中添加 Python 包，就会在基础镜像层之上创建第二个镜像层；如果继续添加一个安全补丁，就会创健第三个镜像层该像。当前已经包含3个镜像层，如下图所示（这只是一个用于演示的很简单的例子）。

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/0b2611805f5556fcb35026eabdcd17a8.png)

**在添加额外的镜像层的同时，镜像始终保持是当前所有镜像的组合**，理解这一点非常重要。下图中举了一个简单的例子，每个镜像层包含3个文件，而镜像包含了来自两个镜像层的6个文件。

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/6c55c550ea191e06ca9cb7fee391fb88.png)

上图中的镜像层跟之前图中的略有区別，主要目的是便于展示文件。

下图中展示了一个稍微复杂的三层镜像，在外部看来整个镜像只有6个文件，这是因为最上层中的文件7是文件5的一个更新版。

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/31982f68d35473f1199b90f69e369518.png)

上述情況下，上层镜像层中的文件覆盖了底层镜像层中的文件。这样就使得文件的更新版本作为一个新镜像层添加到镜像当中

Docker 通过存储引擎（新版本采用快照机制）的方式来实现镜像层堆栈，并保证多镜像层对外展示为统一的文件系统

Linux 上可用的存储引撃有 AUFS、 Overlay2、 Device Mapper、Btrfs 以及 ZFS。顾名思义，每种存储引擎都基于 Linux 中对应的文件系统或者块设备技术，井且每种存储引擎都有其独有的性能特点。

Docker 在 Windows 上仅支持 windowsfilter 一种存储引擎，该引擎基于NTFS文件系统之上实现了分层和CoW [1]。

下图展示了与系统显示相同的三层镜像。所有镜像层堆并合井，对外提供统一的视图。

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/a1f8c0f8afd3d17bb8ad6db0ea5b4664.png)

### 特点

Docker 镜像都是只读的，当容器启动时，一个新的可写层加载到镜像的顶部！

这一层就是我们通常说的容器层，容器之下的都叫镜像层！

我们的所有操作都是基于容器层的，我们无法操作镜像层。

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/3f7a67fc55e15635f5c757d7c8af4a14.png)

## commit 镜像

命令：

```shell
docker commit 提交容器成为一个新的副本

# 命令和git原理类似
docker commit -m="描述信息" -a="作者" 容器id 目标镜像名:[版本TAG]
```

实战测试：

```shell
# 1、后台方式启动一个默认的tomcat
[root@VM-16-12-centos ~]# docker run -d -p 8080:8080 tomcat
53bca03e5f9dfd53d3423835945a9b174b715c9fb45784bd19a5b8c27481ef89

# 2、进入tomcat，发现这个默认的tomcat 的webapps中没有应用，官方的镜像默认webapps下面是没有文件的！
[root@iz2zeak7sgj6i7hrb2g862z ~]# docker exec -it de57d0ace571 /bin/bash
root@de57d0ace571:/usr/local/tomcat# 

# 3、从webapps.dist拷贝文件进去webapp
root@53bca03e5f9d:/usr/local/tomcat# cp -r webapps.dist/* webapps
root@53bca03e5f9d:/usr/local/tomcat# cd webapps
root@53bca03e5f9d:/usr/local/tomcat/webapps# ls
ROOT  docs  examples  host-manager  manager

# 4、将操作过的容器通过commit提交为一个镜像！以后就使用我们修改过的镜像即可，而不需要每次都重新拷贝webapps.dist下的文件到webapps了，这就是我们自己的一个修改过的镜像。
# 命令：
# docker commit -m="描述信息" -a="作者" 容器id 目标镜像名:[TAG]
[root@VM-16-12-centos ~]# docker commit -a="AruNi" -m="add webapps app" 53bca03e5f9d tomcat02:1.0
sha256:6175d1c7c9c26b109e2b15f13e81f81857be14dce1e701f3042a256903056588

[root@VM-16-12-centos ~]# docker images
REPOSITORY            TAG       IMAGE ID       CREATED          SIZE
tomcat02              1.0       6175d1c7c9c2   27 seconds ago   684MB
nginx                 latest    605c77e624dd   4 months ago     141MB
tomcat                9         b8e65a4d736d   4 months ago     680MB
tomcat                9.0       b8e65a4d736d   4 months ago     680MB
tomcat                latest    fb5657adc892   4 months ago     680MB
hello-world           latest    feb5d9fea6a5   7 months ago     13.3kB
centos                latest    5d0da3dc9764   8 months ago     231MB
portainer/portainer   latest    580c0e4e98b0   14 months ago    79.1MB
kibana                7.6.2     f70986bc5191   2 years ago      1.01GB
elasticsearch         7.6.2     f29a1ee41030   2 years ago      791MB
```

如果你想要保存当前容器的状态，就可以通过commit来提交，获得一个镜像，就好比我们我们使用虚拟机的快照。

# 容器数据卷

## 什么是容器数据卷

将应用和环境打包成一个镜像后，如果数据都在容器中，那么我们容器删除，数据就会丢失，所以**数据需要持久化**。

实例：MySQL 部署在容器中，若容器删除了，那么数据库中的数据也没了，无疑为删库跑路！所以我们要将 **MySQL 的数据存储在本地**。

这就要使用到容器之间能实现数据共享的技术，卷技术，它能将 Docker 容器中产生的数据同步到本地中。

卷技术，说白了就是一个目录的挂载，将我们容器内的目录，挂载到Linux上面。

![image-20220518210923687](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/5bea2fcce8620d0fe2143c581d6c9bb5.png)

**容器数据卷总结：容器的持久化和同步操作**。

## 使用数据卷

使用 `-v` 命令挂载数据卷。

```shell
-v, --volume list                    Bind mount a volume

docker run -it -v 主机目录:容器内目录  -p 主机端口:容器内端口

# /home/ceshi：主机home目录下的ceshi文件夹  映射：centos容器中的/home
[root@VM-16-12-centos ~]# docker run -it -v /home/ceshi:/home centos /bin/bash

#这时候主机的/home/ceshi文件夹就和容器的/home文件夹关联了,二者可以实现文件或数据同步了

#通过 docker inspect 容器id 查看
[root@VM-16-12-centos ~]# docker inspect f3be5caf61a4
```

![image-20220518211552249](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/7989bf3b56b27eb3a27b611a2491f154.png)

测试文件的同步：

![image-20220518212008139](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/f4d30b0926d58d9240d724160e8ce842.png)

进一步测试：

![image-20220518213049723](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/630c86d50aec2ecae7ed27c9188bb640.png)

好处：我们以后修改只需要在本地修改即可，容器内会自动同步！

## 实战：安装MySQL

**思考：MySQL的数据持久化的问题**

```shell
# 获取mysql镜像
[root@VM-16-12-centos ~]# docker pull mysql:5.7

# 运行容器，需要做数据挂载 #安装启动mysql，需要配置密码的，这是注意点！
# 参考官网DockerHub：
docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag

#启动我们的
-d 后台运行
-p 端口映射
-v 卷挂载
-e 环境配置
-- name 容器名字
[root@VM-16-12-centos ~]# docker run -d -p 3310:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql03 mysql:5.7
1bc8308bea988ded2dec7782adb3c59887870e9c27cfec613c3ad14f55537ae2


# 启动成功之后，我们在本地使用Navicat来测试一下
# Navicat--连接到服务器的3310--和容器内的3306映射 

# 在本地测试创建一个数据库，查看一下我们映射的路径是否ok！
```

**测试连接**：注意3310端口要在服务器的防火墙中添加此端口，否则无法连接。

![image-20220518214724053](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/0f8507144c30268cc34478da32c1485b.png)



当我们在本地用 Navicat 新建名称为 `docker_test` 的数据库时（相当于在docker容器中创建），绑定的宿主机中也会创建：

![image-20220518215159637](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/65087d98c1b1cd7d019198e44371e9d2.png)

现在将mysql容器删除：`[root@VM-16-12-centos data]# docker rm -f mysql03`

删除后再次查看挂载到本地宿主机中的数据卷，发现依旧没有丢失，这就实现了容器数据持久化功能。

## 具名和匿名挂载

```shell
# 匿名挂载：-v 容器内路径
#  -P: 随机映射端口
$ docker run -d -P --name nginx01 -v /etc/nginx nginx

# 查看所有的volume(卷)的情况
$ docker volume ls    
[root@VM-16-12-centos ~]# docker volume ls
DRIVER    VOLUME NAME（容器内的卷名）
local     356783b2fa4788f8fe352c50562b498b86ea69ded6020cc0163e3e176380ee10
local     bc751d84b9516cc6e511c2553050a7bd501aeec58849bf8cca1b525d21e60617
local     ee46f92db7deac8b1648c05235d18401928c29ef1af78a5bc3d2b736aa37b1a5
         
# 这种就是匿名挂载，我们在 -v 只写了容器内的路径，没有写容器外的路径！
```

```shell
# 具名挂载：-v 容器内路径:宿主机路径
[root@VM-16-12-centos ~]# docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx nginx
9c867e892fd3314c949c0e282227d3bc5c1c63e9245aedea2e5589621b876b06

# 查看所有的volume(卷)的情况
[root@VM-16-12-centos ~]# docker volume ls
DRIVER    VOLUME NAME
local     356783b2fa4788f8fe352c50562b498b86ea69ded6020cc0163e3e176380ee10
local     bc751d84b9516cc6e511c2553050a7bd501aeec58849bf8cca1b525d21e60617
local     ee46f92db7deac8b1648c05235d18401928c29ef1af78a5bc3d2b736aa37b1a5
local     juming-nginx  （多了个名字）


# 通过 -v 卷名：查看容器内路径
# 查看一下这个卷
[root@VM-16-12-centos ~]# docker volume inspect juming-nginx 
[
    {
        "CreatedAt": "2022-05-18T22:07:12+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/juming-nginx/_data",
        "Name": "juming-nginx",
        "Options": null,
        "Scope": "local"
    }
]
```

所有的 docker 容器内的卷，没有指定目录的情况下都是在 `/var/lib/docker/volumes/自定义的卷名/_data` 下，**如果指定了目录，`docker volume ls` 是查看不到的**。



```shell
# 指定目录挂载：-v /宿主机路径:容器内路径
[root@VM-16-12-centos ~]# docker run -d -P --name nginx03 -v /home/nginx03:/etc/nginx/conf.d nginx
3a14a3baced3e0af6506f198684a7ffe50a18cef62a28bf091ae1940525316bc

[root@VM-16-12-centos ~]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                     NAMES
3a14a3baced3   nginx     "/docker-entrypoint.…"   23 seconds ago   Up 22 seconds   0.0.0.0:49155->80/tcp, :::49155->80/tcp   nginx03
9c867e892fd3   nginx     "/docker-entrypoint.…"   25 minutes ago   Up 25 minutes   0.0.0.0:49154->80/tcp, :::49154->80/tcp   nginx02
c8a2d611e817   nginx     "/docker-entrypoint.…"   29 minutes ago   Up 29 minutes   0.0.0.0:49153->80/tcp, :::49153->80/tcp   nginx01

[root@VM-16-12-centos ~]# docker volume ls
DRIVER    VOLUME NAME
local     356783b2fa4788f8fe352c50562b498b86ea69ded6020cc0163e3e176380ee10
local     bc751d84b9516cc6e511c2553050a7bd501aeec58849bf8cca1b525d21e60617
local     ee46f92db7deac8b1648c05235d18401928c29ef1af78a5bc3d2b736aa37b1a5
local     juming-nginx

# 查看不到 nginx03
```



**区分三种挂载方式**：

```shell
# 三种挂载： 匿名挂载、具名挂载、指定路径挂载
-v 容器内路径		#匿名挂载
-v 卷名:容器内路径		     #具名挂载
-v /宿主机路径:容器内路径 	#指定路径挂载 docker volume ls 是查看不到的
```

拓展：

```shell
# 通过 	-v 容器内路径： ro rw 改变读写权限
ro 	#readonly 只读
rw 	#readwrite 可读可写
$ docker run -d -P --name nginx05 -v juming:/etc/nginx:ro nginx
$ docker run -d -P --name nginx05 -v juming:/etc/nginx:rw nginx

# 只要看到ro就说明这个路径只能通过宿主机来操作，容器内部是无法操作的！
```

## 初识 Dockerfile

**Dockerfile 就是用来构建 docker镜像 的构建文件**，命令脚本！先体验一下！

通过这个**脚本可以生成镜像**，镜像是一层一层的，脚本是一个个的命令，每个命令都是一层！

```shell
# 创建一个dockerfile文件，名字可以随便 建议dockerfile
# 我这里把文件放在home目录下的docker-test-volume文件夹下
# 文件中的内容： 指令(大写) + 参数
[root@VM-16-12-centos home]# ls
ceshi  jdk-8u202-linux-x64.rpm  lighthouse  mysql  nginx03
[root@VM-16-12-centos home]# mkdir docker-test-volume
[root@VM-16-12-centos home]# ls
ceshi  docker-test-volume  jdk-8u202-linux-x64.rpm  lighthouse  mysql  nginx03
[root@VM-16-12-centos home]# cd docker-test-volume/
[root@VM-16-12-centos docker-test-volume]# vim dockerfile01
    FROM centos 					# 当前这个镜像是以centos为基础的

    VOLUME ["volume01","volume02"] 	# 挂载卷的卷目录列表(多个目录)

    CMD echo "-----end-----"		# 输出一下用于测试
    CMD /bin/bash					# 默认走bash控制台
# 编辑完毕     :wq	保存退出

# 查看dockerfile01中的内容：
[root@VM-16-12-centos docker-test-volume]# cat dockerfile01 
FROM centos

VOLUME ["volume01","volume02"]

CMD echo "-----end-----"
CMD /bin/bash
# ---------------------------------------------------------------------

# 这里的每个命令，就是镜像的一层！
# 构建出这个镜像 
# 参数：
-f dockerfile01 		# f代表file，指这个当前文件的地址(这里是当前目录下的dockerfile01)
-t aruni/centos 	# t就代表target，指目标目录(注意aruni镜像名前不能加斜杠‘/’)
. 	# 表示生成在当前目录下

[root@VM-16-12-centos docker-test-volume]# docker build -f dockerfile01 -t aruni/centos .
Sending build context to Docker daemon  2.048kB
Step 1/4 : FROM centos
latest: Pulling from library/centos
a1d0c7532777: Pull complete 
Digest: sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177
Status: Downloaded newer image for centos:latest
 ---> 5d0da3dc9764
Step 2/4 : VOLUME ["volume01","volume02"]	# 挂载的数据卷目录
 ---> Running in 9ee2a3d67d9c
Removing intermediate container 9ee2a3d67d9c
 ---> d36b6582d14b
Step 3/4 : CMD echo "-----end-----"		# 输出的脚本
 ---> Running in 8eac77b09566
Removing intermediate container 8eac77b09566
 ---> 7ebcaf6219dc
Step 4/4 : CMD /bin/bash
 ---> Running in 872075f872ad
Removing intermediate container 872075f872ad
 ---> 2b40f24fc014
Successfully built 2b40f24fc014
Successfully tagged aruni/centos:latest

# 查看自己构建的镜像
[root@VM-16-12-centos docker-test-volume]# docker images
REPOSITORY     TAG       IMAGE ID       CREATED              SIZE
aruni/centos   latest    2b40f24fc014   About a minute ago   231MB
```



> 启动自己写的容器镜像

```shell
[root@VM-16-12-centos docker-test-volume]# docker run -it 2b40f24fc014 /bin/bash
[root@6e6c99e6d717 /]# ls -l			# 查看目录							
```

![image-20220518232801301](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/251e89fa8ee51c3c42e5f3a7023c9eb9.png)

这个卷和外部一定有一个同步的目录，我们在生产镜像的时候采用的是匿名挂载。

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/17eedf388e4a7e68d2cb20af3422267d.png)

先在容器中新建一个文件：

```shell
[root@6b2ae59910f5 /]# cd volume01
[root@6b2ae59910f5 volume01]# touch container.txt
[root@6b2ae59910f5 volume01]# ls
container.txt
```

查看一下卷挂载，看看挂载的目录：

```shell
# docker inspect 容器id（注意是容器id，不是镜像id）
[root@VM-16-12-centos docker-test-volume]# docker inspect 6b2ae59910f5
```

![image-20220518233959757](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/0247d3d8a9adf627f3192af47dd1eba6.png)

复制 `volume01` 的 Source 路径，查询一下刚才的文件是否同步到宿主机中了：

```shell
[root@VM-16-12-centos docker-test-volume]# cd /var/lib/docker/volumes/f8b569175ae967d7472253c2be907da404cd701fed9e9c29cee8d9a6cbe3e5b8/_data
[root@VM-16-12-centos _data]# ls
container.txt
```

这种方式使用的十分多，因为我们通常会构建自己的镜像！

假设构建镜像时候没有挂载卷，要手动镜像挂载 `-v 卷名:容器内路径`

# 数据卷容器

命名的容器已挂载数据卷，其他的容器通过挂载这个容器(父容器)实现数据共享，**挂载数据卷的容器，称为数据卷容器**。通过数据卷容器可以实现容器间的数据共享。

![image-20220518234435292](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/b6853b2fd00070211502e27dbe093532.png)

```shell
# 测试 启动3个容器，通过刚才自己写的镜像启动
# 创建docker01：先使用 docker images 查看一下刚才自己写的镜像的TAG
[root@VM-16-12-centos ~]# docker run -it --name docker01 aruni/centos:latest

# 查看容器docekr01内容
[root@8bda3903613f /]# ls -l
total 56
lrwxrwxrwx   1 root root    7 Nov  3  2020 bin -> usr/bin
drwxr-xr-x   5 root root  360 May 18 15:50 dev
drwxr-xr-x   1 root root 4096 May 18 15:50 etc
drwxr-xr-x   2 root root 4096 Nov  3  2020 home
lrwxrwxrwx   1 root root    7 Nov  3  2020 lib -> usr/lib
lrwxrwxrwx   1 root root    9 Nov  3  2020 lib64 -> usr/lib64
drwx------   2 root root 4096 Sep 15  2021 lost+found
drwxr-xr-x   2 root root 4096 Nov  3  2020 media
drwxr-xr-x   2 root root 4096 Nov  3  2020 mnt
drwxr-xr-x   2 root root 4096 Nov  3  2020 opt
dr-xr-xr-x 107 root root    0 May 18 15:50 proc
dr-xr-x---   2 root root 4096 Sep 15  2021 root
drwxr-xr-x  11 root root 4096 Sep 15  2021 run
lrwxrwxrwx   1 root root    8 Nov  3  2020 sbin -> usr/sbin
drwxr-xr-x   2 root root 4096 Nov  3  2020 srv
dr-xr-xr-x  13 root root    0 May 18 15:26 sys
drwxrwxrwt   7 root root 4096 Sep 15  2021 tmp
drwxr-xr-x  12 root root 4096 Sep 15  2021 usr
drwxr-xr-x  20 root root 4096 Sep 15  2021 var
drwxr-xr-x   2 root root 4096 May 18 15:50 volume01	# 数据卷
drwxr-xr-x   2 root root 4096 May 18 15:50 volume02	# 数据卷

# 不关闭该容器退出
CTRL + Q + P  

# 创建docker02: 并且让docker02 继承 docker01
# --volumes-from：继承自......
[root@VM-16-12-centos ~]# docker run -it --name docker02 --volumes-from docker01 aruni/centos:latest

# 查看容器docker02内容
[root@edcf3680467f /]# ls -l
total 56
lrwxrwxrwx   1 root root    7 Nov  3  2020 bin -> usr/bin
drwxr-xr-x   5 root root  360 May 18 15:53 dev
drwxr-xr-x   1 root root 4096 May 18 15:53 etc
drwxr-xr-x   2 root root 4096 Nov  3  2020 home
lrwxrwxrwx   1 root root    7 Nov  3  2020 lib -> usr/lib
lrwxrwxrwx   1 root root    9 Nov  3  2020 lib64 -> usr/lib64
drwx------   2 root root 4096 Sep 15  2021 lost+found
drwxr-xr-x   2 root root 4096 Nov  3  2020 media
drwxr-xr-x   2 root root 4096 Nov  3  2020 mnt
drwxr-xr-x   2 root root 4096 Nov  3  2020 opt
dr-xr-xr-x 109 root root    0 May 18 15:53 proc
dr-xr-x---   2 root root 4096 Sep 15  2021 root
drwxr-xr-x  11 root root 4096 Sep 15  2021 run
lrwxrwxrwx   1 root root    8 Nov  3  2020 sbin -> usr/sbin
drwxr-xr-x   2 root root 4096 Nov  3  2020 srv
dr-xr-xr-x  13 root root    0 May 18 15:26 sys
drwxrwxrwt   7 root root 4096 Sep 15  2021 tmp
drwxr-xr-x  12 root root 4096 Sep 15  2021 usr
drwxr-xr-x  20 root root 4096 Sep 15  2021 var
drwxr-xr-x   2 root root 4096 May 18 15:50 volume01
drwxr-xr-x   2 root root 4096 May 18 15:50 volume02
```

![image-20220519000124020](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/d2d4c25453f51911cc19508059b85b93.png)

```shell
# 再新建一个docker03同样继承docker01
[root@VM-16-12-centos ~]# docker run -it --name docker03 --volumes-from docker01 aruni/centos:latest
[root@feb4354873cf /]# cd volume01	
[root@feb4354873cf volume01]# ls	
docker01		# 同步了 docker01的数据

# 测试：可以删除docker01，查看一下docker02和docker03是否可以访问这个文件
# 测试发现：数据依旧保留在docker02和docker03中没有被删除
```

**原理**：三个容器都公用宿主机上的匿名数据卷中的文件，如果把宿主机上的文件删了，容器里面的文件也就没了。



**多个mysql实现数据共享**

```shell
[root@VM-16-12-centos ~]# docker run -d -p 3306:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7

[root@VM-16-12-centos ~]# docker run -d -p 3310:3306 -e MYSQL_ROOT_PASSWORD=123456 --name mysql02 --volumes-from mysql01  mysql:5.7

# 这个时候，可以实现两个容器数据同步！
```

结论：

**容器之间的配置信息的传递，数据卷容器的生命周期一直持续到没有容器使用为止**。

**但是一旦你持久化到了本地，这个时候，本地的数据是不会删除的**！

# DockerFile

## DockerFile 介绍

`dockerfile` 是用来构建docker镜像的文件！命令参数脚本！

构建步骤：

1. 编写一个dockerfile文件
2. docker build 构建称为一个镜像
3. docker run运行镜像
4. docker push发布镜像（DockerHub 、阿里云仓库)



查看官方的镜像：

![image-20220519105825182](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/c7e39bfe132c0492ea96645375a9723e.png)

点击后跳到一个 Dockerfile

![image-20220519105908422](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/e678c5bfad8467ff3054b9bb029904be.png)

很多官方镜像都是基础包，很多功能没有，我们通常会自己搭建自己的镜像！

官方既然可以制作镜像，那我们也可以！

## DockerFile 构建过程

**基础知识**：

1. 每个保留关键字(指令）都是必须是大写字母
2. 执行从上到下顺序
3. `#` 表示注释
4. 每一个指令都会创建提交一个新的镜像层，并提交

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/413b15ed70dea098b4ed3d3964096471.png)

Dockerfile 是面向开发的，我们以后要发布项目，做镜像，就需要编写 dockerfile 文件，这个文件十分简单。

Docker 镜像逐渐成企业交付的标准，必须要掌握！

DockerFile：构建文件，定义了一切的步骤。

DockerImages：通过 DockerFile 构建生成的镜像，最终发布和运行产品。

Docker 容器：容器就是镜像运行起来提供服务。

## DockerFile 指令

```shell
FROM				# from: 基础镜像，一切从这里开始构建
MAINTAINER			# maintainer: 镜像是谁写的， 姓名+邮箱
RUN					# run: 镜像构建的时候需要运行的命令
ADD					# add: 步骤，tomcat镜像，这个tomcat压缩包！添加内容 添加同目录
WORKDIR				# workdir: 镜像的工作目录
VOLUME				# volume: 挂载的目录
EXPOSE				# expose: 暴露端口配置
CMD					# cmd: 指定这个容器启动的时候要运行的命令，只有最后一个会生效，可被替代
ENTRYPOINT			# entrypoint: 指定这个容器启动的时候要运行的命令，可以追加命令
ONBUILD				# onbuild: 当构建一个被继承 DockerFile 的时候就会运行onbuild的指令，触发指令
COPY				# copy: 类似ADD，将我们文件拷贝到镜像中
ENV					# env: 构建的时候设置环境变量！
```

![image-20220519111320642](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/d649a2d66b8e16db7ad0b5242a8954e7.png)

## DockerFile 构建自己的镜像

Docker Hub 中 99% 的镜像都是 `FROM scratch` 这个基础的镜像而来的，然后添加需要的软件和配置来进行构建。

![image-20220519111510545](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/e678c5bfad8467ff3054b9bb029904be.png)



> 创建自己的 centos

```shell
# 1. /home下新建dockerfile目录
[root@VM-16-12-centos home]# mkdir dockerfile
[root@VM-16-12-centos home]# ls
ceshi  dockerfile  docker-test-volume  jdk-8u202-linux-x64.rpm  lighthouse  mysql  nginx03

# 2. dockerfile目录下新建mydockerfile-centos文件
[root@VM-16-12-centos dockerfile]# vim mydockerfile-centos

# 3.编写Dockerfile配置文件
FROM centos:7						# 基础镜像是官方原生的centos7
MAINTAINER AruNi<1298911600@qq.com> 	# 作者信息

ENV MYPATH /usr/local				# 配置环境变量的目录 ，ENV命令为键值对
WORKDIR $MYPATH						# 将工作目录设置为 MYPATH

RUN yum -y install vim				# 给官方原生的centos 增加 vim指令
RUN yum -y install net-tools		# 给官方原生的centos 增加 ifconfig命令

EXPOSE 80							# 暴露端口号为80

CMD echo $MYPATH					# 输出 MYPATH 路径
CMD echo "-----end----"				
CMD /bin/bash						# 启动后进入 /bin/bash

# 4. 通过这个文件构建镜像
# 命令： docker build -f 文件路径 -t 镜像名:[tag] .	（末尾一定要有 .  表示当前目录）
[root@VM-16-12-centos dockerfile]# docker build -f mydockerfile-centos -t mycentos:0.1 .

# 5.出现下图后则构建成功
```

![image-20220519112907635](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/bb50b8a19c50de48ad1668281edaa87a.png)

```shell
[root@VM-16-12-centos /]# docker images
REPOSITORY     TAG       IMAGE ID       CREATED              SIZE
mycentos       1.0       baa6b4d0e8f1   About a minute ago   592MB

# 6. 测试运行
[root@VM-16-12-centos /]# docker run -it mycentos:1.0 	# 注意带上版本号，否则每次都去找最新版latest

[root@9c95c34d9afc local]# pwd
/usr/local		# 与Dockerfile文件中 WORKDIR 设置的 MYPATH 一致

[root@9c95c34d9afc local]# vim			# vim 指令可以使用
[root@9c95c34d9afc local]# ifconfig     		# ifconfig 指令可以使用

# docker history 镜像id 查看镜像构建历史步骤
[root@VM-16-12-centos /]# docker history baa6b4d0e8f1
IMAGE          CREATED         CREATED BY                                      SIZE      COMMENT
baa6b4d0e8f1   6 minutes ago   /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "/bin…   0B        
f6079e8434f4   6 minutes ago   /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "echo…   0B        
1417a4c9c830   6 minutes ago   /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "echo…   0B        
028ee8b09570   6 minutes ago   /bin/sh -c #(nop)  EXPOSE 80                    0B        
0c86004a707d   6 minutes ago   /bin/sh -c yum -y install net-tools             166MB     
b6a0fb4ec5c3   6 minutes ago   /bin/sh -c yum -y install vim                   221MB     
99e472ca137e   7 minutes ago   /bin/sh -c #(nop) WORKDIR /usr/local            0B        
9be5bd0e76e6   7 minutes ago   /bin/sh -c #(nop)  ENV MYPATH=/usr/local        0B        
0568a1cc15b0   7 minutes ago   /bin/sh -c #(nop)  MAINTAINER AruNi<12989116…   0B  
eeb6ee3f44bd   8 months ago    /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B        
<missing>      8 months ago    /bin/sh -c #(nop)  LABEL org.label-schema.sc…   0B        
<missing>      8 months ago    /bin/sh -c #(nop) ADD file:b3ebbe8bd304723d4…   204MB     
```

我们平时拿到一个镜像，可以用 `docker history 镜像id` 研究一下是什么做的。



## CMD 和 ENTRYPOINT 的区别

```shell
CMD					# 指定这个容器启动的时候要运行的命令，只有最后一个会生效，可被替代。
ENTRYPOINT			# 指定这个容器启动的时候要运行的命令，可以追加命令
```

**测试cmd**：

```shell
# 编写dockerfile文件
$ vim dockerfile-test-cmd
FROM centos
CMD ["ls","-a"]					# CMD方式：启动后就执行 ls -a 命令

# 构建镜像
$ docker build  -f dockerfile-test-cmd -t cmd-test:0.1 .

# 运行镜像
$ docker run cmd-test:0.1		# 由结果可得，运行后就执行了 ls -a 命令
.
..
.dockerenv
bin
dev
etc
home

# 在运行时想追加一个命令 -l 连起来成为ls -al：展示列表详细数据
$ docker run cmd-test:0.1 -l
docker: Error response from daemon: OCI runtime create failed: container_linux.go:349: starting container process caused "exec: \"-l\":
executable file not found in $PATH": unknown.
ERRO[0000] error waiting for container: context canceled 

# 使用cmd的情况下 -l 替换了CMD["ls","-l"] ，而 -l  不是命令所以报错
```

**测试ENTRYPOINT**：

```shell
# 编写dockerfile文件
$ vim dockerfile-test-entrypoint
FROM centos
ENTRYPOINT ["ls","-a"]		# ENTRYPOINT方式：启动后就执行 ls -a 命令

# 构建镜像
$ docker build  -f dockerfile-test-entrypoint -t cmd-test:0.1 .

# 运行镜像
$ docker run entrypoint-test:0.1
.
..
.dockerenv
bin
dev
etc
home
lib
lib64
lost+found ...

# 我们的命令，是直接拼接在我们得ENTRYPOINT命令后面的
$ docker run entrypoint-test:0.1 -l
total 56
drwxr-xr-x   1 root root 4096 May 16 06:32 .
drwxr-xr-x   1 root root 4096 May 16 06:32 ..
-rwxr-xr-x   1 root root    0 May 16 06:32 .dockerenv
lrwxrwxrwx   1 root root    7 May 11  2019 bin -> usr/bin
drwxr-xr-x   5 root root  340 May 16 06:32 dev
drwxr-xr-x   1 root root 4096 May 16 06:32 etc
drwxr-xr-x   2 root root 4096 May 11  2019 home
lrwxrwxrwx   1 root root    7 May 11  2019 lib -> usr/lib
lrwxrwxrwx   1 root root    9 May 11  2019 lib64 -> usr/lib64 ....
```

Dockerfile中很多命令都十分的相似，需要了解它们的区别，最好的学习方法就是对比它们然后测试效果！

## 实战：Tomcat镜像

### 1、准备镜像文件

在自己的 `/home/aruni` 目录下新建 `docker-tomcat` 文件夹，准备 tomcat 和 jdk 到当前目录，编写好README文件。

```shell
[root@VM-16-12-centos aruni]# cd docker-tomcat/
[root@VM-16-12-centos docker-tomcat]# ls
apache-tomcat-9.0.63.tar.gz  jdk-8u333-linux-x64.tar.gz  readme.md
```



### 2、编写 Dokerfile

官方命名：`Dokerfile`，build 时会自动寻找这个文件，就不需要 `-f` 指定了。

```shell
[root@VM-16-12-centos docker-tomcat]# vim dockerfile
FROM centos 										# 基础镜像centos
MAINTAINER AruNi<1298911600@qq.com>					# 作者

COPY readme.md /usr/local/readme.md						# 复制README文件

ADD jdk-8u333-linux-x64.tar.gz /usr/local/ 			# 添加jdk，ADD 命令会自动解压
ADD apache-tomcat-9.0.63.tar.gz /usr/local/ 		# 添加tomcat，ADD 命令会自动解压

RUN yum -y install vim								# 安装 vim 命令
ENV MYPATH /usr/local 								# 环境变量设置 工作目录
WORKDIR $MYPATH

ENV JAVA_HOME /usr/local/jdk1.8.0_333			# 环境变量： JAVA_HOME环境变量
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.63 	# 环境变量： tomcat环境变量
ENV CATALINA_BASH /usr/local/apache-tomcat-9.0.63

# 设置环境变量 分隔符是：
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin 	

EXPOSE 8080 										# 设置暴露的端口

CMD /usr/local/apache-tomcat-9.0.63/bin/startup.sh && tail -F /usr/local/apache-tomcat-9.0.63/logs/catalina.out 					# 设置默认命令
```

### 3、构建镜像

```shell
# 因为dockerfile命名使用默认命名 因此不用使用-f 指定文件
[root@VM-16-12-centos docker-tomcat]# docker build -t mytomcat:0.1 .
```

### 4、run镜像

```shell
# -d:后台运行 -p:暴露端口 --name:别名 -v:绑定路径 
# 这里挂载了test目录和logs目录
[root@VM-16-12-centos docker-tomcat]# docker run -d -p 8080:8080 --name tomcat01 -v /home/aruni/docker-tomcat/test:/usr/local/apache-tomcat-9.0.63/webapps/test -v /home/aruni/docker-tomcat/tomcatlogs:/usr/local/apache-tomcat-9.0.63/logs mytomcat:0.1
12172ff7e7868d4265a7b563a44e0a87589964f5f5a567f6cb8a289bdf5967a8
```

### 5、访问测试

```shell
[root@VM-16-12-centos docker-tomcat]# docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED              STATUS              PORTS                                       NAMES
12172ff7e786   mytomcat:0.1   "/bin/sh -c '/usr/lo…"   About a minute ago   Up About a minute   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   tomcat01
[root@VM-16-12-centos docker-tomcat]# docker exec -it 12172ff7e786 /bin/bash
```

测试访问 `ip:8080` 成功：

![image-20220519144652365](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/1a2741c8d5573bd9ad02b1cc6329895c.png)

### 6、发布项目

由于做了卷挂载，本地的 `/home/aruni/docker-tomcat/test` 目录 和 tomcat的 `webapps中的test目录` 挂载了。我们直接在本地 test 中编写项目就可以发布了！

1、在 test目录 创建一个 `WEB-INF` 文件夹，在里面新建编写 `web.xml` 配置文件：

```shell
[root@VM-16-12-centos test]# pwd
/home/aruni/docker-tomcat/test
[root@VM-16-12-centos test]# mkdir WEB-INF
[root@VM-16-12-centos test]# cd WEB-INF/
[root@VM-16-12-centos WEB-INF]# vim web.xml

<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.4" 
    xmlns="http://java.sun.com/xml/ns/j2ee" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee 
        http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">

</web-app>

# 编写完输入   :wq   保存退出
```

2、在 test 目录 创建一个 `index.html` 文件，作为我们的首页：

```shell
[root@VM-16-12-centos test]# vim index.html

<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>mydocker</title>
</head>
<body>
    <h1>Hello Docker</h1>
</body>
</html>

# 编写完输入   :wq   保存退出
```

在本地编写完成后，已经同步到了 Docker 容器中了，可以直接访问：`ip:8080/test`

![image-20220519150342099](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/08ff503067fcd6510a4fc03cecd2de43.png)

因为本地目录中的 `tomcatlogs` 和 Docker 的 `tomcat中的logs` 也挂载了，所以也可以在本地查看 logs ，在 `/tomcatlogs/catalina.out` 文件中：

```shell
[root@VM-16-12-centos docker-tomcat]# cd tomcatlogs/
[root@VM-16-12-centos tomcatlogs]# ls
catalina.2022-05-19.log  host-manager.2022-05-19.log  localhost_access_log.2022-05-19.txt
catalina.out             localhost.2022-05-19.log     manager.2022-05-19.log
[root@VM-16-12-centos tomcatlogs]# cat catalina.out

.......

org.apache.catalina.startup.HostConfig.deployDirectory Deploying web application directory [/usr/local/apache-tomcat-9.0.63/webapps/test]
19-May-2022 06:44:40.026 INFO [main] org.apache.catalina.startup.HostConfig.deployDirectory Deployment of web application directory [/usr/local/apache-tomcat-9.0.63/webapps/test] has finished in [31] ms
19-May-2022 06:44:40.031 INFO [main] org.apache.coyote.AbstractProtocol.start Starting ProtocolHandler ["http-nio-8080"]
19-May-2022 06:44:40.046 INFO [main] org.apache.catalina.startup.Catalina.start Server startup in [756] milliseconds
19-May-2022 06:56:40.131 INFO [Catalina-utility-2] org.apache.catalina.startup.HostConfig.reload Reloading context [/test]
19-May-2022 06:56:40.131 INFO [Catalina-utility-2] org.apache.catalina.core.StandardContext.reload Reloading Context with name [/test] has started
19-May-2022 06:56:40.155 INFO [Catalina-utility-2] org.apache.catalina.core.StandardContext.reload Reloading Context with name [/test] is completed
.........
```





以后开发的步骤：需要掌握 Dockerfile 的编写！之后的一切都是使用 docker 镜像来发布运行！

## 发布镜像

### 发布到 Docker Hub

1、地址 https://hub.docker.com/

2、确定这个账号可以登录

3、登录

```shell
$ docker login -u 用户名 -p 密码
........
Login Succeeded
```

4、提交 push镜像

```shell
[root@VM-16-12-centos tomcatlogs]# docker push mytomcat:0.1
The push refers to repository [docker.io/library/mytomcat]
7c46d6d75ab1: Preparing 
40589f8cf446: Preparing 
6b34c8359045: Preparing 
10315a21d5b0: Preparing 
174f56854903: Preparing 
denied: requested access to the resource is denied	# 被拒绝了

# 发现push不上去，因为如果没有前缀的话默认是push到 官方的library

# 解决方法：
# 第一种：build的时候添加你的dockerhub用户名，然后在push就可以放到自己的仓库了
[root@VM-16-12-centos tomcatlogs]# docker build -t aruni01/mytomcat:0.1

# 由于前面已经build过了，所以采取第二种方法：给镜像标一个tag
[root@VM-16-12-centos tomcatlogs]# docker tag 442da366082f aruni01/tomcat:1.0
[root@VM-16-12-centos tomcatlogs]# docker images
REPOSITORY       TAG       IMAGE ID       CREATED          SIZE
aruni01/tomcat   1.0       442da366082f   48 minutes ago   810MB
mytomcat         0.1       442da366082f   48 minutes ago   810MB
# 再次push
[root@VM-16-12-centos tomcatlogs]# docker push aruni01/tomcat:1.0
The push refers to repository [docker.io/aruni01/tomcat]
7c46d6d75ab1: Pushed 
40589f8cf446: Pushed 
6b34c8359045: Pushed 
10315a21d5b0: Pushed 
174f56854903: Pushed 
1.0: digest: sha256:0e4994d62f29f722a0c67ee38a865ee5f072c40a2f6c7184ea9d3f5562072c85 size: 1373
```

提交的时候也是按照层级来提交的，在 Docker Hub 中查看自己的此镜像：

![image-20220519155707116](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/7248bdc3441f68566b41c38c45177d09.png)



### 发布到阿里云镜像

官网：https://cr.console.aliyun.com/repository/

1、在阿里云官网找到容器镜像服务，创建一个个人实例

2、创建命名空间

![image-20220519155028247](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/8ef44a03f16c920c62f73bdb662808df.png)

4、创建容器镜像，选择本地仓库

![image-20220519155231013](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/7be7bee536b8bab168b543e28db50d67.png)

> 官方操作指南

1、**登录**阿里云 Docker Registry

```shell
$ docker login --username=aruni registry.cn-hangzhou.aliyuncs.com
```

用于登录的用户名为阿里云账号全名，密码为开通服务时设置的密码。

您可以在访问凭证页面修改凭证密码。

2、从 Registry 中**拉取**镜像

```shell
$ docker pull registry.cn-hangzhou.aliyuncs.com/run-docker-study/mytomcat:[镜像版本号]
```

3、将镜像**推送**到 Registry

```shell
$ docker login --username=aruni registry.cn-hangzhou.aliyuncs.com
$ docker tag [ImageId] registry.cn-hangzhou.aliyuncs.com/run-docker-study/mytomcat:[镜像版本号]
$ docker push registry.cn-hangzhou.aliyuncs.com/run-docker-study/mytomcat:[镜像版本号]
```

请根据实际镜像信息替换示例中的[ImageId]和[镜像版本号]参数。

4、选择合适的镜像仓库地址

从ECS推送镜像时，可以选择使用镜像仓库内网地址。推送速度将得到提升并且将不会损耗您的公网流量。

如果您使用的机器位于VPC网络，请使用 `registry-vpc.cn-hangzhou.aliyuncs.com` 作为Registry的域名登录。

5、示例

使用 `"docker tag"` 命令重命名镜像，并将它通过专有网络地址推送至Registry。

```shell
$ docker imagesREPOSITORY                                                         TAG                 IMAGE ID            CREATED             VIRTUAL SIZEregistry.aliyuncs.com/acs/agent                                    0.7-dfb6816         37bb9c63c8b2        7 days ago          37.89 MB
$ docker tag 37bb9c63c8b2 registry-vpc.cn-hangzhou.aliyuncs.com/acs/agent:0.7-dfb6816
```

使用 `"docker push"` 命令将该镜像推送至远程。

```shell
$ docker push registry-vpc.cn-hangzhou.aliyuncs.com/acs/agent:0.7-dfb6816
```





根据官方文档的第3步操作，将自己的 tomcat 镜像发布到阿里云镜像中：

```shell
# 1. 登录阿里云镜像
[root@VM-16-12-centos tomcatlogs]# docker login --username=aruni registry.cn-hangzhou.aliyuncs.com
......
Login Succeeded

# 2. 给镜像生成一个tag 版本号
[root@VM-16-12-centos ~]# docker tag 442da366082f registry.cn-hangzhou.aliyuncs.com/run-docker-study/mytomcat:1.0
[root@VM-16-12-centos ~]# docker images
REPOSITORY                                                    TAG       IMAGE ID       CREATED             SIZE
aruni01/tomcat                                                1.0       442da366082f   About an hour ago   810MB
mytomcat                                                      0.1       442da366082f   About an hour ago   810MB
registry.cn-hangzhou.aliyuncs.com/run-docker-study/mytomcat   1.0       442da366082f   About an hour ago   810MB

# 3. push到阿里云镜像
[root@VM-16-12-centos ~]# docker push registry.cn-hangzhou.aliyuncs.com/run-docker-study/mytomcat:1.0
The push refers to repository [registry.cn-hangzhou.aliyuncs.com/run-docker-study/mytomcat]
7c46d6d75ab1: Pushed 
40589f8cf446: Pushed 
6b34c8359045: Pushed 
10315a21d5b0: Pushed 
174f56854903: Pushed 
1.0: digest: sha256:0e4994d62f29f722a0c67ee38a865ee5f072c40a2f6c7184ea9d3f5562072c85 size: 1373
```

在阿里云镜像中查看自己发布的镜像：

![image-20220519163247494](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/0cf971b0a2791a16444f1f30086c394b.png)

## 小结

![img](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/09a0c35acf75e283d303fe7d32023ce1.png)

# Docker 网络

## 理解Docker 0

学习之前**清空下前面的docker 镜像、容器**

```shell
# 删除全部容器
$ docker rm -f $(docker ps -aq)

# 删除全部镜像
$ docker rmi -f $(docker images -aq)
12345
```

> 测试

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNTIyMzIzNjc3Mi5wbmc?x-oss-process=image/format,png)

**三个网络**

> 问题： docker 是如果处理容器网络访问的？

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE3MjA0MTk4NS5wbmc?x-oss-process=image/format,png)

```shell
# 测试  运行一个tomcat
$ docker run -d -P --name tomcat01 tomcat

# 查看容器内部网络地址
$ docker exec -it 容器id ip addr

# 发现容器启动的时候会得到一个 eth0@if91 ip地址，docker分配！
$ ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
261: eth0@if91: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:12:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.18.0.2/16 brd 172.18.255.255 scope global eth0
       valid_lft forever preferred_lft forever

       
# 思考？ linux能不能ping通容器内部！ 可以 容器内部可以ping通外界吗？ 可以！
$ ping 172.18.0.2
PING 172.18.0.2 (172.18.0.2) 56(84) bytes of data.
64 bytes from 172.18.0.2: icmp_seq=1 ttl=64 time=0.069 ms
64 bytes from 172.18.0.2: icmp_seq=2 ttl=64 time=0.074 ms
1234567891011121314151617181920212223
```

> 原理

1、我们每启动一个docker容器，docker就会给docker容器分配一个ip，我们只要按照了docker，就会有一个docker0桥接模式，使用的技术是veth-pair技术！

https://www.cnblogs.com/bakari/p/10613710.html

再次测试 ip addr

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNTIyNDAzNjg4My5wbmc?x-oss-process=image/format,png)

2 、再启动一个容器测试，发现又多了一对网络

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE3MzI1OTQ1OC5wbmc?x-oss-process=image/format,png)

```shell
# 我们发现这个容器带来网卡，都是一对对的
# veth-pair 就是一对的虚拟设备接口，他们都是成对出现的，一端连着协议，一端彼此相连
# 正因为有这个特性 veth-pair 充当一个桥梁，连接各种虚拟网络设备的
# OpenStac,Docker容器之间的连接，OVS的连接，都是使用evth-pair技术
1234
```

3、我们来测试下tomcat01和tomcat02是否可以ping通

```shell
# 获取tomcat01的ip 172.17.0.2
$ docker-tomcat docker exec -it tomcat01 ip addr  
550: eth0@if551: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
       
# 让tomcat02 ping tomcat01       
$ docker-tomcat docker exec -it tomcat02 ping 172.17.0.2
PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.098 ms
64 bytes from 172.17.0.2: icmp_seq=2 ttl=64 time=0.071 ms

# 结论：容器和容器之间是可以互相ping通
1234567891011121314
```

**网络模型图**

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE3NDI0ODYyNi5wbmc?x-oss-process=image/format,png)

结论：tomcat01和tomcat02公用一个路由器，docker0。

所有的容器不指定网络的情况下，都是docker0路由的，docker会给我们的容器分配一个默认的可用ip。

> 小结

Docker使用的是Linux的桥接，宿主机是一个Docker容器的网桥 docker0

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE3NDcwMTA2My5wbmc?x-oss-process=image/format,png)

Docker中所有网络接口都是虚拟的，虚拟的转发效率高（内网传递文件）

只要容器删除，对应的网桥一对就没了！

**思考一个场景：我们编写了一个微服务，database url=ip: 项目不重启，数据ip换了，我们希望可以处理这个问题，可以通过名字来进行访问容器**？

## –-link

```shell
$ docker exec -it tomcat02 ping tomca01   # ping不通
ping: tomca01: Name or service not known

# 运行一个tomcat03 --link tomcat02 
$ docker run -d -P --name tomcat03 --link tomcat02 tomcat
5f9331566980a9e92bc54681caaac14e9fc993f14ad13d98534026c08c0a9aef

# 3连接2
# 用tomcat03 ping tomcat02 可以ping通
$ docker exec -it tomcat03 ping tomcat02
PING tomcat02 (172.17.0.3) 56(84) bytes of data.
64 bytes from tomcat02 (172.17.0.3): icmp_seq=1 ttl=64 time=0.115 ms
64 bytes from tomcat02 (172.17.0.3): icmp_seq=2 ttl=64 time=0.080 ms

# 2连接3
# 用tomcat02 ping tomcat03 ping不通
12345678910111213141516
```

**探究：**

docker network inspect 网络id 网段相同

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE3NTkwNDU1MS5wbmc?x-oss-process=image/format,png)

docker inspect tomcat03

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE4MDMwODUzMC5wbmc?x-oss-process=image/format,png)

查看tomcat03里面的/etc/hosts发现有tomcat02的配置

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE4MDYyOTAxMi5wbmc?x-oss-process=image/format,png)

–link 本质就是在hosts配置中添加映射

现在使用Docker已经不建议使用–link了！

自定义网络，不适用docker0！

docker0问题：不支持容器名连接访问！

## 自定义网络

```shell
docker network
connect     -- Connect a container to a network
create      -- Creates a new network with a name specified by the
disconnect  -- Disconnects a container from a network
inspect     -- Displays detailed information on a network
ls          -- Lists all the networks created by the user
prune       -- Remove all unused networks
rm          -- Deletes one or more networks
12345678
```

> 查看所有的docker网络

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE5MDMxNjA3My5wbmc?x-oss-process=image/format,png)

**网络模式**

bridge ：桥接 docker（默认，自己创建也是用bridge模式）

none ：不配置网络，一般不用

host ：和所主机共享网络

container ：容器网络连通（用得少！局限很大）

测试

```shell
# 我们直接启动的命令 --net bridge,而这个就是我们得docker0
# bridge就是docker0
$ docker run -d -P --name tomcat01 tomcat
等价于 => docker run -d -P --name tomcat01 --net bridge tomcat

# docker0，特点：默认，域名不能访问。 --link可以打通连接，但是很麻烦！
# 我们可以 自定义一个网络
$ docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet
12345678
```

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE5MTEzOTk0NC5wbmc?x-oss-process=image/format,png)

```shell
$ docker network inspect mynet;
1
```

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE5MTQwNzA2NS5wbmc?x-oss-process=image/format,png)

启动两个tomcat,再次查看网络情况

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE5MTg0NDI0MC5wbmc?x-oss-process=image/format,png)

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE5MjAwNzM3MS5wbmc?x-oss-process=image/format,png)

在自定义的网络下，服务可以互相ping通，不用使用–link

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE5MjEzNDY3My5wbmc?x-oss-process=image/format,png)

我们自定义的网络docker当我们维护好了对应的关系，推荐我们平时这样使用网络！

好处：

redis -不同的集群使用不同的网络，保证集群是安全和健康的

mysql-不同的集群使用不同的网络，保证集群是安全和健康的

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE5MjUwNDM2Ny5wbmc?x-oss-process=image/format,png)

## 网络连通

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE5MzI0MzE0Ni5wbmc?x-oss-process=image/format,png)

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE5MzI1OTE4NS5wbmc?x-oss-process=image/format,png)

```shell
# 测试两个不同的网络连通  再启动两个tomcat 使用默认网络，即docker0
$ docker run -d -P --name tomcat01 tomcat
$ docker run -d -P --name tomcat02 tomcat
# 此时ping不通
1234
```

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE5MzU1NDkzMS5wbmc?x-oss-process=image/format,png)

```shell
# 要将tomcat01 连通 tomcat—net-01 ，连通就是将 tomcat01加到 mynet网络
# 一个容器两个ip（tomcat01）
12
```

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE5Mzg0ODMzNy5wbmc?x-oss-process=image/format,png)

```shell
# 01连通 ，加入后此时，已经可以tomcat01 和 tomcat-01-net ping通了
# 02是依旧不通的
12
```

结论：假设要跨网络操作别人，就需要使用docker network connect 连通！

# 实战：部署Redis集群

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjE5NDQxOTQ3MS5wbmc?x-oss-process=image/format,png)

```shell
# 创建网卡
docker network create redis --subnet 172.38.0.0/16
# 通过脚本创建六个redis配置
for port in $(seq 1 6);\
do \
mkdir -p /mydata/redis/node-${port}/conf
touch /mydata/redis/node-${port}/conf/redis.conf
cat << EOF >> /mydata/redis/node-${port}/conf/redis.conf
port 6379
bind 0.0.0.0
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
cluster-announce-ip 172.38.0.1${port}
cluster-announce-port 6379
cluster-announce-bus-port 16379
appendonly yes
EOF
done

# 通过脚本运行六个redis
for port in $(seq 1 6);\
docker run -p 637${port}:6379 -p 1667${port}:16379 --name redis-${port} \
-v /mydata/redis/node-${port}/data:/data \
-v /mydata/redis/node-${port}/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.1${port} redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf
docker exec -it redis-1 /bin/sh #redis默认没有bash
redis-cli --cluster create 172.38.0.11:6379 172.38.0.12:6379 172.38.0.13:6379 172.38.0.14:6379 172.38.0.15:6379 172.38.0.16:6379  --cluster-replicas 1

1234567891011121314151617181920212223242526272829
```

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjIwMjkwMjI0MS5wbmc?x-oss-process=image/format,png)

docker搭建redis集群完成！

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL2NoZW5nY29kZXgvY2xvdWRpbWcvbWFzdGVyL2ltZy9pbWFnZS0yMDIwMDUxNjIwMzMyMzk3MS5wbmc?x-oss-process=image/format,png)

我们使用docker之后，所有的技术都会慢慢变得简单起来！

# SpringBoot微服务打包Docker镜像

1、构建SpringBoot项目

2、打包运行

```
mvn package
1
```

3、编写dockerfile

```shell
FROM java:8
COPY *.jar /app.jar
CMD ["--server.port=8080"]
EXPOSE 8080
ENTRYPOINT ["java","-jar","app.jar"]
12345
```

4、构建镜像

```shell
# 1.复制jar和DockerFIle到服务器
# 2.构建镜像
$ docker build -t xxxxx:xx  .
123
```

5、发布运行

以后我们使用了Docker之后，给别人交付就是一个镜像即可！



















