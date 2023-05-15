# Docker

## 概述

概述：基于Go语言开发的应用容器引擎

目的：解决项目部署时需要搭建环境的问题

核心思想：隔离，即打包装箱，每一个容器/箱子都是相互隔离的

作用：

> 1、容器
>
> 2、通过隔离机制，能将服务器利用到极致：Docker是内核级别的虚拟化
>
> 3、跨平台
>
> 4、**更便捷的升级和扩缩容**，如升级了插件

虚拟机：属于虚拟化技术，该虚拟机相当于一台独立的电脑，与其他虚拟机也有隔离，但其占用存储空间大（几个G），启动慢（min，因为冗余的步骤多）

容器化技术：不是模拟一个完整的操作系统，而是只模拟一个环境lib和应用，内核kernel不会模拟，容器是直接运行在宿主机的内核中。

### Docker相对于虚拟机VM的优势

1、轻巧（最小的核心环境才4M）

> 只模拟一个环境lib和应用，内核kernel不会模拟

2、秒级启动（s）

> Docker的抽象层比虚拟机少
>
> Docker用的是宿主机的内核，vm需要的是Guest OS
>

即：新建容器的时候，Docker不需要像虚拟机一样重新加载一个操作系统内核，避免引导性操作。而虚拟机加载操作系统内核Guest OS很慢，min起步。

### 底层原理

Docker是一个Client - Server结构的系统 ，Docker的守护进程运行在主机上。通过Socket从客户端访问。和MySQL类似

DockerServer接收到Docker - Client 的指令，就会执行这个命令

![docker-概念-底层原理](E:\笔记\笔记\image\docker-概念-底层原理.png)

## 安装

### 基本组成

镜像和容器的关系	可以比作	类（构造函数）和实例的关系

#### 镜像（image）

好比是一个模板，可以通过这个模板创建容器服务。镜像不能直接启动服务，而是先run，变为一个容器，然后才能启动变为一个可以使用的服务

#### 容器（container）

可以理解为一个简易的Linux系统

#### 仓库（repository）

存储镜像的地方，分私有云和公有云

### 安装Docker

#### 1、环境查看

```shell
# 系统内核是3.10及以上
[root@VM-24-9-centos /]# uname -r
3.10.0-1160.49.1.el7.x86_64
```

```shell
# 系统版本 尽量是CentOS7
[root@VM-24-9-centos /]# cat /etc/os-release
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

#### 2、跟着文档来

[docker安装文档 CentOS版本](https://docs.docker.com/engine/install/centos/)

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
                  
# 2.安装需要的安装包
yum install -y yum-utils
	# 成功会显示 Nothing to do

# 3.设置镜像仓库
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo		# 默认是国外的，尽量用国内的镜像，如百度搜索 docker阿里云镜像地址 
    # 阿里云：http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
    # 注意：yum需要python2执行，如果系统默认是3，需要vim /usr/bin/yum-config-manager进入，编辑第一行为 /usr/bin/python2 -tt 
    # 原因：yum对python依赖版本
    
# 4.更新yum软件包索引（建议做，但并非必做）
yum makecache fast	# makecache是清空缓存
    
# 5.安装引擎（不需要官方文档上的‘配置’这一步骤）
yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin		# ce是社区，ee是企业

# 6.启动docker
systemctl start docker
	#可以通过docker version 判断是否启动成功
	
# 7.运行镜像
docker run hello-world	# 此处以hello-world镜像为例
```

![docker-run-helloWorld](..\image\docker-run-helloWorld.png)

<img src="E:..\image\docker-run-运行原理.png" alt="docker-run-运行原理" style="zoom: 67%;" />

```shell
# 8.查看下载的镜像
docker images
```

### 卸载Docker

```shell
# 1.卸载依赖
yum remove docker-ce docker-ce-cli containerd.io docker-compose-plugin
# 2.删除资源
rm -rf /var/lib/docker
rm -rf /var/lib/containerd
```

### 阿里云镜像加速

去阿里云的容器镜像服务里面

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://qy1d97yw.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```



## 命令

### 帮助命令

```shell
docker version	 #显示docker的版本信息
docker info		 #显示docker的系统信息，包括镜像和容器数量
docker --help	 #这个命令展示全部docker命令
docker xxx --help #这个会展示xxx的所有可选项命令，如docker images --help 会展示其 -a -f -q的详细信息
```

### 镜像命令

#### 显示镜像：docker images

```shell
[root@VM-24-9-centos /]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
hello-world   latest    feb5d9fea6a5   16 months ago   13.3kB
```

| 属性       | 描述           |
| ---------- | -------------- |
| REPOSITORY | 镜像的仓库源   |
| TAG        | 镜像的标签     |
| IMAGE ID   | 镜像的id       |
| CREATED    | 镜像的创建时间 |
| SIZE       | 镜像的大小     |

