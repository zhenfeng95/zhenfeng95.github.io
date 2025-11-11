
---
title: Node
type: node
order: 1
---

## [#](https://front-end.toimc.com/notes-page/basic/node/#什么是-node-js)什么是 Node.js

Node.js® is a JavaScript runtime built on Chrome's V8 JavaScript engine.

`Node.js` `是`一个基于 Chrome V8 引擎的 `JavaScript` `运行环境`。

通俗的理解：Node.js 为 JavaScript 代码的正常运行，提供的必要的环境。

Node.js 的官网地址: https://nodejs.org/zh-cn/

## [#](https://front-end.toimc.com/notes-page/basic/node/#node运行时)Node运行时

![Node.js runtime architecture](node.assets/1*txgFPN5LaUZvPOXelJlSuA.ac77e90a.jpeg)

注意：

* Node.js 是 JavaScript 的`后端`运行环境。（正常情况下，Nodejs要安装到服务器上）
* Node.js 中无法调用 DOM 和 BOM 等 浏览器内置 API

## [#](https://front-end.toimc.com/notes-page/basic/node/#安装node)安装Node

方案：

* 官网下载直装：[https://nodejs.org/zh-cn/download/(opens new window)](https://nodejs.org/zh-cn/download/)

* 使用nvm（Node Version Management，node版本管理工具）安装**（推荐）**

  * mac：[https://github.com/nvm-sh/nvm (opens new window)](https://github.com/nvm-sh/nvm)， 国内镜像仓库地址：[https://gitee.com/mirrors/nvm/(opens new window)](https://gitee.com/mirrors/nvm/)

    安装脚本：

    ```bash
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
    # or
    wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
    
    # 国内
    curl -o- https://gitee.com/mirrors/nvm/raw/master/install.sh | bash
    ```

    国内加速：

    ```text
    export NVM_NODEJS_ORG_MIRROR=http://npm.taobao.org/mirrors/node
    ```

  * windows：[https://github.com/coreybutler/nvm-windows(opens new window)](https://github.com/coreybutler/nvm-windows)

    安装包：[1.1.9.zip(opens new window)](https://github.91chi.fun/https://github.com//coreybutler/nvm-windows/releases/download/1.1.9/nvm-setup.zip)

  配置nvm环境，找到如下位置的文件（如果没有则新建）：

  * M1：`~/.zprofile`
  * Intel：`~/.bash_profile`， `~/.zshrc`，`~/.profile`或者 `~/.bashrc`

  添加如下内容：

  ```bash
  export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
  [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
  ```

  并重启终端工具。

  常见nvm命令(Mac)：

  ```text
  # 查看有哪些可以下载的版本 mac&Linux
  nvm ls-remote
  
  # 安装指定的版本
  nvm install v16.14.0
  
  # 使用特定的版本
  nvm use v16.14.0
  
  # 设置别名，对应的是nvm unalias mac&Linux
  nvm alias v16 v16.14.0
  # 设置了之后就可以使用别名了
  nvm use v16
  
  # 设置默认的版本 mac&Linux
  nvm alias default v16.14.0
  ```

## [#](https://front-end.toimc.com/notes-page/basic/node/#包管理器npm-yarn-pnpm)包管理器npm&yarn&pnpm

介绍

* npm：node默认的包管理工具(自带)

  npm（node package manage）node 包 管理器，管理node包的工具。

  包是什么？包就是模块，一个包可以包括一个或多个模块。

  > npm不需要额外的安装，只需要安装node，即会自动的安装npm。

* yarn：特点扁平化依赖，并行安装，本地缓存

  ```text
  npm i -g yarn
  
  # 这样才能国内加速安装
  yarn config set registry https://registry.npmmirror.com
  
  # yarn的命令与npm有出入
  # 参考：http://www.imooc.com/wiki/yarnlesson
  ```

* pnpm：特点

  * 节约磁盘空间，缓存技术加持
  * 速度快
  * 支持 monorepo
  * 安全性高

  ```text
  npm i -g pnpm
  ```

  官方地址：[https://pnpm.io/zh/motivation(opens new window)](https://pnpm.io/zh/motivation)

  核心：创建非扁平化的 node_modules 文件夹

  ![img](node.assets/node-modules-structure-8ab301ddaed3b7530858b233f5b3be57.a23ee45d.jpg)

  使用 npm 或 Yarn Classic 安装依赖项时，所有包都被提升到模块目录的根目录。 因此，项目可以访问到未被添加进当前项目的依赖。

  默认情况下，pnpm 使用软链的方式将项目的直接依赖添加进模块文件夹的根目录。

  如果你想了解 pnpm 关于 `node_modules` 结构设计的更多细节，以及为什么它在 Node.js 生态成为了后起之秀，请参考：

  * [扁平的 node_modules 不是唯一的方法(opens new window)](https://pnpm.io/zh/blog/2020/05/27/flat-node-modules-is-not-the-only-way)
  * [基于符号链接的 node_modules 结构(opens new window)](https://pnpm.io/zh/symlinked-node-modules-structure)

下载速度对比：

![Snipaste_2022-02-25_16-53-34](node.assets/Snipaste_2022-02-25_16-53-34.f0785f3a.jpg)

node12下，benchmark对比：

![Snipaste_2022-02-25_16-53-56](node.assets/Snipaste_2022-02-25_16-53-56.2105b285.jpg)

## [#](https://front-end.toimc.com/notes-page/basic/node/#npm国内加速)npm国内加速

初始化之后，就可以在当前文件夹中安装第三方模块了

```bash
# 直接修改npm源
# 参考：https://npmmirror.com/
npm config set registry http://mirrors.cloud.tencent.com/npm/
npm config set registry https://registry.npmmirror.com/
npm config set registry https://mirrors.huaweicloud.com/repository/npm/

# 使用cnpm
npm install -g cnpm --registry=https://registry.npmmirror.com
cnpm install xxx

# 使用nrm
npm install -g nrm --registry=https://registry.npmmirror.com
nrm use taobao
```

## [#](https://front-end.toimc.com/notes-page/basic/node/#常见命令)常见命令

```bash
# 正常的下载安装
npm install 模块名

# 简写install为i
npm i 模块名

# 一次性安装多个模块
npm i 模块名 模块名 模块名

# 卸载模块
npm uninstall 模块名
```

关于本地模块的说明

* 下载安装的模块，存放在当前文件夹的 `node_modules` 文件夹中，同时还会生成一个记录下载的文件 `package-lock.json`
* 下载的模块，在哪里可以使用
  * 在当前文件夹
  * 在当前文件夹的子文件夹
  * 在当前文件夹的子文件夹的子文件夹
  * ......
  * 翻过来讲，当查找一个模块的时候，会在当前文件夹的 node_modules 文件夹查找，如果找不到，则去上层文件夹的node_modules文件夹中查找，.....依次类推。

> **重要**：代码文件夹不能有中文；代码文件夹不能和模块名同名。



# NPM包管理工具

这个部分是扩展npm命令的篇章，主要应用场景：

* 版本号管理
* 发包

## [#](https://front-end.toimc.com/notes-page/basic/node/00-npm.html#版本号管理)版本号管理

![image-20220528102521776](node.assets/image-20220528102521776.2c04e51a.png)

npm version命令是用来管理package.json中的version属性的。

相比于使用git tag命令，npm version = 修改package.json中的version + git tag打标签。

并且，npm version针对于语义化的版本号，设置了不同的命令：

预发布相关：

* prerelease
* prepatch
* preminor
* premajor

正式发布相关

* patch
* minor
* Major

## [#](https://front-end.toimc.com/notes-page/basic/node/00-npm.html#预发布相关)预发布相关

### [#](https://front-end.toimc.com/notes-page/basic/node/00-npm.html#npm-version)npm version

这里主要是`npm version`命令的相关介绍。

```sh
-> npm version --help

npm version [<newversion> | major | minor | patch | premajor | preminor | prepatch | prerelease [--preid=<prerelease-id>] | from-git]
(run in package dir)
'npm -v' or 'npm --version' to print npm version (6.4.1)
'npm view <pkg> version' to view a package's published version
'npm ls' to inspect current package/dependency versions
```

直接使用`npm version`:

```sh
{
  'vue3-toimc-admin-doc': '1.0.0',
  npm: '6.14.17',
  ares: '1.18.1',
  brotli: '1.0.9',
  cldr: '40.0',
  icu: '70.1',
  llhttp: '2.1.4',
  modules: '83',
  napi: '8',
  nghttp2: '1.42.0',
  node: '14.19.2',
  openssl: '1.1.1n',
  tz: '2021a3',
  unicode: '14.0',
  uv: '1.42.0',
  v8: '8.4.371.23-node.87',
  zlib: '1.2.11'
}
```

### [#](https://front-end.toimc.com/notes-page/basic/node/00-npm.html#prerelease)prerelease

pre-预

release-发布版

所以，prerelease就是预发版。

TIP

记住：release是最后面的号。

原则：

* 当执行`npm version prerelease`时，如果没有预发布号，则增加minor，同时prerelease 设为0；
* 如果有prerelease， 则prerelease 增加1。

```bash
# package.json 中的版本号1.0.0变为 1.0.1-0
➜ npm version prerelease
v1.0.1-0

# 再次执行 package.json 中的版本号1.0.1-0变为 1.0.1-1
➜ npm version prerelease
v1.0.1-1

# 如何手动修改，version为1.0.0-1，再次执行
➜ npm version prerelease
v1.0.0-2
```

### [#](https://front-end.toimc.com/notes-page/basic/node/00-npm.html#prepatch)prepatch

直接升级小号，增加预发布号为0。

TIP

因为跟pre预发布有关，所以会带个尾巴。

```bash
# package.json 中的版本号1.0.1-1变为 1.0.2-0
➜ npm version prepatch
v1.0.2-0
```

### [#](https://front-end.toimc.com/notes-page/basic/node/00-npm.html#preminor)preminor

直接升级中号，小号置为0，增加预发布号为0。

```bash
➜ npm version preminor
v1.1.0-0
```

### [#](https://front-end.toimc.com/notes-page/basic/node/00-npm.html#premajor)premajor

直接升级大号，中号、小号置为0，增加预发布号为0。

```bash
➜ npm version premajor
v2.0.0-0
```

## [#](https://front-end.toimc.com/notes-page/basic/node/00-npm.html#正式版本相关)正式版本相关

### [#](https://front-end.toimc.com/notes-page/basic/node/00-npm.html#patch)patch

TIP

记住：patch是小号。

原则：

* patch：如果有prerelease ，则去掉prerelease ，其他保持不变；
* 如果没有prerelease ，则升级minor，即是最小号。

```bash
# package.json 中的版本号2.0.0-0变为 2.0.0;
➜ npm version patch
v2.0.0

# package.json 中的版本号2.0.0变为 2.0.1;
# 再次执行npm version patch
➜ npm version patch
v2.0.1
```

### [#](https://front-end.toimc.com/notes-page/basic/node/00-npm.html#minor)minor

TIP

记住：minor是中间的号。

原则：

* 如果没有prerelease，直接升级minor， 同时patch设置为0；
* 如果有prerelease， 首先需要去掉prerelease；
* 如果patch为0，则不升级minor；
* 如果patch不为0， 则升级minor，同时patch设为0；

```bash
➜ npm version minor
v2.1.0

# npm version premajor 2.1.0–> 3.0.0-0;
# npm version minor 3.0.0-0–> 3.0.0;
# npm version prepatch 3.0.0–>3.0.1-0;
# npm version minor 3.0.1-0–>3.1.0;
```

### [#](https://front-end.toimc.com/notes-page/basic/node/00-npm.html#major)major

TIP

记住： major是主号，也是大号

```bash
# 如果没有premjor，则直接升级major，其他位都置为0；
➜ npm version major
v4.0.0
➜ npm version major
v5.0.0
# 如果有premajor，则会去掉premajor版本号；
➜ npm version premajor
v6.0.0-0
➜ npm version major
v6.0.0

# 如果有preminor/prepatch/prerelease，则会去掉`release`版本；
➜ npm version prepatch
v6.0.3-0
➜ npm version major
v7.0.0

# 如果有minor/patch，则会直接置0，升级主版本号；
➜ npm version patch
v7.2.1
➜ npm version major
v8.0.0
```

主要目的升级major，原则：

* 如果没有premajor，则直接升级major，其他位都置为0；如果有premajor，则会去掉premajor版本号；
* 如果有preminor/prepatch/prerelease，则会去掉`release`版本；
* 如果有minor/patch，则会直接置0，升级主版本号；

## [#](https://front-end.toimc.com/notes-page/basic/node/00-npm.html#发包相关)发包相关

常见的`npm publish`、`npm link`、`npm login`这里不介绍了，主要介绍与发包之后的命令：

* 查看历史版本
* 删除版本
* 废弃版本

### [#](https://front-end.toimc.com/notes-page/basic/node/00-npm.html#查看历史版本)查看历史版本

言归正传，如何查看已发布包的历史版本呢？

使用命令：

```bash
// 查看历史版本信息（最多100条）
npm view [<pkg>][@<version>] versions
// 假设我的包名是test 版本号是1.0.0
npm view test@1.0.0 versions
```

一般来说，这个命令就已经够了，如果发布的版本有100个以上，可能下面的这个命令能用的上。

和上面的类似，只需在后面加上 `--json` 即可查看所有的历史版本。

```bash
// 查看所有版本信息
npm view [<pkg>][@<version>] versions --json
// 假设我的包名是test 版本号是1.0.0
npm view test@1.0.0 versions --json
```

如果想查看最新的版本呢？

```bash
// 查看最新版本信息
npm view [<pkg>][@] version
// 假设我的包名是test 版本号是1.0.0
npm view test@1.0.0 version
```

注意细节哦，`versions`少了一个`s` 。

但是这个是针对正式版本的，如果是测试版本，是不会出现在最新版本的查询里的。

### [#](https://front-end.toimc.com/notes-page/basic/node/00-npm.html#删除版本)删除版本

删除版本前，一定要确认这个版本的包已经没有依赖（废弃）了。

```sh
// 假设我的包名是test 测试版本号是1.0.0-beta.0
// 删除包的指定版本
npm unpublish test@1.0.0-beta.0 
 
// 强制删除包的指定版本
npm unpublish test@1.0.0-beta.0 --force
 
// 删除包
npm unpublish test
 
// 强制删除包
npm unpublish test --force
```

### [#](https://front-end.toimc.com/notes-page/basic/node/00-npm.html#废弃版本)废弃版本

什么是废弃版本？

就是npm包还在，但是受某些因素影响，该包不再维护，不再更新了。

简单来说，就是不影响使用，但是在安装废弃版本的时候会有提示。

```sh
npm deprecate <pkg>[@<version>] <message>
 
// 假设我的包名是test 版本号是1.0.1
npm deprecate test '不再维护'
npm deprecate test@1.0.1 '不再维护'
```



# [#](https://front-end.toimc.com/notes-page/basic/node/01-koa.html#koa-下一代web框架)Koa(下一代web框架)

[koa (opens new window)](https://koajs.com/)([中文网 (opens new window)](https://www.koajs.com.cn/))是基于 Node.js 平台的下一代 web 开发框架，致力于成为应用和 API 开发领域中的一个更小、更富有表现力、更健壮的基石；利用*async 函数*丢弃回调函数，并增强错误处理，koa 没有任何预置的中间件，可快速的编写服务端应用程序。

## [#](https://front-end.toimc.com/notes-page/basic/node/01-koa.html#核心概念)核心概念

![微信图片_20200713125342.jpg](node.assets/1594616036570-1030d9b1-ad5f-42b6-910b-8a90b43a6c00.205fb4e5.jpeg)

* Koa Application（应用程序）
* Context（上下文）
* Request（请求）、Response(响应)

## [#](https://front-end.toimc.com/notes-page/basic/node/01-koa.html#初识-koa)初识 koa

```shell
// 创建一个 acquaintance 的文件夹
$ mkdir acquaintance

// 进入创建的文件夹
$ cd acquaintance

// 初始化 package.json
$ npm init -y

// 安装 koa，安装完之后
$ npm i koa
```

TIP

可以在 package.json 中查看安装的所有依赖

在工程目录里创建一个 app.js，代码如下：

```javascript
const Koa = require('koa') // 引入koa
const app = new Koa() // 实例化koa

app.use(ctx => {
  ctx.body = 'hi koa'
})

// 启动应用程序  参数：端口号
app.listen(3003)
```

在终端中使用`node app.js`命令（前提是终端中的路径必须指向所创建工程文件夹的路径） 打开浏览器访问：`http://localhost:3003` 此时浏览器中就会输出`hi koa`，如下图：

![img](data:image/png;base64,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)

WARNING

上面代码虽然轻松实现了一个 web 服务器，但是返回的数据和所请求都是固定的；并不适应真实的业务场景，比如：获取请求接口时的参数、方法、修改一次代码就要在终端中重新运行启动命令等；

由于使用的`node app.js`启动，所以每次更改都要重新启动，这样给我们开发带来了极大的不便利，所以我们可以使用一些第三方依赖来自动监听文件的变化并重新启动，开发环境可以使用`nodemon` 首先安装`npm i nodemon -D`，也可以全局安装此依赖，生产环境的话可以使用`pm2` 安装之后在`package.json`的`scripts`中添加启动方式；如下

```json
{
  "name": "acquaintance",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "dev": "nodemon app.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "koa": "^2.13.1"
  },
  "devDependencies": {
    "nodemon": "^2.0.7"
  }
}
```

在终端中执行命令：`npm run dev`,这样就不用我们每次修改都重新启动，执行之后就会在终端中提示，如下： ![img](node.assets/e59f418482657d4dd68a3543880292b.36cc8278.png)

TIP

执行命令时，终端的路径必须指向当前程序

## [#](https://front-end.toimc.com/notes-page/basic/node/01-koa.html#路由)路由

路由即是**路径**与**处理函数**之间的对应关系！

Koa中的路由作用：

* 处理不同的 `URL`
* 处理不同的 `HTTP` 方法 (`GET`、`POST`、`PUT`、`DELETE`、`PATCH`、`OPTIONS`)
* 解析 `URL` 上的参数

**于 `HTTP` 协议而言，路由可以理解为，根据不同的 `HTTP` 请求，返回不同的响应；**

使用方法：

* 安装依赖：`@koa/router`
* 定义路由
  * 实例化`@koa/router`
  * 注册router
  * 定义接口

1. 安装依赖：[@koa/router(opens new window)](https://www.npmjs.com/package/@koa/router)

```git
$ npm i @koa/router
```

1. 定义路由

* 在 app.js 中引入`@koa/router`，然后再实例化

```javascript
const Router = require('@koa/router')
const router = new Router({ prefix: '/api/v1' }) // 实例化的时候可以自定义一个接口前缀
```

* 注册 router

```javascript
app.use(router.routes()).use(router.allowedMethods())
```

* 定义接口

```javascript
router.get('/', async ctx => {
  ctx.body = {
    status: 200,
    message: 'hi @koa/router'
  }
})

router.get('/user', async ctx => {
  ctx.body = {
    status: 200,
    message: 'success',
    data: {
      nickname: 'Forest',
      age: 18,
      jobs: '前端攻城狮',
      skills: '搬砖'
    }
  }
})
```

完整代码如下：

```javascript
const Koa = require('koa')
const Router = require('@koa/router')
const app = new Koa()
const router = new Router({ prefix: '/api/v1' }) // 添加接口前缀

router.get('/', async ctx => {
  ctx.body = {
    status: 200,
    message: 'hi @koa/router'
  }
})

router.get('/user', async ctx => {
  ctx.body = {
    status: 200,
    message: 'success',
    data: {
      nickname: 'Forest',
      age: 18,
      jobs: '前端攻城狮',
      skills: '搬砖'
    }
  }
})

app.use(router.routes()).use(router.allowedMethods())
// 启动应用程序  参数：端口号
app.listen(3003)
```

在浏览器中请求：`http://localhost:3003/api/v1`、`http://localhost:3003/api/v1/user`，结果如下图：
![img](data:image/png;base64,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) ![img](data:image/png;base64,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)

## [#](https://front-end.toimc.com/notes-page/basic/node/01-koa.html#中间件)中间件

中间件其实就是一个个函数，中间件是一系列的中间过程执行的函数，它可以通过`app.use()`注册；

* 在 koa 中只会自动执行第一个中间件，后面的都需要我们自己调用。
* koa 在执行中间件的时候都会携带两个参数`context`(可简化为`ctx`)和`next`，其中：
  * `context`是 koa 的上下文对象
  * `next`就是下一个中间件函数，也就是洋葱模型；

> 所谓洋葱模型，就是指每一个 Koa 中间件都是一层洋葱圈，它即可以掌管请求进入，也可以掌管响应返回。
>
> 换句话说：外层的中间件可以影响内层的请求和响应阶段，内层的中间件只能影响外层的响应阶段。

![4c4b9807221b34a21ab0b4548a8f739.jpg](node.assets/1594625703158-0a243950-d870-4399-bbbe-b57b16bbb075.18f48478.jpeg)

执行顺序按照 app.use()的顺序执行，中间件可以通过 `await next()`来执行下一个中间件，同时在最后一个中间件执行完成后，依然有恢复执行的能力。即，通过洋葱模型，`await next()`控制调用 “下游”中间件，直到 “下游”没有中间件且堆栈执行完毕，最终流回“上游”中间件。

下面这段代码的结果就能很好的诠释，示例：

```javascript
const Koa = require('koa')
const app = new Koa()

app.use(async (ctx, next) => {
  console.log(`this is a middleware 1`)
  await next()
  console.log(`this is a middleware 1 end `)
})

app.use(async (ctx, next) => {
  console.log(`this is a middleware 2`)
  await next()
  console.log(`this is a middleware 2 end `)
})

app.use(async (ctx, next) => {
  console.log(`this is a middleware 3`)
  await next()
  console.log(`this is a middleware 3 end `)
})

app.listen(3004)
```

运行结果：

```text
this is a middleware 1
this is a middleware 2
this is a middleware 3
this is a middleware 3 end
this is a middleware 2 end
this is a middleware 1 end
```

**原理：中间件是如何执行的？**

```javascript
// 通过 createServer 方法启动一个 Node.js 服务
listen(...args) {
  const server = http.createServer(this.callback());
  return server.listen(...args);
}
```

Koa 框架通过 http 模块的 createServer 方法创建一个 Node.js 服务，并传入 `this.callback()` 方法， `this.callback()` 方法源码精简实现如下：

```javascript
function compose(middleware) {
  // 这里返回的函数，就是上文中的 fnMiddleware
  return function (context, next) {
    let index = -1
    return dispatch(0)

    function dispatch(i) {
      if (i <= index) return Promise.reject(new Error('next() called multiple times'))
      index = i
      // 取出第 i 个中间件为 fn
      let fn = middleware[i]

      if (i === middleware.length) fn = next

      // 已经取到了最后一个中间件，直接返回一个 Promise 实例，进行串联
      // 这一步的意义是保证最后一个中间件调用 next 方法时，也不会报错
      if (!fn) return Promise.resolve()

      try {
          // 把 ctx 和 next 方法传入到中间件 fn 中，并将执行结果使用 Promise.resolve 包装
          // 这里可以发现，我们在一个中间件中调用的 next 方法，其实就是dispatch.bind(null, i + 1)，即调用下一个中间件
          return Promise.resolve(fn(context, dispatch.bind(null, i + 1)));
      } catch (err) {
          return Promise.reject(err)
      }
    }
  }
}
callback() {
  // 从 this.middleware 数组中，组合中间件
  const fn = compose(this.middleware);

  // handleRequest 方法作为 `http` 模块的 `createServer` 方法参数，
  // 该方法通过 `createContext` 封装了 `http.createServer` 中的 `request` 和 `response`对象，并将这两个对象放到 ctx 中
  const handleRequest = (req, res) => {
    const ctx = this.createContext(req, res);
    // 将 ctx 和组合后的中间件函数 fn 传递给 this.handleRequest 方法
    return this.handleRequest(ctx, fn);
  };

  return handleRequest;
}
handleRequest(ctx, fnMiddleware) {
  const res = ctx.res;
  res.statusCode = 404;
  const onerror = err => ctx.onerror(err);
  const handleResponse = () => respond(ctx);
  // on-finished npm 包提供的方法，该方法在一个 HTTP 请求 closes，finishes 或者 errors 时执行
  onFinished(res, onerror);
  // 将 ctx 对象传递给中间件函数 fnMiddleware
  return fnMiddleware(ctx).then(handleResponse).catch(onerror);
}
```

将 Koa 一个中间件组合和执行流程梳理为以下步骤:

* 通过 compose 方法组合各种中间件，返回一个中间件组合函数 fnMiddleware
* 请求过来时，会先调用 handleRequest 方法，该方法完成：
* 调用 createContext 方法，对该次请求封装出一个 ctx 对象；
* 接着调用 this.handleRequest(ctx, fnMiddleware)处理该次请求。
* 通过 fnMiddleware(ctx).then(handleResponse).catch(onerror)执行中间件。

## [#](https://front-end.toimc.com/notes-page/basic/node/01-koa.html#传参-取参-方式)传参(取参)方式

* 在 params 中取值，`eg：http://localhost:3003/api/v1/user/1`

```javascript
// 前端请求
await axios.post('http://localhost:3003/api/v1/user/1')

// 服务端
router.post('/user/:id',async ctx => {
    //获取url的id
  cosnt { id } = ctx.params; // { id: 1 }
})
```

* 在 query 中取值，也就是获取问号后面的。

```javascript
// 前端
await axios.post('http://localhost:3003/api/v1/user?name=Forest&age=18')

// 服务端
router.post('/user', async ctx => {
  //获取url的id
  const { name, age } = ctx.request.query // { name: Forest, age: 18 }
})
```

* 获取 header 中的参数：

```javascript
//请求接口时设置请求头
axios
  .post(
    'http://localhost/user?name=Forest&age=18',
    {
      headers: {
        Author: 'token'
      }
    }
    //......
  )
  .then(res => console.log('res:', res))

//在服务端获取则是：
router.post('/user', async ctx => {
  //获取 url 的 id
  const { Author } = ctx.request.header // { Author: 'token' }
})
```

* 获取 body 中的数据，在服务端获取 body 中的一些数据只能用一些外部的插件；如：`koa-body`、`koa-bodyparser` 等等。 就以 `koa-body` 为例，首先安装 `npm i koa-body -S`，再引入：

```javascript
// 服务端
const body = require('koa-body);

//然后在注册中间件：
app.use(body());

//在服务端获取则是：
router.post('/user', async ctx => {
    const res = ctx.request.body; // { name: 'Foreset', age: 18 }
});


//请求时有两种传参方式，一种是 json，另一种是 fromData；以 json 为例
axios.post('http://localhost/user', {name: 'Foreset', age: 18}).then(res => {
    console.log('res:', res)
});
```

## [#](https://front-end.toimc.com/notes-page/basic/node/01-koa.html#创建-restful-接口)创建 RESTful 接口

* 路由 koa-router
* 协议解析 koa-body
* 跨域处理 @koa/cors
* JSON美化 koa-json

koa的设计思想就是将数据处理都交给中间件：

```text
npm install -S koa-router koa-body @koa/cors koa-json
```

案例：

```javascript
//index.js

//引入对应的包
const Koa = require('koa')
const Router = require('koa-router')
const cors = require('@koa/cors')
const koaBody = require('koa-body')
const koaJson = require('koa-json')

const app = new Koa()
const router = new Router()

//prefix 访问路径都变为 ～/apiaa/ 为
router.prefix('/apiaa')

//访问 host/apiaa 返回文字 Hello World
router.get('/', ctx => {
  console.log(ctx)
  ctx.body = 'Hello World'
})

//访问 host/apiaa/api 就将get的参数的name和age返回回去 
router.get('/api', ctx => {
  //获取get的参数
  const params = ctx.request.query
  console.log(params.name)
  ctx.body = {
    name: params.name,
    age: params.age
  }
})

//访问 host/apiaa/async 服务器处理2秒后，返回Hello 2s later
router.get('/async', async (ctx) => {
  let result = await new Promise((resolve) => {
    setTimeout(function () {
      resolve('Hello 2s later')
    }, 2000)
  })
  ctx.body = result
})

// post接口，将post的参数再返回给请求
router.post('/post', async (ctx) => {
  let { body } = ctx.request
  console.log(body)
  console.log(ctx.request)
  ctx.body = {
    ...body
  }
})

app.use(koaBody())
app.use(cors())
//get接口，加上&pretty则返回格式化的接口
app.use(koaJson({ pretty: false, param: 'pretty' }))
app.use(router.routes()).use(router.allowedMethods())

app.listen(3000)
```

TIP

koa-body 中间件的引入顺序必须在 router 之前，否则获取不了 post 请求携带的数据

![深度截图_dde-desktop_20200713194258.png](node.assets/1594640583762-b2eff342-5292-4647-a317-0e2610dfa970.a878e869.png)

params传参：

![深度截图_dde-desktop_20200713194533.png](node.assets/1594640739052-1a94df9f-e94b-4786-a1a9-c654afdc9879.9a0c9074.png)

POST传参：

![深度截图_选择区域_20200713194851.png](node.assets/1594640959915-07c5c8a3-e861-4fe0-ada8-b138a758f72e.fadbeb70.png)

## [#](https://front-end.toimc.com/notes-page/basic/node/01-koa.html#小案例)小案例

**任务描述：**

通过header里面传递一个`role`属性`admin`，使用`post`请求，发送给koa这边的`/api/user`接口json数据为`{name: “imooc”, email: ["imooc@test.com](mailto:"imooc@test.com)"}`。

**具体返回格式与要求如下：**

POSTMan中发送请求

情景一：无name或者email

![//img.mukewang.com/climg/5d5e477f0001e9eb05000237.jpg](node.assets/5d5e477f0001e9eb05540262.e9ebf4af.jpg)**

情景二：Header中无admin或者role不等于admin

![//img.mukewang.com/climg/5d5e47930001d75205000291.jpg](node.assets/5d5e47930001d75205540322.d7528639.jpg)

![//img.mukewang.com/climg/5d5e479d0001869a05000293.jpg](node.assets/5d5e479d0001869a05540324.869af069.jpg)

情景三：正常请求

![//img.mukewang.com/climg/5d5e47ab0001dce005000315.jpg](node.assets/5d5e47ab0001dce005540349.dce03b50.jpg)

**效果图展示如下：**

**![//img.mukewang.com/climg/5d5e47dd0001595a05000289.jpg](node.assets/5d5e47dd0001595a00170018.595a587e.jpg)** **任务要求：**

1. koa侧判断role属性是否存在，是否是admin，不是，则返回status 401
2. 判断email与name属性是否存在，并且不为空字符串
3. 返回用户上传的数据，封装到data对象中，给一个code： 200，message： ‘上传成功’。

package.json:

```json
{
  "name": "koa-homework",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "serve": "node app.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@koa/cors": "^3.1.0",
    "koa": "^2.13.0",
    "koa-body": "^4.2.0",
    "koa-router": "^9.1.0"
  }
}
```

app.js

```js
const Koa = require('koa')
const Router = require('koa-router')
const cors = require('@koa/cors')
const koaBody = require('koa-body')

const app = new Koa()
const router = new Router()

router.prefix('/api')

router.post('/user', ctx => {
  require('./router/api/user')(ctx)
})

app.use(koaBody())
app.use(cors())
app.use(router.routes()).use(router.allowedMethods())
app.listen(3000)
```



# [#](https://front-end.toimc.com/notes-page/basic/node/02-webpack5构建.html#webpack5构建加持)webpack5构建加持

更多webpack相关的内容，可以翻看[webpack的专栏](https://front-end.toimc.com/notes-page/devops/webpack/)。

## [#](https://front-end.toimc.com/notes-page/basic/node/02-webpack5构建.html#项目目标)项目目标

* 支持 es6+语法
* 开发热更新
* webpack5 构建
* 接口搭建
* 路由合并，路由自动注册
* 添加项目规范
* 配置自定义别名

## [#](https://front-end.toimc.com/notes-page/basic/node/02-webpack5构建.html#项目目录结构)项目目录结构

```shell
service
├─ .husky
│   ├─ _
│   │   └─ husky.sh
│   ├─ .gitignore
│   ├─ pre-commit
├─ config
│   ├─ webpack.config.base.js
│   ├─ webpack.config.dev.js
│   └─ webpack.config.prod.js
├─ src
│   ├─ api
│   │  └─ v1
│   │     ├─ demo.js
│   │     └─ test.js
│   ├─ config
│   ├─ controller
│   │  └─ v1
│   │     ├─ demo.js
│   │     └─ test.js
│   ├─ model
│   ├─ routes
│   │   └─ index.js
│   └─ app.js
├─ package-lock.json
├─ package.json
├─ .prettierrc
├─ .babelrc
├─ .editorconfig
├─ .eslintrc.js
└─ .gitignore
```

## [#](https://front-end.toimc.com/notes-page/basic/node/02-webpack5构建.html#搭建项目)搭建项目

```shell
// 创建项目目录
$ mkdir service

// 进入service文件夹
$ cd service

// 初始化package.json
$ npm init -y

// 创建源码目录
mkdir src
```

1. **安装`koa`、`@koa/router` （如果已经配置可路过）**

```shell
$ yarn add koa @koa/router
```

1. **创建入口文件**

```shell
$ touch src/app.js
```

1. **安装构建依赖**

```shell
$ yarn add -D webpack webpack-cli @babel/node @babel/core @babel/preset-env babel-loader clean-webpack-plugin nodemon webpack-node-externals webpack-merge rimraf
```

1. **在项目根目录添加`.babelrc`文件**

```json
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": {
          "node": "current"
        }
      }
    ]
  ]
}
```

1. **添加测试接口**

> 在`app.js`中添加测试接口，由于已经配置了`babel`解析，所以可以直接在`app.js`中写 es6+语法

```javascript
import Koa from 'koa'
import Router from '@koa/router'

const app = new Koa()
const router = new Router()

router.get('/', async ctx => {
  ctx.body = {
    status: 200,
    message: 'success',
    data: {
      nickname: 'Forest',
      title: '前端工程师',
      content: 'webpack5构建node应用'
    }
  }
})

app.use(router.routes()).use(router.allowedMethods())

const port = 3002
app.listen(port, () => console.log(`服务启动在${port}端口`))
```

1. **启动服务**

```shell
$ npx babel-node src/app.js
```

1. **在 postman 中请求接口**

![image-20210504072925109](node.assets/image-20210504072925109.5a1b059f.png)

## [#](https://front-end.toimc.com/notes-page/basic/node/02-webpack5构建.html#配置-webpack)配置 webpack

[英文文档(opens new window)](https://webpack.js.org/concepts/)

[中文文档(opens new window)](https://webpack.docschina.org/concepts/)

核心概念

* **entry**：入口；指示 `webpack` 应该使用哪个模块，默认值是 `./src/index.js`
* **output**：输出；`output` 属性告诉 `webpack` 在哪里输出它所创建的 *bundle*，默认值是 `./dist/main.js`
* **loader**：loader 负责完成项目中各种各样资源模块的加载
* **plugins**：插件；用来解决项目中除了资源模块打包以外的其他自动化工作。包括：打包优化，资源管理，注入环境变量
* **mode**：模式；通过选择 `development`, `production` 或 `none` 之中的一个，来设置 `mode` 参数，你可以启用 webpack 内置在相应环境下的优化。其默认值为 `production`。

> 在项目根目录创建`webpack.config.js`文件

```javascript
const { DefinePlugin } = require('webpack')
const nodeExternals = require('webpack-node-externals')
const { CleanWebpackPlugin } = require('clean-webpack-plugin')

module.exports = {
  // 打包编译为某一端侧的可使用代码  默认值：web  https://webpack.docschina.org/configuration/target/
  target: 'node',

  // 打包模式，可选择值：development、production
  mode: 'development',

  // 控制是否生成，以及如何生成 source map。 https://webpack.docschina.org/configuration/devtool/#root
  devtool: 'eval-cheap-source-map',

  // 打包模块入口文件
  entry: {
    server: `${process.cwd()}/src/app.js`
  },

  // 打包后的输入文件
  output: {
    filename: '[name].bundle.js',
    path: `${process.cwd()}/dist`
  },

  // 匹配解析规则
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        use: {
          loader: 'babel-loader'
        },
        exclude: [`${process.cwd()}/node_modules`]
      }
    ]
  },

  // 构建过程中使用的插件
  plugins: [
    new CleanWebpackPlugin(),
    new DefinePlugin({
      'process.env': {
        NODE_ENV: JSON.stringify(
          process.env.NODE_ENV === 'production' ||
            process.env.NODE_ENV === 'prod'
            ? 'production'
            : 'development'
        )
      }
    })
  ],

  // 防止第三方依赖被打包
  externals: [nodeExternals()]
}
```

### [#](https://front-end.toimc.com/notes-page/basic/node/02-webpack5构建.html#测试构建)测试构建

```shell
$ npx webpack
```

![image-20210504083042708](node.assets/image-20210504083042708.512973a2.png)![image-20210504083112577](data:image/png;base64,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)

> 构建成功！

思考

在实际开发中可能会存在开发环境和生产环境的构建，所以单凭一个配置还不能达到实际的需求，接下来对开发环境和生产环境分别配置。

> 在项目根目录创建 config 文件，并创建三个文件分别是`webpack.config.base.js`、`webpack.config.dev.js`、`webpack.config.prod.js`
>
> * `webpack.config.base.js` 文件存放开发环境和生产环境都是需要的构建配置
> * `webpack.config.dev.js` 文件存放开发环境的构建配置
> * `webpack.config.prod.js` 存放生产环境的构建配置

### [#](https://front-end.toimc.com/notes-page/basic/node/02-webpack5构建.html#优化构建配置)优化构建配置

> * mode 独立于构建环境，开发环境为(`development`)、生产环境为(`production`)
> * devtool 只有在开发环境下才会存在
> * [stats (opens new window)](https://webpack.docschina.org/configuration/stats/)属性让你更精确地控制打包后的信息该怎么显示

TIP

由于每个开发环境和生产环境都是独立的构建配置，所以要在构建时要合并基础配置；安装`webpack-merge`合并构建配置

```shell
$ npm i -D webpack-merge
```

* 优化 webpack.config.base.js

```javascript
// config/webpack.config.base.js
const { DefinePlugin } = require('webpack')
const nodeExternals = require('webpack-node-externals')
const { CleanWebpackPlugin } = require('clean-webpack-plugin')

module.exports = {
  // 打包编译为某一端侧的可使用代码  默认值：web  https://webpack.docschina.org/configuration/target/
  target: 'node',

  // 打包模式，可选择值：development、production
  // mode: "development",

  // 控制是否生成，以及如何生成 source map。 https://webpack.docschina.org/configuration/devtool/#root
  // devtool: "eval-cheap-source-map",

  // 打包模块入口文件
  entry: {
    server: `${process.cwd()}/src/app.js`
  },

  // 打包后的输入文件
  output: {
    filename: '[name].bundle.js',
    path: `${process.cwd()}/dist`
  },

  // 匹配解析规则
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        use: {
          loader: 'babel-loader'
        },
        exclude: [`${process.cwd()}/node_modules`]
      }
    ]
  },

  // 构建过程中使用的插件
  plugins: [
    new CleanWebpackPlugin(),
    new DefinePlugin({
      'process.env': {
        // 设置环境变量 NODE_ENV
        NODE_ENV: JSON.stringify(
          process.env.NODE_ENV === 'production' ||
            process.env.NODE_ENV === 'prod'
            ? 'production'
            : 'development'
        )
      }
    })
  ],

  // 防止第三方依赖被打包
  externals: [nodeExternals()]
}
```

* 开发环境的构建配置

```javascript
// config/webpack.config.dev.js
const { merge } = require('webpack-merge')
const baseWebpackConfig = require('./webpack.config.base')

const webpackConfig = merge(baseWebpackConfig, {
  devtool: 'eval-cheap-source-map',
  mode: 'development',

  // 是否添加关于子模块的信息。
  stats: { children: false }
})

module.exports = webpackConfig
```

* 生产环境的构建配置

> 生产环境构建时要进行代码压缩，安装`terser-webpack-plugin`， 命令：`npm i -D terser-webpack-plugin`

```javascript
// config/webpack.config.prod.js
const { merge } = require('webpack-merge')
const TersetWebpackPlugin = require('terser-webpack-plugin')
const baseWebpackConfig = require('./webpack.config.base')

const webpackConfig = merge(baseWebpackConfig, {
  devtool: 'eval-cheap-source-map',
  mode: 'production',
  stats: { children: false },

  // 优化配置
  optimization: {
    // 压缩配置
    minimize: true,
    minimizer: [new TersetWebpackPlugin()],

    // 分块策略
    splitChunks: {
      // 缓存组 https://webpack.docschina.org/plugins/split-chunks-plugin/#splitchunkscachegroups
      cacheGroups: {
        commens: {
          name: 'commons',
          chunks: 'initial',
          minChunks: 3,
          enforce: true
        }
      }
    }
  }
})

module.exports = webpackConfig
```

* 添加构建脚本命令

  > 设置环境变量`NODE_ENV`，由于各环境配置的差异问题，`cross-env`可以有效的解决跨平台设置环境变量的问题；它是运行跨平台设置和使用环境变量(Node 中的环境变量)的脚本。安装命令：`npm i -D cross-env` 安装成功后配置构建命令:
  >
  > * 在`package.json`的`scripts`中添加如下命令：
  >
  > ```json
  > "build": "cross-env NODE_ENV=prod webpack --config config/webpack.config.prod.js",
  > "dev": "cross-env NODE_ENV=dev nodemon --exec babel-node --inspect src/app.js",
  > ```

* 启动开发环境服务

```shell
$ npm run dev
```

运行之后的效果图如下：

![image-20210504095318351](node.assets/image-20210504095318351.4860df63.png)

* 启动编译构建命令

```shell
$ npm run build
```

运行效果如下图：

![image-20210504095540666](node.assets/image-20210504095540666.8b5ae3f8.png)

查看 dist 文件夹下被编译后的文件：

![image-20210504095736714](node.assets/image-20210504095736714.e22b4743.png)

> 被压缩成了一整行！

## [#](https://front-end.toimc.com/notes-page/basic/node/02-webpack5构建.html#路由自动注册)路由自动注册

### [#](https://front-end.toimc.com/notes-page/basic/node/02-webpack5构建.html#使用require-directory)使用require-directory

> 在 src 文件夹下新建 routes 和 api 两文件夹；routes 是集成当前项目的所有路由，api 文件是存放项目的所有接口文件。

* 安装 [`require-directory` (opens new window)](https://github.com/troygoode/node-require-directory/)，这个包的作用可以将一个目录下的所有模块文件

  ```shell
  $ npm i require-dirctory
  ```

* 创建`src/api/v1`下创建`demo.js`和`test.js`文件

  ```js
  // src/api/v1/demo.js
  import Router from '@koa/router'
  
  const router = new Router({ prefix: '/api/v1' })
  
  router.get('/demo', async ctx => {
    ctx.body = {
      status: 200,
      message: 'message',
      data: {
        file: 'demo.js',
        title: 'webpack 5 构建node应用',
        content: 'koa + @koa/router + require-dirctory'
      }
    }
  })
  
  export default router
  ```

  ```js
  //  src/api/v1/test.js
  import Router from '@koa/router'
  
  const router = new Router({ prefix: '/api/v1' })
  
  router.get('/test', async ctx => {
    ctx.body = {
      status: 200,
      message: 'message',
      data: {
        file: 'test.js',
        title: 'webpack 5 构建node应用',
        content: 'koa + @koa/router + require-dirctory'
      }
    }
  })
  
  export default router
  ```

* 配置`src/routes/index.js`

  ```js
  import Router from '@koa/router'
  import requireDirectory from 'require-directory'
  
  // 接口存放目录路径
  const apiDirectory = `${process.cwd()}/src/api`
  
  function initLoadRoutes(app) {
    requireDirectory(module, apiDirectory, {
      visit({ default: router }) {
        if (router instanceof Router) {
          app.use(router.routes())
        }
      }
    })
  }
  
  export default initLoadRoutes
  ```

* 修改`src/app.js`文件

  ```javascript
  import Koa from 'koa'
  import initLoadRoutes from './routes/index'
  
  const app = new Koa()
  
  // 在入口文件中执行
  initLoadRoutes(app)
  
  const port = 3002
  app.listen(port, () => console.log(`服务启动在${port}端口`))
  ```

* 在 postman 中测试请求如下图

  ![image-20210504125524850](node.assets/image-20210504125524850.b5fb774a.png)

  ![image-20210504125437662](node.assets/image-20210504125437662.75866bf7.png)

  到此自动注册路由就大功告成了，后面我们定义接口的时候就用手动一个一个的引入，只管往 api 文件夹里写接口就好了。

### [#](https://front-end.toimc.com/notes-page/basic/node/02-webpack5构建.html#使用require-context-webpack-功能)使用require.context（webpack）功能

官方文档：[require(opens new window)](https://webpack.docschina.org/guides/dependency-management/)

举例：

```text
.
├── modules
│   ├── adminRouter.js
│   ├── commentsRouter.js
│   ├── contentRouter.js
│   ├── loginRouter.js
│   ├── publicRouter.js
│   ├── userRouter.js
│   └── wxRouter.js
└── routes.js
```

目标：使用`routes.js`来动态加载`modules`目录中的`.js`的路由文件，其他的比如：`vuex`、`vue-router`等场景，都适合。

先上实现出来的代码：

`routes.js`文件

```js
import combineRoutes from 'koa-combine-routers'

// 加载目录中的Router中间件
const moduleFiles = require.context('./modules', true, /\.js$/)

// reduce方法去拼接 koa-combine-router所需的数据结构 Object[]
const modules = moduleFiles.keys().reduce((items, path) => {
  const value = moduleFiles(path)
  items.push(value.default)
  return items
}, [])

export default combineRoutes(modules)
```

使用方法，在`index.js`入口文件中：

```js
import router from './routes/routes'


app.use(router())
```

这里有两个知识点：

1. 使用`koa-combine-routers`可以合并多个路由

2. 使用`require.context`可以动态引入多个文件

   ![image-20210612192129199](node.assets/image-20210612192129199.30075345.png)

   说明：

   * require.context返回的是一个函数

   * 这个函数的键值，正是文件

     ```text
     moduleFiles.keys()
     (14) ['./adminRouter.js', './commentsRouter.js', './contentRouter.js', './loginRouter.js', './publicRouter.js', './userRouter.js', './wxRouter.js', 'routes/modules/adminRouter.js', 'routes/modules/commentsRouter.js', 'routes/modules/contentRouter.js', 'routes/modules/loginRouter.js', 'routes/modules/publicRouter.js', 'routes/modules/userRouter.js', 'routes/modules/wxRouter.js']
     ```

   * 这个函数接收文件名后，可以返回文件的内容，这个内容正好匹配路由，输出一个数组，传递给conbineRoutes方法，即可合并。

     ```text
     const value = moduleFiles(path)
     ```

     ![image-20210612192350748](node.assets/image-20210612192350748.1bc30e21.png)

## [#](https://front-end.toimc.com/notes-page/basic/node/02-webpack5构建.html#配置别名)配置别名

在日常开发中我们引入一些封装好的方法或者模块总是写很长很长的文件路径；比如：`require('../../../../some/very/deep/module')`、`import format from '../../../../utils/format'`，为了告别这种又臭又长的路径我们就可以使用一些解放生产力的方法了（哈哈哈哈，不会偷懒的程序员不是好程序员 🤭）

配置别名有两种方式，一种是 webpack，另一种是通过[`module-alias` (opens new window)](https://www.npmjs.com/package/module-alias)包

### [#](https://front-end.toimc.com/notes-page/basic/node/02-webpack5构建.html#使用webpack的别名功能)使用webpack的别名功能

官方文档： [resolve.alias(opens new window)](https://webpack.docschina.org/configuration/resolve/)

配置方式，非常的简单方便：

```text
const path = require('path');

module.exports = {
  //...
  resolve: {
    alias: {
      '@': path.resolve(__dirname, 'src/'),
      // ...
    },
  },
};
```

### [#](https://front-end.toimc.com/notes-page/basic/node/02-webpack5构建.html#使用module-alias)使用module-alias

* 安装依赖

  ```shell
  npm i module-alias
  ```

* 在`package.json`中添加自定义别名

  ```json
  "_moduleAliases": {
      "@": "./src",
      "@controller": "./src/controller"
  }
  ```

  ![image-20210504141154503](node.assets/image-20210504141154503.7e1ea32c.png)

* 在入口文件的顶部引入`module-alias/register`，也就是在`app.js`的顶部引入

  ```javascript
  require('module-alias/register')
  ```

  ![image-20210504142411612](node.assets/image-20210504142411612.3ddee265.png)

> 配置成功后，将`/src/api/v1`内的逻辑全部提到`src/controller`中，使用别名引入`controller`中文件，修改后如下：

```javascript
// src/api/v1/demo.js
import Router from '@koa/router'
import DemoController from '@controller/demo/'

const router = new Router({ prefix: '/api/v1' })

router.get('/demo', DemoController.demo)

export default router
// src/api/v1/test.js
import Router from '@koa/router'
import TestController from '@controller/test'

const router = new Router({ prefix: '/api/v1' })

router.get('/test', TestController.test)

export default router
// src/controller/v1/demo.js
class DemoController {
  constructor() {}

  async demo(ctx) {
    ctx.body = {
      status: 200,
      message: 'message',
      data: {
        file: 'test.js',
        title: 'webpack 5 构建node应用',
        content: 'koa + @koa/router + require-dirctory'
      }
    }
  }
}

export default new DemoController()
// src/controller/v1/test.js
class TestController {
  constructor() {}

  async test(ctx) {
    ctx.body = {
      status: 200,
      message: 'message',
      data: {
        file: 'test.js',
        title: 'webpack 5 构建node应用',
        content: 'koa + @koa/router + require-dirctory'
      }
    }
  }
}

export default new TestController()
```

* postman 中测试接口

  ![image-20210504143159248](node.assets/image-20210504143159248.375ecc14.png)

  ![image-20210504143225351](node.assets/image-20210504143225351.f93228ef.png)

commit 时 lint-staged 没有通过：

![image-20210504152231237](node.assets/image-20210504152231237.333b1077.png)

> 上述问题是 eslint 发现`@controller/*`开头的在 node_modules 中没有找到，所以配置 eslint 就好了：
>
> ```js
> // src/eslintrc.js
> module.exports = {
>   //...
>   rules: {
>     'import/no-unresolved': [2, { ignore: ['^@/', '@controller'] }] // @和@controller 是设置的路径别名
>   }
> }
> ```

![image-20210504154049823](data:image/png;base64,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)

> 这个问题是由于`constructor`构造函数为空引起的，在`eslintrc.js`添加配置即可：'no-empty-function': ['error', { allow: ['constructors'] }]

完整代码见：[GitHub(opens new window)](https://github.com/big-front-end/webpack5-node)



# [#](https://front-end.toimc.com/notes-page/basic/node/03-项目规范及工具.html#项目规范及工具)项目规范及工具

## [#](https://front-end.toimc.com/notes-page/basic/node/03-项目规范及工具.html#集成-editorconfig)集成 EditorConfig

> **[EditorConfig (opens new window)](https://editorconfig.org/)**有助于为不同 IDE 编辑器上处理同一项目的多个开发人员维护一致的编码风格。

在项目根目录下增加 `.editorconfig` 文件， 并配置以下内容：

```yaml
# Editor configuration, see http://editorconfig.org

# 表示是最顶层的 EditorConfig 配置文件
root = true

# 表示所有文件适用
[*]

# 设置文件字符集为 utf-8
charset = utf-8

# 缩进风格（tab | space）
indent_style = space

# 缩进大小
indent_size = 4

# 控制换行类型(lf | cr | crlf)
end_of_line = lf

# 去除行首的任意空白字符
trim_trailing_whitespace = true

# 始终在文件末尾插入一个新行
insert_final_newline = true

# md 文件适用以下规则
[*.md]
max_line_length = off
trim_trailing_whitespace = false
```

注意

VSCode 使用 EditorConfig 需要去插件市场下载插件 `EditorConfig for VS Code` 。WebStorm 则不需要安装，直接使用 EditorConfig 配置即可。

![image-20210504101228402](node.assets/image-20210504101228402.5e7543f7.png)

## [#](https://front-end.toimc.com/notes-page/basic/node/03-项目规范及工具.html#集成-prettier)集成 Prettier

> **[Prettier (opens new window)](https://prettier.io/)**是一款强大的代码格式化工具，支持 `JavaScript、TypeScript、CSS、SCSS、Less、JSX、Angular、Vue、GraphQL、JSON、Markdown` 等语言，基本上前端能用到的文件格式它都可以搞定，是当下最流行的代码格式化工具。

* 安装 Prettier

```shell
$ npm i prettier -D
```

* 创建 Prettier 配置文件 Prettier 支持多种格式的配置文件，比如 `.json`、`.yml`、`.yaml`、`.js` 等。 在本项目根目录下创建 `.prettierrc` 文件。
* 配置 `.prettierrc` 在本项目中，进行如下简单配置，关于更多配置项信息，请前往官网查看 [Prettier-Options (opens new window)](https://prettier.io/docs/en/options.html)。

```json
{
  "useTabs": false,
  "tabWidth": 4,
  "printWidth": 100,
  "singleQuote": true,
  "trailingComma": "none",
  "bracketSpacing": true,
  "semi": false
}
```

Prettier 安装且配置好之后，就能使用命令来格式化代码

* 格式化所有文件（. 表示所有文件）

```shell
$ npx prettier --write .
```

注意

VSCode 编辑器使用 `Prettier` 配置需要下载插件 `Prettier - Code formatter`； WebStorm 则不需要安装，直接使用 EditorConfig 配置即可。

![image-20210504102416728](node.assets/image-20210504102416728.3dbda827.png)

## [#](https://front-end.toimc.com/notes-page/basic/node/03-项目规范及工具.html#集成-eslint)集成 ESLint

[ESLint (opens new window)](https://eslint.org/)是一款用于查找并报告代码中问题的工具，并且支持部分问题自动修复。其核心是通过对代码解析得到的 `AST`（Abstract Syntax Tree 抽象语法树）进行模式匹配，来分析代码达到检查代码质量和风格问题的能力。 使用 `ESLint` 可以尽可能的避免团队成员之间编程能力和编码习惯不同所造成的代码质量问题，一边写代码一边查找问题，如果发现错误，就给出规则提示，并且自动修复，长期下去，可以促使团队成员往同一种编码风格靠拢。

* 安装 eslint

```shell
$ npm i -D eslint
```

* 配置 ESLint

  > ESLint 安装成功后，执行 `npx eslint --init`，然后按照终端操作提示完成一系列设置来创建配置文件。

![image-20210504103213588](node.assets/image-20210504103213588.d63b5ec3.png)

* How would you like to use ESLint? ...(你想如何使用 ESLint?…)

  > 我这里选择第三个，检查语法，发现问题，并强制代码样式

![image-20210504103418834](node.assets/image-20210504103418834.acabf1c4.png)

* What type of modules does your project use? ... （你的项目使用什么类型的模块?…）

  > 项目支持 es6+语法，所以这里就直接选用第一项：JavaScript modules (import/export)

* Which framework does your project use? ... （你的项目使用哪种框架?…）

  > 这里并未使用 vue 和 react，所以选择 none of these

![image-20210504104452973](node.assets/image-20210504104452973.81e4374c.png)

* Does your project use TypeScript? (你的项目使用 TypeScript 吗?)

  > 项目中并没有使用 Typescript，所以选择 No

![image-20210504104610444](node.assets/image-20210504104610444.bc5623d2.png)

* Where does your code run?(你的代码在哪里运行?)

  > 这是 node 项目，所以不需要选择浏览器环境

![image-20210504105031201](node.assets/image-20210504105031201.4dd08695.png)

* How would you like to define a style for your project? ... (你想怎样为你的项目定义风格？)

  > 这里选择 Use a popular style guide（使用一种流行的风格指南）

![image-20210504105437959](node.assets/image-20210504105437959.f4a9a07c.png)

* Which style guide do you want to follow? ... (你想遵循哪种风格指南?…)

  ![image-20210504105647664](node.assets/image-20210504105647664.46c543a7.png)

* What format do you want your config file to be in? ... (您希望配置文件的格式是什么?…)

  > 我这里选择 JavaScript

* Would you like to install them now with npm?（你想现在用 npm 安装它们吗?）

  > 默认 Yes，所以可以直接回车

  ![image-20210504110100677](node.assets/image-20210504110100677.15a7a58d.png)

* 所有配置如下

  ![image-20210504110235680](node.assets/image-20210504110235680.d03c16f5.png)

安装成功后，项目的根目录就会多一个`.eslintrc.js`文件，其中的内容就是在终端中选择的相应配置。

注意

VSCode 使用 ESLint 配置文件需要去插件市场下载插件 ESLint 。 ![image-20210504111438312](node.assets/image-20210504111438312.727bc6a5.png)

### [#](https://front-end.toimc.com/notes-page/basic/node/03-项目规范及工具.html#webpack-中使用-eslint-不推荐)Webpack 中使用 ESLint(不推荐)

首先需要安装`eslint-loader`：

```bash
npm install -D eslint-loader
```

然后在 webpack.config.js 中`module.rules`添加如下代码：

```js
{
    test: /\.js$/,
    loader: 'eslint-loader',
    enforce: 'pre',
    include: [path.resolve(__dirname, 'src')], // 指定检查的目录
    options: { // 这里的配置项参数将会被传递到 eslint 的 CLIEngine
        formatter: require('eslint-friendly-formatter') // 指定错误报告的格式规范
    }
}
```

1. ESLint 的配置单独做了一个 Webpack 的`module.rule`配置，所以使用了`enforce: 'pre'`来调整了 loader 加载顺序，保证先检测代码风格，之后再做 Babel 转换等工作；
2. 也可以放到 Babel 放到一起，不过要将 `eslint-loader`放到`babel-loader` 之前检测；
3. 这里为了让 ESLint 报错更加好看一些，使用了[eslint-formatter-friendly (opens new window)](https://github.com/royriojas/eslint-friendly-formatter)这个 ESLint`formatter`，记得安装它：`npm i -D eslint-formatter-friendly`

新建一个 entry 文件，内容如下：

```js
import _ from 'lodash';
const a = 1;
const b = 2;
alert(b);
console.log(b);
```

不推荐理由：使用lint-stage 结合 husky就可以做eslint的代码检查与优化了

### [#](https://front-end.toimc.com/notes-page/basic/node/03-项目规范及工具.html#webpack-中使用-stylelint-不推荐)Webpack 中使用 StyleLint(不推荐)

Webpack 中使用 StyleLint 是通过插件的方式来使用，这个插件的名字是 [stylelint-webpack-plugin (opens new window)](https://www.npmjs.com/package/stylelint-webpack-plugin)。

```bash
npm install -D stylelint-webpack-plugin
```

安装之后，按照插件的使用方式在 webpack.config.js 添加配置：

```js
const StyleLintPlugin = require('stylelint-webpack-plugin');

module.exports = {
    // ...
    plugins: [new StyleLintPlugin(options)]
    // ...
};
```

默认 StyleLint-webpack-plugin 会查找项目中的 StyleLint 配置文件，根据配置文件的配置来检测 CSS 代码。

在 stylelint-webpack-plugin 插件中有两个跟 Webpack 编译相关的配置项：

* `emitErrors`：默认是`true`，将遇见的错误信息发送给 webpack 的编辑器处理；
* `failOnError`：默认是`false`，如果是 `true`遇见 StyleLint 报错则终止 Webpack 编译。

## [#](https://front-end.toimc.com/notes-page/basic/node/03-项目规范及工具.html#解决-prettier-和-eslint-的冲突)解决 Prettier 和 ESLint 的冲突

本项目中的 ESLint 配置中使用了 `Airbnb JavaScript` 风格指南校验，其规则之一是代码结束后面要加分号，而在 Prettier 配置文件中加了代码结束后面不加分号的配置项，这样就有冲突了，会出现用 Prettier 格式化后的代码，ESLint 检测到格式有问题的，从而抛出错误提示。 解决两者冲突问题，需要用到 `eslint-plugin-prettier` 和 `eslint-config-prettier`。

> `eslint-plugin-prettier` 将 Prettier 的规则设置到 ESLint 的规则中。

> `eslint-config-prettier` 关闭 ESLint 中与 Prettier 中会发生冲突的规则。

最后形成优先级：Prettier 配置规则 > ESLint 配置规则。

* 安装插件

```shell
$ npm i eslint-plugin-prettier eslint-config-prettier -D
```

* 在 `.eslintrc.js` 添加 prettier 插件

```javascript
module.exports = {
    ...
    extends: [
        'airbnb-base',
        'plugin:prettier/recommended' // 添加 prettier 插件
    ],
    ...
}
```

这样，在执行 `eslint --fix` 命令时，ESLint 就会按照 Prettier 的配置规则来格式化代码，轻松解决二者冲突问题。

## [#](https://front-end.toimc.com/notes-page/basic/node/03-项目规范及工具.html#集成-husky-和-lint-staged)集成 husky 和 lint-staged

在项目中已集成 `ESLint` 和 `Prettier`，在编码时，这些工具可以对写的代码进行实时校验，在一定程度上能有效规范写的代码，但团队可能会有些人觉得这些条条框框的限制很麻烦，选择视“提示”而不见，依旧按自己的一套风格来写代码，或者干脆禁用掉这些工具，开发完成就直接把代码提交到了仓库，日积月累，`ESLint` 也就形同虚设。 所以，还需要做一些限制，让没通过 `ESLint` 检测和修复的代码禁止提交，从而保证仓库代码都是符合规范的。 为了解决这个问题，需要用到 `Git Hook`，在本地执行 `git commit` 的时候，就对所提交的代码进行 `ESLint` 检测和修复（即执行 `eslint --fix`），如果这些代码没通过 `ESLint` 规则校验，则禁止提交。 实现这一功能，借助 `husky + lint-staged` 。

> husky —— Git Hook 工具，可以设置在 git 各个阶段（pre-commit、commit-msg、pre-push 等）触发的命令。 lint-staged —— 在 git 暂存的文件上运行 linters。

### [#](https://front-end.toimc.com/notes-page/basic/node/03-项目规范及工具.html#配置-husky)配置 husky

TIP

使用 `husky-init` 命令快速在项目初始化一个 `husky` 配置。在配置 `husky` 之前必须初始化 `git`，否则可能会配置不成功

```shell
$ npx husky-init && npm install
```

命令执行会经历以下四步流程：

* 安装`husky`为开发依赖

  ![image-20210504120333648](node.assets/image-20210504120333648.39036f64.png)

* 创建`.husky`文件夹

  ![image-20210504120510820](node.assets/image-20210504120510820.7751bb3c.png)

* 在 `.husky` 目录创建 `pre-commit` hook，并初始化 `pre-commit` 命令为 `npm test`

  ![image-20210504120612932](node.assets/image-20210504120612932.25edc2fb.png)

* 修改 `package.json` 的 `scripts`，增加 `"prepare": "husky install"`

  ![image-20210504120708693](node.assets/image-20210504120708693.fd343f7b.png)

### [#](https://front-end.toimc.com/notes-page/basic/node/03-项目规范及工具.html#配置-lint-staged)配置 lint-staged

lint-staged 这个工具一般结合 husky 来使用，它可以让 husky 的 `hook` 触发的命令只作用于 `git add`那些文件（即 git 暂存区的文件），而不会影响到其他文件。

接下来，使用 lint-staged 继续优化项目。

* 安装 lint-staged

  ```shell
  $ npm i lint-staged -D
  ```

* 在 `package.json`里增加 lint-staged 配置项

  ```json
  "lint-staged": {
    "*.{vue,js,ts}": "eslint --fix"
  },
  ```

  ![image-20210504121302298](node.assets/image-20210504121302298.97f6e65c.png)

* 修改 `.husky/pre-commit hook` 的触发命令为：`npx lint-staged`

  ![image-20210504121450870](node.assets/image-20210504121450870.40fc550a.png)

至此，husky 和 lint-staged 组合配置完成。

## [#](https://front-end.toimc.com/notes-page/basic/node/03-项目规范及工具.html#集成stylelint)集成stylelint

检测 CSS 语法使用[StyleLint (opens new window)](https://stylelint.io/)。

StyleLint 和 ESLint 很像，它们都只是提供了工具与规则，如何配置这些规则完全取决于使用者，所以要根据需要自己引入或配置规则。StyleLint 的代码风格也有很多社区开源版本，官方推荐的代码风格有两个：

* [stylelint-config-recommended(opens new window)](https://github.com/stylelint/stylelint-config-recommended)
* [stylelint-config-standard(opens new window)](https://github.com/stylelint/stylelint-config-standard)

要使用 StyleLint 需要先安装它：

```bash
npm install -D stylelint
```

> 除了 StyleLint 本身之外，还可以安装[stylelint-order 插件 (opens new window)](https://github.com/hudochenkov/stylelint-order)，该插件的作用是强制在写 CSS 的时候按照某个顺序来编写。
>
> 例如先写定位，再写盒模型，再写内容区样式，最后写 CSS3 相关属性。这样可以极大的保证代码的可读性和风格统一。

StyleLint 的配置文件是`.stylelintrc.json`，其中的写法跟 ESLint 的配置类似，都是包含`extend`和`rules`等内容，下面是一个示例：

```json
{
    "extends": ["stylelint-config-standard", "stylelint-config-recess-order"],
    "rules": {
        "at-rule-no-unknown": [true, {"ignoreAtRules": ["mixin", "extend", "content"]}]
    }
}
```

配置文件中单独配置 `at-rule-no-unknown` 是为了让 StyleLint 支持 SCSS 语法中的 `mixin`、`extend`、`content` 语法，更多详细的规则，可以查看[官方文档 (opens new window)](https://stylelint.io/user-guide/rules/)。

如何在webpack中使用stylelint，可以参考专栏文章：[资源处理](https://front-end.toimc.com/notes-page/devops/webpack/06-资源处理.html#扩展3-webpack-中使用-stylelint)



# [#](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#正则表达式入门)正则表达式入门

## [#](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#目录)目录



* [目录](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#目录)
* 入门
  * [正则表达式是什么？](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#正则表达式是什么)
  * [如何学习正则表达式？](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#如何学习正则表达式)
  * [测试正则表达式](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#测试正则表达式)
* 核心概念
  * [元字符](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#元字符)
  * [字符转义](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#字符转义)
  * [重复](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#重复)
  * [字符类](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#字符类)
  * [分枝条件](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#分枝条件)
  * [分组](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#分组)
  * [反义](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#反义)
  * [后向引用](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#后向引用)
  * [零宽断言](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#零宽断言)
  * [负向零宽断言](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#负向零宽断言)
  * [注释](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#注释)
  * [贪婪与懒惰](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#贪婪与懒惰)
  * [处理选项](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#处理选项)
  * [平衡组/递归匹配](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#平衡组-递归匹配)
* [扩展知识](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#扩展知识)
* [参考文献](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#参考文献)



## [#](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#入门)入门

除了作为入门教程之外，本文还试图成为可以在日常工作中使用的正则表达式语法参考手册。

本文介绍的大部分正则语法，在不同的正则表达式引擎中都可以使用，但也有一些会有所差异。

### [#](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#正则表达式是什么)正则表达式是什么？

在编写处理字符串的程序或网页时，经常会有查找符合某些复杂规则的字符串的需要。**正则表达式**就是用于描述这些规则的工具。换句话说，正则表达式就是记录文本规则的代码。

很可能你使用过Windows/Dos下用于文件查找的**通配符(wildcard)**，也就是*和?。如果你想查找某个目录下的所有的Word文档的话，你会搜索*.doc。在这里，*会被解释成任意的字符串。和通配符类似，正则表达式也是用来进行文本匹配的工具，只不过比起通配符，它能更精确地描述你的需求——当然，代价就是更复杂——比如你可以编写一个正则表达式，用来查找所有以0开头，后面跟着2-3个数字，然后是一个连字号“-”，最后是7或8位数字的字符串(像*010-12345678*或*0376-7654321*)。

**字符**是计算机软件处理文字时最基本的单位，可能是字母，数字，标点符号，空格，换行符，汉字等等。**字符串**是0个或更多个字符的序列。**文本**也就是文字，字符串。说某个字符串**匹配**某个正则表达式，通常是指这个字符串里有一部分（或几部分分别）能满足表达式给出的条件。

### [#](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#如何学习正则表达式)如何学习正则表达式？

学习正则表达式的最好方法是从例子开始，理解例子之后再自己对例子进行修改，实验。

下面给出了不少简单的例子，并对它们作了详细的说明。

假设你在一篇英文小说里查找hi，你可以使用正则表达式`hi`。

这几乎是最简单的正则表达式了，它可以精确匹配这样的字符串：由两个字符组成，前一个字符是h，后一个是i。通常，处理正则表达式的工具会提供一个忽略大小写的选项，如果选中了这个选项，它可以匹配hi,HI,Hi,hI这四种情况中的任意一种。

不幸的是，很多单词里包含*hi*这两个连续的字符，比如*him*,*history*,*high*等等。用`hi`来查找的话，这里边的*hi*也会被找出来。如果要精确地查找hi这个单词的话，我们应该使用`\bhi\b`。

`\b`是正则表达式规定的一个特殊代码（好吧，某些人叫它**元字符，metacharacter**），代表着单词的开头或结尾，也就是单词的分界处。虽然通常英文的单词是由空格，标点符号或者换行来分隔的，但是\b并不匹配这些单词分隔字符中的任何一个，它**只匹配一个位置**。

> 如果需要更精确的说法，`\b`匹配这样的位置：它的前一个字符和后一个字符不全是(一个是`,`一个不是或不存在)`\w`。

假如你要找的是hi后面不远处跟着一个Lucy，你应该用`\bhi\b.*\bLucy\b`。

这里，.是另一个元字符，匹配除了换行符以外的任意字符。*同样是元字符，不过它代表的不是字符，也不是位置，而是数量——它指定*前边的内容可以连续重复使用任意次以使整个表达式得到匹配。因此，.*连在一起就意味着任意数量的不包含换行的字符。现在`\bhi\b.*\bLucy\b`的意思就很明显了：先是一个单词hi，然后是任意个任意字符(但不能是换行)，最后是Lucy这个单词。

> 换行符就是'\n'，ASCII编码为10(十六进制0x0A)的字符。

如果同时使用其它元字符，我们就能构造出功能更强大的正则表达式。

比如下面这个例子：

`0\d\d-\d\d\d\d\d\d\d\d`匹配这样的字符串：以0开头，然后是两个数字，然后是一个连字号“-”，最后是8个数字(也就是中国的电话号码。当然，这个例子只能匹配区号为3位的情形)。

这里的`\d`是个新的元字符，匹配一位数字(0，或1，或2，或……)。-不是元字符，只匹配它本身——连字符(或者减号，或者中横线，或者随你怎么称呼它)。

为了避免那么多烦人的重复，我们也可以这样写这个表达式`：0\d{2}-\d{8}`。这里`\d`后面的`{2}`(`{8}`)的意思是前面`\d`必须连续重复匹配2次(8次)。

### [#](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#测试正则表达式)测试正则表达式

如果你不觉得正则表达式很难读写的话，要么你是一个天才，要么，你不是地球人。

正则表达式的语法很令人头疼，即使对经常使用它的人来说也是如此。

不同的环境下正则表达式的一些细节是不相同的，请参考[该页面 (opens new window)](https://tool.oschina.net/regex/)的说明来测试正则。

## [#](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#核心概念)核心概念

### [#](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#元字符)元字符

现在你已经知道几个很有用的元字符了，如`\b,.,*`，还有`\d`.正则表达式里还有更多的元字符，比如`\s`匹配任意的空白符，包括空格，制表符(Tab)，换行符，中文全角空格等。`\w`匹配字母或数字或下划线或汉字等。

> 对中文/汉字的特殊处理是由.Net提供的正则表达式引擎支持的，其它环境下的具体情况请查看相关文档。

下面来看看更多的例子：

\ba\w*\b匹配以字母a开头的单词——先是某个单词开始处(\b)，然后是字母a，然后是任意数量的字母或数字(\w*)，最后是单词结束处(\b)。

\d+匹配1个或更多连续的数字。这里的+是和*类似的元字符，不同的是*匹配重复任意次(可能是0次)，而+则匹配重复1次或更多次。

\b\w{6}\b 匹配刚好6个字符的单词。

> 好吧，现在我们说说正则表达式里的单词是什么意思吧：就是不少于一个的连续的`\w`。不错，这与学习英文时要背的成千上万个同名的东西的确关系不大 😃

| 代码 | 说明                         |
| ---- | ---------------------------- |
| `.`  | 匹配除换行符以外的任意字符   |
| `\w` | 匹配字母或数字或下划线或汉字 |
| `\s` | 匹配任意的空白符             |
| `\d` | 匹配数字                     |
| `\b` | 匹配单词的开始或结束         |
| `^`  | 匹配字符串的开始             |
| `$`  | 匹配字符串的结束             |

元字符`^`（和数字6在同一个键位上的符号）和`$`都匹配一个位置，这和`\b`有点类似。^匹配你要用来查找的字符串的开头，`$`匹配结尾。

> 这两个代码在验证输入的内容时非常有用，比如一个网站如果要求你填写的QQ号必须为5位到12位数字时，可以使用：`^\d{5,12}$`。

这里的{5,12}和前面介绍过的{2}是类似的，只不过{2}匹配只能不多不少重复2次，{5,12}则是重复的次数不能少于5次，不能多于12次，否则都不匹配。

因为使用了`^`和`$`，所以输入的整个字符串都要用来和`\d{5,12}`来匹配，也就是说整个输入必须是5到12个数字，因此如果输入的QQ号能匹配这个正则表达式的话，那就符合要求了。

和忽略大小写的选项类似，有些正则表达式处理工具还有一个处理多行的选项。如果选中了这个选项，`^`和`$`的意义就变成了匹配行的开始处和结束处。

正则表达式引擎通常会提供一个“测试指定的字符串是否匹配一个正则表达式”的方法，如JavaScript里的`RegExp.test()`方法或.NET里的`Regex.IsMatch()`方法。这里的匹配是指是字符串里有没有符合表达式规则的部分。如果不使用`^`和`$`的话，对于\d{5,12}而言，使用这样的方法就只能保证字符串里包含5到12连续位数字，而不是整个字符串就是5到12位数字。

### [#](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#字符转义)字符转义

如果你想查找元字符本身的话，比如你查找`.,`或者`*,`就出现了问题：你没办法指定它们，因为它们会被解释成别的意思。这时你就得使用\来取消这些字符的特殊意义。因此，你应该使用`\.`和`\*`。当然，要查找\本身，你也得用\.

例如：`toimc\.com`匹配toimc.com，`C:\\Windows`匹配C:\Windows。

### [#](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#重复)重复

你已经看过了前面的`*,+,{2},{5,12}`这几个匹配重复的方式了。下面是正则表达式中所有的限定符(指定数量的代码，例如`*,{5,12}`等)：

| 代码/语法 | 说明             |
| --------- | ---------------- |
| `*`       | 重复零次或更多次 |
| `+`       | 重复一次或更多次 |
| ?         | 重复零次或一次   |
| `{n}`     | 重复n次          |
| `{n,}`    | 重复n次或更多次  |
| `{n,m}`   | 重复n到m次       |

下面是一些使用重复的例子：

`Windows\d+`匹配Windows后面跟1个或更多数字

`^\w+`匹配一行的第一个单词(或整个字符串的第一个单词，具体匹配哪个意思得看选项设置)

### [#](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#字符类)字符类

要想查找数字，字母或数字，空白是很简单的，因为已经有了对应这些字符集合的元字符，但是如果你想匹配没有预定义元字符的字符集合(比如元音字母a,e,i,o,u),应该怎么办？

很简单，你只需要在方括号里列出它们就行了，像`[aeiou]`就匹配任何一个英文元音字母，`[.?!]`匹配标点符号(.或?或!)。

我们也可以轻松地指定一个字符**范围**，像`[0-9]`代表的含意与`\d`就是完全一致的：一位数字；同理`[a-z0-9A-Z_]`也完全等同于`\w`（如果只考虑英文的话）。

下面是一个更复杂的表达式：`\(?0\d{2}[) -]?\d{8}`。

这个表达式可以匹配几种格式的电话号码，像*(010)88886666*，或*022-22334455*，或*02912345678*等。我们对它进行一些分析吧：首先是一个转义字符(,它能出现0次或1次(?),然后是一个0，后面跟着2个数字(\d{2})，然后是)或-或空格中的一个，它出现1次或不出现(?)，最后是8个数字(\d{8})。

> “(”和“)”也是元字符，后面的[分组节](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#分组)里会提到，所以在这里需要使用[转义](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#转义)。

### [#](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#分枝条件)分枝条件

不幸的是，刚才那个表达式也能匹配*010)12345678*或*(022-87654321*这样的“不正确”的格式。要解决这个问题，我们需要用到**分枝条件**。正则表达式里的**分枝条件**指的是有几种规则，如果满足其中任意一种规则都应该当成匹配，具体方法是用|把不同的规则分隔开。听不明白？没关系，看例子：

* `0\d{2}-\d{8}|0\d{3}-\d{7}`这个表达式能匹配两种以连字号分隔的电话号码：一种是三位区号，8位本地号(如010-12345678)，一种是4位区号，7位本地号(0376-2233445)。

* `\(0\d{2}\)[- ]?\d{8}|0\d{2}[- ]?\d{8}`这个表达式匹配3位区号的电话号码，其中区号可以用小括号括起来，也可以不用，区号与本地号间可以用连字号或空格间隔，也可以没有间隔。

  你可以试试用分枝条件把这个表达式扩展成也支持4位区号的。

* `\d{5}-\d{4}|\d{5}`这个表达式用于匹配美国的邮政编码。美国邮编的规则是5位数字，或者用连字号间隔的9位数字。

  之所以要给出这个例子是因为它能说明一个问题：**使用分枝条件时，要注意各个条件的顺序**。

  如果你把它改成\d{5}|\d{5}-\d{4}的话，那么就只会匹配5位的邮编(以及9位邮编的前5位)。

  原因是匹配分枝条件时，将会从左到右地测试每个条件，如果满足了某个分枝的话，就不会去再管其它的条件了。

### [#](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#分组)分组

我们已经提到了怎么重复单个字符（直接在字符后面加上限定符就行了）；但如果想要重复多个字符又该怎么办？

你可以用小括号来指定**子表达式**(也叫做**分组**)，然后你就可以指定这个子表达式的重复次数了，你也可以对子表达式进行其它一些操作(后面会有介绍)。

`(\d{1,3}\.){3}\d{1,3}`是一个简单的IP地址匹配表达式。

要理解这个表达式，请按下列顺序分析它：

* `\d{1,3}`匹配1到3位的数字-
* `(\d{1,3}\.){3}`匹配三位数字加上一个英文句号(这个整体也就是这个**分组**)重复3次
* 最后再加上一个一到三位的数字(\d{1,3})。

不幸的是，它也将匹配*256.300.888.999*这种不可能存在的IP地址。

如果能使用算术比较的话，或许能简单地解决这个问题，但是正则表达式中并不提供关于数学的任何功能，所以只能使用冗长的分组，选择，字符类来描述一个正确的IP地址：`((2[0-4]\d|25[0-5]|[01]?\d\d?)\.){3}(2[0-4]\d|25[0-5]|[01]?\d\d?)`。

理解这个表达式的关键是理解`2[0-4]\d|25[0-5]|[01]?\d\d?`，这里我就不细说了。

> IP地址中每个数字都不能大于255. 经常有人问, 01.02.03.04 这样前面带有0的数字, 是不是正确的IP地址呢?
>
> 答案是: 是的, IP 地址里的数字可以包含有前导 0 (leading zeroes).

### [#](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#反义)反义

有时需要查找不属于某个能简单定义的字符类的字符。比如想查找除了数字以外，其它任意字符都行的情况，这时需要用到**反义**：

| 代码/语法  | 说明                                       |
| ---------- | ------------------------------------------ |
| `\W`       | 匹配任意不是字母，数字，下划线，汉字的字符 |
| `\S`       | 匹配任意不是空白符的字符                   |
| `\D`       | 匹配任意非数字的字符                       |
| `\B`       | 匹配不是单词开头或结束的位置               |
| `[^x]`     | 匹配除了x以外的任意字符                    |
| `[^aeiou]` | 匹配除了aeiou这几个字母以外的任意字符      |

例子：`\S+`匹配不包含空白符的字符串。

`<a[^>]+>`匹配用尖括号括起来的以a开头的字符串。

### [#](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#后向引用)后向引用

使用小括号指定一个子表达式后，**匹配这个子表达式的文本**(也就是此分组捕获的内容)可以在表达式或其它程序中作进一步的处理。

默认情况下，每个分组会自动拥有一个**组号**，规则是：从左向右，以分组的左括号为标志，第一个出现的分组的组号为1，第二个为2，以此类推。

> 呃……其实,组号分配还不像我刚说得那么简单：
>
> * 分组0对应整个正则表达式
> * 实际上组号分配过程是要从左向右扫描两遍的：第一遍只给未命名组分配，第二遍只给命名组分配－－因此所有命名组的组号都大于未命名的组号
> * 你可以使用(?:exp)这样的语法来剥夺一个分组对组号分配的参与权．

**后向引用**用于重复搜索前面某个分组匹配的文本。

例如，`\1`代表分组1匹配的文本。难以理解？请看示例：

`\b(\w+)\b\s+\1\b`可以用来匹配重复的单词，像*go go*, 或者*kitty kitty*。这个表达式首先是一个单词，也就是单词开始处和结束处之间的多于一个的字母或数字(`\b(\w+)\b`)，这个单词会被捕获到编号为1的分组中，然后是1个或几个空白符(`\s+`)，最后是分组1中捕获的内容（也就是前面匹配的那个单词）(`\1`)。

你也可以自己指定子表达式的**组名**。要指定一个子表达式的组名，请使用这样的语法：`(?<Word>\w+)`(或者把尖括号换成'也行：`(?'Word'\w+)`),这样就把\w+的组名指定为Word了。要反向引用这个分组**捕获**的内容，你可以使用`\k<Word>`,所以上一个例子也可以写成这样：`\b(?<Word>\w+)\b\s+\k<Word>\b`。

使用小括号的时候，还有很多特定用途的语法。

下面列出了最常用的一些：

| 分类           | 代码/语法                                                    | 说明                                                         |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 捕获           | `(exp)`                                                      | 匹配exp,并捕获文本到自动命名的组里                           |
| `(?<name>exp)` | 匹配exp,并捕获文本到名称为name的组里，也可以写成`(?'name'exp)` |                                                              |
| `(?:exp)`      | 匹配exp,不捕获匹配的文本，也不给此分组分配组号               |                                                              |
| 零宽断言       | `(?=exp)`                                                    | 匹配exp前面的位置                                            |
| `(?<=exp)`     | 匹配exp后面的位置                                            |                                                              |
| `(?!exp)`      | 匹配后面跟的不是exp的位置                                    |                                                              |
| `(?<!exp)`     | 匹配前面不是exp的位置                                        |                                                              |
| 注释           | `(?#comment)`                                                | 这种类型的分组不对正则表达式的处理产生任何影响，用于提供注释让人阅读 |

我们已经讨论了前两种语法。第三个`(?:exp)`不会改变正则表达式的处理方式，只是这样的组匹配的内容不会像前两种那样被捕获到某个组里面，也不会拥有组号。“我为什么会想要这样做？”——好问题，你觉得为什么呢？

### [#](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#零宽断言)零宽断言

接下来的四个用于查找在某些内容(但并不包括这些内容)之前或之后的东西，也就是说它们像\b,^,$那样用于指定一个位置，这个位置应该满足一定的条件(即断言)，因此它们也被称为**零宽断言**。最好还是拿例子来说明吧：

断言用来声明一个应该为真的事实。正则表达式中只有当断言为真时才会继续进行匹配。

* `(?=exp)`也叫**零宽度正预测先行断言**，它断言自身出现的位置的后面能匹配表达式exp。比如`\b\w+(?=ing\b)`，匹配以ing结尾的单词的前面部分(除了ing以外的部分)，如查找*I'm singing while you're dancing.*时，它会匹配sing和danc。
* `(?<=exp)`也叫**零宽度正回顾后发断言**，它断言自身出现的位置的前面能匹配表达式exp。比如`(?<=\bre)\w+\b`会匹配以re开头的单词的后半部分(除了re以外的部分)，例如在查找*reading a book*时，它匹配ading。

假如你想要给一个很长的数字中每三位间加一个逗号(当然是从右边加起了)，你可以这样查找需要在前面和里面添加逗号的部分：`((?<=\d)\d{3})+\b`，用它对*1234567890*进行查找时结果是234567890。

下面这个例子同时使用了这两种断言：`(?<=\s)\d+(?=\s)`匹配以空白符间隔的数字(再次强调，不包括这些空白符)。

### [#](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#负向零宽断言)负向零宽断言

前面我们提到过怎么查找**不是某个字符或不在某个字符类里**的字符的方法(反义)。

但是如果我们只是想要**确保某个字符没有出现，但并不想去匹配它**时怎么办？例如，如果我们想查找这样的单词--它里面出现了字母q,但是q后面跟的不是字母u,我们可以尝试这样：`\b\w*q[^u]\w*\b`匹配包含**后面不是字母u的字母q**的单词。

但是如果多做测试(或者你思维足够敏锐，直接就观察出来了，你会发现，如果q出现在单词的结尾的话，像**Iraq**,**Benq**，这个表达式就会出错。

这是因为`[^u]`总要匹配一个字符，所以如果q是单词的最后一个字符的话，后面的`[^u]`将会匹配q后面的单词分隔符(可能是空格，或者是句号或其它的什么)，后面的`\w*\b`将会匹配下一个单词，于是`\b\w*q[^u]\w*\b`就能匹配整个*Iraq fighting*。**负向零宽断言**能解决这样的问题，因为它只匹配一个位置，并不**消费**任何字符。现在，我们可以这样来解决这个问题：`\b\w*q(?!u)\w*\b`。

**零宽度负预测先行断言**(?!exp)，断言此位置的后面不能匹配表达式exp。

例如：`\d{3}(?!\d)`匹配三位数字，而且这三位数字的后面不能是数字；`\b((?!abc)\w)+\b`匹配不包含连续字符串abc的单词。

同理，我们可以用(?<!exp),**零宽度负回顾后发断言**来断言此位置的前面不能匹配表达式exp：`(?<![a-z])\d{7}`匹配前面不是小写字母的七位数字。

一个更复杂的例子：`(?<=<(\w+)>).*(?=<\/\1>)`匹配不包含属性的简单HTML标签内里的内容。`(?<=<(\w+)>)`指定了这样的**前缀**：被尖括号括起来的单词(比如可能是`<b>`)，然后是.*(任意的字符串),最后是一个**后缀**`(?=<\/\1>)`。注意后缀里的/，它用到了前面提过的字符转义；\1则是一个反向引用，引用的正是捕获的第一组，前面的(\w+)匹配的内容，这样如果前缀实际上是`<b>`的话，后缀就是`</b>`了。整个表达式匹配的是`<b>`和`</b>`之间的内容(再次提醒，不包括前缀和后缀本身)。

### [#](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#注释)注释

小括号的另一种用途是通过语法`(?#comment)`来包含注释。例如：`2[0-4]\d(?#200-249)|25[0-5](?#250-255)|[01]?\d\d?(?#0-199)`。

要包含注释的话，最好是启用“忽略模式里的空白符”选项，这样在编写表达式时能任意的添加空格，Tab，换行，而实际使用时这些都将被忽略。启用这个选项后，在#后面到这一行结束的所有文本都将被当成注释忽略掉。例如，我们可以前面的一个表达式写成这样：

```text
      (?<=    # 断言要匹配的文本的前缀
      <(\w+)> # 查找尖括号括起来的内容
              # (即HTML/XML标签)
      )       # 前缀结束
      .*      # 匹配任意文本
      (?=     # 断言要匹配的文本的后缀
      <\/\1>  # 查找尖括号括起来的内容
              # 查找尖括号括起来的内容
      )       # 后缀结束
```

### [#](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#贪婪与懒惰)贪婪与懒惰

当正则表达式中包含能接受重复的限定符时，通常的行为是（在使整个表达式能得到匹配的前提下）匹配**尽可能多**的字符。以这个表达式为例：`a.*b`，它将会匹配最长的以a开始，以b结束的字符串。如果用它来搜索*aabab*的话，它会匹配整个字符串`aabab`。这被称为**贪婪**匹配。

有时，我们更需要**懒惰**匹配，也就是匹配**尽可能少**的字符。前面给出的限定符都可以被转化为懒惰匹配模式，只要在它后面加上一个问号`?`。这样`.*?`就意味着匹配任意数量的重复，但是在能使整个匹配成功的前提下使用最少的重复。现在看看懒惰版的例子吧：

`a.*?b`匹配最短的，以a开始，以b结束的字符串。如果把它应用于*aabab*的话，它会匹配aab（第一到第三个字符）和ab（第四到第五个字符）。

> 为什么第一个匹配是aab（第一到第三个字符）而不是ab（第二到第三个字符）？简单地说，因为正则表达式有另一条规则，比懒惰／贪婪规则的优先级更高：最先开始的匹配拥有最高的优先权——The match that begins earliest wins。

| 代码/语法 | 说明                            |
| --------- | ------------------------------- |
| `*?`      | 重复任意次，但尽可能少重复      |
| `+?`      | 重复1次或更多次，但尽可能少重复 |
| `??`      | 重复0次或1次，但尽可能少重复    |
| `{n,m}?`  | 重复n到m次，但尽可能少重复      |
| `{n,}?`   | 重复n次以上，但尽可能少重复     |

### [#](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#处理选项)处理选项

上面介绍了几个选项如忽略大小写，处理多行等，这些选项能用来改变处理正则表达式的方式。下面是.Net中常用的正则表达式选项：

| 名称                              | 说明                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| IgnoreCase(忽略大小写)            | 匹配时不区分大小写。                                         |
| Multiline(多行模式)               | 更改^和$的含义，使它们分别在任意一行的行首和行尾匹配，而不仅仅在整个字符串的开头和结尾匹配。(在此模式下,$的精确含意是:匹配\n之前的位置以及字符串结束前的位置.) |
| Singleline(单行模式)              | 更改.的含义，使它与每一个字符匹配（包括换行符\n）。          |
| IgnorePatternWhitespace(忽略空白) | 忽略表达式中的非转义空白并启用由#标记的注释。                |
| ExplicitCapture(显式捕获)         | 仅捕获已被显式命名的组。                                     |

> 在C#中，你可以使用[Regex(String, RegexOptions)构造函数 (opens new window)](http://msdn2.microsoft.com/zh-cn/library/h5845fdz.aspx)来设置正则表达式的处理选项。如：Regex regex = new Regex(@"\ba\w{6}\b", RegexOptions.IgnoreCase);
>
> 2019/06：只有基于 Webkit/Chromium 的浏览器（如 Chrome, Safari等）才支持 dotAll 选项。

一个经常被问到的问题是：是不是只能同时使用多行模式和单行模式中的一种？

答案是：不是。这两个选项之间没有任何关系，除了它们的名字比较相似（以至于让人感到疑惑）以外。事实上，为了避免混淆，在最新的 JavaScript 中，单行模式其实名叫 dotAll，意为点可以匹配所有字符，然而在指定该选项时，用的还是 Singleline 的首字母 s.

### [#](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#平衡组-递归匹配)平衡组/递归匹配

有时我们需要匹配像( 100 * ( 50 + 15 ) )这样的可嵌套的层次性结构，这时简单地使用`\(.+\)`则只会匹配到最左边的左括号和最右边的右括号之间的内容(这里我们讨论的是贪婪模式，懒惰模式也有下面的问题)。假如原来的字符串里的左括号和右括号出现的次数不相等，比如*( 5 / ( 3 + 2 ) ) )*，那我们的匹配结果里两者的个数也不会相等。有没有办法在这样的字符串里匹配到最长的，配对的括号之间的内容呢？

这里介绍的平衡组语法是由.Net Framework支持的；其它语言／库不一定支持这种功能，或者支持此功能但需要使用不同的语法。

为了避免(和(把你的大脑彻底搞糊涂，我们还是用尖括号代替圆括号吧。现在我们的问题变成了如何把`xx <aa <bbb> <bbb> aa> yy`这样的字符串里，最长的配对的尖括号内的内容捕获出来？

这里需要用到以下的语法构造：

* `(?'group')` 把捕获的内容命名为group,并压入**堆栈(Stack)**
* `(?'-group')` 从堆栈上弹出最后压入堆栈的名为group的捕获内容，如果堆栈本来为空，则本分组的匹配失败
* `(?(group)yes|no)` 如果堆栈上存在以名为group的捕获内容的话，继续匹配yes部分的表达式，否则继续匹配no部分
* `(?!)` 零宽负向先行断言，由于没有后缀表达式，试图匹配总是失败

我们需要做的是每碰到了左括号，就在压入一个"Open",每碰到一个右括号，就弹出一个，到了最后就看看堆栈是否为空－－如果不为空那就证明左括号比右括号多，那匹配就应该失败。正则表达式引擎会进行回溯(放弃最前面或最后面的一些字符)，尽量使整个表达式得到匹配。

```text
<                   #最外层的左括号
  [^<>]*            #它后面非括号的内容
  (
      (
        (?'Open'<)  #左括号，压入"Open"
        [^<>]*      #左括号后面的内容
      )+
      (
        (?'-Open'>) #右括号，弹出一个"Open"
        [^<>]*      #右括号后面的内容
      )+
  )*
  (?(Open)(?!))     #最外层的右括号前检查
                    #若还有未弹出的"Open"
                    #则匹配失败

>                #最外层的右括号
```

平衡组的一个最常见的应用就是匹配HTML,下面这个例子可以匹配嵌套的`<div>`标签：`<div[^>]*>[^<>]*(((?'Open'<div[^>]*>)[^<>]*)+((?'-Open'</div>)[^<>]*)+)*(?(Open)(?!))</div>`.

> 如果你不是一个程序员（或者你自称程序员但是不知道堆栈是什么东西），你就这样理解上面的三种语法吧：第一个就是在黑板上写一个"group"，第二个就是从黑板上擦掉一个"group"，第三个就是看黑板上写的还有没有"group"，如果有就继续匹配yes部分，否则就匹配no部分。

## [#](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#扩展知识)扩展知识

上边已经描述了构造正则表达式的大量元素，但是还有很多没有提到的东西。

下面是一些未提到的元素的列表，包含语法和简单的说明。你可以在网上找到更详细的参考资料来学习它们--当你需要用到它们的时候。如果你安装了MSDN Library,你也可以在里面找到.Net下正则表达式详细的文档。这里的介绍很简略，如果你需要更详细的信息，而又没有在电脑上安装MSDN Library，可以查看[关于正则表达式语言元素的MSDN在线文档 (opens new window)](http://msdn.microsoft.com/zh-cn/library/az24scfc.aspx)。

| 代码/语法          | 说明                                                         |
| ------------------ | ------------------------------------------------------------ |
| `\a`               | 报警字符(打印它的效果是电脑嘀一声)                           |
| `\b`               | 通常是单词分界位置，但如果在字符类里使用代表退格             |
| `\t`               | 制表符，Tab                                                  |
| `\r`               | 回车                                                         |
| `\v`               | 竖向制表符                                                   |
| `\f`               | 换页符                                                       |
| `\n`               | 换行符                                                       |
| `\e`               | Escape                                                       |
| `\0nn`             | ASCII代码中八进制代码为nn的字符                              |
| `\xnn`             | ASCII代码中十六进制代码为nn的字符                            |
| `\unnnn`           | Unicode代码中十六进制代码为nnnn的字符                        |
| `\cN`              | ASCII控制字符。比如\cC代表Ctrl+C                             |
| `\A`               | 字符串开头(类似^，但不受处理多行选项的影响)                  |
| `\Z`               | 字符串结尾或行尾(不受处理多行选项的影响)                     |
| `\z`               | 字符串结尾(类似$，但不受处理多行选项的影响)                  |
| `\G`               | 当前搜索的开头                                               |
| `\p{name}`         | Unicode中命名为name的字符类，例如\p{IsGreek}                 |
| `(?>exp)`          | 贪婪子表达式                                                 |
| `(?<x>-<y>exp)`    | 平衡组                                                       |
| `(?im-nsx:exp)`    | 在子表达式exp中改变处理选项                                  |
| `(?im-nsx)`        | 为表达式后面的部分改变处理选项                               |
| `(?(exp)yes\|no)`  | 把exp当作零宽正向先行断言，如果在这个位置能匹配，使用yes作为此组的表达式；否则使用no |
| `(?(exp)yes)`      | 同上，只是使用空表达式作为no                                 |
| `(?(name)yes\|no)` | 如果命名为name的组捕获到了内容，使用yes作为表达式；否则使用no |
| `(?(name)yes)`     | 同上，只是使用空表达式作为no                                 |

## [#](https://front-end.toimc.com/notes-page/basic/node/04-正则表达式入门.html#参考文献)参考文献

* [精通正则表达式(第3版)(opens new window)](https://u.jd.com/0yfKdc)
* [微软的正则表达式教程(opens new window)](https://docs.microsoft.com/zh-cn/dotnet/standard/base-types/regular-expressions)
* [Regex类(微软文档)(opens new window)](https://docs.microsoft.com/zh-cn/dotnet/api/system.text.regularexpressions.regex)
* [专业的正则表达式教学网站(英文)(opens new window)](http://www.regular-expressions.info/)