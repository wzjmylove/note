# Liunx下的环境配置

## lsof

是一款功能强大的开源工具，用于列出当前系统上打开的文件和进程。该工具可以帮助系统管理员和开发人员快速查找正在使用某个文件的进程，以及在系统上使用磁盘空间最多的进程。

1、安装

```shell
apt-get install lsof

# or

yum install lsof
```

2、基本用法

> 查看网络连接
>
> ```shell
> lsof -i			  # 查看全部进程的网络使用情况
> lsof -i:端口号		# 查看指定端口号的使用情况
> ```

> 列出所有打开的文件
>
> ```shell
> lsof
> ```

> 列出指定进程打开的文件
>
> ```shell
> lsof -p <PID>
> ```

> 列出指定用户打开的文件
>
> ```shell
> lsof -u <USER>
> # 如 lsof -u root
> ```

## git

```shell
yum install git

git --version
```

## nodejs

### 安装

参考：https://www.cnblogs.com/stx0529/articles/16984571.html

注意不要安装最新版node，会启动不了node

1、下载

```shell
wget https://cdn.npm.taobao.org/dist/node/v12.13.1/node-v12.13.1-linux-x64.tar.xz
```

2、解压：

```shell
xz -d node-v12.13.1-linux-x64.tar.xz
tar -xvf node-v12.13.1-linux-x64.tar
```

3、移动

```shell
mv node-v12.13.1-linux-x64  /home/wangze/
```

4、重命名

```shell
mv /usr/local/node-v12.13.1-linux-x64/  /home/wangze/nodejs
```

5\编辑配置文件

```shell
vi /etc/profile

export NODE_HOME=/home/wangze/nodejs
export PATH=$NODE_HOME/bin:$PATH
# ps:内容新增后，先按esc键，退出插入模式，然后按住shift键，并连按两次z字符，即可保存刚才的编辑并退出vim编辑状态
# 或者输入 :wq

# 更新环境变量
source /etc/profile
```

6、安装成功后，查看对应版本信息

```shell
node -v
npm -v
```

### 换源

查看npm的下载地址

```shell
npm config get registry
```

方法一（不推荐）：将npm的下载地址换成国内镜像

```shell
npm config set registry https://registry.npm.taobao.org

# 同理，换回源地址：
# npm config set registry https://registry.npmjs.org/
```

方法二：利用cnpm下载国内镜像

```shell
npm install -g cnpm --registry=https://registry.npm.taobao.org

# 后续就可以直接 cnpm install 插件名 
# 注意cnpm和npm版本可能不一致导致cnpm无法使用
```

### npx

注意npx是npm版本高于5.25.2才有的，方便在项目中使用全局包

如果没有npx命令，可以 `npm install -g npx` 安装

### 常见问题

node版本过高

报错：

```shell
node: /lib64/libm.so.6: version `GLIBC_2.27' not found (required by node)
node: /lib64/libc.so.6: version `GLIBC_2.25' not found (required by node)
node: /lib64/libc.so.6: version `GLIBC_2.28' not found (required by node)
node: /lib64/libstdc++.so.6: version `CXXABI_1.3.9' not found (required by node)
node: /lib64/libstdc++.so.6: version `GLIBCXX_3.4.20' not found (required by node)
node: /lib64/libstdc++.so.6: version `GLIBCXX_3.4.21' not found (required by node)
```

解决：nvm切低版本

## NVM

作用：用于管理node版本

### 安装

```shell
# 下载
wget https://github.com/nvm-sh/nvm/archive/refs/tags/v0.38.0.tar.gz

# 解压
tar -zxvf v0.38.0.tar.gz

# 换名字
mv /home/wangze/nvm-0.38.0 /home/wangze/nvm

# 配置环境
vim ~/.bashrc

export NVM_DIR="/home/wangze/nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  

source ~/.bashrc

# 检查

nvm -v
```

### 使用

```shell
nvm list        		 	 # 查找本电脑上所有的node版本
nvm install          		 # 安装最新版本nvm
nvm install <version>         # 安装指定版本的node		安装完后node会默认使用新安装的版本
nvm uninstall <version>       # 卸载我们指定的node版本号
nvm use <version>             # 切换使用指定的版本号
```



## TypeScript

1、typescript编译器

> 安装：npm install typescript
>
> 编译：npx tsc xxx.ts	（如果typescript是全局包，则可以省去npx）		（编译的意思：将ts编译为js）
>
> 缺点：
>
> > 1、当变量类型改变时，虽报错，但仍能编译成功
> >
> > 2、只编译成js，不能直接执行ts文件

