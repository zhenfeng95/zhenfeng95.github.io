---
title: Docker的安装与使用
type: docker
order: 1
---

## 一、Docker容器技术的特点

1. 文件系统隔离：每个进程容器运行在完全独立的根文件系统里。

2. 资源隔离：可以使用cgroup为每个进程容器分配不同的系统资源，例如CPU和内存。

3. 网络隔离：每个进程容器运行在自己的网络命名空间里，拥有自己的虚拟接口和IP地址。

4. 写时复制：采用写时复制方式创建根文件系统，这让部署变得极其快捷，并且节省内存和硬盘空间。

5. 日志记录：Docker将会收集和记录每个进程容器的标准流（stdout/stderr/stdin），用于实时检索或批量检索。

6. 变更管理：容器文件系统的变更可以提交到新的映像中，并可重复使用以创建更多的容器。无需使用模板或手动配置。

7. 交互式Shell：Docker可以分配一个虚拟终端并关联到任何容器的标准输入上，例如运行一个一次性交互shell。

8. 
   Docker 几乎可以安装（更准确地说是运行）任何服务，包括：

   * MySQL（数据库）


   * MongoDB（NoSQL 数据库）


   * Node.js（应用运行环境）


   * Nginx（反向代理 / 静态资源服务器）


   * 这些服务都是 Docker 的“经典组合”，在实际项目部署中非常常见。

## 二、Docker的安装与使用

docker服务可以通过宝塔面板安装

1. 安装 MongoDB

   ```shell
   docker run -d \
     --name mongo \
     -v /data/mongo:/data/db \
     -p 27017:27017 \
     mongo:latest
   ```

2. 安装 MySQL

   ```shell
   docker run -d \
     --name mysql \
     -v /data/mysql:/var/lib/mysql \
     -e MYSQL_ROOT_PASSWORD=123456 \
     -p 3306:3306 \
     mysql:8.0
   ```

3. 安装 Nginx

   ```shell
   docker run -d \
     --name nginx \
     -v /data/nginx/html:/usr/share/nginx/html \
     -v /data/nginx/conf:/etc/nginx/conf.d \
     -p 80:80 \
     nginx:latest
   ```

4. 安装 Node.js 应用

   ```shell
   docker run -d \
     --name nodeapp \
     -v /data/nodeapp:/app \
     -w /app \
     -p 3000:3000 \
     node:18 \
     node index.js
   ```

## 三、Docker 的优势

| 功能         | 传统安装（宝塔）       | Docker 安装      |
| ------------ | ---------------------- | ---------------- |
| 环境一致性   | ❌ 容易出错（版本差异） | ✅ 一致，可复制   |
| 安装速度     | 慢                     | ⚡ 快（几秒启动） |
| 升级维护     | ⚠️ 易冲突               | ✅ 可随时换版本   |
| 数据备份迁移 | 手动复制               | 直接拷贝挂载目录 |
| 多版本共存   | 几乎不可能             | ✅ 完全隔离       |
| 系统污染     | 安装包多               | ✅ 不污染系统环境 |

## 四、Docker常用命令

查看所有容器运行状态

```bash
docker ps
```

查看某个容器的运行状态

```bash
docker ps | grep mongodb
```

查看日志

```bash
docker logs -f mongo
```

停止单个服务

```bash
docker compose stop mongo
```

停止所有服务

```bash
docker compose down
```

启动所有服务

```bash
docker compose up -d 
```

启动单个服务

```bash
docker compose up -d mongo
```

重启单个服务

```bash
docker compose restart mongo
```

## 五、Docker 通常怎么用

在生产部署中，通常这样安装环境：

* Mongo + MySQL 都用 Docker 启动，挂载数据卷；

* Node 项目打包成 Docker 镜像；

* Nginx 作为反向代理放在最前端；

* 所有容器用 Docker Compose 一键管理启动。


例如一个典型的 docker-compose.yml：

