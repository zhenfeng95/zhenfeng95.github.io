---
title: 网站上线部署流程
type: process
order: 1
---

## 一、网站运行机制

### 1、名词解释

#### 域名

> -   www.baidu.com
> -   www.taobao.com
> -   www.qq.com

> 域名俗称网址，是由一串用点分隔的名字组成，用于标识互联网上的计算机。
> 原本用于标识互联网上计算机使用的是 IP 地址，但是由于 IP 地址不便于记忆，所以人们便设计出来比较容易记忆的域名，然后通过 DNS 将域名和 IP 地址关联，这样人们便可以通过记忆域名直接访问到对应的计算机。

#### DNS 服务器

> DNS (Domain Name System)，可以理解为互联网上的一项服务，他可以将域名转换成其对应的 IP 地址。
> 可以将其理解为字典，字典中存储的就是域名和 IP 地址一一对应的键值对。
> 本地 hosts 文件
> windows: c:\\windows\\system32\\drivers\\etc\\hosts
> mac: /etc/hosts

#### 服务器

> 服务器其实就是一台计算机，但是这台计算机并和我们自己的的 PC 不一样，不是日常使用的，而是提供某项互联网服务的。
> 比如 web 服务器，能为我们提供网页服务，email 服务器，能为我们提供电子邮件服务，FTP 服务器能为我们提供文件存储服务等等。
> 为计算机安装不同的服务应用程序，即可提供相应的服务。
> 常见的 web 服务应用程序： Apache、Nginx、IIS、Node.js

### 2、 网站请求流程（简化版）

#### 静态页面

网页只请求和响应简单的 HTML、CSS、JavaScript 文件，未和服务端进行任何数据通信。这样的页面叫做静态页面。

#### 动态页面

页面内有和服务器进行数据通信，这样的页面叫做动态页面。

#### 前后端分离的页面

> 前后端分离的项目中，页面中的数据渲染是在浏览器中完成的。

前后端分离的页面请求分为两部分： 静态页面请求 + ajax 数据请求

![image](index.assets/1748243726987.png)

![image](index.assets/1748243333844.png)

#### 前后端不分离的页面

> 前后端不分离的项目中，页面中的数据渲染操作是在服务器端完成的。

前后端不分离的页面一次请求就能完成。

![image](index.assets/1748241289183.png)

## 二、网站上线部署流程

### 1、服务器购买