2、typescript-node

> 安装：npm install typescript-node
>
> 使用：npx ts-node xxx.ts
>
> 优点：一步完成编译+执行

## pm2

参考：https://blog.csdn.net/leonnew/article/details/121989900

### 概念

pm2是node进程管理工具

![Linux环境配置_pm2_优势](.\image\Linux环境配置_pm2_优势.png)

### 特征

> 1、后台运行：普通启动方式：node index.js，关闭终端就结束进程；
> pm2可以后台运行，终端关闭不影响
>
> 2、自动重启：可以监听某些文件改动，自动重启
>
> 3、停止不稳定的进程：限制不稳定的重启的次数，达到上限就停止进程
>
> 4、0 秒停机重启：集群模式下，可以达到重启时不停止服务
>
> 5、简单日志管理：pm2可以收集日志，并有插件配合进行管理。后面会提到
>
> 6、自动负载均衡：cluster模式下，会自动使用轮询的方式达到负载均衡，从而减轻服务器的压力
>
> 7、提供实时的接口：pm2插件提供实时的接口，返回服务器与进程的信息，后面会提到
>
> 8、集成管理：对于多个进程，不同环境，可以统一配置，方便管理

### 下载

```shell
npm install pm2@latest -g
```

### 启动

```shell
pm2 start app.js
# app.js是node的启动文件
```

![Linux环境配置_pm2_启动](.\image\Linux环境配置_pm2_启动.png)

> app name 和id都是这个进程的标识，可以对他们进行别的操作，比如stop，delete等
>
> mode：进程模式，cluster或fork。cluster有多个进程，而fork只有一个
>
> restart：重启次数
>
> status：进程是否在线
>
> cpu：cpu占用率
>
> mem：内存占用大小

```shell
# 启动命令（start）还可以带参数

--watch 以监听模式启动，当文件发生变化时自动重启
--max-memory-restart <200MB> 设置应用重载占用的最大内存
--log <log_path> 指定日志文件
-- arg1 arg2 arg3 给启动脚本传递额外的参数
--restart-delay <delay in ms> 延时 x 毫秒自动重启
--time 日志里添加时间前缀
--no-autorestart 不自动重启
--cron <cron_pattern> 按指定的定时任务规则强制重启
--no-daemon 以非守护进程模式启动
```

### 常用命令

```shell
# 停止
pm2 stop app_name|app_id|all

#删除
pm2 delete app_name|app_id|all

#重启
pm2 restart/reload app_name|app_id|all		# 集群模式下，restart中断服务，而reload不会

# 查看所有的进程
pm2 list/ls/status

# 查看某一个进程的信息
pm2 show app_name|app_id

# 查看日志
pm2 logs

# 监控所有进程
pm2 monit
```

## MongoDB

参考：https://blog.csdn.net/weixin_38568503/article/details/127885059

1、[官网](https://www.mongodb.com/)下载对应环境和版本

![Linux环境配置_mongodb_download](.\image\Linux环境配置_mongodb_download.png)

2、上传 MongoDB 安装包到linux系统中

3、解压 MongoDB 安装包：`tar -zxvf mongodb-linux-x86_64-rhel70-4.2.23.tgz`	（注意文件名可能不同）
然后移动到自己的文件夹（我一般是`/home/wangze/mongodb`）

4、配置环境变量

```shell
vim /etc/profile

export MONGODB_HOME=/home/wangze/mongodb
export PATH=$MONGODB_HOME/bin:$PATH

# 更新环境变量
source /etc/profile
```

5、添加 MongoDB 配置文件

```shell
vim /etc/mongodb.conf
```

```shell
#指定数据库路径
dbpath=/home/wangze/mongodb/data
#指定MongoDB日志文件
logpath=/home/wangze/mongodb/logs/mongodb.log
# 使用追加的方式写日志
logappend=true
#端口号
port=901
#方便外网访问,外网所有ip都可以访问，不要写成固定的linux的ip
bind_ip=0.0.0.0
fork=true # 以守护进程的方式运行MongoDB，创建服务器进程
#auth=true #启用用户验证
#bind_ip=0.0.0.0 #绑定服务IP，若绑定127.0.0.1，则只能本机访问，不指定则默认本地所有IP
```

6、启动和关闭 MongoDB

进入mongodb的bin目录

```shell
cd /home/home/wangze/mongodb/bin
```

启动 MongoDB

```shell
mongod -f /etc/mongodb.conf

# 结果
# about to fork child process, waiting until server is ready for connections.
# forked process: 2520
# child process started successfully, parent exiting
```

关闭MongoDB

```shell
mongod --shutdown -f /etc/mongodb.conf
```

查看进程

```shell
ps -ef | grep mongod
```

## yapi

参考：https://www.wenjiangs.com/doc/25gwef55#f80abc8cc8aab6c354beca52b3781a4e

### 环境要求

- nodejs（7.6+)
- mongodb（2.6+）