```shell
version: "3"
services:
	# 安装并启动mongodb服务
  mongo:
    image: mongo
    container_name: "mongodb"
    restart: always
    volumes: # 挂载宿主机目录
      - /var/lib/mongodb:/data/db  # 数据持久化目录
      - /var/lib/mongodb/initdb.d:/docker-entrypoint-initdb.d/ # 自定义配置（可选）
      # .sh & .js
    ports:
      - "27018:27017" # 宿主机端口:镜像端口，将镜像的27017端口映射到宿主机的27018端口，也可以映射到27017端口
    environment:
      MONGO_INITDB_ROOT_USERNAME: root # 数据库账号
      MONGO_INITDB_ROOT_PASSWORD: 密码 # 数据库密码
    command: ["--auth", "--bind_ip_all"]
    
  # 安装并启动redis服务
  redis:
    image: redis
    container_name: "redis"
    restart: always
    volumes:
      - /var/lib/redis:/data
    ports:
      - "6379:6379"
    command: ["redis-server", "--requirepass", "密码"]
  
  # 安装并启动jenkins服务
  jenkins:
    image: jenkins/jenkins:lts
    container_name: "jenkins"
    restart: always
    user: root
    ports:
      - "10050:8080"
      - "50000:50000"
      - "10051:10051"
    environment:
      - JAVA_OPTS=-Xms256m -Xmx768m
    volumes:
      - /var/lib/jenkins:/var/jenkins_home
      - /usr/bin/docker:/usr/bin/docker
      - /var/run/docker.sock:/var/run/docker.sock

	# 安装并启动mysql服务
  mysql:
    image: mysql
    container_name: "mysql_server"
    restart: always
    ports:
      - "3306:3306"  # 映射到宿主机3306端口
    environment:
      MYSQL_ROOT_PASSWORD: root123456       # root用户密码
      MYSQL_DATABASE: mydb                  # 初始化创建的数据库
      MYSQL_USER: myuser                    # 普通用户
      MYSQL_PASSWORD: mypassword            # 普通用户密码
    volumes:
      - /var/lib/mysql:/var/lib/mysql  # 数据持久化目录
      - /var/lib/mysql/conf.d:/etc/mysql/conf.d  # 自定义配置（可选）
    
```

👉 然后只需执行：

```bash
docker compose up -d
```

注意：compose 是新版命令，如果你是旧版 docker，请用 docker-compose up -d

就能一键启动 Mongo、MySQL、Node、Nginx 四个环境。

验证运行情况：

```bash
docker ps
```

进入交互式终端的命令：

```bash
docker exec -it 容器名称 指令名称
```

登录 MySQL 测试：

```bash
docker exec -it mysql_server mysql -u root -p
# 输入 root123456
```

登录redis测试：

```bash
1.进入redis交互式终端：docker exec -it redis redis-cli
2.登录：auth 密码
```

登录MongoDB测试：

```bash
1.连接数据库，进入mongodb的交互式终端，这一步不需要输入用户名和密码
docker exec -it mongodb mongosh
2.切换到管理员数据库，登录上去
use admin
db.auth('root','Feng950925@')
3.使用show dbs，如果能显示数据库名称，则登录成功，拥有管理员操作权限
4.创建一个新的数据库，
use 数据库名称
5.为该数据库配置一个用户
db.createUser({user:'koa',pwd:'925926',roles:[{role:'dbOwner',db:'koa'}]})
6.鉴权登录
db.auth('koa','925926')
7.为该数据库添加一个表，并且添加一条数据
db.users.insertOne({name:'koa',age:7})
8.如何备份数据
备份：docker exec -it mongodb mongodump -h localhost -u root -p Feng950925@ -o /tmp/test
拷贝出来到主机：docker cp mongodb:/tmp/test /tmp/test
9.如何恢复数据
拷贝出来到容器: docker cp /tmp/test mongodb:/tmp/test
恢复：docker exec -it mongodb mongorestore -h localhost -u root -p Feng950925@ --dir /tmp/test
```

📁 推荐目录结构
/www/
├── docker-compose.yml
├── wwwroot/
│   └── zzf.net.cn/
│       ├── server/        # Node.js 后端项目
│       ├── web/           # 前端打包文件
│       └── nginx_conf/    # Nginx 配置文件
├── mongo_data/            # Mongo 数据持久化
└── mysql_data/            # MySQL 数据持久化

## 六、Docker安装jenkins的详细步骤

1. 参考上面配置好的docker-compose.yml文件

2. 启动jenkins容器

   ```bash
   docker compose up -d jenkins
   如果容器已经存在，先停止容器，然后再删除容易
   docker stop jenkins && docker rm jenkins
   ```

3. 查看初始密码

   ```bash
   docker logs -f jenkins
   ```
4. 在云服务平台安全组放行jenkins端口

5. 用ip+端口访问jenkins服务

