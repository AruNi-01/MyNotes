# Nginx 是什么

Nginx (engine x) 是一个高性能的HTTP和反向代理web服务器，同时也提供了IMAP/POP3/SMTP服务。Nginx是由伊戈尔·赛索耶夫为俄罗斯访问量第二的Rambler.ru站点（俄文：Рамблер）开发的，第一个公开版本0.1.0发布于2004年10月4日。2011年6月1日，nginx 1.0.4发布。

其特点是占有内存少，并发能力强，事实上nginx的并发能力在同类型的网页服务器中表现较好，中国大陆使用nginx网站用户有：百度、京东、新浪、网易、腾讯、淘宝等。在全球活跃的网站中有12.18%的使用比率，大约为2220万个网站。Nginx 是一个安装非常的简单、配置文件非常简洁（还能够支持perl语法）、Bug非常少的服务。

Nginx 启动特别容易，并且几乎可以做到7*24不间断运行，即使运行数个月也不需要重新启动。你还能够不间断服务的情况下进行软件版本的升级。

Nginx代码完全用C语言从头写成。官方数据测试表明能够支持高达 50,000 个并发连接数的响应。

# Nginx 作用

## 反向代理

Http 代理，反向代理作为web服务器最常用的功能之一，尤其是反向代理。

> 正向代理

正向代理可以简单理解为客户端的代理，访问墙外的网站就属于正向代理。

![image-20220602163403457](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220602163403457.png)

> 反向代理

反向代理可以简单理解为服务器的代理，通常说的 Nginx 和 Apache 就属于反向代理。

![image-20220602163454453](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220602163454453.png)



## 负载均衡

nginx 默认有三种负载均衡策略，分别是：轮询、权重、ip_hash

1. 轮询:  将客户端的请求平均的分配给每一台服务器
2. 权重：将客户端的请求，根据服务器权重的不同进行分配
3. ip_hash：根据客户端ip地址，将请求分配到指定的服务器上去

> 轮询

![image-20220602164455374](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220602164455374.png)

> 加权轮询

![image-20220602164530870](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220602164530870.png)

> iphash

iphash 对客户端请求的ip进行hash操作，然后根据hash结果将同一个客户端ip的请求分发给同一台服务器进行处理，可以解决session不共享的问题。

![image-20220602164607701](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220602164607701.png)

## 动静分离

在软件开发中，有些请求是需要后台处理的，有些请求是不需要经过后台处理的（如：css、html、jpg、js等等文件），这些不需要经过后台处理的文件称为静态文件。

让动态网站里的动态网页根据一定规则把不变的资源和经常变的资源区分开来，动静资源做好了拆分以后，我们就可以根据静态资源的特点将其做缓存操作。提高资源响应的速度。

![image-20220602164010204](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220602164010204.png)

> 前后端分开部署

前后端的代码没必要都部署到相同的服务器上，也可以分开部署到不同的服务器上，下图是前端服务将 API 请求转发至后端服务的方案。

![image-20220602164100414](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220602164100414.png)

上面的部署方案中，所有浏览器的请求都是直接访问前端服务，而如果是浏览器直接访问后端API服务的部署模式下，如下图。

![image-20220602164200573](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220602164200573.png)

此时前端和后端通常不在同一个域下，我们还需要在后端代码中添加跨域支持。

# Nginx 安装

## Win

1、下载 Nginx http://nginx.org/en/download.html 下载稳定版本 ：

![image-20220602153242760](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220602153242760.png)

2、启动 Nginx 有多种方法：

- 直接双击nginx.exe，双击后一个黑色的弹窗一闪而过
- 打开cmd命令窗口，切换到nginx解压目录下，输入命令nginx.exe，回车即可

3、检查 Nginx 是否启动成功，在浏览器地址栏输入网址 http://localhost:80 ：
 ![image-20220602153407002](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220602153407002.png)

4、配置监听 Nginx 的配置文件是 conf 目录下的 `Nginx.conf`， 默认配置的 Nginx 监听的端口为80，如果80端口被占用可以修改为未被占用的端口即可。
![image-20220602153530024](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220602153530024.png)

当修改了Nginx 的配置文件 `nginx.conf` 时，不需要关闭 Nginx 后重新启动，只需要执行命令 `nginx -s reload` 即可让改动生效。

5、关闭 Nginx 如果使用 cmd 命令窗口启动 Nginx， 只关闭cmd窗口是不能结束 Nginx 进程的，可使用两种方法关闭 Nginx：

- 输入Nginx命令 `nginx -s stop` (快速停止 Nginx) 或 `nginx -s quit`（完整有序的停止 Nginx）
- 使用 `taskkill taskkill /f /t /im nginx.exe`
  1. `taskkill`：是用来终止进程的.
  2. `/f`：强制终止 
  3. `/t`：终止指定的进程和任何由此启动的子进程。
  4. `/im`：进程名称