### 安装

可视化安装

1、执行 yapi server 启动可视化部署程序，输入相应的配置和点击开始部署，就能完成整个网站的部署

```shell
npm install -g yapi-cli --registry https://registry.npm.taobao.org

yapi server
# 在浏览器打开 http://0.0.0.0:9090 访问。非本地服务器，请将 0.0.0.0 替换成指定的域名或ip 
```

9090网页的配置项

![Linux环境配置_yapi_9090页面配置](.\image\Linux环境配置_yapi_9090页面配置.png)

注意：

> 部署版本要1.8.8及以上，否则会`Error: getaddrinfo ENOTFOUND yapi.demo.qunar.com`
>
> 当部署到后面会报错`~Error: /home/risk/my-yapi/vendors/server/utils/commons.js:25	jsf.extend('mock', function () {^`
> 解决办法：9090网页部署版本换成更高的 或者  https://github.com/YMFE/yapi/issues/2251

### 启动

用node启动也行，但是建议用pm2启动

```shell
cd /home/wangze/yapi/vendors/server
pm2 start app.js
```

### 升级

```shell
cd  {项目目录}
yapi ls //查看版本号列表
yapi update //升级到最新版本
yapi update -v v1.1.0 //升级到指定版本
```

### 使用

利用yapi-to-typescript工具包，自动生成请求接口函数

1、安装yapi-to-typescript工具包

```shell
yarn add -D yapi-to-typescript	# 推进yarn安装，npm安装容器出现init出错
# 或者
npm i yapi-to-typescript -d
```

2、配置工具，会在项目根目录下自动生成yyt.config.ts配置文件

```shell
npx ytt init -c ytt.config.ts
# 注意一定要生成ytt.config.ts文件或js文件，否则npm run ytt会报错
```

生成的ytt.config.ts文件如下：

```ts
import { defineConfig } from 'yapi-to-typescript'

export default defineConfig([
  {
    serverUrl: 'http://foo.bar',		// 更改为自己yapi的地址，如http://47.108.211.92:701
    typesOnly: false,
    target: 'typescript',
    reactHooks: {
      enabled: false,
    },
    prodEnvName: 'production',
    outputFilePath: 'src/api/index.ts',
    requestFunctionFilePath: 'src/api/request.ts',
    dataKey: 'data',
    projects: [
      {
        token: 'hello',				// 更改为项目token
        categories: [
          {
            id: 0,					// 每次更新的接口：0 表示该项目下的所有接口，[1560, 1561]等之类的表示该项目下url中后缀为1560、1561的接口
            getRequestFunctionName(interfaceInfo, changeCase) {
              // 以接口全路径生成请求函数名
              return changeCase.camelCase(interfaceInfo.path)

              // 若生成的请求函数名存在语法关键词报错、或想通过某个关键词触发 IDE 自动引入提示，可考虑加前缀，如:
              // return changeCase.camelCase(`api_${interfaceInfo.path}`)

              // 若生成的请求函数名有重复报错，可考虑将接口请求方式纳入生成条件，如:
              // return changeCase.camelCase(`${interfaceInfo.method}_${interfaceInfo.path}`)
            },
          },
        ],
      },
    ],
  },
])
```

3、运行ytt

```shell
npm run ytt
# 或者
yarn ytt
```

此时就会在"./src/api"路径下生成对应的接口函数，后续项目中可以直接import后使用

## Docker

参考：“笔记\笔记\非前端\Docker\01-Docker”

### 安装

1、环境查看

```shell
# 系统内核是3.10及以上
uname -r

3.10.0-1160.49.1.el7.x86_64
```

```shell
# 系统版本 尽量是CentOS7
cat /etc/os-release

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

2、跟着文档来

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

![docker-run-helloWorld](.\image\docker-run-helloWorld.png)

<img src=".\image\docker-run-运行原理.png" alt="docker-run-运行原理" style="zoom: 67%;" />

```shell
# 8.查看下载的镜像
docker images
```

### 卸载

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