国内服务器： 阿里云 ECS(Elastic Compute Service)，腾讯云 CVM(Cloud Virtual Machine) 等
国外服务器： 日本 [Vultr](https://www.vultr.com/), 美国 Linode, 谷歌云，微软 Azure，亚马逊 AWS 等
这一步需要创建好服务器实例，分配好外网 IP 地址。

### 2、域名购买

国内： 万网（阿里）、腾讯等
国外： Godaddy

### 3、域名解析（配置 DNS）

注册好域名之后需要将域名映射到自己服务器对应的 IP 地址，这样别人才能通过域名访问到我们的服务器。
这个步骤叫做域名解析，通过域名服务商提供的后台就可以操作，一般域名解析都会有延迟，不是即时生效的。

### 4、服务器环境搭建

**配置环境也可以参考[Docker](/website/docker/)，部署更方便**

数据库环境：由于使用命令安装 Mongodb，MySql 数据库容易出现各种各样的问题，建议用 Docker 拉取镜像安装，用 docker compose 去管理

redis 服务：建议用 Docker 拉取镜像安装

Node 项目：打包成 Docker 镜像，在项目中创建 Dockerfile 文件，文件内容包括拉取镜像，下载依赖，运行项目，在 jenkins 中配置 shell 脚本文件，或者在服务器上找个目录配置 shell 文件，具体参考下面的自动化部署

nginx 和 node 可以直接在宿主机上进行下载

配置服务器，Mac 系统下直接用终端就 ok
windows 下需要用到 git bash, 或者别的工具（Putty）

```shell
# 需要用到的 Linux 系统操作命令

# 远程连接命令
ssh root@域名

# 展示当前文件夹路径
pwd

# 切换文件夹目录
cd 目录路径

# 展示当前文件夹中内容
ls

# 编辑文件
vim 文件路径

# 传输文件
scp 本地文件路径 root@域名:远程路径

# 解压文件命令
unzip
```

#### 4.1 安装 CentOS 开发人员相关包

```shell
yum groupinstall 'Development tools'
```

#### 4.2 配置免密登陆

在自己电脑上 生成本地 秘钥对，参考[git 如何生成密钥](/tool/git/)

```shell
# 生成的位置
# mac 在 ~/.ssh
# windows 在 C:\users\你的用户名\.ssh

# 在服务器创建了一个.ssh 文件夹
mkdir .ssh

# 切换到这个文件夹
cd .ssh

# 创建了一个文件
touch authorized_keys

# 我们把自己电脑上的 id_rsa.pub 文件中的内容 放到 authorized_keys文件中
echo "公钥内容" >> authorized_keys

# 退出服务器，下次直接就能免密登陆了
exit
```

#### 4.3 安装 Nginx

```shell
# 添加 Nginx 源
sudo yum install epel-release

# 安装 Nginx
sudo yum install nginx

# 启动 Nginx
nginx

# 配置防火墙规则
sudo firewall-cmd --permanent --zone=public --add-service=http
sudo firewall-cmd --permanent --zone=public --add-service=https
sudo firewall-cmd --reload
```

#### 4.4 安装 Node.js

```shell
# yum自带源中没有Node.js,所以首先要获取Node.js资源：
curl --silent --location https://rpm.nodesource.com/setup_14.x | bash -

# 安装 Node.js
yum install -y nodejs

# 安装完成之后使用如下指令测试安装是否成功
node -v

# 安装pm2 node.js程序管理工具
npm i pm2 -g

# 使用pm2 启动node.js项目
pm2 start 文件名

# 停止
pm2 stop 文件名或者id

# 从pm2的管理列表中删除
pm2 delete 文件名或者id
```

#### 4.5 安装 MySQL

```shell
# 下载并安装 MySQL 源
wget https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
sudo yum localinstall mysql80-community-release-el7-3.noarch.rpm

# 安装 MySQL
sudo yum install mysql-community-server -y


#如果上一步报认证密钥的错，那么将：/etc/yum.repos.d/mysql-community.repo中的gpgcheck改为0
gpgcheck=0

# 如果上一步报错 执行下面的语句 之后 再次执行一下上面的安装Mysql的语句
sudo yum module disable mysql

# 启动MySQL
sudo systemctl start msyqld

# 找到默认密码
# MySQL安装完毕之后会自动设置一个默认密码，我们需要找到默认密码
grep 'temporary password' /var/log/mysqld.log

# 连接到MySQL数据库，修改密码
mysql -uroot -p
ALTER USER 'root'@'localhost' IDENTIFIED BY '要修改的密码';

# 远程连接到MySQL数据库，如果报错1130-host ... is not allowed to connect to this MySql server
解决方案是在服务器上先连接到数据库，然后按照以下命令操作
查看所有数据库：SHOW DATABASES;
切换到 mysql 数据库：USE mysql;
查看 mysql 数据库中的表：SHOW TABLES;
你应该能看到一个名为 user 的表，它存储了MySQL用户的登录权限信息
查看 user 表中的 Host 和 User 字段：SELECT Host, User FROM user;
修改 user 表中的 Host 值：UPDATE user SET Host = '%' WHERE User = 'root';
执行完后刷新权限，使更改立即生效：FLUSH PRIVILEGES;
重新使用可视化工具测试连接

参考文档：https://blog.csdn.net/qq_42943927/article/details/147632834
```

### 5、上传网站资源

可以使用 `scp` 命令，也可以安装 FTP （vsftpd）工具。

```shell
scp 本地文件 root@域名:远程路径

# 在服务器创建文件夹
mkdir /home/nginx/

# 把网页文件移动到创建好的文件夹里
mv ./dist.zip /home/nginx/

# 解压压缩文件
cd /home/ningx
unzip ./dist.zip

# 修改文件夹名字
mv dist admin
# 结果就是  /home/nginx/admin  这个文件夹中放的就是我们的网页文件了
```

### 6、配置 Nginx

创建一个 ilovefe.conf 文件

```shell
cd /etc/nginx/conf.d

# 创建配置文件
touch ilovefe.conf
vim ilovefe.conf

# 按i键 进出插入模式

# 复制下面的内容，粘贴进去

# 保存退出

# 按一下esc退出编辑模式

# 然后输入 下面的内容 敲回车

:wq
```

ilovefe.conf

```configure
server {
    listen       80;
    server_name  zzf.net.cn;
    location / {
         `  root   /usr/local/src/dist;`
         `  index  index.html index.htm;`
         `  try_files $uri $uri/ /index.html;`
    }
}
```

完整的配置文件示例，开启了 https

```configure
# nginx.conf 或 server 配置中添加
map $http_user_agent $is_mobile {
    default 0;
    ~*(iphone|ipod|android|blackberry|opera\ mini|windows\sce|palm|mobile) 1;
}

server
{
    listen 80;
    listen 443 ssl;
    listen 443 quic;
    http2 on;
    server_name zzf.net.cn www.zzf.net.cn;
    #

    # root /www/wwwroot/zzf.net.cn/;
    #CERT-APPLY-CHECK--START
    # 用于SSL证书申请时的文件验证相关配置 -- 请勿删除
    # include /www/server/panel/vhost/nginx/well-known/zzf.net.cn.conf;
    #CERT-APPLY-CHECK--END

    #SSL-START SSL相关配置，请勿删除或修改下一行带注释的404规则
    #error_page 404/404.html;
    #HTTP_TO_HTTPS_START
    set $isRedcert 1;
    if ($server_port != 443) {
        set $isRedcert 2;
    }
    if ( $uri ~ /\.well-known/ ) {
        set $isRedcert 1;
    }
    if ($isRedcert != 1) {
        rewrite ^(/.*)$ https://$host$1 permanent;
    }
    #HTTP_TO_HTTPS_END

    ssl_certificate     /root/.acme.sh/zzf.net.cn_ecc/fullchain.cer;
    ssl_certificate_key /root/.acme.sh/zzf.net.cn_ecc/zzf.net.cn.key;
    # ssl_certificate    /www/server/panel/vhost/cert/zzf.net.cn/fullchain.pem;
    # ssl_certificate_key    /www/server/panel/vhost/cert/zzf.net.cn/privkey.pem;
    ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3;
    ssl_ciphers EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+AES128:RSA+AES128:EECDH+AES256:RSA+AES256:EECDH+3DES:RSA+3DES:!MD5;
    ssl_prefer_server_ciphers on;
    ssl_session_tickets on;
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 10m;
    add_header Strict-Transport-Security "max-age=31536000";
    add_header Alt-Svc 'quic=":443"; h3=":443"; h3-29=":443"; h3-27=":443";h3-25=":443"; h3-T050=":443"; h3-Q050=":443";h3-Q049=":443";h3-Q048=":443"; h3-Q046=":443"; h3-Q043=":443"';
    error_page 497  https://$host$request_uri;

    #SSL-END

    #ERROR-PAGE-START  错误页配置，可以注释、删除或修改
    error_page 404 /404.html;
    #error_page 502 /502.html;
    #ERROR-PAGE-END

    #PHP-INFO-START  PHP引用配置，可以注释或修改
    # include enable-php-00.conf;
    #PHP-INFO-END

    #REWRITE-START URL重写规则引用,修改后将导致面板设置的伪静态规则失效
    # include /www/server/panel/vhost/rewrite/zzf.net.cn.conf;
    #REWRITE-END


    # 默认根目录指向前台 dist
    root /www/wwwroot/zzf.net.cn/dist;
    index index.html index.htm;

   location / {
     try_files $uri $uri/ /index.html;
     if ($is_mobile = 1) {
          return 301 https://m.zzf.net.cn$request_uri;
      }
    }

     location /webhook {
        proxy_pass http://127.0.0.1:4000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    location /webhook-admin {
        proxy_pass http://127.0.0.1:4001;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    location /webhook-server {
        proxy_pass http://127.0.0.1:4002;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    location /webhook-h5 {
        proxy_pass http://127.0.0.1:4003;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    location /webhook-compc {
        proxy_pass http://127.0.0.1:4005;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    location /webhook-comkoa {
        proxy_pass http://127.0.0.1:4006;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    # 代理后端接口服务
    location ^~ /api/ {
        rewrite ^/api/(.*)$ /api/$1 break;
        proxy_pass http://127.0.0.1:3001;
    }

    # 代理websocket服务
    location ^~ /websocket {
        proxy_pass http://127.0.0.1:11007/;  # 内部转发到11007端口
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        # 超时设置
        proxy_read_timeout 3600s;
        proxy_send_timeout 3600s;
    }


    #禁止访问的文件或目录
    location ~ ^/(\.user.ini|\.htaccess|\.git|\.env|\.svn|\.project|LICENSE|README.md)
    {
        return 404;
    }

    #一键申请SSL证书验证目录相关设置
    location ~ \.well-known{
        allow all;
    }

    #禁止在证书验证目录放入敏感文件
    if ( $uri ~ "^/\.well-known/.*\.(php|jsp|py|js|css|lua|ts|go|zip|tar\.gz|rar|7z|sql|bak)$" ) {
        return 403;
    }

    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
    {
        expires      30d;
        error_log /dev/null;
        access_log /dev/null;
    }

    location ~ .*\.(js|css)?$
    {
        expires      12h;
        error_log /dev/null;
        access_log /dev/null;
    }
    access_log  /www/wwwlogs/zzf.net.cn.log;
    error_log  /www/wwwlogs/zzf.net.cn.error.log;
}
```

### 7、接口项目部署步骤

1. 修改配置文件中的 mysql 数据库密码
2. 上传项目压缩文件到服务器
3. 在服务器上解压项目文件到 /home/nginx/ilovefeadmin
4. 为项目安装依赖项 npm i
5. 修改 mysql 数据库 密码规则

```mysql
use mysql;
ALTER USER 'root'@'localhost' IDENTIFIED BY '新密码' PASSWORD EXPIRE NEVER;
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '新密码';

# 查看是否已经修改成功
select user,host,plugin from user where user='root';

```

6. 创建新的数据库

```mysql
create database mysite;
```

7. nginx 配置文件中添加反向代理的配置：

```

location ^\~ /api/ {
    `  rewrite ^/api/(.\*)$ /api/$1 break;`
    `  proxy\_pass http://127.0.0.1:3001;`
}
```

8. nginx 配置后台管理系统的目录：

```
location /admin {
    `  alias /usr/local/src/admin;`
    `  index index.html index.htm;`
    `  try\_files $uri $uri/ /index.html;`
}
```

9. nginx 配置静态文件服务器：

```

