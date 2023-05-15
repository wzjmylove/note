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
> ```
> lsof -u <USER>
> # 如 lsof -u root
> ```

## git

```shell
yum install git

git --version
```



## nodejs

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

```
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
> 当部署到后面会报错~Error: /home/risk/my-yapi/vendors/server/utils/commons.js:25	jsf.extend('mock', function () {^
> 解决办法：9090网页部署版本换成更高的 或者  https://github.com/YMFE/yapi/issues/2251

### 启动

用node启动也行，但是建议用pm2启动

```shell
cd /home/wangze/yapi/server
pm2 start app.js
```

### 升级

```shell
cd  {项目目录}
yapi ls //查看版本号列表
yapi update //升级到最新版本
yapi update -v v1.1.0 //升级到指定版本
```