| 可选项      | 描述           |
| ----------- | -------------- |
| -a，--all   | 列出所有镜像   |
| -q，--quiet | 只显示镜像的id |

注：可选项可以连用，如 `docker images -a -q`

#### 搜索镜像：docker search 

和官网搜索一样，但官网慢

```shell
[root@VM-24-9-centos /]# docker search mysql
NAME                            DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                           MySQL is a widely used, open-source relation…   13695     [OK]       
mariadb                         MariaDB Server is a high performing open sou…   5230      [OK]  
```



| 可选项       | 描述 | 例子                                                |
| ------------ | ---- | --------------------------------------------------- |
| -f，--filter | 过滤 | --filter=STARS=300    表示只保留收藏STARS大于3000的 |

#### 下载镜像：docker pull 

可选项同 images

```shell
[root@VM-24-9-centos /]# docker pull mysql
Using default tag: latest				# 不写tag，默认是latest
latest: Pulling from library/mysql
72a69066d2fe: Pull complete 			# 分层下载，docker image的核心	联合文件系统
93619dbc5b36: Pull complete 
99da31dd6142: Pull complete 
626033c43d70: Pull complete 
37d5d7efb64e: Pull complete 
ac563158d721: Pull complete 
d2ba16033dad: Pull complete 
688ba7d5c01a: Pull complete 
00e060b6d11d: Pull complete 
1c04857f594f: Pull complete 
4d7cfa90e6ea: Pull complete 
e0431212d27d: Pull complete 
Digest: sha256:e9027fe4d91c0153429607251656806cc784e914937271037f7738bd5b8e7709		# 签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest			# 真实地址

# 等价于
# docker pull mysql
# docker pull docker .io/library/mysql:latest
```

注：默认是使用latest最新版，如果需要指定版本，则 **`docker pull mysql[:tag]`**，tag表示指定的版本，如 `docker pull mysql:5.7`

##### 下载核心：分层下载

docker pull下载时，因为是分层下载，因此当下载一个已有但版本不同的镜像时，可以复用新老镜像中共用的一些内容。**极大的节省内存**

如：下载mysql:latest和mysql:5.7时，可以复用一些内容

```shell
[root@VM-24-9-centos /]# docker pull mysql:5.7
5.7: Pulling from library/mysql
72a69066d2fe: Already exists 			# 与mysql:latest复用的层
93619dbc5b36: Already exists 
99da31dd6142: Already exists 
626033c43d70: Already exists 
37d5d7efb64e: Already exists 
ac563158d721: Already exists 
d2ba16033dad: Already exists 
0ceb82207cd7: Pull complete 
37f2405cae96: Pull complete 
e2482e017e53: Pull complete 
70deed891d42: Pull complete 
Digest: sha256:f2ad209efe9c67104167fc609cca6973c8422939491c9345270175a300419f94
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7
```

<img src="..\image\docker-pull-result.png" alt="docker-pull-result" style="zoom:150%;" />

#### 删除镜像：docker rmi

| 可选项      | 描述                           |
| ----------- | ------------------------------ |
| -f，--force | 强制执行，省略删除时的确认步骤 |

```shell
docker rmi -f 镜像id					# 删除指定id的镜像
docker rmi -f 镜像id 镜像id 镜像id	  # 删除多个id的镜像
docker rmi -f $(docker images -aq)	   # 删除全部容器
```

##### 删除指定id的镜像

```shell
[root@VM-24-9-centos /]# docker rmi -f c20987f18b13		# 删除id为c20987f18b13的镜像
Untagged: mysql:5.7
Untagged: mysql@sha256:f2ad209efe9c67104167fc609cca6973c8422939491c9345270175a300419f94
.
.
.
```

##### 删除全部镜像

1、docker rmi -f $(docker images -aq)

```shell
[root@VM-24-9-centos /]# docker rmi -f $(docker images -aq)		# $(docker images -aq) 表示遍历全部镜像
Untagged: mysql:latest
Untagged: mysql@sha256:e9027fe4d91c0153429607251656806cc784e914937271037f7738bd5b8e7709
.
.
.
Untagged: hello-world:latest
Untagged: hello-world@sha256:aa0cc8055b82dc2509bed2e19b275c8f463506616377219d9642221ab53cf9fe
[root@VM-24-9-centos /]# docker images
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
```

2、

### 容器命令

有了镜像才可以创建容器，可以下载一个CentOs来学习

```shell
docker pull centos
```

#### 新建容器并启用：docker run image

```shell
docker run [可选项] image
```

参数说明

| 可选项                | 描述                                                       |
| --------------------- | ---------------------------------------------------------- |
| --name="自己取的名字" | 容器名字，用来区分容器。常见的命名有tomcat01               |
| -d                    | 后台运行方式。加了表示后台运行                             |
| -it                   | -i和-t的简写，表示使用交互方式运行，并能够进入容器查看内容 |
| -p                    | ！！！指定容器端口，可以和主机端口映射                     |
| -P                    | 随机指定容器的端口                                         |