6. 选择默认插件安装，创建用户

7. 如果不能科学上网，可以设置国内加速源下载插件

  ![1](index.assets/1.png)

8. 安装以下插件
  GitHub Integration
  Generic Webhook Trigger
  下面这些插件会在初始化中默认安装
  Git
  GitHub Branch Source
  Pipeline: GitHub Groovy Libraries
  Credentials Binding
  GitHub API

9. 如何备份jenkinns数据，先进入tmp目录

   ```bash
   cd /tmp
   docker ps | grep jenkins
   找到运行的ID
   docker cp 15fe80dc5a43:/var/jenkins_home /tmp/
   复制完成后，可以移动到其他目录，因为tmp目录的内容在云服务器重启后会清空
   ```
10. 如何配置权限，安装以下插件
    pam
    role
    默认已经安装过的
    Matrix Authorization Strategy
    LDAP

11. 以下可选插件
    slaves
    Dashboard View
    ThinBackup
    AnsiColor
    Build With Parameters

12. 配置
    （1）在 GitHub 上生成 Personal Access Token（PAT）

    ```basic
    打开 GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic) → Generate new token → Generate new token (classic)
    选择仓库读写权限（repo、admin:repo_hook、read:org）
    复制 Token
    ```

    （2）在 Jenkins 中添加凭据

    ```basic
    打开 Manage Jenkins → Credentials → System → Global credentials
    添加：
    类型：Secret Text
    ID：github-token
    Secret：粘贴 Token
    ```

    （3）在 Jenkins 中配置 GitHub Server

    ```basic
    Manage Jenkins → Configure System → GitHub
    添加 GitHub Server，选择上面添加的凭据
    点击 “Test Connection” 确认连接成功，若成功，会显示 GitHub 用户名
    ```

    （4）在 GitHub 上配置 Webhook

    ```basic
    填入 Jenkins 的 webhook 地址：
    http://<你的Jenkins服务器>:8080/github-webhook/
    Content type 选：application/json
    事件：Just the push event
    ```

    （5）在jenkins创建一个项目任务，选择自由风格项目

    （6）在General中勾选参数化构建过程，添加几对字符参数，下面shell脚本中在创建web_pc镜像过程中会用到，分别是

    ```basic
    名称：container_name
    默认值：web_pc
    名称：port
    默认值：11006
    名称：image_name
    默认值：web_pc
    名称：tag
    默认值：1.0
    ```

    （7）配置源码管理，选择 Git，在 Repository URL 填入你的 GitHub 仓库地址，例如：https://github.com/yourname/yourrepo.git

    （8）在 Credentials（凭证） 下拉框中，选择你刚才配置的 GitHub Token（github-token），如果仓库是私有的，必须选择凭证

    （9）在Branches to build中，确保分支是：*/main

    （10）配置构建触发器（Build Triggers），勾选GitHub hook trigger for GITScm polling，前提是 GitHub 仓库中配置了 Webhook

    （11）配置构建步骤（Build Steps），选择shell，然后输入以下命令

    ```bash
    #!/bin/bash
    CONTAINER=${container_name}
    PORT=${port}
    
    # echo $CONTAINER
    # echo $PORT
    # 完成镜像的构建
    # docker built -t web_pc:1.0 .
    # 后面的.代表使用工程目录下面的Docker file文件
    docker build --no-cache -t ${image_name}:${tag} .
    
    RUNNING=${docker inspect --format="{{ .State.Running}}" $CONTAINER 2 > /dev/null}
    # 条件判断
    if [ ! -n $RUNNING ]; then
    	echo "$CONTAINER does not exit"
        return 1
    fi
    
    if [ $RUNNING == "false" ]; then
    	echo "$CONTAINER is not running"
        return 2
    else  
    	echo "$CONTAINER is running"
        # 删除相同名字的容器
        matchingStarted=$(docker ps --filter="name=$CONTAINER" -q | xargs)
        if [ -n $matchingStarted ]; then
        	docker stop $matchingStarted
        fi
        
        matching=$(docker ps -a --filter="name=$CONTAINER" -q | xargs)
        if [ -n $matching ]; then
        	docker rm $matching
        fi
    fi
    
    echo "RUNNING is ${RUNNING}"
        
    # 运行镜像
    docker run -itd --name $CONTAINER -p $PORT:80 ${image_name}:${tag}
    # echo "开始构建 Vue 项目"
    ```

    