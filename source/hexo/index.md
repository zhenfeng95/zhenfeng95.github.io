---
hexo如何搭建个人博客
---
[Hexo网址](https://hexo.io/)

###  一、搭建项目

```bash
npm install hexo-cli -g
hexo init blog
cd blog
npm install
hexo server
```

### 二、部署到github

#### 1. 在 GitHub 上创建仓库

仓库名必须为：

```bash
your-github-username.github.io
```

#### 2.安装部署插件

```bash
npm install hexo-deployer-git --save
```

#### 3.修改 `_config.yml`（位于 Hexo 根目录）

找到deploy：修改成下面的配置

```bash
deploy:
  type: git
  repo: git@github.com:zhenfeng95/zhenfeng95.github.io.git
  branch: main
  name: zzf
  email: 285273676@qq.com
```

#### 4.一键部署

```bash
hexo clean && hexo generate && hexo deploy
```

执行后会自动推送生成的静态网页到 GitHub Pages。
几秒后访问：

```bash
https://zhenfeng95.github.io
```

### 三、绑定自定义域名

假设你已经在阿里云或腾讯云购买了域名。

登录阿里云后台，[域名解析](https://dc.console.aliyun.com/#/domain-list/all)

找到你的域名，添加以下记录：

| 记录类型 | 主机记录 | 记录值               |
| -------- | -------- | -------------------- |
| CNAME    | blog     | zhenfeng95.github.io |

### 四、启用 HTTPS（强烈推荐）

#### 1.打开你的博客仓库

#### 2.进入：

 **Settings → Pages → Custom domain**

#### 3.输入你的域名：

```bash
blog.zzf.net.cn
```

#### 4.点击保存，记得勾选：

```bash
Enforce HTTPS
```

GitHub 会自动为你的子域名签发免费证书。

### 五、验证是否成功

几分钟后（DNS 生效），你可以访问：

```bash
https://blog.zzf.net.cn
```

能看到你的 Hexo 博客页面 
就代表部署成功！