location /static/ {
    `  alias /usr/local/src/server/public/static/;`
}
```

## 三、自动化部署

**自动化部署常用的插件是 jenkins，参考[Docker 安装 jenkins](/website/docker/)，如果你不想借助 jenkins，可以参考以下解决方案**

通过配置 Webhook + Shell 脚本 来实现「Git 有最新提交时，仅拉取指定文件」的部署需求
当远程 Git 仓库有新提交时，只拉取特定文件（如 /dist/\*\* 或 /public/index.html）而不是整个项目

### 1、准备条件

-   你已有 Git 仓库（GitHub、Gitee 等）
-   宝塔服务器已安装 Git、Node.js、PM2（可选）
-   项目已初始化为 Git 仓库（git clone 方式）

### 2、创建自动拉取脚本（只同步指定文件）

#### 前端项目的做法

假设前端项目部署在 /www/wwwroot/zzf.net.cn，你只想更新仓库中的 dist/ 到这个目录
（1）在服务器上创建一个暂存文件，拉取完整仓库到缓存目录，如果想使用 ssh 方式拉取代码，可以参考[git](/tool/git/)

```bash
mkdir -p /www/git-temp
cd /www/git-temp
git clone https://github.com/zhenfeng95/mysite.git
```

（2）创建部署脚本 /www/deploy/deploy.sh

```bash
#!/bin/bash

# 1. 进入临时目录
cd /www/git-temp/mysite || exit

# 2. 拉取最新代码
echo "Pulling latest code from GitHub..."
git reset --hard HEAD
git clean -df
git pull origin main