-p：

> `-p 主机端口:容器端口`				实现主机端口映射到容器端口，可以实现外部访问容器	最常用
>
> `-p 容器端口`	`容器端口`（此处可以省略-p）							可以让容器不被外部访问，只能在主机内部被访问
>
> `-p ip:主机端口:容器端口`

例子：

```shell
[root@VM-24-9-centos /]# docker run -it centos /bin/bash		#启动并进入容器
[root@cf30744c9e8c /]# 此处的主机名变成了容器centos的容器id了（非镜像id）
```

#### 列出运行的容器：docker ps

docker ps [可选项]

```shell
[root@VM-24-9-centos /]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[root@VM-24-9-centos /]# docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED          STATUS                     PORTS     NAMES
cf30744c9e8c   centos         "/bin/bash"   13 minutes ago   Exited (0) 6 minutes ago             wizardly_hofstadter
e1714d2805f6   feb5d9fea6a5   "/hello"      3 days ago       Exited (0) 3 days ago                gifted_brahmagupta
```

| 可选项 | 描述                                     |
| ------ | ---------------------------------------- |
| 无     | 列出当前正在运行的容器                   |
| -a     | 列出当前正在运行的容器+历史运行过的容器  |
| -n=?   | 显示最近创建的容器，？表示个数    如-n=1 |
| -q     | 只显示容器的id                           |

#### 退出容器

##### 停止容器并退出：exit

##### 退出容器不停止：ctrl + q + p

#### 删除容器：docker rm 容器id

删除方法和技巧同镜像，即：`docker rm -f $(docker ps -aq)`

删除全部容器的另外方法：`docker ps -aq|xargs docker rm`

注：没加-f时，不能删除运行中的容器

#### 启动和停止容器

```shell
docker start 容器id		#启动容器
docker restart 容器id		#重动容器
docker stop 容器id		#停止当前正则运行的容器
docker kill 容器id		#强制停止当前容器容器
```

启动容器与新建容器并启动不同，此处的启动容器是指启动已停止的容器，而非拉一个新的容器

### 其他命令

#### 后台启动容器

```shell
docker run -d 镜像名
```

例子

```shell
docker run -d centos 

# 问题：执行docker ps 发现centos停止了

# 常见的坑：docker容器使用后台运行，就必须要有一个前台进程，docker发现没有应用（容器没有提供服务），就会自动停止
```

#### 日志

```shell
docker -tf --tail num 容器id
# 注：如果容器里面没有提供服务，那么日志是空
```

| 可选项     | 描述                               |
| ---------- | ---------------------------------- |
| -t         | 显示日志                           |
| -f         | 带上时间戳                         |
| --tail num | 显示num条日志（tail后面必须跟num） |

例子：

```shell
docker run -d centos /bin/sh -c "while true;do echo wangze;sleep 1;done"		# 表示后台运行centos，且centos启动一个服务
# while true;do echo wangze;sleep 1;done 表示每隔一秒循环打印wangze
```

#### 查看容器中进程信息

```shell
docker top 容器id
```

例子：

```shell
[root@VM-24-9-centos ~]# docker top dce7b86171bf
UID		PID		PPID		C		STIME		TTY
root	22891	22875		0		21:21		?
root	23351	22891		0		21:25		?

# UID：当前用户ID	PID：进程ID		PPID：父ID
```

#### 查看容器的元数据

```shell
docker inspect 容器id
```

作用：查看容器的详情信息，如完整id、创建时间、传递的参数、当前状态、PID、来自哪个镜像等

#### 进入正在运行的容器

容器一般是使用后台方式运行的，经常需要进入容器以修改一些配置

方式一（常用）：

```shell
docker exec -it 容器id
# 表示进入容器后开启一个新的终端，可以在里面操作
```

如：

```shell
[root@VM-24-9-centos ~]# docker exec -it bf6c1b679ea0 /bin/bash
[root@bf6c1b679ea0 /]# ls
bin  etc   lib	  lost+found  mnt  proc  run   srv  tmp  var
dev  home  lib64  media       opt  root  sbin  sys  usr
[root@bf6c1b679ea0 /]# ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 Feb07 pts/0    00:00:00 /bin/bash
root        14     0  0 09:23 pts/1    00:00:00 /bin/bash
root        29    14  0 09:23 pts/1    00:00:00 ps -ef
```

方式二：

```shell
docker attach 容器id	# 不需要-it
# 表示进入容器正在执行的终端，不会启动新的进程
```

如：

```shell
[root@VM-24-9-centos ~]# docker attach bf6c1b679ea0
wangze
wangze
wangze
wangze
```

#### 容器内文件拷贝到主机上

```shell
docker cp 容器id:容器内文件路径 目的主机目录 # 注意是以当前命令路径来实现相对路径
```

注：此方法不管容器是否运行，都能实现拷贝