## Linux

**1、安装gcc**
 安装 nginx 需要先将官网下载的源码进行编译，编译依赖 gcc 环境，如果没有 gcc 环境，则需要安装：
 yum install gcc-c++

**2、PCRE pcre-devel 安装**
 PCRE(Perl Compatible  Regular Expressions) 是一个Perl库，包括 perl 兼容的正则表达式库。nginx 的 http 模块使用 pcre  来解析正则表达式，所以需要在 linux 上安装 pcre库，pcre-devel 是使用 pcre  开发的一个二次开发库。nginx也需要此库。命令：
 yum install -y pcre pcre-devel

**3、zlib 安装**
 zlib 库提供了很多种压缩和解压缩的方式， nginx 使用 zlib 对 http 包的内容进行 gzip ，所以需要在 Centos 上安装 zlib 库。
 yum install -y zlib zlib-devel

**4、OpenSSL**
 安装OpenSSL 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及 SSL 协议，并提供丰富的应用程序供测试或其它目的使用。
 nginx 不仅支持 http 协议，还支持 https（即在ssl协议上传输http），所以需要在 Centos安装 OpenSSL 库。
 yum install -y openssl openssl-devel

5、官网下载 `nginx.tar.gz` 安装包，地址：https://nginx.org/en/download.html

6、解压

```shell
[root@VM-16-12-centos tools]# tar -zxvf nginx-1.22.0.tar.gz
......
[root@VM-16-12-centos tools]# cd nginx-1.22.0/
[root@VM-16-12-centos nginx-1.22.0]# ls -l
total 824
drwxr-xr-x 6 1001 1001   4096 Jun  2 15:49 auto
-rw-r--r-- 1 1001 1001 317070 May 24 07:59 CHANGES
-rw-r--r-- 1 1001 1001 484445 May 24 07:59 CHANGES.ru
drwxr-xr-x 2 1001 1001   4096 Jun  2 15:49 conf
-rwxr-xr-x 1 1001 1001   2590 May 24 07:59 configure
drwxr-xr-x 4 1001 1001   4096 Jun  2 15:49 contrib
drwxr-xr-x 2 1001 1001   4096 Jun  2 15:49 html
-rw-r--r-- 1 1001 1001   1397 May 24 07:59 LICENSE
drwxr-xr-x 2 1001 1001   4096 Jun  2 15:49 man
-rw-r--r-- 1 1001 1001     49 May 24 07:59 README
drwxr-xr-x 9 1001 1001   4096 Jun  2 15:49 src
```

7、配置使用默认配置，在 nginx 根目录下执行：

1. `./configure`
2. `make`
3. `make install`

查找安装路径：

```shell
[root@VM-16-12-centos nginx-1.22.0]# whereis nginx
nginx: /usr/local/nginx
```

# Nginx 常用命令

1、进入 Nginx 目录：

```shell
[root@VM-16-12-centos home]# cd /usr/local/nginx/
[root@VM-16-12-centos nginx]# ls -l
total 16
drwxr-xr-x 2 root root 4096 Jun  2 15:53 conf
drwxr-xr-x 2 root root 4096 Jun  2 15:53 html
drwxr-xr-x 2 root root 4096 Jun  2 15:53 logs
drwxr-xr-x 2 root root 4096 Jun  2 15:53 sbin
```

2、进入 sbin（执行文件）目录，启动 nginx：

```shell
[root@VM-16-12-centos nginx]# cd sbin
[root@VM-16-12-centos sbin]# ls
nginx
[root@VM-16-12-centos sbin]# ./nginx 
[root@VM-16-12-centos sbin]# 		# 没有任何反应就是启动成功了
```

使用服务器的公网ip访问测试（服务器需要开启80端口）：

![image-20220602160324259](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/image-20220602160324259.png)

3、停止/安全退出 nginx：

```shell
[root@VM-16-12-centos sbin]# ./nginx -s stop	# 停止
[root@VM-16-12-centos sbin]# 

[root@VM-16-12-centos sbin]# ./nginx -s quit	# 安全退出
[root@VM-16-12-centos sbin]# 
```

4、重新加载配置文件：

```shell
[root@VM-16-12-centos sbin]# ./nginx -s reload
[root@VM-16-12-centos sbin]# 
```

5、查看 nginx 进程：

```shell
[root@VM-16-12-centos sbin]# ps aux|grep nginx
root     11602  0.0  0.0 112816   984 pts/0    S+   16:07   0:00 grep --color=auto nginx
```

# Nginx 实战













