---
sidebar_position: 12
---
# 容器化

容器可以理解为轻量的虚拟机，可以让你的程序在不同系统上运行，例如：在windows写的代码运行在mac上，mac的代码运行在Linux上。

容器化 最初使用Linux Containers (LXC)，但后来改用runC（以前称为libcontainer），它运行在与其主机相同的操作系统中。这允许它共享大量的主机操作系统资源。此外，它使用分层文件系统 ( AuFS ) 并管理网络。

AuFS 是一个分层文件系统，因此您可以将只读部分和写入部分合并在一起。可以将操作系统的公共部分设为只读（并在所有容器之间共享），然后为每个容器提供自己的挂载以供写入。

因此，假设您有一个 1 GB 的容器映像；如果您想使用完整的 VM，则需要 1 GB x 所需的 VM 数。使用 Docker 和 AuFS，您可以在所有容器之间共享 1 GB 的大部分，如果您有 1000 个容器，您仍然可能只有 1 GB 多一点的空间用于容器操作系统（假设它们都运行相同的操作系统映像） .

一个完整的虚拟化系统获得分配给它自己的一组资源，并进行最少的共享。你得到更多的隔离，但它更重（需要更多的资源）。使用 容器化，您可以获得更少的隔离，需要更少的资源。因此，您可以轻松地在主机上运行数千个容器。

一个完整的虚拟化系统通常需要几分钟才能启动，而 Docker/LXC/runC 容器需要几秒钟，通常甚至不到一秒钟。

每种类型的虚拟化系统各有利弊。如果您想要完全隔离并保证资源，那么完整的 VM 是您的最佳选择。如果您只是想将进程彼此隔离并希望在合理大小的主机上运行大量进程，那么 Docker/LXC/runC 似乎是要走的路。

## Docker

