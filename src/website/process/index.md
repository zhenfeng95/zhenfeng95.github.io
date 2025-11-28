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


    location ^~ /api/ {
        rewrite ^/api/(.*)$ /api/$1 break;
        proxy_pass http://127.0.0.1:3001;
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

​ 登录阿里云后台 [RAM 控制台](https://ram.console.aliyun.com/users)

​ 左侧点击 用户 → 创建用户

步骤二：创建子用户

​ 选择 普通用户（RAM 用户）

​ 填写用户名（随便，比如 acme-cert-user，https 等）

​ 勾选 AccessKey 访问（AccessKey 方式）

​ 不要选控制台访问，否则多余

​ 点击 确认。

步骤三：给用户授权

​ 创建完成后，进入刚建好的用户详情页

​ 找到 权限管理 → 添加权限

​ 搜索 AliyunDNSFullAccess

​ 勾选该策略，点击 确定

​ 这样，这个用户就有了对阿里云 DNS 服务的完全操作权限。

步骤四：生成 AccessKey

​ 在用户详情页 → 安全设置 → AccessKey 管理

​ 点击 创建 AccessKey

​ 系统会生成 AccessKeyId 和 AccessKeySecret

​ 记得保存下来，Secret 只会显示一次。

步骤五：在服务器配置环境变量

​ 在你的阿里云服务器终端执行：

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

​ 执行以下命令测试申请证书：

​ 然后执行下面的命令：

```bash
acme.sh --issue --dns dns_ali -d zzf.net.cn -d *.zzf.net.cn
```

​ 如果执行命令报错可能没注册账号，执行以下命令

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