# 3. 拷贝指定文件（只同步 dist 目录）
cp -r dist/* /www/wwwroot/zzf.net.cn/dist/

echo "Deploy finished at $(date)"
```

带邮件通知的做法，用的是centos系统自带curl发送邮件的

```shell
#!/bin/bash
# deploy_with_email.sh

# ========== 配置 ==========
CONFIG_QQ_EMAIL="您的QQ邮箱@qq.com"
CONFIG_QQ_AUTH_CODE="您的授权码"
CONFIG_TO_EMAIL="您的接收邮箱@example.com"
PROJECT_NAME="您的项目名字"
# ==========================

# 邮件发送函数
send_deploy_email() {
    local status=$1  # 0=成功, 1=失败
    local message="$2"
    
    if [ $status -eq 0 ]; then
        subject="✅ ${PROJECT_NAME}部署成功 - $(date '+%m-%d %H:%M')"
    else
        subject="❌ ${PROJECT_NAME}部署失败 - $(date '+%m-%d %H:%M')"
    fi
    
    # 创建邮件内容
    cat > /tmp/deploy_email.txt << EOF
From: ${CONFIG_QQ_EMAIL}
To: ${CONFIG_TO_EMAIL}
Subject: ${subject}
Date: $(date -R)

项目: ${PROJECT_NAME}
时间: $(date)
服务器: $(hostname)
状态: ${subject%% *}

详情:
${message}

文件统计:
$(find /www/wwwroot/mgt.zzf.net.cn/dist/ -type f 2>/dev/null | wc -l) 个文件

--- 自动发送 ---
EOF
    
    # 发送邮件
    curl --ssl-reqd --silent \
      --url 'smtps://smtp.qq.com:465' \
      --user "${CONFIG_QQ_EMAIL}:${CONFIG_QQ_AUTH_CODE}" \
      --mail-from "${CONFIG_QQ_EMAIL}" \
      --mail-rcpt "${CONFIG_TO_EMAIL}" \
      --upload-file /tmp/deploy_email.txt
    
    rm -f /tmp/deploy_email.txt
}

# 主部署流程
main() {
    echo "开始部署..."
    
    # 原有部署步骤
    cd /www/git-temp/community-admin || {
        send_deploy_email 1 "无法进入目录 /www/git-temp/community-admin"
        exit 1
    }
    
    git reset --hard HEAD
    git clean -df
    
    if ! git pull origin main; then
        send_deploy_email 1 "Git拉取失败"
        exit 1
    fi
    
    if cp -r dist/* /www/wwwroot/mgt.zzf.net.cn/dist/; then
        file_count=$(find /www/wwwroot/mgt.zzf.net.cn/dist/ -type f | wc -l)
        send_deploy_email 0 "成功同步 ${file_count} 个文件"
        echo "✅ 部署完成"
    else
        send_deploy_email 1 "文件拷贝失败"
        exit 1
    fi
}

# 运行
main
```



保存后赋权：

```bash
chmod +x /www/deploy/deploy.sh
```

如果你希望只拉取特定文件（例如只拉 config/ 目录或 index.html），可以在 deploy.sh 中用如下命令：

```bash
git checkout origin/main -- config/
git checkout origin/main -- index.html
```

#### 后端项目的做法

（1）后端项目部署在/www/wwwroot/community-koa，直接在/www/wwwroot/目录下拉取代码，后端项目运行需要整个仓库

```bash
git clone https://github.com/zhenfeng95/community-koa.git
```

**Docker 镜像做法**

（2）创建部署脚本 /www/deploy/deploy-comkoa.sh，该脚本文件会读取项目根目录下的 Dockerfile 文件，并生成一个 web_api 的镜像，运行镜像即可访问后端服务，通过 docker ps | grep web_api 查看是否运行

```shell
#!/bin/bash

# 1. 进入项目目录
cd /www/wwwroot/community-koa || exit

# 2. 拉取最新代码
echo "Pulling latest code from GitHub..."
git pull origin main

CONTAINER="web_api"
PORT=11005
PORT_WEBSOCKET=11007 # 启动websocket服务
image_name="web_api"
tag="1.0"

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
# docker run -itd --name $CONTAINER -p $PORT:3002 ${image_name}:${tag}

docker run -d --name $CONTAINER -p $PORT:3002 -p $PORT_WEBSOCKET:3003 ${image_name}:${tag}

echo "Deploy finished at $(date)"

exit 0
```



带邮件通知的做法：

（1）包含热更新流程（服务中断<30秒），使后端服务没有长时间的暂停（推荐）

```shell
代码拉取 → 构建镜像 → 启动新容器 → 等待就绪 → 快速切换 → 清理旧容器
                                 ↑
                         仅此阶段服务中断
```

```shell
#!/bin/bash
# ============================================
# 热更新部署脚本 - 修复健康检查问题版
# ============================================

# --------------------------------
# 配置区域，参数需要改动
# --------------------------------
PROJECT_NAME="community-koa"
PROJECT_PATH="/www/wwwroot/${PROJECT_NAME}"
GIT_BRANCH="main"

# 容器配置
CONTAINER_NAME="web_api"
CONTAINER_NEW_NAME="web_api_new"
IMAGE_NAME="web_api"
IMAGE_TAG="1.0"

# 端口配置
STANDARD_WEB_PORT=11005      # 最终Web端口
STANDARD_WS_PORT=11007       # 最终WebSocket端口
TEMP_WEB_PORT=11015          # 临时Web端口（标准+10）
TEMP_WS_PORT=11017           # 临时WebSocket端口（标准+10）
CONTAINER_PORT_WEB=3002
CONTAINER_PORT_WS=3003

# 邮件配置，参数需要改动
QQ_EMAIL="您的QQ邮箱@qq.com"
QQ_AUTH_CODE="您的QQ邮箱授权码"
NOTIFY_EMAIL="接收通知的邮箱@example.com"

# 部署参数
MAX_STARTUP_WAIT=45          # 增加到45秒（Koa应用启动可能需要更长时间）
HEALTH_CHECK_INTERVAL=3      # 检查间隔增加到3秒

# 健康检查配置
HEALTH_CHECK_PATH="/"        # 健康检查路径，根据您的应用调整
HEALTH_CHECK_TIMEOUT=5       # curl超时时间
READINESS_KEYWORDS="listening|started|ready|Server running|MongoDB.*connection opened"  # 应用就绪关键词

# 日志配置
LOG_FILE="/var/log/deploy_${PROJECT_NAME}.log"
# --------------------------------

# 全局变量
DEPLOY_START_TIME=""
DEPLOY_DETAILS=""
CURRENT_STAGE=""

# 初始化
init_deployment() {
    DEPLOY_START_TIME=$(date '+%Y-%m-%d %H:%M:%S')
    echo "========== 热更新部署开始: ${DEPLOY_START_TIME} ==========" | tee -a "$LOG_FILE"
    log "INFO" "使用临时端口避免冲突: ${TEMP_WEB_PORT}/${TEMP_WS_PORT}"
}

# 设置当前阶段（修复缺失的函数）
set_stage() {
    CURRENT_STAGE="$1"
    log "INFO" "=== ${CURRENT_STAGE} ==="
}

# 日志函数
log() {
    local level="$1"
    local message="$2"
    local timestamp=$(date '+%Y-%m-%d %H:%M:%S')
    echo "[${timestamp}] [${level}] ${message}" | tee -a "$LOG_FILE"
}

# 发送邮件函数（简版，确保可用）
send_deploy_email() {
    local status="$1"
    local subject="$2"
    local message="$3"
    
    local email_subject="${status} ${PROJECT_NAME}部署${subject} - $(date '+%m-%d %H:%M')"
    local mail_file="/tmp/deploy_email_$(date +%s).txt"
    
    cat > "$mail_file" << EOF
From: ${QQ_EMAIL}
To: ${NOTIFY_EMAIL}
Subject: ${email_subject}
Date: $(date -R)

项目: ${PROJECT_NAME}
状态: ${status}
时间: $(date '+%Y-%m-%d %H:%M:%S')
服务器: $(hostname)

${message}

最近日志:
$(tail -20 "$LOG_FILE" 2>/dev/null || echo "无日志")
EOF
    
    curl --ssl-reqd --silent \
        --url 'smtps://smtp.qq.com:465' \
        --user "${QQ_EMAIL}:${QQ_AUTH_CODE}" \
        --mail-from "${QQ_EMAIL}" \
        --mail-rcpt "${NOTIFY_EMAIL}" \
        --upload-file "$mail_file"
    
    local result=$?
    rm -f "$mail_file"
    
    if [ $result -eq 0 ]; then
        log "INFO" "邮件发送成功"
    else
        log "ERROR" "邮件发送失败"
    fi
    
    return $result
}

# 检查端口占用
check_port_availability() {
    local port=$1
    local type=$2
    
    if ss -tulpn | grep -q ":${port}\s"; then
        log "WARN" "端口 ${port} (${type}) 已被占用"
        return 1
    else
        log "INFO" "端口 ${port} (${type}) 可用"
        return 0
    fi
}

# 检查应用健康状态（多维度检查）
check_container_health() {
    local container_name="$1"
    local port="$2"
    
    # 1. 检查容器进程状态
    if [ "$(docker inspect -f '{{.State.Running}}' "$container_name" 2>/dev/null)" != "true" ]; then
        log "DEBUG" "容器进程未运行"
        return 1
    fi
    
    # 2. 检查应用日志中的就绪关键词
    local container_logs=$(docker logs --tail=10 "$container_name" 2>&1)
    if echo "$container_logs" | grep -q -E -i "$READINESS_KEYWORDS"; then
        log "DEBUG" "日志检查通过，找到就绪关键词"
        return 0
    fi
    
    # 3. HTTP健康检查（可选，如果应用有HTTP接口）
    if command -v curl &> /dev/null; then
        if curl -s -f --max-time "$HEALTH_CHECK_TIMEOUT" \
           "http://localhost:${port}${HEALTH_CHECK_PATH}" > /dev/null 2>&1; then
            log "DEBUG" "HTTP健康检查通过 (端口:${port})"
            return 0
        fi
    fi
    
    # 4. 检查特定进程是否在运行（例如node进程）
    if docker exec "$container_name" ps aux 2>/dev/null | grep -q -E "node|npm"; then
        log "DEBUG" "应用进程在运行"
        return 0
    fi
    
    return 1
}

# 启动新容器（使用临时端口）
start_new_container_temp_port() {
    set_stage "阶段2：启动新容器(临时端口)"
    
    log "INFO" "检查临时端口可用性..."
    check_port_availability "$TEMP_WEB_PORT" "临时Web"
    check_port_availability "$TEMP_WS_PORT" "临时WebSocket"
    
    log "INFO" "使用临时端口启动新容器..."
    log "INFO" "临时端口: ${TEMP_WEB_PORT}->${CONTAINER_PORT_WEB}, ${TEMP_WS_PORT}->${CONTAINER_PORT_WS}"
    
    # 启动新容器（临时端口）
    local container_id
    if ! container_id=$(docker run -d \
        --name "$CONTAINER_NEW_NAME" \
        -p "${TEMP_WEB_PORT}:${CONTAINER_PORT_WEB}" \
        -p "${TEMP_WS_PORT}:${CONTAINER_PORT_WS}" \
        "${IMAGE_NAME}:${IMAGE_TAG}" 2>&1); then
        log "ERROR" "新容器启动失败: ${container_id}"
        return 1
    fi
    
    # 提取容器ID
    container_id=$(echo "$container_id" | tr -d '\n')
    if [ ${#container_id} -eq 64 ]; then
        log "INFO" "新容器启动成功，ID: ${container_id:0:12}"
        log "INFO" "临时访问地址: http://$(hostname -I | awk '{print $1}'):${TEMP_WEB_PORT}"
        
        # 立即查看启动日志
        sleep 2
        local startup_logs=$(docker logs --tail=5 "$CONTAINER_NEW_NAME" 2>&1)
        log "DEBUG" "容器启动日志: ${startup_logs}"
        
        DEPLOY_DETAILS="${DEPLOY_DETAILS}新容器ID: ${container_id:0:12}\n临时端口: ${TEMP_WEB_PORT},${TEMP_WS_PORT}\n"
        return 0
    else
        log "ERROR" "获取容器ID失败: ${container_id}"
        return 1
    fi
}

# 等待新容器就绪（改进的健康检查）
wait_for_new_container_temp() {
    set_stage "阶段3：检查新容器健康"
    
    log "INFO" "等待新容器就绪，最多${MAX_STARTUP_WAIT}秒..."
    log "INFO" "就绪关键词: ${READINESS_KEYWORDS}"
    
    local wait_time=0
    local last_log_check=0
    local consecutive_healthy_checks=0
    local required_healthy_checks=2  # 需要连续2次检查通过
    
    while [ $wait_time -lt $MAX_STARTUP_WAIT ]; do
        # 定期输出日志（每10秒）
        if [ $((wait_time % 10)) -eq 0 ]; then
            log "INFO" "等待容器启动... ${wait_time}/${MAX_STARTUP_WAIT}秒"
            
            # 每10秒输出一次最新日志
            if [ $last_log_check -eq 0 ] || [ $((wait_time - last_log_check)) -ge 10 ]; then
                local recent_logs=$(docker logs --tail=5 "$CONTAINER_NEW_NAME" 2>&1 | sed ':a;N;$!ba;s/\n/; /g')
                if [ -n "$recent_logs" ]; then
                    log "DEBUG" "最近日志: ${recent_logs}"
                fi
                last_log_check=$wait_time
            fi
        fi
        
        # 检查容器健康状态
        if check_container_health "$CONTAINER_NEW_NAME" "$TEMP_WEB_PORT"; then
            consecutive_healthy_checks=$((consecutive_healthy_checks + 1))
            log "DEBUG" "健康检查通过 (${consecutive_healthy_checks}/${required_healthy_checks})"
            
            if [ $consecutive_healthy_checks -ge $required_healthy_checks ]; then
                # 最终确认：输出完整就绪日志
                local ready_logs=$(docker logs "$CONTAINER_NEW_NAME" 2>&1 | grep -E -i "$READINESS_KEYWORDS" | tail -3)
                log "INFO" "✅ 新容器就绪！等待时间: ${wait_time}秒"
                log "INFO" "就绪日志: ${ready_logs}"
                
                DEPLOY_DETAILS="${DEPLOY_DETAILS}容器就绪时间: ${wait_time}秒\n"
                return 0
            fi
        else
            consecutive_healthy_checks=0
        fi
        
        sleep "$HEALTH_CHECK_INTERVAL"
        wait_time=$((wait_time + HEALTH_CHECK_INTERVAL))
    done
    
    # 超时处理：输出详细日志
    log "ERROR" "新容器在${MAX_STARTUP_WAIT}秒内未就绪"
    log "ERROR" "=== 容器最后20行日志 ==="
    docker logs --tail=20 "$CONTAINER_NEW_NAME" 2>&1 | tee -a "$LOG_FILE"
    log "ERROR" "=== 容器状态 ==="
    docker inspect --format='{{json .State}}' "$CONTAINER_NEW_NAME" 2>&1 | tee -a "$LOG_FILE"
    
    return 1
}

# 执行热切换
perform_hot_swap_with_ports() {
    set_stage "阶段4：执行热切换"
    
    local swap_start=$(date +%s)
    log "INFO" "开始热切换（服务中断开始）..."
    
    # 1. 停止旧容器（释放标准端口）
    if docker inspect "$CONTAINER_NAME" > /dev/null 2>&1; then
        log "INFO" "停止旧容器: ${CONTAINER_NAME}"
        if docker stop "$CONTAINER_NAME"; then
            log "INFO" "旧容器已停止"
            sleep 2  # 等待端口释放
        else
            log "WARN" "停止旧容器失败，可能已不存在"
        fi
    fi
    
    # 2. 停止新容器（临时端口）
    log "INFO" "停止新容器准备端口切换..."
    docker stop "$CONTAINER_NEW_NAME"
    
    # 3. 移除旧容器
    docker rm "$CONTAINER_NAME" 2>/dev/null && log "INFO" "旧容器已移除"
    
    # 4. 重命名新容器
    docker rename "$CONTAINER_NEW_NAME" "$CONTAINER_NAME"
    
    # 5. 修改容器端口绑定（需要重新创建容器）
    log "INFO" "重新创建容器使用标准端口: ${STANDARD_WEB_PORT}/${STANDARD_WS_PORT}"
    
    # 先获取容器的其他配置（环境变量、卷等）
    local env_vars=$(docker inspect --format='{{range .Config.Env}}{{println .}}{{end}}' "$CONTAINER_NAME" 2>/dev/null | grep -v "^$" | sed 's/^/-e "/;s/$/"/' | tr '\n' ' ')
    
    # 移除旧容器实例
    docker rm "$CONTAINER_NAME" 2>/dev/null
    
    # 使用标准端口重新创建
    if ! docker run -d \
        --name "$CONTAINER_NAME" \
        -p "${STANDARD_WEB_PORT}:${CONTAINER_PORT_WEB}" \
        -p "${STANDARD_WS_PORT}:${CONTAINER_PORT_WS}" \
        ${env_vars} \
        "${IMAGE_NAME}:${IMAGE_TAG}"; then
        log "ERROR" "重新创建容器失败"
        return 1
    fi
    
    # 6. 验证最终容器
    sleep 3
    if check_container_health "$CONTAINER_NAME" "$STANDARD_WEB_PORT"; then
        log "INFO" "✅ 最终容器运行正常"
    else
        log "ERROR" "最终容器健康检查失败"
        docker logs --tail=10 "$CONTAINER_NAME" 2>&1 | tee -a "$LOG_FILE"
        return 1
    fi
    
    local swap_end=$(date +%s)
    local swap_duration=$((swap_end - swap_start))
    
    log "INFO" "热切换完成，耗时: ${swap_duration}秒"
    DEPLOY_DETAILS="${DEPLOY_DETAILS}热切换耗时: ${swap_duration}秒\n"
    
    return 0
}

# 主部署流程
main_deployment() {
    init_deployment
    
    set_stage "阶段1：准备工作"
    cd "$PROJECT_PATH" || {
        log "ERROR" "项目目录不存在: ${PROJECT_PATH}"
        send_deploy_email "failure" "目录错误" "项目目录不存在"
        return 1
    }
    
    # 拉取代码
    log "INFO" "拉取最新代码..."
    if ! git pull origin "$GIT_BRANCH"; then
        log "ERROR" "代码拉取失败"
        send_deploy_email "failure" "代码拉取失败" "无法从${GIT_BRANCH}分支拉取代码"
        return 1
    fi
    
    # 构建镜像
    log "INFO" "构建Docker镜像..."
    if ! docker build --no-cache -t "${IMAGE_NAME}:${IMAGE_TAG}" .; then
        log "ERROR" "镜像构建失败"
        send_deploy_email "failure" "镜像构建失败" "Docker镜像构建失败，请检查Dockerfile"
        return 1
    fi
    
    # 启动新容器
    if ! start_new_container_temp_port; then
        log "ERROR" "新容器启动失败"
        send_deploy_email "failure" "容器启动失败" "无法启动新容器"
        return 1
    fi
    
    # 等待新容器就绪
    if ! wait_for_new_container_temp; then
        log "ERROR" "新容器健康检查失败"
        docker stop "$CONTAINER_NEW_NAME" 2>/dev/null
        docker rm "$CONTAINER_NEW_NAME" 2>/dev/null
        send_deploy_email "failure" "健康检查失败" "新容器在${MAX_STARTUP_WAIT}秒内未通过健康检查"
        return 1
    fi
    
    # 执行热切换
    if ! perform_hot_swap_with_ports; then
        log "ERROR" "热切换失败"
        send_deploy_email "failure" "热切换失败" "端口切换过程中出错"
        return 1
    fi
    
    # 部署成功
    set_stage "阶段5：部署完成"
    
    local success_message="✅ 热更新部署成功完成\n\n项目: ${PROJECT_NAME}\n服务中断时间: <30秒\n最终端口: ${STANDARD_WEB_PORT} (Web), ${STANDARD_WS_PORT} (WebSocket)\n数据库连接: 正常\n应用状态: 运行中"
    
    echo ""
    echo "✅ 部署成功完成！"
    echo "   应用已运行在端口: ${STANDARD_WEB_PORT}"
    echo "   可通过 http://$(hostname -I | awk '{print $1}'):${STANDARD_WEB_PORT} 访问"
    echo ""
    
    send_deploy_email "success" "成功" "$success_message"
    
    return 0
}

# 异常处理
handle_error() {
    local error_line="${BASH_LINENO[0]}"
    local error_code=$?
    
    log "ERROR" "部署失败于第${error_line}行 (退出码: ${error_code})"
    
    # 清理临时容器
    docker stop "$CONTAINER_NEW_NAME" 2>/dev/null
    docker rm "$CONTAINER_NEW_NAME" 2>/dev/null
    
    # 尝试恢复旧服务
    if docker ps -a --filter "name=${CONTAINER_NAME}" | grep -q "${CONTAINER_NAME}"; then
        docker start "$CONTAINER_NAME" 2>/dev/null
        log "INFO" "已恢复旧容器服务"
    fi
    
    echo ""
    echo "❌ 部署失败！已恢复旧服务。"
    echo "   请查看日志: ${LOG_FILE}"
    echo ""
    
    send_deploy_email "failure" "脚本错误" "部署脚本在第${error_line}行失败，错误码: ${error_code}"
    
    exit $error_code
}

# 设置错误处理
trap 'handle_error' ERR

# 执行主函数
main_deployment
```

（2）没有热更新，较长时间服务的暂停

```shell
代码拉取 → 停止旧容器 → 构建镜像 → 启动新容器
整个过程服务中断
```



```shell
#!/bin/bash
# ================================
# 基于Docker的Koa应用部署脚本
# 支持邮件通知、自动构建和容器管理
# ================================

# --------------------------------
# 配置区域 (请根据实际情况修改)
# --------------------------------
# 项目配置
PROJECT_NAME="community-koa"
PROJECT_PATH="/www/wwwroot/${PROJECT_NAME}"
GIT_BRANCH="main"

# Docker配置
CONTAINER_NAME="web_api"
IMAGE_NAME="web_api"
IMAGE_TAG="1.0"
PORT_WEB=11005          # 外部Web访问端口
PORT_WEBSOCKET=11007    # 外部WebSocket访问端口
CONTAINER_PORT_WEB=3002 # 容器内部Web端口
CONTAINER_PORT_WS=3003  # 容器内部WebSocket端口

# 邮件通知配置 (使用curl发送邮件)
QQ_EMAIL="您的QQ邮箱@qq.com"
QQ_AUTH_CODE="您的QQ邮箱授权码"
NOTIFY_EMAIL="接收通知的邮箱@example.com"

# 日志配置
LOG_FILE="/var/log/deploy_${PROJECT_NAME}.log"
# --------------------------------

# 初始化日志
init_log() {
    echo "========== 部署开始: $(date '+%Y-%m-%d %H:%M:%S') ==========" | tee -a "$LOG_FILE"
}

# 记录日志函数
log() {
    local level="$1"
    local message="$2"
    local timestamp=$(date '+%Y-%m-%d %H:%M:%S')
    echo "[${timestamp}] [${level}] ${message}" | tee -a "$LOG_FILE"
}

# 发送邮件函数 (使用curl)
send_deploy_email() {
    local status="$1"      # "success" 或 "failure"
    local subject="$2"
    local message="$3"
    
    # 根据状态设置主题前缀
    if [ "$status" = "success" ]; then
        email_prefix="✅"
    else
        email_prefix="❌"
    fi
    
    local full_subject="${email_prefix} ${PROJECT_NAME}部署${subject} - $(date '+%m-%d %H:%M')"
    
    # 创建邮件内容文件
    local mail_file="/tmp/deploy_email_$(date +%s).txt"
    cat > "$mail_file" << EOF
From: ${QQ_EMAIL}
To: ${NOTIFY_EMAIL}
Subject: ${full_subject}
Date: $(date -R)
Content-Type: text/plain; charset="utf-8"

项目名称: ${PROJECT_NAME}
部署状态: ${status}
通知时间: $(date '+%Y-%m-%d %H:%M:%S')
服务器信息: $(hostname) ($(curl -s ifconfig.me 2>/dev/null || echo "N/A"))

${message}

===== 部署详情 =====
${DEPLOY_DETAILS:-"无额外详情"}

===== 最近日志 =====
$(tail -10 "$LOG_FILE" 2>/dev/null || echo "日志文件不存在")

---
此邮件由部署系统自动发送
EOF
    
    # 使用curl发送邮件
    log "INFO" "正在发送${status}通知邮件..."
    curl --ssl-reqd --silent \
        --url 'smtps://smtp.qq.com:465' \
        --user "${QQ_EMAIL}:${QQ_AUTH_CODE}" \
        --mail-from "${QQ_EMAIL}" \
        --mail-rcpt "${NOTIFY_EMAIL}" \
        --upload-file "$mail_file"
    
    local curl_result=$?
    
    if [ $curl_result -eq 0 ]; then
        log "INFO" "邮件发送成功"
    else
        log "ERROR" "邮件发送失败 (curl返回值: ${curl_result})"
    fi
    
    # 清理临时文件
    rm -f "$mail_file"
    return $curl_result
}

# 检查并进入项目目录
check_project_dir() {
    log "INFO" "检查项目目录: ${PROJECT_PATH}"
    
    if [ ! -d "$PROJECT_PATH" ]; then
        log "ERROR" "项目目录不存在: ${PROJECT_PATH}"
        send_deploy_email "failure" "目录检查失败" "项目目录不存在: ${PROJECT_PATH}"
        return 1
    fi
    
    cd "$PROJECT_PATH" || {
        log "ERROR" "无法进入项目目录: ${PROJECT_PATH}"
        send_deploy_email "failure" "目录访问失败" "无法进入项目目录: ${PROJECT_PATH}"
        return 1
    }
    
    log "INFO" "当前目录: $(pwd)"
    return 0
}

# 拉取最新代码
pull_latest_code() {
    log "INFO" "从GitHub拉取最新代码 (分支: ${GIT_BRANCH})..."
    
    # 检查git仓库
    if [ ! -d ".git" ]; then
        log "ERROR" "当前目录不是Git仓库"
        send_deploy_email "failure" "Git仓库检查失败" "当前目录不是Git仓库"
        return 1
    fi
    
    # 拉取代码
    git fetch origin
    if ! git pull origin "$GIT_BRANCH"; then
        log "ERROR" "Git拉取失败"
        send_deploy_email "failure" "代码拉取失败" "从${GIT_BRANCH}分支拉取代码失败"
        return 1
    fi
    
    local latest_commit=$(git log -1 --oneline 2>/dev/null || echo "未知")
    log "INFO" "代码拉取成功，最新提交: ${latest_commit}"
    
    # 记录部署详情
    DEPLOY_DETAILS="代码分支: ${GIT_BRANCH}\n最新提交: ${latest_commit}\n拉取时间: $(date '+%H:%M:%S')"
    
    return 0
}

# 检查Docker容器状态
check_container_status() {
    log "INFO" "检查Docker容器状态: ${CONTAINER_NAME}"
    
    # 检查容器是否存在
    if ! docker inspect "$CONTAINER_NAME" > /dev/null 2>&1; then
        log "INFO" "容器不存在: ${CONTAINER_NAME}"
        CONTAINER_EXISTS=false
        CONTAINER_RUNNING=false
        return 0
    fi
    
    CONTAINER_EXISTS=true
    
    # 检查容器是否在运行
    if [ "$(docker inspect -f '{{.State.Running}}' "$CONTAINER_NAME" 2>/dev/null)" = "true" ]; then
        log "INFO" "容器正在运行: ${CONTAINER_NAME}"
        CONTAINER_RUNNING=true
        
        # 获取容器ID
        CONTAINER_ID=$(docker ps -q --filter "name=${CONTAINER_NAME}")
        log "INFO" "容器ID: ${CONTAINER_ID}"
    else
        log "INFO" "容器已停止: ${CONTAINER_NAME}"
        CONTAINER_RUNNING=false
    fi
    
    return 0
}

# 停止并移除旧容器
cleanup_old_container() {
    log "INFO" "清理旧容器..."
    
    # 停止正在运行的容器
    local running_containers=$(docker ps -q --filter "name=${CONTAINER_NAME}")
    if [ -n "$running_containers" ]; then
        log "INFO" "停止运行中的容器: ${running_containers}"
        docker stop $running_containers
    fi
    
    # 移除已停止的容器
    local all_containers=$(docker ps -aq --filter "name=${CONTAINER_NAME}")
    if [ -n "$all_containers" ]; then
        log "INFO" "移除容器: ${all_containers}"
        docker rm $all_containers
    fi
    
    # 清理未使用的镜像
    log "INFO" "清理未使用的Docker资源..."
    docker system prune -f
    
    return 0
}

# 构建Docker镜像
build_docker_image() {
    log "INFO" "构建Docker镜像: ${IMAGE_NAME}:${IMAGE_TAG}"
    
    # 检查Dockerfile
    if [ ! -f "Dockerfile" ]; then
        log "ERROR" "Dockerfile不存在"
        send_deploy_email "failure" "Docker构建失败" "Dockerfile不存在"
        return 1
    fi
    
    # 构建镜像
    if ! docker build --no-cache -t "${IMAGE_NAME}:${IMAGE_TAG}" .; then
        log "ERROR" "Docker镜像构建失败"
        send_deploy_email "failure" "Docker构建失败" "镜像构建失败，请检查Dockerfile"
        return 1
    fi
    
    log "INFO" "Docker镜像构建成功: ${IMAGE_NAME}:${IMAGE_TAG}"
    
    # 记录镜像信息
    local image_info=$(docker images | grep "${IMAGE_NAME}" | grep "${IMAGE_TAG}" | head -1)
    DEPLOY_DETAILS="${DEPLOY_DETAILS}\nDocker镜像: ${image_info}"
    
    return 0
}

# 运行Docker容器
run_docker_container() {
    log "INFO" "启动Docker容器..."
    
    # 检查端口是否被占用
    if ss -tulpn | grep -q ":${PORT_WEB}\s"; then
        log "WARN" "端口 ${PORT_WEB} 可能已被占用"
    fi
    
    if ss -tulpn | grep -q ":${PORT_WEBSOCKET}\s"; then
        log "WARN" "端口 ${PORT_WEBSOCKET} 可能已被占用"
    fi
    
    # 运行容器
    local run_cmd="docker run -d \
        --name ${CONTAINER_NAME} \
        -p ${PORT_WEB}:${CONTAINER_PORT_WEB} \
        -p ${PORT_WEBSOCKET}:${CONTAINER_PORT_WS} \
        ${IMAGE_NAME}:${IMAGE_TAG}"
    
    log "INFO" "执行命令: ${run_cmd}"
    
    if ! eval "$run_cmd"; then
        log "ERROR" "容器启动失败"
        send_deploy_email "failure" "容器启动失败" "Docker容器启动失败，命令: ${run_cmd}"
        return 1
    fi
    
    # 等待容器启动
    sleep 3
    
    # 验证容器运行状态
    if [ "$(docker inspect -f '{{.State.Running}}' "$CONTAINER_NAME")" = "true" ]; then
        log "INFO" "容器启动成功: ${CONTAINER_NAME}"
        
        # 获取容器信息
        local container_id=$(docker ps -q --filter "name=${CONTAINER_NAME}")
        local container_ip=$(docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' "$CONTAINER_NAME")
        
        DEPLOY_DETAILS="${DEPLOY_DETAILS}\n容器状态: 运行中\n容器ID: ${container_id}\n内部IP: ${container_ip}\n映射端口: ${PORT_WEB}->${CONTAINER_PORT_WEB}, ${PORT_WEBSOCKET}->${CONTAINER_PORT_WS}"
        
        return 0
    else
        log "ERROR" "容器启动后未运行"
        send_deploy_email "failure" "容器启动异常" "容器${CONTAINER_NAME}启动后未进入运行状态"
        return 1
    fi
}

# 验证部署结果
verify_deployment() {
    log "INFO" "验证部署结果..."
    
    # 检查容器是否响应
    local max_retries=10
    local retry_count=0
    
    while [ $retry_count -lt $max_retries ]; do
        if docker logs "$CONTAINER_NAME" 2>&1 | tail -5 | grep -q "listening\|started\|running"; then
            log "INFO" "应用启动成功检测到"
            break
        fi
        
        retry_count=$((retry_count + 1))
        log "INFO" "等待应用启动... (${retry_count}/${max_retries})"
        sleep 2
    done
    
    # 获取容器日志
    local container_logs=$(docker logs "$CONTAINER_NAME" 2>&1 | tail -20)
    DEPLOY_DETAILS="${DEPLOY_DETAILS}\n应用日志: ${container_logs}"
    
    return 0
}

# 主部署流程
main_deployment() {
    init_log
    
    log "INFO" "开始部署项目: ${PROJECT_NAME}"
    
    # 步骤1: 检查项目目录
    if ! check_project_dir; then
        return 1
    fi
    
    # 步骤2: 拉取最新代码
    if ! pull_latest_code; then
        return 1
    fi
    
    # 步骤3: 检查容器状态
    check_container_status
    
    # 步骤4: 清理旧容器
    if ! cleanup_old_container; then
        send_deploy_email "failure" "容器清理失败" "清理旧Docker容器时出错"
        return 1
    fi
    
    # 步骤5: 构建Docker镜像
    if ! build_docker_image; then
        return 1
    fi
    
    # 步骤6: 运行Docker容器
    if ! run_docker_container; then
        return 1
    fi
    
    # 步骤7: 验证部署
    verify_deployment
    
    # 步骤8: 发送成功通知
    local success_message="✅ 部署成功完成\n\n项目: ${PROJECT_NAME}\n时间: $(date '+%Y-%m-%d %H:%M:%S')\n状态: 所有步骤执行成功\n\n访问地址:\n- Web: http://服务器IP:${PORT_WEB}\n- WebSocket: ws://服务器IP:${PORT_WEBSOCKET}"
    
    if send_deploy_email "success" "成功" "$success_message"; then
        log "INFO" "========== 部署成功完成: $(date '+%Y-%m-%d %H:%M:%S') =========="
        echo "✅ 部署成功完成!"
        echo "   项目: ${PROJECT_NAME}"
        echo "   容器: ${CONTAINER_NAME}"
        echo "   端口: ${PORT_WEB} (Web), ${PORT_WEBSOCKET} (WebSocket)"
        echo "   时间: $(date '+%Y-%m-%d %H:%M:%S')"
    else
        log "WARN" "部署成功但邮件通知发送失败"
        echo "⚠️  部署成功，但邮件通知发送失败"
    fi
    
    return 0
}

# 异常处理
handle_error() {
    local error_code=$?
    local error_message="部署过程在第${BASH_LINENO[0]}行发生错误 (退出码: ${error_code})"
    
    log "ERROR" "$error_message"
    
    # 发送失败邮件
    local failure_message="❌ 部署失败\n\n项目: ${PROJECT_NAME}\n时间: $(date '+%Y-%m-%d %H:%M:%S')\n错误: ${error_message}\n\n请查看服务器日志: ${LOG_FILE}"
    
    send_deploy_email "failure" "失败" "$failure_message"
    
    echo "❌ 部署失败!"
    echo "   错误详情请查看: ${LOG_FILE}"
    exit $error_code
}

# 设置错误处理
trap handle_error ERR

# 执行主函数
main_deployment
```



**普通 Node 项目做法**

（2）创建部署脚本 /www/deploy/deploy-server.sh，用 pm2 即可访问后端服务

```shell
#!/bin/bash

# 1. 进入项目目录
cd /www/wwwroot/mysite-express || exit

# 2. 拉取最新代码
echo "Pulling latest code from GitHub..."
git pull origin main

# 3. 安装依赖
npm install

# 4. 重启PM2应用
pm2 restart server

# 5. 保存PM2状态
pm2 save

echo "Deploy finished at $(date)"

```

pm2部署成功发送邮件的做法

```shell
#!/bin/bash
# deploy_with_email.sh

# ========== 配置 ==========
CONFIG_QQ_EMAIL="您的QQ邮箱@qq.com"
CONFIG_QQ_AUTH_CODE="您的QQ邮箱授权码"
CONFIG_TO_EMAIL="接收通知的邮箱@example.com"
PROJECT_NAME="mysite-express"
# ==========================


# 邮件发送函数
send_deploy_email() {
    local status=$1  # 0=成功, 1=失败
    local message="$2"
    
    if [ $status -eq 0 ]; then
        subject="✅ ${PROJECT_NAME}部署成功 - $(date '+%m-%d %H:%M')"
    else
        subject="❌ ${PROJECT_NAME}部署失败 - $(date '+%m-%d %H:%M')"
    fi
    
    # 创建邮件内容
    cat > /tmp/deploy_email.txt << EOF
From: ${CONFIG_QQ_EMAIL}
To: ${CONFIG_TO_EMAIL}
Subject: ${subject}
Date: $(date -R)

项目: ${PROJECT_NAME}
时间: $(date)
服务器: $(hostname)
状态: ${subject%% *}


--- 自动发送 ---
EOF
    
    # 发送邮件
    curl --ssl-reqd --silent \
      --url 'smtps://smtp.qq.com:465' \
      --user "${CONFIG_QQ_EMAIL}:${CONFIG_QQ_AUTH_CODE}" \
      --mail-from "${CONFIG_QQ_EMAIL}" \
      --mail-rcpt "${CONFIG_TO_EMAIL}" \
      --upload-file /tmp/deploy_email.txt
    
    rm -f /tmp/deploy_email.txt
}

# 主部署流程
main() {
    echo "开始部署..."
    
    # 原有部署步骤
    cd /www/wwwroot/mysite-express || {
        send_deploy_email 1 "无法进入目录 /www/wwwroot/mysite-express"
        exit 1
    }
    
    
    if ! git pull origin main; then
        send_deploy_email 1 "Git拉取失败"
        exit 1
    fi

    if ! npm install; then
        send_deploy_email 1 "npm安装失败"
        exit 1
    fi

    if ! npm run build; then
        send_deploy_email 1 "构建失败"
        exit 1
    fi

    if ! pm2 restart server; then
        send_deploy_email 1 "PM2重启失败"
        exit 1
    fi

    pm2 save

     # 发送成功通知
    send_deploy_email 0 "所有步骤执行成功"
    echo "部署完成于 $(date)"
    
}

# 运行
main

```



### 3、创建 webhook 接收服务（Node 示例）

安装依赖：

```bash
mkdir -p /www/webhook && cd /www/webhook
npm init -y
npm install express body-parser
```

创建 index.js：

```js
const express = require('express');
const bodyParser = require('body-parser');
const { exec } = require('child_process');

const app = express();
app.use(bodyParser.json());

app.post('/webhook', (req, res) => {
    console.log('Git 推送事件已接收');
    exec('sh /www/deploy/deploy.sh', (err, stdout, stderr) => {
        if (err) {
            console.error('执行失败', stderr);
            return res.status(500).send('error');
        }
        console.log('部署成功', stdout);
        res.send('部署成功');
    });
});

app.listen(4000, '0.0.0.0', () => {
    console.log('Webhook 服务已运行，端口 4000');
});
```

**在阿里云后台安全组中的入方向一定要放开 4000 的端口**
启动服务（建议用 pm2 保活）：

```bash
npm install -g pm2
pm2 start index.js --name webhook
```

对于 Docker 镜像运行的项目，在创建 webhook 时，由于构建镜像需要时间，所以要提前给 github 返回状态

```js
const express = require('express');
const bodyParser = require('body-parser');
const { exec } = require('child_process');

const app = express();
app.use(bodyParser.json());

app.post('/webhook-comkoa', (req, res) => {
    console.log('Git 推送事件已接收');
    res.send('已接收，正在后台部署中...');
    exec('sh /www/deploy/deploy-comkoa.sh', (err, stdout, stderr) => {
        if (err) {
            console.error('执行失败', stderr);
            return;
            // return res.status(500).send('error')
        }
        console.log('部署成功', stdout);
        // res.send('后台部署成功')
    });
});

app.listen(4006, '0.0.0.0', () => {
    console.log('Webhook 服务已运行，端口 4006');
});
```

### 4、在 GitHub/Gitee 中配置 Webhook

前往仓库设置：

1. 找到具体项目，进入 Settings > Webhooks > Add Webhook。
2. Payload URL: http://你的服务器公网 IP:4000/webhook（注意公网可访问）
3. Content type: application/json
4. Secret: （可选，用于校验）
5. 事件类型：只选 push
6. 保存

## 四、申请免费证书文件

参考文档：[acme.sh](https://github.com/acmesh-official/acme.sh/wiki/%E8%AF%B4%E6%98%8E)

### 1、安装 acme.sh

```bash
curl https://get.acme.sh | sh -s email=my@example.com
```

安装成功后重新登录终端，或者执行：

```bash
source ~/.bashrc
```

确认是否安装成功：

```bash
acme.sh --version
```

### 2、使用 DSAPI 的方式

打开链接：[dsnpi](https://github.com/acmesh-official/acme.sh/wiki/dnsapi)
在这个页面中找到 Aliyun，在阿里云后台创建子用户，获取到 Ali_Key 和 Ali_Secret，然后到控制台输入下面两条命令

步骤一：进入阿里云 RAM 控制台

 登录阿里云后台 [RAM 控制台](https://ram.console.aliyun.com/users)

 左侧点击 用户 → 创建用户

步骤二：创建子用户

 选择 普通用户（RAM 用户）

 填写用户名（随便，比如 acme-cert-user，https 等）

 勾选 AccessKey 访问（AccessKey 方式）

 不要选控制台访问，否则多余

 点击 确认。

步骤三：给用户授权

 创建完成后，进入刚建好的用户详情页

 找到 权限管理 → 添加权限

 搜索 AliyunDNSFullAccess

 勾选该策略，点击 确定

 这样，这个用户就有了对阿里云 DNS 服务的完全操作权限。

步骤四：生成 AccessKey

 在用户详情页 → 安全设置 → AccessKey 管理

 点击 创建 AccessKey

 系统会生成 AccessKeyId 和 AccessKeySecret

 记得保存下来，Secret 只会显示一次。

步骤五：在服务器配置环境变量

 在你的阿里云服务器终端执行：

```bash
export Ali_Key="<key>"
export Ali_Secret="<secret>"
```

为了长期生效，可以写入 ~/.bashrc：

```bash
echo 'export Ali_Key="你的AccessKeyId"' >> ~/.bashrc
echo 'export Ali_Secret="你的AccessKeySecret"' >> ~/.bashrc
source ~/.bashrc
```

切换 CA：使用 Let's Encrypt 替代 ZeroSSL

```bash
acme.sh --set-default-ca --server letsencrypt
```

步骤六：测试

 执行以下命令测试申请证书：

 然后执行下面的命令：

```bash
acme.sh --issue --dns dns_ali -d zzf.net.cn -d *.zzf.net.cn
```

 如果执行命令报错可能没注册账号，执行以下命令

```bash
acme.sh --register-account -m your@email.com
```

如果配置正确，acme.sh 会自动在[阿里云 DNS](https://dnsnext.console.aliyun.com/authoritative) 里添加 \_acme-challenge.zzf.net.cn 这种 TXT 记录，并通过验证，最终生成证书。

🚀 到这一步，你的 acme.sh + 阿里云 DNS 自动化申请 SSL 证书就可以跑通了。

如果生成的证书文件不在 nginx 配置中的路径时，需要执行以下命令，才能确保 acme 自动续期后的文件自动同步到对应的文件夹下，如果 nginx 配置的路径直接指向 acme 的目录文件，则不需要执行以下的操作
nginx：

```shell
acme.sh --install-cert -d yourdomain.com \
--key-file /etc/nginx/ssl/yourdomain.key \
--fullchain-file /etc/nginx/ssl/yourdomain.cer \
--reloadcmd "systemctl reload nginx"
```

宝塔面板用户

宝塔一般存放证书的路径是：

```shell
/www/server/panel/vhost/cert/yourdomain.com/
```

可以这样安装：

```shell
acme.sh --install-cert -d yourdomain.com \
--key-file /www/server/panel/vhost/cert/yourdomain.com/privkey.pem \
--fullchain-file /www/server/panel/vhost/cert/yourdomain.com/fullchain.pem \
--reloadcmd "bt reload"
```

```shell
acme.sh --install-cert -d zzf.net.cn \
--key-file /www/server/panel/vhost/cert/zzf.net.cn/privkey.pem \
--fullchain-file /www/server/panel/vhost/cert/zzf.net.cn/fullchain.pem \
--reloadcmd "bt reload"
```