[官方文档](https://yeasy.gitbook.io/docker_practice/)
[民间文档](http://shouce.jb51.net/docker_practice/)

### 安装

#### win和mac安装

https://www.docker.com/get-started

#### Linux-Ubuntu安装

```
curl -sSL https://get.daocloud.io/docker | sh
```

#### Linux-宝塔面板安装

![image.png](/upload/computerselfeducationroad/image-145c19a352e4465d96be084dff23bf5b.png)

### 检查安装版本确认是否安装成功

win检查是否安装成功的命令：

```
docker version
```

Linux检查是否安装成功的命令：

```
docker -v
```

### Docker原理图

![image.png](/upload/computerselfeducationroad/image-a8bc2ebbb9594825b0a8e013520d706d.png)

### 核心配置

#### build

build一个镜像的时候，如下关键字必不可少：

```language
FROM mcr.microsoft.com/playwright/python:v1.22.0-focal
RUN mkdir -p /opt/yueji/spiderCore && mkdir -p /opt/dervice
ADD spiderCore.tar.gz /opt/yueji/spiderCore
RUN pip3 install --upgrade pip -i https://mirrors.aliyun.com/pypi/simple/ && pip install -r /opt/yueji/spiderCore/requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple/ && rm -rf /root/.cache/pip/
RUN playwright install chromium
RUN apt-get update 
RUN apt-get install wget  
RUN apt-get install vim
WORKDIR /opt/yueji/spiderCore
ENTRYPOINT ["python3","main.py"]
```

#### FROM

FROM 指定基础镜像
所谓定制镜像，那一定是以一个镜像为基础，在其上进行定制。就像我们之前运行了一个 nginx 镜像的容器，再进行修改一样，基础镜像是必须指定的。而 FROM 就是指定基础镜像，因此一个 Dockerfile 中 FROM 是必备的指令，并且必须是第一条指令。

在 Docker Hub (https://hub.docker.com/explore/) 上有非常多的高质量的官方镜像， 有可以直接拿来使用的服务类的镜像，如 nginx、redis、mongo、mysql、httpd、php、tomcat 等； 也有一些方便开发、构建、运行各种语言应用的镜像，如 node、openjdk、python、ruby、golang 等。 可以在其中寻找一个最符合我们最终目标的镜像为基础镜像进行定制。 如果没有找到对应服务的镜像，官方镜像中还提供了一些更为基础的操作系统镜像，如 ubuntu、debian、centos、fedora、alpine 等，这些操作系统的软件库为我们提供了更广阔的扩展空间。

除了选择现有镜像为基础镜像外，Docker 还存在一个特殊的镜像，名为 scratch。这个镜像是虚拟的概念，并不实际存在，它表示一个空白的镜像。

FROM scratch

如果你以 scratch 为基础镜像的话，意味着你不以任何镜像为基础，接下来所写的指令将作为镜像第一层开始存在。

不以任何系统为基础，直接将可执行文件复制进镜像的做法并不罕见，比如 swarm、coreos/etcd。对于 Linux 下静态编译的程序来说，并不需要有操作系统提供运行时支持，所需的一切库都已经在可执行文件里了，因此直接 FROM scratch 会让镜像体积更加小巧。使用 Go 语言 开发的应用很多会使用这种方式来制作镜像，这也是为什么有人认为 Go 是特别适合容器微服务架构的语言的原因之一。

#### ADD & COPY

ADD 指令和 COPY 的格式和性质基本一致。但是在 COPY 基础上增加了一些功能。

比如 <源路径> 可以是一个 URL，这种情况下，Docker 引擎会试图去下载这个链接的文件放到 <目标路径> 去。下载后的文件权限自动设置为 600，如果这并不是想要的权限，那么还需要增加额外的一层 RUN 进行权限调整，另外，如果下载的是个压缩包，需要解压缩，也一样还需要额外的一层 RUN 指令进行解压缩。所以不如直接使用 RUN 指令，然后使用 wget 或者 curl 工具下载，处理权限、解压缩、然后清理无用文件更合理。因此，这个功能其实并不实用，而且不推荐使用。

如果 <源路径> 为一个 tar 压缩文件的话，压缩格式为 gzip, bzip2 以及 xz 的情况下，ADD 指令将会自动解压缩这个压缩文件到 <目标路径> 去。

#### ENTRYPOINT & RUN

ENTRYPOINT 的格式和 RUN 指令格式一样，分为 exec 格式和 shell 格式。

ENTRYPOINT 的目的和 CMD 一样，都是在指定容器启动程序及参数。ENTRYPOINT 在运行时也可以替代，不过比 CMD 要略显繁琐，需要通过 docker run 的参数 --entrypoint 来指定。

#### WORKDIR

切换到镜像中的指定路径，设置工作目录
在 WORKDIR 中需要使用绝对路径，如果镜像中对应的路径不存在，会自动创建此目录
一般用 WORKDIR 来替代 RUN cd `<path>` && `<do something>` 切换目录进行操作的指令
WORKDIR 指令为 Dockerfile 中跟随它的任何 RUN、CMD、ENTRYPOINT、COPY、ADD 指令设置工作目录
如果 WORKDIR 不存在，即使它没有在任何后续 Dockerfile 指令中使用，它也会被创建

### Demo

开启指定服务

```
Docker run -p 8080:80 -d {name}

Docker run -p 8080:80 -d nginx
```

拷贝文件至image(类似git里的add暂存)

```
docker cp {path} {id}{id_path}

示例：
docker cp C:/Users/SY/Desktop/docktest/index.html {id}://usr/share/nginx/html
```

保存(类似git里的commit)

```
Docker commit -m 'fun' {id} 'name'

示例：
Docker commit(固定语法) -m（主分支） 'fun'（注释） 'name'(image的名字)
```

删除多余的image

```
Docker rmi {id} 
Docker rmi（删除）
```

如果是在自己的电脑上操作，可以在vscode内下载docker插件，即可查看对应容器状态。选中容器可以右键进行：删除、启动、停止等操作。也可以在Docker可视化界面进行操作。下方分别为：vscode界面与Docker界面

![image.png](/upload/computerselfeducationroad/image-1d636c1c43f84420af4e4501251d0257.png)

![image.png](/upload/computerselfeducationroad/image-c6461a3269ea4692984a883fa303afc6.png)

可以通过如下命令进入到容器内部的命令窗口：

```
docker ps -a
# 查看所有容器id

docker exec -it  【容器的id】/bin/bash
# 根据id选择容器进入容器内部命令窗口

pip install pandas
python -m unittest test.test_spider.MyTestCase.test_spider
# 接着像平常一样输入命令代码即可，上方两句代码只是举例。
```

更多命令可以查看以下链接:
https://www.runoob.com/docker/docker-tutorial.html

### 简易排错

这个容器好像卡死了，查一下。

#### 观察并记录

接手后通过以下命令先观察下

```
# 查看资源占用
top

# 静态查看容器资源占用
docker stats --no-stream
# 动态查看容器资源占用
docker stats
```

CPU资源占用升到了280%，持续了30分钟，比较离谱。
内存占了8个G，这个程序需要模拟诸多浏览器且要保持缓存以便通信，所以8个G也不算离谱。

> docker显示的cpu占用是可以超100%的，表示使用了多个核，200%表示用了2个核。合理的飙升：大数运算环节。异常的飙升：死循环、报错后日志不停歇的高速打印。

#### 分析并思考

2个可能：
1.因为CPU异常，所以可能是阻塞导致的，要查下阻塞的原因，因为代码里写了很多线程。
2.但是我等了30分钟，也可能是阻塞导致死锁了，这个可能性我觉得更大，内存一直被占着可以解释为死锁后资源无法释放，也合理。

> （1）阻塞是由于资源不足引起的排队等待现象。
> （2）死锁是由于两个对象在拥有一份资源的情况下申请另一份资源，而另一份资源恰好又是这两对象正持有的，导致两对象无法完成操作，且所持资源无法释放。

#### 缩小范围

```
# 进入容器
docker exec -iter 【你的容器id】 /bin/bash
# 示例
docker exec -iter 6d711f6ee058 /bin/bash
# 检查日志文件
cd var/log
ls

# 退出容器
exit

# 复制日志
docker cp 容器id:文件路径 本机路径
```
## k8s

Kubernetes 是一个开源的容器编排引擎，用来对容器化应用进行自动化部署、 扩缩和管理。