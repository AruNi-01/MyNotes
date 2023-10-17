## 1. 容器技术原理

提起容器就不得不说 **chroot**，因为 chroot 是最早的容器雏形。

chroot 意味着 **切换根目录**，有了 chroot 就意味着我们可以 **把任何目录更改为当前进程的根目录**，这与容器非常相似，下面我们通过一个实例了解下 chroot。

### 1.1 chroot

chroot 针对正在运作的 **进程和其子进程**，**改变它外显的根目录**。一个运行在这个环境下，经由 chroot 设置根目录的程序，**它不能够对这个指定根目录之外的文件进行访问动作**（读写都禁止）。

但是，此时还不能称之为一个容器，因为 **网络信息并没有隔离**，实际上 **进程等信息此时也并未隔离**。要想实现一个完整的容器，我们还需要 Linux 的其他三项技术： **Namespace、Cgroups 和联合文件系统**。

Docker 是利用 Linux 的 Namespace 、Cgroups 和联合文件系统三大机制来保证实现的，它的原理是：

- 使用 **Namespace** 做 **主机名、网络、PID 等资源的隔离**；
- 使用 **Cgroups** 对 **进程或者进程组做资源（例如：CPU、内存等）的限制**；
- 使用 **联合文件系统** 用于 **镜像构建和容器运行环境**。

### 1.2 Namespace

Namespace 是 Linux 内核的一项功能，该功能 **对内核资源进行隔离**，使得容器中的进程都可以在 **单独的命名空间中运行，并且只可以访问当前容器命名空间的资源**。Namespace 可以 **隔离进程 ID、主机名、用户 ID、文件名、网络访问和进程间通信等相关资源**。

Docker 主要用到以下五种命名空间。

- pid namespace：用于隔离进程 ID。
- net namespace：隔离网络接口，在虚拟的 net namespace 内用户可以拥有自己独立的 IP、路由、端口等。
- mnt namespace：文件系统挂载点隔离。
- ipc namespace：信号量,消息队列和共享内存的隔离。
- uts namespace：主机名和域名的隔离。

### 1.3 Cgroups

Cgroups 是一种 Linux 内核功能，可以 **限制和隔离进程的资源使用情况**（CPU、内存、磁盘 I/O、网络等）。在容器的实现中，Cgroups 通常用来 **限制容器的 CPU 和内存等资源的使用**。

### 1.4 联合文件系统

联合文件系统，又叫 UnionFS，是一种通过创建文件层进程操作的文件系统，因此，联合文件系统非常轻快。Docker 使用联合文件系统为容器 **提供构建层**，使得 **容器可以实现写时复制以及镜像的分层构建和存储**。常用的联合文件系统有 AUFS、Overlay 和 Devicemapper 等。

## 2. 镜像

**镜像是只读的**，基于镜像生成的容器才可读写的，想要生成镜像：

- 使用 `docker commit` 命令基于容器生成一个新的镜像；
- 使用 Dockerfile 文件，配合 `docker build` 命令生成一个镜像。

### 2.1 镜像的实现原理

其实 Docker 镜像是由 **一系列镜像层（layer）组成** 的，**每一层代表了镜像构建过程中的一次提交**。下面以一个镜像构建的 Dockerfile 来说明镜像是如何分层的。

```bash
FROM busybox
COPY test /tmp/test
RUN mkdir /tmp/testdir
```

上面的 Dockerfile 由三步组成：

- 第一行基于 busybox 创建一个镜像层；
- 第二行拷贝本机 test 文件到镜像内；
- 第三行在 /test 文件夹下创建一个目录 testdir。

为了验证镜像的存储结构，我们使用 `docker build` 命令在上面 Dockerfile 所在目录构建一个镜像：

```ruby
$ docker build -t mybusybox .
```

这里我的 Docker 使用的是 overlay2 文件驱动，进入 到`/var/lib/docker/overlay2` 目录下使用 `tree .` 命令查看产生的镜像文件：

```lua
$ tree .
# 以下为 tree . 命令输出内容
|-- 3e89b959f921227acab94f5ab4524252ae0a829ff8a3687178e3aca56d605679
|   |-- diff  # 这一层为基础层，对应上述 Dockerfile 第一行，包含 busybox 镜像所有文件内容，例如 /etc,/bin,/var 等目录
... 此次省略部分原始镜像文件内容
|   `-- link 
|-- 6591d4e47eb2488e6297a0a07a2439f550cdb22845b6d2ddb1be2466ae7a9391
|   |-- diff   # 这一层对应上述 Dockerfile 第二行，拷贝 test 文件到 /tmp 文件夹下，因此 diff 文件夹下有了 /tmp/test 文件
|   |   `-- tmp
|   |       `-- test
|   |-- link
|   |-- lower
|   `-- work
|-- backingFsBlockDev
|-- bec6a018080f7b808565728dee8447b9e86b3093b16ad5e6a1ac3976528a8bb1
|   |-- diff  # 这一层对应上述 Dockerfile 第三行，在 /tmp 文件夹下创建 testdir 文件夹，因此 diff 文件夹下有了 /tmp/testdir 文件夹
|   |   `-- tmp
|   |       `-- testdir
|   |-- link
|   |-- lower
|   `-- work
...
```

通过上面的目录结构可以看到，Dockerfile 的每一行命令，都生成了一个镜像层，**每一层的 diff 夹下只存放了增量数据**，如图 2 所示。

![image-20230718145326279](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202307181453616.png)

分层的结构使得 Docker 镜像非常轻量，**每一层根据镜像的内容都有一个唯一的 ID 值**，当不同的镜像之间有相同的镜像层时，便可以实现不同的镜像之间 **共享镜像层** 的效果。

总结一下， Docker 镜像是静态的分层管理的文件组合，**镜像底层的实现依赖于联合文件系统**（UnionFS）。充分掌握镜像的原理，可以帮助我们在生产实践中构建出最优的镜像，同时也可以帮助我们更好地理解容器和镜像的关系。

## 3. 容器

容器是基于镜像创建的可运行实例，并且单独存在，一个镜像可以创建出多个容器。运行容器化环境时，实际上是在容器内部创建该文件系统的读写副本。 这将 **添加一个容器层，该层允许修改镜像的整个副本**。

![image-20230718151423621](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202307181514607.png)

容器的生命周期：

![image-20230718151609847](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202307181516069.png)

## 4. Dockerfile 最佳实践

### 4.1 单一职责

由于容器的本质是进程，一个容器代表一个进程，因此不同功能的应用应该尽量拆分为不同的容器，每个容器只负责单一业务进程。

### 4.2 提供注释信息

Dockerfile 也是一种代码，我们应该保持良好的代码编写习惯，晦涩难懂的代码尽量添加注释，让协作者可以一目了然地知道每一行代码的作用，并且方便扩展和使用。

### 4.3 保持容器最小化

应该避免安装无用的软件包，比如在一个 nginx 镜像中，我并不需要安装 vim 、gcc 等开发编译工具。这样不仅可以加快容器构建速度，而且可以避免镜像体积过大。

### 4.4 合理选择基础镜像

容器的核心是应用，因此只要基础镜像能够满足应用的运行环境即可。例如一个 `Java` 类型的应用运行时只需要 `JRE`，并不需要 `JDK`，因此我们的基础镜像只需要安装 `JRE` 环境即可。

### 4.5 使用 .dockerignore 文件

在使用 `git` 时，我们可以使用 `.gitignore` 文件忽略一些不需要做版本管理的文件。同理，使用 `.dockerignore` 文件允许我们在构建时，忽略一些不需要参与构建的文件，从而提升构建效率。`.dockerignore `的定义类似于 `.gitignore`。

### 4.6 最小化镜像层数

在构建镜像时尽可能地减少 Dockerfile 指令行数。例如我们要在 CentOS 系统中安装 `make` 和 `net-tools` 两个软件包，应该在 Dockerfile 中使用以下指令：

```go
RUN yum install -y make net-tools
```

而不应该写成这样：

```go
RUN yum install -y make
RUN yum install -y net-tools
```

### 4.7 Dockerfile 指令

使用 RUN 指令时应该尽量遵循以下原则：

- 当 `RUN` 指令后面跟的内容比较复杂时，建议使用反斜杠（\） 结尾并且换行；
- `RUN `指令后面的内容尽量按照字母顺序排序，提高可读性。

使用 CMD 和 ENTRYPOINT 需要特别注意两点：

- **CMD**: 指定容器启动时要运行的命令，在 docker run 的时候运行。**注意**：

  - **DockerFile 中可以有多个 CMD，但只有最后一个会生效**；
  - **当 docker run 后面有命令时，CMD 命令会被覆盖**。因为如果在 run 后面加了命令参数，**相当于又在 DockerFile 最后一行加了这个 CMD 命令**，就把原来的覆盖了。

  ![image-20230522172843533](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305221728257.png)

- **ENTRYPOINT**: 也是用来指定容器启动时要运行的命令，类似于 CMD，但是：

  - **不会被 docker run 后面的命令覆盖**，而且这些命令参数会被当做参数传给 ENTRYPOINT 指令指定的程序；
  - **也只有最后一个 ENTRYPOINT 会生效**。

  ![image-20230522173929988](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202305221739566.png)

在使用 ADD 和 COPY 时：

`ADD` 和 `COPY` 指令功能类似，都是从外部往容器内添加文件。但是：

- `COPY `指令只支持基本的文件和文件夹拷贝功能；
- `ADD `则支持更多文件来源类型，比如自动提取 tar 包，并且可以支持源文件为 URL 格式。

**更推荐使用 `COPY` 指令**，因为 `COPY` 指令更加透明，仅支持本地文件向容器拷贝，而且使用 `COPY` 指令可以更好地利用构建缓存，有效减小镜像体积。

## 5. Docker 安全

### 5.1 Docker 与虚拟机的区别

![image-20230718164951222](https://run-notes.oss-cn-beijing.aliyuncs.com/notes/202307181649306.png)

**虚拟机** 是通过 **管理系统 (Hypervisor) 模拟出 CPU、内存、网络等硬件**，然后在这些 **模拟的硬件上创建客户内核和操作系统**。这样做的好处就是 **虚拟机有自己的内核和操作系统**，并且硬件都是通过虚拟机管理系统模拟出来的，**用户程序无法直接使用到主机的操作系统和硬件资源**，因此 **虚拟机也对隔离性和安全性有着更好的保证**。

而 Docker 容器则是通过 Linux 内核的 Namespace 技术实现了文件系统、进程、设备以及网络的隔离，然后再d通过 Cgroups 对 CPU、 内存等资源进行限制，最终实现了容器之间相互不受影响，由于 **容器的隔离性仅仅依靠内核来提供，因此容器的隔离性也远弱于虚拟机**。

但容器与虚拟机相比，**容器的性能损耗非常小，并且镜像也非常小**，而且在业务快速开发和迭代的今天，容器秒级的启动等特性也非常匹配业务快速迭代的业务场景。

既然我们要利用容器的优点，那有没有什么办法可以尽量弥补容器弱隔离的安全性缺点呢？

### 5.2 Docker 容器的安全问题

**1、Docker 自身安全**

Docker 作为一款容器引擎，本身也会存在一些安全漏洞，CVE 目前已经记录了多项与 Docker 相关的安全漏洞，主要有权限提升、信息泄露等几类安全问题。

**2、镜像安全**

由于 Docker 容器是基于镜像创建并启动，因此镜像的安全直接影响到容器的安全。具体影响镜像安全的总结如下。

- 镜像软件存在安全漏洞：由于容器需要安装基础的软件包，如果软件包存在漏洞，则可能会被不法分子利用并且侵入容器，影响其他容器或主机安全。
- 仓库漏洞：无论是 Docker 官方的镜像仓库还是我们私有的镜像仓库，都有可能被攻击，然后篡改镜像，当我们使用镜像时，就可能成为攻击者的目标对象。
- 用户程序漏洞：用户自己构建的软件包可能存在漏洞或者被植入恶意脚本，这样会导致运行时提权影响其他容器或主机安全。

**3、Linux 内核隔离性不够**

尽管目前 Namespace 已经提供了非常多的资源隔离类型，但是仍有部分关键内容没有被完全隔离，其中包括一些系统的关键性目录（如 /sys、/proc 等），这些关键性的目录可能会泄露主机上一些关键性的信息，让攻击者利用这些信息对整个主机甚至云计算中心发起攻击。

而且仅仅依靠 Namespace 的隔离是远远不够的，因为一旦内核的 Namespace 被突破，使用者就有可能直接提权获取到主机的超级权限，从而影响主机安全。

**4、所有容器共享主机内核**

由于同一宿主机上所有容器共享主机内核，所以攻击者可以利用一些特殊手段导致内核崩溃，进而导致主机宕机影响主机上其他服务。

### 5.3 如何解决

**1、Docker 自身安全性改进**

事实上，Docker 从 2013 年诞生到现在，在安全性上面已经做了非常多的努力。目前 Docker 在默认配置和默认行为下是足够安全的。

Docker 自身是基于 Linux 的多种 Namespace 实现的，其中有一个很重要的 Namespace 叫作 User Namespace，**User Namespace 主要是用来做容器内用户和主机的用户隔离的**。

在过去容器里的 root 用户就是主机上的 root 用户，如果容器受到攻击，或者容器本身含有恶意程序，在容器内就可以直接获取到主机 root 权限。Docker 从 1.10 版本开始，**使用 User Namespace 做用户隔离，实现了容器中的 root 用户映射到主机上的非 root 用户**，从而大大减轻了容器被突破的风险。

在大多数情况下，容器并不需要主机的 root 权限，Docker 默认情况下也是不开启额外特权的。在执行 `docker run` 命令启动容器时，如非特殊可控情况，`--privileged` 参数不允许设置为 `true`。

因此，我们尽可能地使用 Docker 最新版本就可以得到更好的安全保障。

**2、保障镜像安全**

为保障镜像安全，我们可以在私有镜像仓库 **安装镜像安全扫描组件**，对上传的镜像进行检查，通过与 CVE 数据库对比，一旦发现有漏洞的镜像及时通知用户或阻止非安全镜像继续构建和分发。同时为了确保我们使用的镜像足够安全，在拉取镜像时，要确保只从受信任的镜像仓库拉取，并且与镜像仓库通信一定要使用 HTTPS 协议。

**3、加强内核安全和管理**

由于仅仅依赖内核的隔离可能会引发安全问题，因此我们对于内核的安全应该更加重视。可以从以下几个方面进行加强：

- **宿主机及时升级内核漏洞**：宿主机内核应该尽量安装最新补丁，因为更新的内核补丁往往有着更好的安全性和稳定性。

- **使用 Capabilities 划分权限**：Capabilities 是 Linux 内核的概念，Linux 将系统权限分为了多个 Capabilities，它们都可以单独地开启或关闭，Capabilities 实现了系统更细粒度的访问控制。

- **资源限制**：在生产环境中，建议每个容器都添加相应的资源限制。下面给出一些执行`docker run`命令启动容器时可以传递的资源限制参数：

  ```css
    --cpus                          限制 CPU 配额
    -m, --memory                    限制内存配额
    --pids-limit                    限制容器的 PID 个数
  ```

  例如我想要启动一个 1 核 2G 的容器，并且限制在容器内最多只能创建 1000 个 PID，启动命令如下：

  ```lua
  $ docker run -it --cpus=1 -m=2048m --pids-limit=1000 busybox sh
  ```

  推荐在生产环境中限制 CPU、内存、PID 等资源，这样即便应用程序有漏洞，也不会导致主机的资源完全耗尽，最大限度降低安全风险。

**4、使用安全容器**

容器有着轻便快速启动的优点，虚拟机有着安全隔离的优点，有没有一种技术可以兼顾两者的优点，做到既轻量又安全呢？

答案是有，那就是 **安全容器**。安全容器是相较于普通容器的，安全容器与普通容器的主要区别在于，**安全容器中的每个容器都运行在一个单独的微型虚拟机中，拥有独立的操作系统和内核，并且有虚拟化层的安全隔离**。

安全容器目前推荐的技术方案是 [Kata Containers](https://github.com/kata-containers)，Kata Container 并不包含一个完整的操作系统，只有一个精简版的 Guest Kernel 运行着容器本身的应用，并且通过减少不必要的内存，尽量共享可以共享的内存来进一步减少内存的开销。另外，Kata Container 实现了 OCI 规范，可以直接使用 Docker 的镜像启动 Kata 容器，具有开销更小、秒级启动、安全隔离等许多优点。

## 6. Namespace 资源隔离

Namespace 是 Linux 内核的一个特性，该特性可以实现 **在同一主机系统中，对进程 ID、主机名、用户 ID、文件名、网络和进程间通信等资源的隔离**。Docker 利用 Linux 内核的 Namespace 特性，实现了每个容器的资源相互隔离，从而保证容器内部只能访问到自己 Namespace 的资源。

最新的 Linux 5.6 内核中提供了 8 种类型的 Namespace：

| Namespace 名称                   | 作用                                      | 内核版本 |
| :------------------------------- | :---------------------------------------- | :------- |
| Mount（mnt）                     | 隔离挂载点                                | 2.4.19   |
| Process ID (pid)                 | 隔离进程 ID                               | 2.6.24   |
| Network (net)                    | 隔离网络设备，端口号等                    | 2.6.29   |
| Interprocess Communication (ipc) | 隔离 System V IPC 和 POSIX message queues | 2.6.19   |
| UTS Namespace(uts)               | 隔离主机名和域名                          | 2.6.19   |
| User Namespace (user)            | 隔离用户和用户组                          | 3.8      |
| Control group (cgroup) Namespace | 隔离 Cgroups 根目录                       | 4.6      |
| Time Namespace                   | 隔离系统时间                              | 5.6      |

最新版本的 Docker 只使用了 Mount Namespace、PID Namespace、Net Namespace、IPC Namespace、UTS Namespace、User Namespace。

### 6.1 Mount Namespace

Mount Namespace 可以用来隔离不同的进程或进程组看到的挂载点。通俗地说，就是可以实现在不同的进程中看到不同的挂载目录。

使用 Mount Namespace 可以实现 **容器内只能看到自己的挂载信息，在容器内的挂载操作不会影响主机的挂载目录**。

> 挂载（Mount）是指将一个文件系统与文件系统树中的一个目录建立关联的过程。通过挂载，我们可以将一个磁盘分区、CD-ROM、USB存储设备、网络文件系统等设备或文件，与Linux文件系统中的一个目录关联起来，一旦挂载成功，我们就可以通过挂载点来访问和操作这些设备或文件，就像访问本地文件系统一样。

### 6.2 PID Namespace

PID Namespace 的作用是用来 **隔离进程**。**在不同的 PID Namespace 中，进程可以拥有相同的 PID 号**，利用 PID Namespace 可以实现每个容器的主进程为 1 号进程，**而容器内的进程在主机上却拥有不同的PID**。

例如一个进程在主机上 PID 为 122，使用 PID Namespace 可以实现该进程在容器内看到的 PID 为 1。

### 6.3 UTS Namespace

UTS Namespace 主要是用来 **隔离主机名的**，它允许 **每个 UTS Namespace 拥有一个独立的主机名**。

例如我们的主机名称为 docker，使用 UTS Namespace 可以实现在容器内的主机名称为 lagoudocker 或者其他任意自定义主机名。

### 6.4 IPC Namespace

IPC Namespace 主要是用来 **隔离进程间通信的**。

例如 **PID Namespace 和 IPC Namespace 一起使用可以实现同一 IPC Namespace 内的进程彼此可以通信，不同 IPC Namespace 的进程却不能通信**。

### 6.5 User Namespace

User Namespace 主要是用来 **隔离用户和用户组的**。一个比较典型的应用场景就是 **在主机上以非 root 用户运行的进程可以在一个单独的 User Namespace 中映射成 root 用户**。使用 User Namespace 可以实现 **进程在容器内拥有 root 权限，而在主机上却只是普通用户**。

### 6.6 Net Namespace

Net Namespace 是用来 **隔离网络设备、IP 地址和端口等信息的**。Net Namespace 可以让每个进程拥有自己独立的 IP 地址，端口和网卡信息。

例如主机 IP 地址为 172.16.4.1 ，容器内可以设置独立的 IP 地址为 192.168.1.1。

### 6.7 为什么需要 Namespace

当 Docker 新建一个容器时， 它会创建这六种 Namespace，然后将容器中的进程加入这些 Namespace 之中，使得 Docker 容器中的进程只能看到当前 Namespace 中的系统资源。

正是由于 Docker 使用了 Linux 的这些 Namespace 技术，才实现了 Docker 容器的隔离，可以说没有 Namespace，就没有 Docker 容器。



## 7. Cgroups 资源隔离

容器内的进程仍然可以任意地使用主机的 CPU 、内存等资源，如果某一个容器使用的主机资源过多，可能导致主机的资源竞争，进而影响业务。

那如果我们想 **限制一个容器资源的使用**（如 CPU、内存等）应该如何做呢？这里就需要用到 Linux 内核的另一个核心技术cgroups。

cgroups（全称：control groups）是 Linux 内核的一个功能，它可以 **限制进程或者进程组的资源**（如 CPU、内存、磁盘 IO 等）。

### 7.1 Cgroups 功能

cgroups 主要提供了如下功能。

- **资源限制**： 限制资源的使用量，例如我们可以通过限制某个业务的内存上限，从而保护主机其他业务的安全运行。
- **优先级控制**：不同的组可以有不同的资源（ CPU 、磁盘 IO 等）使用优先级。
- **审计**：计算控制组的资源使用情况。
- **控制**：控制进程的挂起或恢复。

Cgroups 使用到了 Linux 的一个核心概念—**子系统**（subsystem）：是一个内核的组件，**一个子系统代表一类资源调度控制器**。例如 **内存子系统可以限制内存的使用量，CPU 子系统可以限制 CPU 的使用时间**。

### 7.2 Docker 如何使用 Cgroups

首先，我们使用以下命令创建一个 nginx 容器：

```sh
docker run -it -m=1g nginx
```

上述命令创建并启动了一个 nginx 容器，并且限制内存为 1G。然后我们进入 cgroups 内存子系统的目录，使用 ls 命令查看一下该目录下的内容：

```sh
# ls -l /sys/fs/cgroup/memory

total 0

-rw-r--r--.  1 root root 0 Sep  1 11:50 cgroup.clone_children

--w--w--w-.  1 root root 0 Sep  1 11:50 cgroup.event_control

-rw-r--r--.  1 root root 0 Sep  1 11:50 cgroup.procs

-r--r--r--.  1 root root 0 Sep  1 11:50 cgroup.sane_behavior

drwxr-xr-x.  3 root root 0 Sep  5 10:50 docker

... 省略部分输出
```

通过上面输出可以看到，该目录下有一个 docker 目录，该目录正是 Docker 在内存子系统下创建的。我们进入到 docker 目录下查看一下相关内容：

```shell
# cd /sys/fs/cgroup/memory/docker

# ls -l

total 0

drwxr-xr-x. 2 root root 0 Sep  5 10:49 cb5c5391177b44ad87636bf3840ecdda83529e51b76a6406d6742f56a2535d5e

-rw-r--r--. 1 root root 0 Sep  4 10:40 cgroup.clone_children

--w--w--w-. 1 root root 0 Sep  4 10:40 cgroup.event_control

-rw-r--r--. 1 root root 0 Sep  4 10:40 cgroup.procs

... 省略部分输出

-rw-r--r--. 1 root root 0 Sep  4 10:40 tasks
```

可以看到 docker 的目录下有一个一串随机 ID 的目录，该目录即为我们上面创建的 nginx 容器的 ID。然后我们进入该目录，查看一下该容器的 memory.limit_in_bytes 文件的内容。

```shell
# cd cb5c5391177b44ad87636bf3840ecdda83529e51b76a6406d6742f56a2535d5e

# cat memory.limit_in_bytes

1073741824
```

可以看到内存限制值正好为 1G。 事实上，**Docker 创建容器时，Docker 会根据启动容器的参数，在对应的 cgroups 子系统下创建以容器 ID 为名称的目录, 然后根据容器启动时设置的资源限制参数, 修改对应的 cgroups 子系统资源限制文件, 从而达到资源限制的效果**。













