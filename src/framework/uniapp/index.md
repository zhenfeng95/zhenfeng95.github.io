
---
title: uniapp介绍
type: uniapp
order: 1
---

# [#](https://front-end.toimc.com/notes-page/project/community-miniapp/#uniapp介绍)uniapp介绍

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/#什么是快应用)什么是快应用？

快应用是指用户无需下载安装，即点即用，享受原生性能体验的应用，例如：微信小程序，支付宝小程序，百度小程序等。

快应用的优势：

* 无需下载安装App，节约手机空间；
* 性能好，体验接近原生；
* 背靠流量；

快应用的缺点：

* 平台多，语法多，开发成本高；
* 管控难；

快应用的发展趋势：平台底层支持，扫码即用，无需安装微信、支付宝等平台，可以参看，[链接(opens new window)](https://www.zhihu.com/question/269267011)

为什么PWA不香了？

* 浏览器环境需要考虑兼容性；
* 支付会被限制；
* 缺乏原生能力；
* 追求原生体验；
* 更高的安全性的要求（内容、代码）；

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/#背景介绍)背景介绍

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/#技术人想偷懒)技术人想偷懒

困境：

* 小程序的平台太多，需要学习多个平台的语法；
* 前端人也想用vue，react语法来写小程序；
* 想在小程序中使用npm包（庞大的第三方包）；
* 小程序代码 -> 原生App，一套代码多端运行；

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/#各个平台之间的对比)各个平台之间的对比

![test-frame-11.png](uniapp.assets/test-frame-11.9ab52396.png)

数据源：[链接 (opens new window)](https://juejin.cn/post/6844903810788245511)， 2020年横评：[链接(opens new window)](https://juejin.cn/post/6844904118901817351)

**结论：**

* 跨端支持度测评结论：`uni-app` > `taro` > `chameleon` > `mpvue` >`wepy`、`原生微信小程序`
* 微信原生框架可达到更好的性能，但 `uni-app`、`taro` 相比微信原生，性能差距并不大；
* 微信原生开发手工优化, `uni-app`>微信原生开发未手工优化, `taro` > `chameleon`> `wepy` > `mpvue`
* `mpvue`支持绝大部分的Vue语法；`uni-app` 编译到微信端曾经使用过`mpvue`，但后来重新编写，支持了更多vue语法如`filter`、复杂 `JavaScript` 表达式等；`wepy`、`chameleon` 都是 `类Vue` 的实现，仅支持 `Vue` 的部分语法，开发时需要单独学习它们的规则；`taro` 对于 `JSX` 的语法支持是相对完善的，React技术栈友好；
* 学习资料完善度：`uni-app` > `mpvue` , `taro` > `chameleon` > `wepy`
* 社区活跃度：`uni-app` > `taro`> `chameleon` > `wepy` >`mpvue`

TIP

没有最好的，只有最适合的。

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/#应用场景)应用场景

* 熟悉Vue技术栈，推荐：uniapp > wepy > mpvue；熟悉React技术栈，taro；
* 项目初期，想法论证可以使用uniapp -> 多端开发；后续，推荐Flutter跨端开发，再到后期推荐原生开发；
* 框架的更新本身，不能作为使用框架的绝对指标；选择合适的框架，解决当下的问题；

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/#什么是uniapp)什么是uniapp?

`uni-app` 是一个使用 [Vue.js (opens new window)](https://vuejs.org/)开发所有前端应用的框架，开发者编写一套代码，可发布到iOS、Android、Web（响应式）、以及各种小程序（微信/支付宝/百度/头条/QQ/钉钉/淘宝）、快应用等多个平台。

TIP

uni-app与mpvue的渊源：`uni-app`在初期借鉴了`mpvue`，实现了微信小程序端的快速兼容，[参考链接 (opens new window)](https://ask.dcloud.net.cn/article/35699)。

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/#开发规范)开发规范

为了实现多端兼容，综合考虑编译速度、运行性能等因素，`uni-app` 约定了如下开发规范：

* 页面文件遵循 [Vue 单文件组件 (SFC) 规范(opens new window)](https://vue-loader.vuejs.org/zh/spec.html)

* 组件标签靠近小程序规范，详见[uni-app 组件规范(opens new window)](https://uniapp.dcloud.io/component/README)

  有几点特别要注意的：

  1. 注意：所有组件与属性名都是小写，单词之间以连字符`-`连接；
  2. 每个vue文件的根节点必须为 `<template>`，且这个 `<template>` 下只能且必须有一个根 `<view>` 组件；
  3. 不推荐使用HTML标签，为了管理方便、策略统一，新写代码时仍然建议使用view等组件；
  4. 组件上的事件绑定，需要以 vue 的事件绑定语法来绑定，如 bindchange="eventName" 事件，需要写成 `@change="eventName"`；
  5. uni-app支持的组件分为vue组件和小程序自定义组件；如果扩展组件符合uni-app的`easycom`组件规范，则可以免注册，直接使用；如果组件不符合easycom规范，则需要在代码里手动import和注册组件，然后才能使用

* 接口能力（JS API）靠近微信小程序规范，但需将前缀 `wx` 替换为 `uni`，详见[uni-app接口规范(opens new window)](https://uniapp.dcloud.io/api/README)

* 数据绑定及事件处理同 `Vue.js` 规范，同时补充了App及页面的生命周期

* 为兼容多端运行，建议使用flex布局进行开发

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/#目录结构)目录结构

一个uni-app工程，默认包含如下目录及文件：

```markdown
┌─uniCloud              云空间目录，阿里云为uniCloud-aliyun,腾讯云为uniCloud-tcb（详见uniCloud）
│─components            符合vue组件规范的uni-app组件目录
│  └─comp-a.vue         可复用的a组件
├─hybrid                App端存放本地html文件的目录，详见
├─platforms             存放各平台专用页面的目录，详见
├─pages                 业务页面文件存放的目录
│  ├─index
│  │  └─index.vue       index页面
│  └─list
│     └─list.vue        list页面
├─static                存放应用引用的本地静态资源（如图片、视频等）的目录，注意：静态资源只能存放于此
├─uni_modules           存放uni_module规范的插件。
├─wxcomponents          存放小程序组件的目录，详见
├─main.js               Vue初始化入口文件
├─App.vue               应用配置，用来配置App全局样式以及监听 应用生命周期
├─manifest.json         配置应用名称、appid、logo、版本等打包信息
└─pages.json            配置页面路由、导航条、选项卡等页面类信息
```

TIP

* 编译到任意平台时，`static` 目录下的文件均会被完整打包进去，且不会编译。非 `static` 目录下的文件（vue、js、css 等）只有被引用到才会被打包编译进去。
* `static` 目录下的 `js` 文件不会被编译，如果里面有 `es6` 的代码，不经过转换直接运行，在手机设备上会报错。
* `css`、`less/scss` 等资源不要放在 `static` 目录下，建议这些公用的资源放在自建的 `common` 目录下。
* HbuilderX 1.9.0+ 支持在根目录创建 `ext.json`、`sitemap.json` 等小程序需要的文件。

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/#导入静态资源)导入静态资源

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/#模板内引入静态资源)模板内引入静态资源

`template`内引入静态资源，如`image`、`video`等标签的`src`属性时，可以使用相对路径或者绝对路径，形式如下

```html
<!-- 绝对路径，/static指根目录下的static目录，在cli项目中/static指src目录下的static目录 -->
<image class="logo" src="/static/logo.png"></image>
<image class="logo" src="@/static/logo.png"></image>
<!-- 相对路径 -->
<image class="logo" src="../../static/logo.png"></image>
```

特别说明：

TIP

* `@`开头的绝对路径以及相对路径会经过base64转换规则校验
* 引入的静态资源在非h5平台，均不转为base64。
* H5平台，小于4kb的资源会被转换成base64，其余不转。
* 自`HBuilderX 2.6.6`起`template`内支持`@`开头路径引入静态资源，旧版本不支持此方式
* App平台自`HBuilderX 2.6.9`起`template`节点中引用静态资源文件时（如：图片），调整查找策略为【基于当前文件的路径搜索】，与其他平台保持一致
* 支付宝小程序组件内 image 标签不可使用相对路径

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/#js文件引入)js文件引入

`js`文件或`script`标签内（包括renderjs等）引入`js`文件时，可以使用相对路径和绝对路径，形式如下

```js
// 绝对路径，@指向项目根目录，在cli项目中@指向src目录
import add from '@/common/add.js'
// 相对路径
import add from '../../common/add.js'
```

WARNING

js文件不支持使用`/`开头的方式引入

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/#css引入静态资源)css引入静态资源

1. `css`文件或`style标签`内引入`css`文件时（scss、less文件同理），可以使用相对路径或绝对路径（`HBuilderX 2.6.6`）

```css
/* 绝对路径 */
@import url('/common/uni.css');
@import url('@/common/uni.css');
/* 相对路径 */
@import url('../../common/uni.css');
```

WARNING

自`HBuilderX 2.6.6`起支持绝对路径引入静态资源，旧版本不支持此方式

1. `css`文件或`style标签`内引用的图片路径可以使用相对路径也可以使用绝对路径，需要注意的是，有些小程序端css文件不允许引用本地文件（请看注意事项）。

```css
/* 绝对路径 */
background-image: url(/static/logo.png);
background-image: url(@/static/logo.png);
/* 相对路径 */
background-image: url(../../static/logo.png);
```

注意事项：

TIP

* 引入字体图标请参考，[字体图标(opens new window)](https://uniapp.dcloud.io/frame?id=字体图标)
* `@`开头的绝对路径以及相对路径会经过base64转换规则校验
* 不支持本地图片的平台，小于40kb，一定会转base64。（共四个平台mp-weixin, mp-qq, mp-toutiao, app v2）
* h5平台，小于4kb会转base64，超出4kb时不转。
* 其余平台不会转base64



# [#](https://front-end.toimc.com/notes-page/project/community-miniapp/01-搭建开发环境.html#搭建开发环境)搭建开发环境

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/01-搭建开发环境.html#集成scss-sass编译)集成scss/sass编译

为了方便编写样式（例如`<style lang="scss">`），建议大家安装`sass/scss编译`插件，插件的下载地址：[scss/sass编译(opens new window)](https://ext.dcloud.net.cn/plugin?name=compile-node-sass)

![image-20210419163056984](uniapp.assets/image-20210419163056984.8ea29f1f.png)

登录账号 -> 无账号，即注册（邮箱验证） -> 再次点击安装插件 -> 打开HBuilderX

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/01-搭建开发环境.html#自定义主题、快捷键等)自定义主题、快捷键等

1. 快捷键切换

在`工具 -> 预设快捷键方案切换` 中可以切换自己喜欢的快捷键方案，对HBuilderX进行自定义：

![image-20210419163655412](uniapp.assets/image-20210419163655412.cd023f65.png)

1. 设置主题

![image-20210419163851076](uniapp.assets/image-20210419163851076.d0d9fcc5.png)

1. 字号设置

macOS的快捷键是 `Command + ,`，windows的快捷键是`Ctrl + ,`

![image-20210419164115182](uniapp.assets/image-20210419164115182.daa3ac9b.png)

常见配置：

```json
{
    "editor.colorScheme" : "Default",
    "editor.fontFamily" : "Consolas",
    "editor.fontSize" : 14,
    "editor.insertSpaces" : true,
    "editor.lineHeight" : "1.5",
    "editor.mouseWheelZoom" : true,
    "editor.onlyHighlightWord" : false,
    "editor.tabSize" : 2,
    "editor.wordWrap" : true,
    "editor.codeassist.px2rem.enabel": false,
    "editor.codeassist.px2upx.enabel": false
}
```

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/01-搭建开发环境.html#创建项目)创建项目

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/01-搭建开发环境.html#使用hbuilderx)使用HBuilderX

步骤：

* 下载HBuilderX：[官方IDE下载地址 (opens new window)](https://www.dcloud.io/hbuilderx.html)——建议使用标准版本

  > HBuilderX标准版可直接用于web开发、markdown、字处理场景。做App仍需要安装插件。
  >
  > App开发版预置了App/uni-app开发所需的插件，开箱即用。
  >
  > 标准版也可以在插件安装界面安装App开发所需插件，App开发版只是一个预集成作用。
  >
  > App开发插件体积大的原因主要有2方面：
  >
  > 1. 真机运行基座，Android版、iOS版、iOS模拟器版，加起来体积就1百多M。真机运行基座需要把所有模块都内置进去，方便大家开发调试。开发者自己做app打包是不会这么大的，因为可以在manifest里选模块来控制体积。
  > 2. uni-app的编译器，依赖webpack和各种node模块，node_modules就是这么一个生态现状，文件超级多，几万个文件，解压起来很慢。

* 在点击工具栏里的文件 -> 新建 -> 项目：

  ![img](uniapp.assets/b925a1c0-4f19-11eb-97b7-0dc4655d6e68.0528b5af.png)

* 选择`uni-app`类型，输入工程名，选择模板，点击创建，即可成功创建。

  ![image-20210419161501579](uniapp.assets/image-20210419161501579.57b59a7b.png)

* 在微信开发者工具里运行：进入hello-uniapp项目，点击工具栏的运行 -> 运行到小程序模拟器 -> 微信开发者工具，即可在微信开发者工具里面体验uni-app。

   ![img](uniapp.assets/d89fd6f0-4f1a-11eb-97b7-0dc4655d6e68.627f3d67.png)

  第一次运行的提示：

  ![image-20210419170249359](uniapp.assets/image-20210419170249359.6b82ec00.png)

  成功运行：

  ![image-20210419170454810](uniapp.assets/image-20210419170454810.8b9f0528.png)

  **注意：**

  * 如果是第一次使用，需要先配置小程序ide的相关路径，才能运行成功。如下图，需在输入框输入微信开发者工具的安装路径，uni-app默认把项目编译到根目录的unpackage目录。

    ![image-20210419171415089](uniapp.assets/image-20210419171415089.8ee56032.png)

  * 若HBuilderX不能正常启动微信开发者工具，需要开发者手动启动，然后将uni-app生成小程序工程的路径拷贝到微信开发者工具里面，在HBuilderX里面开发，在微信开发者工具里面就可看到实时的效果。

  * 如果提示`[error] 工具的服务端口已关闭。要使用命令行调用工具，请在下方输入 y 以确认开启，或手动打开工具 -> 设置 -> 安全设置，将服务端口开启`，如图：

    ![image-20210419170330616](uniapp.assets/image-20210419170330616.ed49854b.png)

    微信开发者工具设置菜单，安全中打开服务端口：

    ![image-20210419165932479](uniapp.assets/image-20210419165932479.6a975e1a.png)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/01-搭建开发环境.html#使用vue-cli命令行-vscode)使用vue-cli命令行(VSCode)

1. 初始化项目

```text
// 全局安装 vue-cli 3.x（如已安装请跳过此步骤）
npm install -g @vue/cli

// 通过 CLI 创建 uni-app 项目
vue create -p dcloudio/uni-preset-vue my-project
```

![img](uniapp.assets/1190eb9efa120f8db46d2aa81773a8a8.ca422b25.png)

1. 安装组件语法提示

组件语法提示是uni-app的亮点，其他框架很少能提供。

```text
npm i @dcloudio/uni-helper-json
```

如果是HBuilderX的项目，可以使用

```text
npm i @types/uni-app @types/html5plus -D
```

另外，uni-app 项目下的 manifest.json、pages.json 等文件可以包含注释。vscode 里需要改用 jsonc 编辑器打开。

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/01-搭建开发环境.html#配置appid)配置AppID

![image-20210419171015789](uniapp.assets/image-20210419171015789.c41b33ae.png)

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/01-搭建开发环境.html#eslint与代码格式化)ESLint与代码格式化

TIP

ESLint与代码保存即自动格式化，仅在VSCode上有效

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/01-搭建开发环境.html#使用第三方npm包)使用第三方npm包

uni-app支持使用**npm**安装第三方包。

此文档要求开发者们对**npm**有一定的了解，因此不会再去介绍**npm**的基本功能。如若之前未接触过**npm**，请翻阅[NPM官方文档 (opens new window)](https://docs.npmjs.com/getting-started/what-is-npm)进行学习。

**1. 初始化npm工程**

若项目之前未使用npm管理依赖（项目根目录下无package.json文件），先在项目根目录执行命令初始化npm工程：

```shell
npm init -y
```

cli项目默认已经有package.json了。HBuilderX创建的项目默认没有，需要通过初始化命令来创建。

**2. 安装依赖**

在项目根目录执行命令安装npm包：

```shell
npm install packageName --save
```

**3. 使用**

安装完即可使用npm包，js中引入npm包：

TIP

* 为多端兼容考虑，建议优先从 [uni-app插件市场 (opens new window)](https://ext.dcloud.net.cn/)获取插件。直接从 npm 下载库很容易只兼容H5端。
* 非 H5 端不支持使用含有 dom、window 等操作的 vue 组件和 js 模块，安装的模块及其依赖的模块使用的 API 必须是 uni-app 已有的 [API (opens new window)](https://uniapp.dcloud.io/api/README)（兼容小程序 API），比如：支持[高德地图微信小程序 SDK (opens new window)](https://www.npmjs.com/package/amap-wx)。类似[jQuery (opens new window)](https://www.npmjs.com/package/jquery)等库只能用于H5端。
* node_modules 目录必须在项目根目录下。不管是cli项目还是HBuilderX创建的项目。
* 支持安装 mpvue 组件，但npm方式不支持小程序自定义组件（如 wxml格式的vant-weapp），使用小程序自定义组件请参考：[小程序组件支持 (opens new window)](https://uniapp.dcloud.io/frame?id=小程序组件支持)。
* 关于ui库的获取，详见[多端UI库(opens new window)](https://ask.dcloud.net.cn/article/35489)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/01-搭建开发环境.html#初始化eslint)初始化ESLint

```text
# 初始化npm包管理
npm init -y

# 安装eslint依赖
npm i -D eslint eslint-config-standard eslint-plugin-import eslint-plugin-node eslint-plugin-promise eslint-plugin-vue
```

package.json文件配置如下：

```json
  "devDependencies": {
    "eslint": "^7.24.0",
    "eslint-config-standard": "^16.0.2",
    "eslint-plugin-import": "^2.22.1",
    "eslint-plugin-node": "^11.1.0",
    "eslint-plugin-promise": "^4.3.1",
    "eslint-plugin-vue": "^7.8.0"
  }
```

新建 两个文件，`.eslintrc.js`：

```js
module.exports = {
  env: {
    browser: true,
    commonjs: true,
    es2021: true,
    node: true
  },
  extends: ['eslint:recommended', 'standard', 'plugin:vue/essential'],
  parserOptions: {
    ecmaVersion: 12
  },
  plugins: ['vue'],
  rules: {
    // 这里有一些自定义配置
    'no-console': [
      'warn',
      {
        allow: ['warn', 'error']
      }
    ],
    'no-eval': 'error',
    'no-alert': 'error'
  },
  globals: {
    uni: 'readonly',
    plus: 'readonly',
    wx: 'readonly'
  }
}
```

创建`.eslintignore`文件：

```text
node_modules
.hbuilderx
static
uni_modules
unpackage
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/01-搭建开发环境.html#配置vscode自动修复功能)配置vscode自动修复功能

安装`vetur`、`eslint`插件

打开vscode的首选项配置，`settings.json`文件

```json
{
  // ... 你自己的配置
  "editor.codeActionsOnSave": {
  "source.fixAll.eslint": true
  },
  "eslint.format.enable": true,
  //autoFix默认开启，只需输入字符串数组即可
  "eslint.validate": ["javascript", "vue", "html"],

  // 关闭vue文件的自动格式化工具, vetur，使用eslint
  "[vue]": {
  	"editor.defaultFormatter": "octref.vetur"
  },

  "vetur.format.defaultFormatter.ts": "none",
  "vetur.format.defaultFormatter.js": "none",

  // ... 
}
```

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/01-搭建开发环境.html#下载官方代码提示)下载官方代码提示

点击 [下载地址 (opens new window)](https://github.com/zhetengbiji/uniapp-snippets-vscode)，放到项目目录下的 .vscode 目录即可拥有和 HBuilderX 一样的代码块。

![img](uniapp.assets/d39d378c0821a67c8bf72c7965833378.436a861e.png)



# [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#首页模块)首页模块

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#首页tabbar)首页tabBar

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#代码依赖分析)代码依赖分析

可以在基本信息中选择代码依赖分析

![img](uniapp.assets/image-20210428193747590.a3e78cc2.png)

查看本地代码与分包大小：

![img](uniapp.assets/image-20210428193958813.de26ed19.png)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#导入静态资源)导入静态资源

* 删除原`static`目录中的文件；
* 下载课程的静态资源文件夹，并放置于`static`的`images`目录中。

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#布局与样式)布局与样式

完成效果：

![img](uniapp.assets/image-20210428194953556.b102bed2.png)

创建tabBar的步骤：

* 创建tabBar对应的页面；
* 修改pages.json中的配置项：`pages`和`tabBar`

创建页面HBuilderx方式：

![img](uniapp.assets/image-20210419173303417.abf903c9.png)

创建页面的VSCode插件方式：

![image-20210428194346415](uniapp.assets/image-20210428194346415.a9dc6d4b.png)

快速创建4个页面

![image-20210428194614781](uniapp.assets/image-20210428194614781.39a556bc.png)

并调整`pages.json`：

```json
{
	"pages": [
		{
			"path": "pages/home/home"
		},
		{
			"path": "pages/msg/msg"
		},
		{
			"path": "pages/hot/hot"
		},
		{
			"path": "pages/center/center"
		}
	],
	"globalStyle": {
		"navigationBarTextStyle": "black",
		"navigationBarTitleText": "uni-app",
		"navigationBarBackgroundColor": "#F8F8F8",
		"backgroundColor": "#F8F8F8",
		"app-plus": {
			"background": "#efeff4"
		}
	}
}
```

新建tabBar属性：

```json
"tabBar": {
	"color": "#999",
	"backgroundColor": "#fafafa",
	"selectedColor": "#02D199",
	"borderStyle": "white",
	"list": [
		{
			"text": "首页",
			"pagePath": "pages/home/home",
			"iconPath": "static/images/tab_home_no.png",
			"selectedIconPath": "static/images/tab_home_yes.png"
		},
		{
			"text": "消息",
			"pagePath": "pages/msg/msg",
			"iconPath": "static/images/tab_news_no.png",
			"selectedIconPath": "static/images/tab_news_yes.png"
		},
		{
			"text": "热门",
			"pagePath": "pages/hot/hot",
			"iconPath": "static/images/tab_popular_no.png",
			"selectedIconPath": "static/images/tab_popular_yes.png"
		},
		{
			"text": "我的",
			"pagePath": "pages/center/center",
			"iconPath": "static/images/tab_my_no.png",
			"selectedIconPath": "static/images/tab_my_yes.png"
		}
	],
	"position": "bottom"
}
```

配置package.json中的eslint修复命令：

```text
"lint": "eslint --ext vue --ext js pages --fix"
```

底部的阴影：

```vue
<view class="bottom-line"></view>

<style lang="scss">
.bottom-line {
  position: fixed;
  bottom: -5px;
  left: 0;
  width: 100vw;
  height: 5px;
  background: transparent;
  box-shadow: 0 -5px 5px rgba(0, 0, 0, 0.05);
}
</style>
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#导入uview-ui)导入uView UI

准备工作

```js
// 安装sass依赖
npm i node-sass sass-loader@10 -D

// 安装uView
npm install uview-ui
```

在`main.js`中引入：

```text
import uView from 'uview-ui'
Vue.use(uView)
```

在项目根目录的`uni.scss`中引入样式文件：

```css
/* uni.scss */
@import 'uview-ui/theme.scss';
```

调整`App.vue`的样式

```vue
<style lang="scss">
/* 注意要写在第一行，同时给style标签加入lang="scss"属性 */
@import "uview-ui/index.scss";
</style>
```

配置`pages.json`：

```json
	// ....
	"easycom": {
		"^u-(.*)": "uview-ui/components/u-$1/u-$1.vue"
	}
```

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#首页tabs)首页Tabs

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#布局与样式-2)布局与样式

`home.vue`添加tabs

```vue
<u-tabs ref="uTabs" :is-scroll="true" active-color="#02D199" height="88" gutter="50"></u-tabs>
```

添加tabs数据

```vue
<u-tabs ref="uTabs" :list="tabs" :name="'value'" :current="current" :is-scroll="true" active-color="#02D199" height="88" gutter="50"></u-tabs>
<script>
  export default {
  data: () => ({
    tabs: [
      {
        key: '',
        value: '首页'
      },
      {
        key: 'ask',
        value: '提问'
      },
      {
        key: 'share',
        value: '分享'
      },
      {
        key: 'discuss',
        value: '讨论'
      },
      {
        key: 'advise',
        value: '建议'
      },
      {
        key: 'advise',
        value: '公告'
      },
      {
        key: 'advise',
        value: '动态'
      }
    ],
    // 因为内部的滑动机制限制，请将tabs组件和swiper组件的current用不同变量赋值
    current: 0, // tabs组件的current值，表示当前活动的tab选项
    swiperCurrent: 0, // swiper组件的current值，表示当前那个swiper-item是活动的
  })
}
</script>
```

完成效果

![image-20210526225831609](uniapp.assets/image-20210526225831609.c1003bc6.png)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#添加事件)添加事件

tabs添加切换事件`tabsChange`

```vue
<u-tabs ref="uTabs" :list="tabs" :name="'value'" :current="current" @change="tabsChange" :is-scroll="true" active-color="#02D199" height="88" gutter="50"></u-tabs>

<script>
	export default {
    ...
    methods: {
      // tabs通知swiper切换
      tabsChange (index) {
        this.current = index
      }
    }
  }
</script>
```

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#接口封装)接口封装

问题：小程序原生提供了request请求，但是是callback的使用方式，并不支持Promise，见[链接 (opens new window)](https://developers.weixin.qq.com/miniprogram/dev/api/network/request/wx.request.html)。

![image-20210714162901877](uniapp.assets/image-20210714162901877.3807c355.png)

需求：我们前端里面写网络请求，习惯了Axios + async/await的书写风格，那在小程序中怎么去实现接口请求的封装呢？

拆分需求：

* Promise化 -> async/await支持
* 接口请求封装
  * 拦截器
  * 统一错误处理
  * 取消重复请求
  * ..

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#小程序api-promise化)小程序API Promise化

扩展微信小程序api支持promise

首先，本地小程序npm初始化

```text
npm init -y

// 安装依赖包
npm install --save miniprogram-api-promise
```

在小程序入口（app.js）调用一次promisifyAll，只需要调用一次。

```js
import { promisifyAll, promisify } from 'miniprogram-api-promise';

const wxp = {}
// promisify all wx's api
promisifyAll(wx, wxp)
console.log(wxp.getSystemInfoSync())
wxp.getSystemInfo().then(console.log)
wxp.showModal().then(wxp.openSetting())

// compatible usage
wxp.getSystemInfo({success(res) {console.log(res)}})

// promisify single api
promisify(wx.getSystemInfo)().then(console.log)
```

建议的写法：

```text
import { promisifyAll, promisify } from 'miniprogram-api-promise';

const wx.p = {}

promisifyAll(wx, wx.p)

// 全局使用wx.p
wx.p.getSystemInfo().then(console.log)
```

async/await支持的方法比较简单：开户`将 JS 代码编译成 ES5`即可：[链接(opens new window)](https://developers.weixin.qq.com/miniprogram/dev/devtools/codecompile.html)

> 在工具 1.05.2106091 版本之后，原有的`ES6 转 ES5` 和 `增强编译` 选项统一合并为`将 JS 代码编译成 ES5`，此功能和原有的`增强编译`逻辑一致。如需了解旧版本的文档，请[点此查看 (opens new window)](https://developers.weixin.qq.com/miniprogram/dev/devtools/codecompile_old.html)。

支持async/await语法，按需注入`regeneratorRuntime`，目录位置与辅助函数一致

![image-20210714164109331](uniapp.assets/image-20210714164109331.812b4718.png)

平时开发的时候，一定要注意该选项有没有打开哦！！

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#uniapp小程序请求封装)uniapp小程序请求封装

接口请求封装

* 拦截器
* 统一错误处理
* 取消重复请求

TIP

uniapp中已经支持Promise + async/await，但是，对于request请求的错误的处理需要自己来处理。

请求工具js：

```js
// 这个文件我们使用uni.request进行封装
// 同样适合于原生小程序的request封装 -> Promise API

// 需求分析

const errorHandle = (err) => {
  if (err.statusCode === 401) {
    // todo 4.业务 —> refreshToken -> 请求响应401 -> 刷新token
  } else {
    // 其他的错误
    // showToast提示用户
    // 3.对错误进行统一的处理 -> showToast
    const { data: { msg } } = err
    uni.showToast({
      icon: 'none',
      title: msg || '请求异常，请重试',
      duration: 2000
    })
  }
}

export const request = (options = {}) => {
  const { success, fail } = options
  // 1.在头部请求的时候，token带上 -> 请求拦截器
  const publicArr = [/\/public/, /\/login/]
  // local store -> uni.getStorageSync('token')
  let isPublic = false
  publicArr.forEach(path => {
    isPublic = isPublic || path.test(options.url)
  })
  const token = uni.getStorageSync('token')
  if (!isPublic && token) {
    options.header = Object.assign({},
      {
        Authorization: 'Bearer ' + token
      }, options.header)
  }
  return new Promise((resolve, reject) => {
    uni.request(Object.assign({}, options, {
      success: (res) => {
        // 响应拦截器
        if (res.statusCode >= 200 && res.statusCode < 300) {
          if (success && typeof success === 'function') {
            // 2.在响应的时候，处理data数据
            success(res.data)
            return
          }
          // 请求成功
          // 2.在响应的时候，处理data数据
          resolve(res.data)
        } else {
          // 请求失败
          errorHandle(res)
          reject(res)
        }
      },
      fail: (err) => {
        if (fail && typeof fail === 'function') {
          fail(err)
          return
        }
        errorHandle(err)
        reject(err)
      }
    }))
  })
}
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#uview中请求封装)uview中请求封装

工作原理：

uView提供了 http封装：

平台差异说明

| App  |  H5  | 微信小程序 | 支付宝小程序 | 百度小程序 | 头条小程序 | QQ小程序 |
| :--: | :--: | :--------: | :----------: | :--------: | :--------: | :------: |
|  √   |  √   |     √      |      √       |     √      |     √      |    √     |

由于某些小程序平台的限制：

* delete请求，不支持支付宝和头条小程序(HX2.6.15)
* put请求，不支持支付宝小程序(HX2.6.15)

```text
语法：

get | post | put | delete(url, params, header).then(res => {}).catch(res => {})
```

* `url<String>` 请求的URL，可以完整的URL(http开头)，或者是路径的一部分，这时会自动拼接上`baseUrl`(一般为api的域名部分)
* `params<Object>` 请求的参数，对象形式，如`"{name: 'lisa', age: 23}"`，该参数是可选的
* `header <Object>` 请求的header，对象形式，如果token等字段，建议通过配置写入，该参数是可选的`get`和`post`都挂载在`$u`对象下，其中`get`和`post`使用方法完全一致，只是一个为`this.$u.get`

但是如果直接按照上面的写会报错，由于没有设置baseURL： ![image-20210716134642927](uniapp.assets/image-20210716134642927.6c997f5f.png)

官方文档也有说明：

![image-20210716134734477](uniapp.assets/image-20210716134734477.4f953221.png)

设置baseURL的方法：配置参数的时候，需要调用`$u.http.setConfig()`，打开`main.js`：

```js
Vue.prototype.$u.http.setConfig({
  baseUrl: 'http://localhost:3000'
})
```

修改接口：

```js
this.$u.get('/public/getCaptcha?sid=123123123'
).then(res => {
  console.log(res)
}).catch(err => {
  console.log('err', err)
})
```

然后就可以正常的请求了：

![image-20210716134858589](uniapp.assets/image-20210716134858589.a1e4dff2.png)

uview对于拦截器（请求/响应）的支持：

```js
Vue.prototype.$u.http.interceptor.request = (config) => {
  console.log('config)
  return config
}

Vue.prototype.$u.http.interceptor.response = (data) => {
  console.log('data)
  return data
}
```

请求封装：

`config.js`配置文件：

```js
// export const baseUrl = 'https://mp.toimc.com'
export const baseUrl =
  process.env.NODE_ENV === 'development'
    ? 'http://localhsot:3000'
    : 'https://mp.toimc.com'
```

`common/request.js`文件

```js
// console.log(process.env)
import store from '@/store'
import { authNav } from '@/common/checkAuth'
import { baseUrl } from '@/config'
import { simpleHttp } from '@/common/utils/simple-http'

export const config = {
  baseUrl: baseUrl, // 请求的本域名
  // baseUrl: 'https://mp.toimc.com', // 请求的本域名
  // 设置为json，返回后会对数据进行一次JSON.parse()
  dataType: 'json',
  showLoading: true, // 是否显示请求中的loading
  loadingText: '请求中...', // 请求loading中的文字提示
  loadingTime: 800, // 在此时间内，请求还没回来的话，就显示加载中动画，单位ms
  originalData: false, // 是否在拦截器中返回服务端的原始数据
  loadingMask: true, // 展示loading的时候，是否给一个透明的蒙层，防止触摸穿透
  // 配置请求头信息
  header: {
    'content-type': 'application/json;charset=UTF-8'
  },
}

const install = (Vue) => {
  const http = Vue.prototype.$u.http
  http.setConfig(config)

  http.interceptor.request = (config) => {
    let isPublic = false
    const publicPath = [/^\/public/, /^\/login/]
    publicPath.forEach((path) => {
      isPublic = isPublic || path.test(config.url)
    })
    const token = store.state.token
    // if (token) {
    if (!isPublic && token) {
      config.header.Authorization = 'Bearer ' + token
    }
    return config
    // 如果return一个false值，则会取消本次请求
    // if(config.url == '/user/rest') return false; // 取消某次请求
  }

  http.interceptor.response = (data) => {
    return data
  }
}

export default {
  install
}
```

问题是如何进行统一的错误处理？

* uview-ui的http工具类没有错误的统一处理
* 统一的错误处理的应用场景：401跳转到鉴权、给用户友好提示、refreshToken的场景

具体做法，修改uview的源码，并把uview的代码移动到项目的代码中，利用hbuilder&uniapp的按需加载，按需打包的机制，使用uview中的组件。

步骤：

* 复制`node_modules`中的`uview-ui`到项目的根目录；

* 删除`package.json`中的`dependencies`

* 修改pages.json中的`easycom`：

  ```js
  	"easycom": {
  		"^u-(.*)": "@/uview-ui/components/u-$1/u-$1.vue"
  	}
  ```

* 修改main.js中的引入 ：

  ```js
  import uView from './uview-ui'
  ```

  `uni.scss`中的引入 ：

  ```js
  /* uni.scss */
  @import "./uview-ui/theme.scss";
  ```

  还有App.vue中的引入 ：

  ```js
  /* 注意要写在第一行，同时给style标签加入lang="scss"属性 */
  @import "./uview-ui/index.scss";
  ```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#合并uview请求)合并uview请求

目的：

* 动态引入 API 接口
* 统一所有的请求的使用方式 `this.$u.api.接口`

思路：

* 使用`require.context`动态引入api接口
* 使用插件的方式，在Vue.prototype.$u来添加api属性

目录结构：

![img](uniapp.assets/image-20210717223433286.90a35ff7.png)

其中`index.js`代码：

```js
const req = require.context('./modules', false, /\.js$/)

const install = (Vue) => {
  let api = Vue.prototype.$u.api || {}
  req.keys().forEach(item => {
    const module = req(item)
    // 1.取得所有的方法 -> key值,
    const keys = Object.keys(module)
    keys.forEach(key => {
      api = {
        ...api,
        // 2.取得所有方法对应的function -> value值
        // 3.把上面的对象 -> $u.api
        [key]: module[key]
      }
    })
    // 4.把它封装成为一个插件，以便在后面的方法中使用 this.$u.api.方法名(参数).then(res) ....
  })
  Vue.prototype.$u.api = api
}

export default {
  install
}
```

修改`main.js`：

```js
import apis from '@/api'

Vue.use(apis)
```

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#内容列表)内容列表

测试图片地址：[链接(opens new window)](https://toimc-public.obs.cn-east-3.myhuaweicloud.com/1322928787@qq.com/2021-05-30/107382e7-024d-45ad-a4f4-c5df8a40e91f-tmp_40bdb731a1cae24e217376f9473ed92c.jpg)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#布局与样式-3)布局与样式

添加`list-item`组件

```vue
<template>
  <view class="list-item">
    <view class="list-head">
      <!-- 标题部分 -->
      <text class="type" :class="['type-'+item.catalog]">{{tabs.filter(o => o.key === item.catalog)[0].value}}</text>
      <text class="title">{{item.title}}</text>
    </view>
    <!-- 用户部分 -->
    <view class="author u-flex u-m-b-18">
      <u-image :src="item.uid.pic" class="head" width="40" height="40" shape="circle" error-icon="/static/images/header.jpg"></u-image>
      <text class="name u-m-l-10">{{item.uid.name}}</text>
    </view>
    <!-- 摘要部分 + 右侧的图片 -->
    <view class="list-body u-m-b-30 u-flex u-col-top">
      <view class="info u-m-r-20 u-flex-1">{{item.content}}</view>
      <image class="fmt" :src="item.snapshot" v-if="item.snapshot" mode="aspectFill" />
    </view>
    <!-- 回复 + 文章发表的时间 -->
    <view class="list-footer u-flex">
      <view class="left">
        <text class="reply-num u-m-r-25">{{item.answer}} 回复</text>
        <text class="timer">{{item.created | moment}}</text>
      </view>
    </view>
  </view>
</template>

<script>
import { tabs } from '@/config/const'
export default {
  props: {
    item: {
      type: Object,
      default: () => ({})
    }
  },
  data: () => ({
    tabs
  })
}
</script>

<style lang="scss" scoped>
.list-item {
  background: #fff;
  margin-top: 20rpx;
  padding: 30rpx;
}

.list-head {
  margin-bottom: 18rpx;
  .type {
    display: inline-block;
    height: 36rpx;
    width: 72rpx;
    text-align: center;
    line-height: 36rpx;
    white-space: nowrap;
    margin-right: 10rpx;
    font-size: 24rpx;
    border-radius: 18rpx;
    border-bottom-left-radius: 0;
    color: #fff;
    background-color: red;
    position: relative;
    top: -4rpx;
    transform: scale(0.9);
  }
  .type-share {
    background-color: #feb21e;
  }
  .type-ask {
    background-color: #02d199;
  }
  .type-discuss {
    background-color: #fe1e1e;
  }
  .type-advise {
    background-color: #0166f8;
  }
  .type-notice {
    background-color: #00a3b8;
  }
  .type-logs {
    background-color: #33cb61;
  }
  .title {
    color: #333;
    font-size: 32rpx;
    line-height: 44rpx;
    font-weight: bold;
  }
}

.author {
  color: #666;
  font-size: 24rpx;
}

.list-body {
  .info {
    font-size: 28rpx;
    color: #666;
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;
  }
  .fmt {
    width: 192rpx;
    height: 122rpx;
    border-radius: 8rpx;
    flex-shrink: 0;
  }
}

.list-footer {
  color: #999;
  font-size: 24rpx;
}
</style>
```

`home.vue`添加内容展示区

```vue
<template>
  <view class="home">
    <view class="fixed-top">
      <search @click="handle()"></search>
      <u-tabs name="value" :list="tabs" :is-scroll="true" :current="current" @change="change" gutter="50" height="88" active-color="#02D199"></u-tabs>
    </view>
    <!-- 首页的列表 -->
    <view class="content" :style="{'padding-top': offsetTop + 'px'}">
      <view class="wrapper">
        <view class="list-box" v-for="(item,index) in lists" :key="index">
          <list-item :item="item" v-if="item.status === '0' && item.title && item.catalog"></list-item>
        </view>
        <view class="end">{{msg}}</view>
      </view>
    </view>
    <view class="bottom-line"></view>
  </view>
</template>

<script>
// import { request } from '@/common/http'
import { tabs } from '@/config/const'
export default {
  components: {},
  data: () => ({
    current: 0,
    offsetTop: 50,
    page: {
      page: 0,
      limit: 10,
      catalog: '',
      sort: 'created'
    },
    lists: [],
    loading: false,
    pullDown: false,
    isEnd: false,
    tabs
  }),
  methods: {
    handle () {
      uni.navigateTo({
        url: '/subcom-pkg/search/search'
      })
    },
    change (index) {
      this.isEnd = false
      this.current = index
      this.page = {
        page: 0,
        limit: 10,
        catalog: this.tabs[index].key,
        sort: 'created'
      }
      this.lists = []
      this.getList()
    },
    async getList () {
      this.loading = true
      const { data } = await this.$u.api.getList(this.page)
      if (data.length < this.page.limit) {
        this.isEnd = true
      }
      this.lists = [...this.lists, ...data]
      this.loading = false
      this.page.page++
    }
  },
  mounted () {
    this.getList()
  },
  computed: {
    msg () {
      let str = ''
      if (this.loading) {
        str = '加载中...'
      } else {
        if (this.lists.length > 0) {
          // 说明有数据
          if (this.isEnd) {
            str = '您已经到底啦~没有更多了！'
          }
        } else {
          // 说明没有数据
          str = '暂无内容，请下拉刷新'
        }
      }
      return str
    }
  },

  // 页面周期函数--监听页面加载
  onLoad () {},
  // 页面周期函数--监听页面初次渲染完成
  onReady () {
    const query = uni.createSelectorQuery().in(this)
    query.select('.fixed-top').boundingClientRect((data) => {
      this.offsetTop = data.height
    }).exec()
  },
  // 页面周期函数--监听页面显示(not-nvue)
  onShow () {},
  // 页面周期函数--监听页面隐藏
  onHide () {},
  // 页面周期函数--监听页面卸载
  onUnload () {},
  // 页面处理函数--监听用户下拉动作
  onPullDownRefresh () {
    this.pullDown = true
    this.change(this.current)
    // setTimeout(() => {
    //   console.log(2222)
    //   uni.stopPullDownRefresh()
    // }, 2000)
  },
}
</script>

<style lang="scss">
.fixed-top {
  position: fixed;
  left: 0;
  top: 0;
  width: 100vw;
  z-index: 999;
}

.search {
  width: 70%;
}

.content {
  background: #f5f6f7;
  .wrapper {
    padding-bottom: 24rpx;
  }
}

.end {
  color: #999;
  text-align: center;
  background: #fff;
  padding: 25rpx 0;
  // background: transparent;
}
</style>
```

监听页面加载，处理内容列表容器的padding

```javascript
onLoad () {
  const query = uni.createSelectorQuery().in(this)
  query.select('.fixed-top').boundingClientRect(data => {
    this.offsetTop = data.height
  }).exec()
  // const { windowHeight } = uni.getSystemInfoSync()
  // const query = uni.createSelectorQuery().in(this)
  // query.select('#tabs').boundingClientRect(data => {
  // }).exec()
}
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#获取数据)获取数据

在通过后台接口获取数据前，我们需要对request和api进行简单的封装。

创建`common`文件夹，新建 request.js

```javascript
// 1.请求拦截器
// 2.在响应的时候，处理data数据
// 3.统一的错误处理
// 这里的vm，就是我们在vue文件里面的this，所以我们能在这里获取vuex的变量，比如存放在里面的token变量
const install = (Vue, vm) => {
  // 此为自定义配置参数，具体参数见上方说明
  Vue.prototype.$u.http.setConfig({
    baseUrl: 'http://localhost:3000',
    dataType: 'json',
    showLoading: true, // 是否显示请求中的loading
    loadingText: '请求中...', // 请求loading中的文字提示
    loadingTime: 800, // 在此时间内，请求还没回来的话，就显示加载中动画，单位ms
    originalData: false, // 是否在拦截器中返回服务端的原始数据
    loadingMask: true, // 展示loading的时候，是否给一个透明的蒙层，防止触摸穿透
    // 配置请求头信息
    header: {
      'content-type': 'application/json;charset=UTF-8'
    },
    errorHandle: (err) => {
      if (err.statusCode === 401) {
        // todo 4.业务 —> refreshToken -> 请求响应401 -> 刷新token
        uni.showToast({
          icon: 'none',
          title: '鉴权失败，请重新登录',
          duration: 2000
        })
      } else {
        // 其他的错误
        // showToast提示用户
        // 3.对错误进行统一的处理 -> showToast
        const { data: { msg } } = err
        uni.showToast({
          icon: 'none',
          title: msg || '请求异常，请重试',
          duration: 2000
        })
      }
    }
  })

  // 请求拦截，配置Token等参数
  Vue.prototype.$u.http.interceptor.request = (config) => {
    // 引用token
    // 1.在头部请求的时候，token带上 -> 请求拦截器
    const publicArr = [/\/public/, /\/login/]
    // local store -> uni.getStorageSync('token')
    let isPublic = false
    publicArr.forEach(path => {
      isPublic = isPublic || path.test(config.url)
    })
    const token = uni.getStorageSync('token')
    if (!isPublic && token) {
      config.header = Object.assign({},
        {
          Authorization: 'Bearer ' + token
        }, config.header)
    }
    // 最后需要将config进行return
    return config
    // 如果return一个false值，则会取消本次请求
    // if(config.url === '/user/rest') return false; // 取消某次请求
  }

  // 响应拦截，判断状态码是否通过
  Vue.prototype.$u.http.interceptor.response = (res) => {
    console.log('🚀 ~ file: request.js ~ line 46 ~ install ~ res', res)
    return res
  }
}

export default {
  install
}
```

创建`api`文件夹，新建 index.js，按模块进行接口划分

![image-20210526235920712](uniapp.assets/image-20210526235920712.8bf13693.png)

index.js

```javascript
const req = require.context('./modules', false, /\.js$/)

const install = (Vue) => {
  let api = Vue.prototype.$u.api || {}
  req.keys().forEach(item => {
    const module = req(item)
    // 1.取得所有的方法 -> key值,
    const keys = Object.keys(module)
    keys.forEach(key => {
      api = {
        ...api,
        // 2.取得所有方法对应的function -> value值
        // 3.把上面的对象 -> $u.api
        [key]: module[key]
      }
    })
    // 4.把它封装成为一个插件，以便在后面的方法中使用 this.$u.api.方法名(参数).then(res) ....
  })
  Vue.prototype.$u.api = api
}

export default {
  install
}
```

public.js

```javascript
import Vue from 'vue'

const HttpRequest = Vue.prototype.$u

// ---------------------------------------首页----------------------------------------- //
// 获取首页列表数据
export const getContentList = params => HttpRequest.get('/public/list', params)
```

在`main.js`中引入

```javascript
import apis from '@/api/index'
import interceptors from '@/common/request'

Vue.use(interceptors)
Vue.use(apis)
```

下面我们来获取首页列表数据，创建`getList`方法

```javascript
  methods: {
    async getList () {
      this.loading = true
      const { data } = await this.$u.api.getList(this.page)
      if (data.length < this.page.limit) {
        this.isEnd = true
      }
      this.lists = [...this.lists, ...data]
      this.loading = false
      this.page.page++
    }
  },
```

最后在`onshow`周期请求数据

```javascript
onShow () {
    this.getList()
}
```

完成效果：

![image-20210527003502507](uniapp.assets/image-20210527003502507.3b9a3d15.png)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#分页与切换)分页与切换

上拉分页加载列表数据

```javascript
  watch: {
    loading (newval) {
      if (this.pullDown && !newval) {
        this.pullDown = false
        uni.stopPullDownRefresh()
      }
    }
  },
  // 页面处理函数--监听用户上拉触底
  onReachBottom () {
    // 1. 触发条件的设置，距离底部多远的时候触发 - 50
    // console.log('reach bottom')
    // 2. 当没有更多的时候，需要给用户一个提示，同时设置页面样式，不再发请新的请求
    if (this.isEnd || this.loading) return
    this.getList()
  },
```

同时在tab切换时加载不同分类的数据，在tabs切换方法`tabsChange`中添加 getList 方法

```javascript
tabsChange (index) {
  this.current = index
  this.page = {
    page: 0,
    limit: 10,
    catalog: this.tabs[this.current].key || '',
    sort: 'created'
  }
  this.getList()
}
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#日期过滤器)日期过滤器

注意：用法与vue中的过滤器一致

1. 安装依赖：

```text
npm i dayjs
```

1. 添加filter.js文件：

```js
import dayjs from 'dayjs'
import relativeTime from 'dayjs/plugin/relativeTime'
import 'dayjs/locale/zh-cn'

dayjs.extend(relativeTime)

const moment = (date) => {
  // 超过7天，显示日期
  if (dayjs(date).isBefore(dayjs().subtract(7, 'days'))) {
    return dayjs(date).format('YYYY-MM-DD')
  } else {
    // 1小前，xx小时前，X天前
    return dayjs(date).locale('zh-cn').from(dayjs())
  }
}

const hours = (date) => {
  if (dayjs(date).isBefore(dayjs(dayjs().format('YYYY-MM-DD 00:00:00')))) {
    return dayjs(date).format('YYYY-MM-DD')
  } else {
    // 1天内
    return dayjs(date).format('HH:mm:ss')
  }
}

// ...

export default {
  moment,
  hours
}
```

1. main.js中使用：

```js
import filters from '@/common/filter'

Object.keys(filters).forEach((key) => {
  Vue.filter(key, filters[key])
})
```

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#搜索)搜索

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#首页搜索按钮)首页搜索按钮

关于设计：

参考：[链接1 (opens new window)](https://developers.weixin.qq.com/miniprogram/design/#图标)， [链接2(opens new window)](https://segmentfault.com/a/1190000018733860)

官方的文档目前没有对右侧按钮的说明：

![image-20210712184845616](uniapp.assets/image-20210712184845616.663c9f4c.png)

在旧的文档中有说明：

![image-20210712184922032](uniapp.assets/image-20210712184922032.fce664cd.png)

从图中分析，我们可以得到如下信息：

* Android跟iOS有差异，表现在顶部到胶囊按钮之间的距离差了6pt
* 胶囊按钮高度为32pt， iOS和Android一致

**关于px，em与pt单位的说明：**

px是像素单位，em是相对单位，pt是绝对单位。

它们各自的好处是：

* px可以在计算机屏幕上，能达到预期的效果，在打印机和其它的高分辨率设备上，它又能取得所希望的效果。
* em的优点很多，比如在一个页面上，你给定了一个父元素的字体大小，这样就可以通过调整一个元素来成比例的改变所有元素大小。它可以自由缩放，比如用来制作可伸缩的样式表。
* pt是一种固定长度的度量单位，是能够使用测量设备测得的长度。绝对单位作用有限，因为它们不能够缩放，通常只用在已经知道是用在哪种输出媒体的情况下才使用。但大多数情况下最好使用相对单位。一般都是用px和em这两种种配搭比较好。

使用`wx.getSystemInfoSync()`可以在console中快速查看设备的信息。

两种方式创建uni的easycom组件：

1. 创建`components/组件名同名文件夹/组件名.vue`
2. 使用hbuilderx来创建组件，勾选`创建目录`

![image-20210712193718265](uniapp.assets/image-20210712193718265.0bf6f757.png)

创建search组件：

```vue
<template>
  <view class="search" :style="{'padding-top': barHeight + 'px'}" @click="$emit('click')">
    <view class="search-box">
      <u-icon name="search" color="#CCC" size="28" class="icon"></u-icon>
      <text>搜索社区内容</text>
    </view>
  </view>
</template>

<script>
export default {
  name: 'search',
  data () {
    return {
      barHeight: 0
    }
  },
  beforeMount () {
    this.getNavBarHeight()
  },
  methods: {
    getNavBarHeight () {
      uni.getSystemInfo({
        success: (result) => {
          console.log('🚀 ~ file: home.vue ~ line 24 ~ getNavBarHeight ~ result', result)
          const statusBarHeight = result.statusBarHeight
          const isiOS = result.system.indexOf('iOS') > -1
          if (isiOS) {
            this.barHeight = statusBarHeight + 5
          } else {
            this.barHeight = statusBarHeight + 8
          }
        }
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.search {
  position: relative;
  width: 100vw;
  background: #fff;
  padding: 0 32rpx 12rpx;
  z-index: 999;
  display: flex;
  justify-content: flex-start;
  align-items: center;
  color: #ccc;
  .search-box {
    position: relative;
    width: 60%;
    @media screen and (max-width: 320px) {
      width: 50%;
    }
    background: #f3f3f3;
    height: 64rpx;
    border-radius: 32rpx;
    line-height: 64rpx;
    columns: #ccc;
    font-size: 26rpx;
    padding-left: 74rpx;
  }
  .icon {
    position: absolute;
    left: 32rpx;
    top: 19rpx;
  }
}
</style>
```

注意：

* 在uniapp中自定义的组件是无法直接绑定事件的；
* 可以在子组件中的最外侧的view上绑定`$emit('click')`

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#添加分包)添加分包

在uniapp中添加分包

* 新增页面，比如`/sub-pkg/search/search`
* 配置`pages.json`

注意：

**分包的作用是为了提升小程序的加载性能，首页tabs相关的页面是不能放置在分包中的。**

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#布局与样式-4)布局与样式

```vue
<template>
  <view class="search">
    <div class="search-box">
      <u-search :focus="true"></u-search>
    </div>
    <!-- 搜索建议列表 -->
    <view class="list" v-if="searchResults.length !== 0">
      <view class="item" v-for="(item, i) in searchResults" :key="i">
        <view class="name">{{item.name}}</view>
        <u-icon type="arrow-right" size="16"></u-icon>
      </view>
    </view>
    <!-- 搜索历史 -->
    <view class="history-box" v-else>
      <!-- 标题区域 -->
      <view class="history-title" v-if="historyList.length !== 0">
        <text>搜索历史</text>
        <u-icon type="trash" size="17" @click="clean"></u-icon>
      </view>
      <!-- 列表区域 -->
      <view class="history-list">
        <uni-tag :text="item" v-for="(item, i) in historyList" :key="i"></uni-tag>
      </view>
    </view>
    <!-- 热门推荐 -->
    <view class="history-box">
      <!-- 标题区域 -->
      <view class="history-title" v-if="hotList.length !== 0">
        <text>热门推荐</text>
      </view>
      <!-- 列表区域 -->
      <view class="history-list">
        <uni-tag :text="item" v-for="(item, i) in hotList" :key="i"></uni-tag>
      </view>
    </view>
  </view>
</template>

<style lang="scss" scoped>
.search {
  padding: 24rpx;
}

.search-box {
  position: sticky;
  top: 0;
  z-index: 999;
  padding-bottom: 50rpx;
}

.list {
  padding: 0 5px;
  .item {
    font-size: 12px;
    padding: 13px 0;
    border-bottom: 1px solid #efefef;
    display: flex;
    align-items: center;
    justify-content: space-between;
    .name {
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      margin-right: 3px;
    }
  }
}

.history-box {
  padding: 0 10rpx 50rpx;

  .history-title {
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 40px;
    font-size: 16px;
    font-weight: bold;
  }

  .history-list {
    display: flex;
    flex-wrap: wrap;
    ::v-deep .uni-tag {
      margin-top: 5px;
      margin-right: 5px;
      border-radius: 25rpx;
    }
  }
}
</style>
```

`pages.json`中配置分包：

```json
"subPackages": [
	{
		"root": "subpkg",
		"pages": [
			{
				"path": "search/search",
				"style": {
					"navigationBarTitleText": "搜索"
				}
			},
      // ...
    ]
  }
]
```

使用easycom，添加`search.vue`组件：

```vue
<template>
  <view :style="{'padding-top': barHeight + 'px'}" class="search" @click="$emit('click')">
    <view class="search-box">
      <u-icon name="search" color="#CCCCCC" size="28" class="icon"></u-icon>
      <text>搜索社区内容</text>
    </view>
  </view>
</template>

<script>

export default {
  props: {},
  data: () => ({
    barHeight: 80
  }),
  computed: {},
  methods: {
    getNavBarHeight () {
      uni.getSystemInfo({
        success: (result) => {
          const statusBarHeight = result.statusBarHeight
          const isiOS = result.system.indexOf('iOS') > -1
          if (isiOS) {
            this.barHeight = statusBarHeight + 5
          } else {
            this.barHeight = statusBarHeight + 7
          }
          // getApp().globalData.barHeight = this.barHeight
          // 存储至store中
          // uni.setStorage({
          //   key: 'setBarHeight',
          //   data: this.barHeight
          // })
        },
        fail: () => {},
        complete: () => {}
      })
    }
  },
  beforeMount () {
    this.getNavBarHeight()
  }
}
</script>

<style lang="scss" scoped>
// search
.search {
  // padding-top: 25px;
  position: relative;
  background: #fff;
  width: 100vw;
  padding: 0 32rpx 12rpx;
  z-index: 999;
  .search-box {
    position: relative;
    width: 70%;
    @media (max-width: 320px) {
      width: 60%;
    }
    height: 64rpx;
    line-height: 64rpx;
    background: #f3f3f3;
    border-radius: 32rpx;
    color: #ccc;
    font-size: 26rpx;
    padding-left: 74rpx;
  }
  .icon {
    position: absolute;
    left: 32rpx;
    top: 19rpx;
  }
}
</style>
```

`home.vue`添加导航：

```vue
<search @click="gotoSearch"></search>
```

完成效果：

![image-20210523231442459](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAgQAAABOCAYAAABfXg61AAAEDmlDQ1BrQ0dDb2xvclNwYWNlR2VuZXJpY1JHQgAAOI2NVV1oHFUUPpu5syskzoPUpqaSDv41lLRsUtGE2uj+ZbNt3CyTbLRBkMns3Z1pJjPj/KRpKT4UQRDBqOCT4P9bwSchaqvtiy2itFCiBIMo+ND6R6HSFwnruTOzu5O4a73L3PnmnO9+595z7t4LkLgsW5beJQIsGq4t5dPis8fmxMQ6dMF90A190C0rjpUqlSYBG+PCv9rt7yDG3tf2t/f/Z+uuUEcBiN2F2Kw4yiLiZQD+FcWyXYAEQfvICddi+AnEO2ycIOISw7UAVxieD/Cyz5mRMohfRSwoqoz+xNuIB+cj9loEB3Pw2448NaitKSLLRck2q5pOI9O9g/t/tkXda8Tbg0+PszB9FN8DuPaXKnKW4YcQn1Xk3HSIry5ps8UQ/2W5aQnxIwBdu7yFcgrxPsRjVXu8HOh0qao30cArp9SZZxDfg3h1wTzKxu5E/LUxX5wKdX5SnAzmDx4A4OIqLbB69yMesE1pKojLjVdoNsfyiPi45hZmAn3uLWdpOtfQOaVmikEs7ovj8hFWpz7EV6mel0L9Xy23FMYlPYZenAx0yDB1/PX6dledmQjikjkXCxqMJS9WtfFCyH9XtSekEF+2dH+P4tzITduTygGfv58a5VCTH5PtXD7EFZiNyUDBhHnsFTBgE0SQIA9pfFtgo6cKGuhooeilaKH41eDs38Ip+f4At1Rq/sjr6NEwQqb/I/DQqsLvaFUjvAx+eWirddAJZnAj1DFJL0mSg/gcIpPkMBkhoyCSJ8lTZIxk0TpKDjXHliJzZPO50dR5ASNSnzeLvIvod0HG/mdkmOC0z8VKnzcQ2M/Yz2vKldduXjp9bleLu0ZWn7vWc+l0JGcaai10yNrUnXLP/8Jf59ewX+c3Wgz+B34Df+vbVrc16zTMVgp9um9bxEfzPU5kPqUtVWxhs6OiWTVW+gIfywB9uXi7CGcGW/zk98k/kmvJ95IfJn/j3uQ+4c5zn3Kfcd+AyF3gLnJfcl9xH3OfR2rUee80a+6vo7EK5mmXUdyfQlrYLTwoZIU9wsPCZEtP6BWGhAlhL3p2N6sTjRdduwbHsG9kq32sgBepc+xurLPW4T9URpYGJ3ym4+8zA05u44QjST8ZIoVtu3qE7fWmdn5LPdqvgcZz8Ww8BWJ8X3w0PhQ/wnCDGd+LvlHs8dRy6bLLDuKMaZ20tZrqisPJ5ONiCq8yKhYM5cCgKOu66Lsc0aYOtZdo5QCwezI4wm9J/v0X23mlZXOfBjj8Jzv3WrY5D+CsA9D7aMs2gGfjve8ArD6mePZSeCfEYt8CONWDw8FXTxrPqx/r9Vt4biXeANh8vV7/+/16ffMD1N8AuKD/A/8leAvFY9bLAAAAbGVYSWZNTQAqAAAACAAEARoABQAAAAEAAAA+ARsABQAAAAEAAABGASgAAwAAAAEAAgAAh2kABAAAAAEAAABOAAAAAAAAAJAAAAABAAAAkAAAAAEAAqACAAQAAAABAAACBKADAAQAAAABAAAATgAAAAC40TxvAAAACXBIWXMAABYlAAAWJQFJUiTwAAAXBklEQVR4Ae2d65LUxhmGZ3cWWDDG5xOYg43BxofyH18ITlUuI5VfTnwJqbgqF5KqxEmlKteQH/4RnzjYBi/ngw02LBjYZdfpR/BuerWSRpptzWhm364SmpVare6nhb63v/4kzfQK0q+//rotbD7+ePkgrPeGZXdYnEzABEzABEzABCaLwJ1Q3cth+Sws/2CZmZlZDut1aWbdX+GPIAY+DKtPwnI4v89/m4AJmIAJmIAJTDyBM6EFHwVR8GnckjVBEITAbNjxJzLFGfzbBEzABEzABExgKgkw+P84CINVWjcXNdFiIILhnyZgAiZgAiYw5QTkAPgD7cw8BI+nCf4+5Q1380zABEzABEzABDYS+A3TBzOPAwhPhv2OGdgIyVtMwARMwARMYNoJEFNwjLgBniawGJj27nb7TMAETMAETKCYABrguARBcRZvNQETMAETMAET2AoEjhNUyHsGOp/C1Ebv4cOHvZWVld7q6mq2ZhuLkwmYgAmYgAmMm0CYh++x9Pv93uzsbLaem5vLto27bjXO/wExBIshYydfOoQAWF5ezoQAIsDJBEzABEzABCaNAOIAYbBt27Zs3dH630EQdGqITXUePHiQCQGLgI5eNq6WCZiACZjAUAQQBwiDHTt2dM5z0BlBICGAGHAyARMwARMwgWkngCjokjDohCBYWlrq3b9/3/EA0371u30mYAImYALrCBBzMD8/39u+ffu67eP4Y6yCYGV1pXf/3v0sRmAcjfc5TcAETMAETKALBIgxmN853+vP9sdWnbEJArwC9+7dG1vDfWITMAETMAET6BqBnTt3js1bMBZBgBBAEDiZgAmYgAmYgAmsJ8D0AcJg1GnkguCXX37JniAYdUN9PhMwARMwAROYFAI8ibBr166RVpc3FY4sWQyMDLVPZAImYAImMMEEeAcPNnOUaWSCwGJglN3qc5mACZiACUw6gVGLgpEIAmIGaJiTCZiACZiACZhAfQLYzlEF4LcuCAgedABh/c53ThMwARMwAROICYzKjrYqCHjPwKiUTQzPv03ABEzABExgmghgS7GpbaZWBQEvHXIyARMwARMwARPYPIG2bWprggAXB18rdDIBEzABEzABE9g8AWxqm1PwrQgCPlTEtwmcTMAETMAETMAE0hFo87s/rQgCvljYsa8qp+sNl2QCJmACJmACYyKgLwO3cfrkgqDNyrYBwGWagAmYgAmYwCQRaGvQPZcaAhXtckKwEK3JoldD8pUpJxMwARMwAROYFALYWj6bnDIlt4RdfAHR6upq7+rVq73FxcVekbLavn1b+JDErt5LL7008g9K/PTTT707d+5k76x+7rnnCvuW+vPNbAJKLl++nAmZvXv3FuYdZiN9trCwkJ3j9ddf783ODu84unHjRo820ZZnnnlmYHWuX7+e9clTTz3V27Nnz8D8Xctw7ty5TFzSVq6fNtKVK1fWrpF9+/a1cQqXaQImMGEEuG93WhBgsDBeXUoY2wsXzofIzPI3JbJvaelW7/bt29lNva0bexGXH3/8MXtfNR6LvCC4e/duJgB47TMfucAY/Pzzz1kxCIIffvhhXZE7duwYyqjSb3pnNoJpM1/ZQnhR3vLyUi1BcO3atbVrpo4goOwLFy6sa/dm/4DbsAILkbmy0u77NjiHvFrUE3HoZAImsLUJYGu5H6b0cCf1EHTNO3Dp0qUeBlcJcLt3786MK0YP40fEJoaXGy7TCRi0W7du9V577bVsJK5jR7mmk0+fPr3usU3qFz9uQh68BXFiZP/ee+/Fm7LfFy9erHx1NGUpkbfqAmPf/v37lX3D+sUXX8zqhciCL8Y2ZeI/AcItZcLADisIuGZI/X5/qCoh6gb9v2FqS9fn999/P3BUQP4XXnhhqPr4IBMwgckhwL2j6n7dtCVJBUFsWJpWJHV+hEAsBp5//vnsph+PrhAHSoy8MYYa7eEKfuONN7R7pGvqIJYYGowsi0bxVAbjL2Mb5y+q6M2bN2s/9RGfo6gs+FUJAgwRLm4MJetDhw4VFZNkG+2P+3PYQoc15pxPgmDYaRYEaBOvGt4ClqpEXSwIqgh5nwlMBwHZiVStSSYIuDE2ubGlakBROYykMUYkbo7Miz/xxBNFWde2Pf3005n3gBEYRhGvAaO3cd9Y33333bU6xj9o11tvvZVtYsRMvcsSrviqC4d+YwRKwnNSZdwkQuJz4YkhdkBJRhJPyxdffJFtfvnllzNRozwp1m+++WZjQYCiZhppUHwD/U/9ByW1VdM7Vfnph1iExnlhPjc3nJdB5Tx8uNKZ/4Oqk9cmYALtEeDezT0oxcCIWiYTBFUGpz0cxSUzupc4wRU8SAyoFFwvjGhPnTqVHc/ojZt4kRHUMcOs8UZo/l8vcGL97bffZsUdOHBgmGJLjxk0SkcMfPPNN9nxjP6bxhDQ9zKM+UpoO16MNhNGvszYxufl2sB4X716pXfs2NvxrnW/8aqw1E0wlKgqOwahWlZHPFivvPLKukOZEiJ/PrYCocL1ko91QQQTpOlkAiawdQhw/2WaMEVKJgjavuHXbSyje7m9n3zyyQ2BeoPKAeyrr77aO3/+fCYKmHZIHdnNDV11VH0wnPlt7Pvyyy+zLNQrLxS++uqrzBDL6KqseI3noGo/eWMxR7sHXVyoUWIslBj985RAPmGk5Tko2p/PP+zfBBlivBFuR48eLfVwYJARA6R+v/6lXzalAFcJT0b4ZSq96f8N6ok4pF8QjkxdSdRS/4XwRAgJ44/gzQejZjv9jwmYwJYgwP1l0D27Loj6d8UBJerGOCBb67tjo5ofQdU9Oe5kRmfckOPy6h4/KB+jPm76JEZ6sMOg8AhJ3viIa2y0VX4dQ9M0AI/6sDRJGOIiLwpTCST28ZREW0lGniDGr7/+OjOgRV4OxI5SVRyE8rCmX8qmbeIROXnKBAHCrayv6HOuhe3bt2enxchruosNTGVJDPA3HJ999tlMAHFtEPdy/fq1IBYPrgXLtsmaOjiZgAl0h4BsRIoaJRMEZTe8FJVsUkbsti0yCnXL4qaKMW1qHOuUz4hOozpGgogOjOaRI0eywzFsSvFTA/m6aB/1xBVelTAqRUabMiV62B8bn3x51EvGN78v/zeGTSKGUSyGa3Fx49MBupgfvY9hY7Dc3r37Cr0P8fmIpTh79mwWbEd5TH/gTYnjBKi36k7fbuba0LnxgJCqvAPKW7ZWnzONhJgRM8QFbUAQ4Ilg+oo13hjEDOuF4Cmg73ii47vvvsvy4iFxMgET2DoEUtreZIJgkFt6VN0TGzdu1MMmCQIMTBuPz9WtV1UbtE/rqjJ5SiE/F80IXrwwQBgZDJASxil+pGVQ8KKOQ5RplAtHzsvfGK6yxPVTtB8jXme6gcBR3mmA4SThDaBtTPdQNoJBKT/1ou1N1xJulE+w4jBuO4QA/SAhQB3wGhw+fDhjz/V38uTJtf0Ip2PHjmXnQkzAh2khbgqUxRMITOdUCbum7XR+EzCB7hJIaXunThCsrj4KXsu73pt2Z2xkuVkXja6bljlMfkaNSnHHx9s1yla+QWuM15kzZzKhQ16MR/yGQgwdnguMDCKhydQLrBitkhAZGDYSbm6NqLMNj//RlAbCA/GQT2VBePl8/E09GfkzcoYV8R+IAsoWI4L3UvQlxlfKnHMRiIqnookogEfs2eGNmfv3H1gLPKSfeB+FzkMb4YtA4AkL2kXfMV1Bm4lNIS/88R7F1zDHOpmACUwfgdgubLZ1yQTBZiuS6vj5+Z3ZSDPvXm9afnx8CgNS5/wYLwLkMFpK8chR21iXbY/z8JvRNXlpA0aRETSGkotIbmlG8ORhwdDpCQiOxygpUQZGm+OKEmViwGR8Mc4ySjy+WfQIJ0GT5KcOdef1i86tbZSDYWbaAOMoDwj7MaCpAkSZAokTbUAUMHrnPHWSniBglE+94ikOgjHxHOg/+8GDB7MiERD004kTJ9amFNhxKDwdQ99x/TBFI+516uE8JmACJgCBeneuCWIVu/pxXQ87VyxDgqeh7g1+M5ioK6NyEjfzIrc2IkWPlRXtL/KKYCiUMPSxscfYxCNU5WNNm/EaxO/KRhBozjvOy28M4unTpzJjpX0wPHnyRAjyO9Jo5Kzjh10ToPfOO+9kRhPjqZRCcFAWfaBRO4ILw47xlihAkNS9ZuKnNSib+i6E0b7iHRBfPGUg7wkeCLw76jueruBa4HxM98RTPpTnZAImYAJ1CUydIIgFACOm+O+6UHCZa35YN+K6x9bNh7sY4xwHQXIsRhc3sKLO4/IwsBIE8WgyzlP1G2PB8bOzM2GU/+gxOQyL2qpjiTfgmXgF6jE6rTI0Ra5tlUVcAC5ujPEwdVY5TdcYylgMcDzip6lbP39ejD6xCiR5WBBwMJBQwEuCIGmS6AdEhR7T5FiMPAIsvha4Npgu0JQOMQNMHxGkipehzHvTpC7OawImsDUJJBME3Ii4qY07MVpjpMwIDoOLIWsiCjTyUluazJ/XbXvRY2gYFb1RkblgjEpV0vsJ4jyIiaooc0aXMlQYMNzL8WtweW8Dhlvz4Ho0EkNK/xYF98FJL3KiLrjsFRdA/AHGkzwE+dHGojLiNqT4zSOjsScELoieeASvNjY5n4QP5ZC4NmgTCQGFAJEQwWCXeVOyAx7/Q5nUlz7XNQdryi679mgPcQOwFV+EBOeGP8JgmPbF9fJvEzCBySCQchAwdYKAGzRGbeFxYBmGCCNZFxpz7Bq1c3NlRJY6qS7UFSPAIs8A58KwyOiUnbtovwx41TEYyps3b6xF9FMHAv4wPtSDc2M8WTPqxFiR4EmAYD7Ij7bIkBEjgAiQWEFgwJAgN/K1LQZgQj1jkYM7Hc8EkfgIFfIMEwCoKR21lQDAvMHmuiMfC94cRvxVMQtw0dSA+gxOxJCwljdI+/Jr5UUM0C7qhrBggT0C08kETGC6CXAfSJWSCQJG5UVGKlVFm5SD4cEzwJQB8+6MtrlZVxl3DCA3cI5RwnXeRqIuuIOZjmAkifGIE4YYnogZbvISOXEe/WbqQW5mjEBRQijgti9K9BlBhiyDElMIeuQtzosHBubwyl8D7MNNn98eH5/iN8JFwZKUx38SBIz6nLl6GWDq0kQUICQQGhIDtKnME8N8Py58cdX5i9oYj+LJx7Uqz1ZR/kHb6H+JoTj2Y9Bx3m8CJjC5BIpix4ZtTTJBgNHqUmJkxkiN0S4LgVgYWm6a3NC5GXPT1miO0RiiIE5Ekut58Hj7Zn8zah6UEDQYB+pAPXn+PB+AhpCQGOCiUCR6vux4Dpp9UpTxWkFyCBXyUx4Lf7PINY24yr+5b5BrPDZ8+bpt9m/ajxiAkRL1zc+9sw9jLQFG/rqiACMrMYDwORQFauqcWvP/gGuG88A3z175WNNfGG48KxxHfegHftdlxjWrvsMjQLtgUvRER3xu/zYBE5gOAiltbzJBkFKlpOgmjAKjOIwFN0hu6PFImPpy89SNXufkOPYhIhixISTaEAU6X9UaAYMQwK3MKJVH6TBqXAAYaL2EhzKoY1V6//331+2m3DjI7/PPP8/24/LnvLSfxFSG1sQSVI14s4wj/kfTAJwWLszlx49t5quDUEDU0Lf0P6Jq0FSGxCVtz3+AKF8+f+P54XsYGPtBQan5aQeOZ1omL/7YXpSYHsB7oQQDiwHR8NoEpp9AStubTBBgSLuWuDlyY2ZEfunSxbV5c+qpUZXqTF5uztxM2YcQwGiMWxQgABTtjzcDd/TcXH+tLdSbPHg96iTEhd5sh0AqGt1jKBEfrDFozMPjscAg1h251qlLijyM1nkmn/pVzdfH51KUPrETg8SAjoNxk4SocjIBEzCBtgmktL3JrDjuUYwTRqRrCUGwZ8/b2aiXESELxhWQGFJGwYzKBJY1I+6uiAJcwbGre2npEWN489QA60GJ9hIjEQexlc0zE/muvoQVrmw4Ia66Jgi47vTkxCAG8f4iIRTv928TMAET6DoB7tOa+k1R12SCgMpgSAdFuqeo9LBlYPhZYld5WVlFooDH9Oq6csvKzW+vElAYb54KYA67KB/b8BggePBs5N3T7GdqQY/C6dy4mBAZ+fzaDyOMLOdHRCAmWBAliAiEQZOpA9z6TFEUJU3Z3LmzWPiSJC54gjCLEl6MVP8Z8BQUue+LzttkG+2TN6puXeXFqXMexJuTCZjA1iSgQWyq1icVBIweuywImkLLi4J80GHT8pQfYaGRuubqFXymrwIuLW280WPIcUWz1ktwMPo8GcGCwaEc3NvUHWOOGFDCuDK/rnlw9mOwYqMSz0dh9InDoE+ZZtAUCtH6epxPZVetmZpQ9HtZPtq7tPT/JzzifGWCgPqkSnBIIQh4xwR9IuPPb6U6QpS8CAhElJMJmIAJVBFI7bFNKggwQnI1VzVikvbRJtzL3KCbjIqr2ogBlhBQPj3iiIs+FgPwZDqDYL84ToD81InAQgwjBo0F402dSRh+BAEjfo5nnj1O7IsNFvuK5tQRGcy7E8CGmOE8dY0bZeJVwMOQIvEfAI8IdUiZUvVtvz+bGfR8/egT6l0nIcrKpnPyxy8vL627XvL7/bcJmMB0EsA26F6fqoUz4caV9M6Kccobu1SVnZZyMPp634FG/epYDPpCiBrHMBP0VlcBUiYGnvx1R7o8gaEvEGL0ERll0wgxezwlqm+8Xb8JgmSUeygE/NWtv47typonU1jgQjvqJnjK+6NjeNS1Dle8QxxPv0sgqoyyNf3O+yq4jhwXUUbJ201g+ggw0Ks7cKjb+uSCAH1hd2dd/M5nAiZgAiZgAs0J4HHU1GTzo4uPGByeXnxc6VYqiHJxMgETMAETMAETSE8AG5taDFDL5IKAQtuqLGU7mYAJmIAJmMBWJdDmoLsVQUCFU89tbNXOd7tNwARMwARMQASwrW14Byi/FUFAwQRjVQWekcfJBEzABEzABEygHgFsKra1rdSaIKDC8zvn26q3yzUBEzABEzCBLUWgbZvaqiDoz/bXPTu/pXrOjTUBEzABEzCBRAR4Dw02tc3UqiCg4rg32nRxtAnHZZuACZiACZjAuAmMyo62LggAibKZ1BfUjPtC8PlNwARMwAS2LgFsZ/yW2jZJjEQQ0ADe1GZR0GZXumwTMAETMIFpIoDNrPOW01RtHpkgoMIWBam6zeWYgAmYgAlMM4FRiwFYJn91cZ0O4kM30/RVxDptdh4TMAETMAETqEOAmIFRTRPE9RmLIKACCIJUX8CLG+TfJmACJmACJjCpBBAC4wrEH5sgoLNWVld69+/d7/H1PCcTMAETMAET2KoEeOkQ7xlo+9HCKr5jFQSqGN4CPpuc+EvMKt5rEzABEzABE+gkAb3qf1xegRgKgmAxbNgdbxzHb8TAgwcPsmUc5/c5TcAETMAETGCUBPgQYIc+BngbQXA6ADg6SghV55IwWF5e7q2urlZl9T4TMAETMAETmCgCs7Oz2SP4HRIC4ndiLvz6LCydEQRyn/BFJ2ILEAasLQ7UZ16bgAmYgAlMEgFEADECPErY4Y/+/RcPwW8D2L92HS6eA4TByspKJg5Ys43FyQRMwARMwATGTYABLUu/3+8hAlgjANg2Aek4gmBbqOjJsByegAq7iiZgAiZgAiZgAmkJnA3FHZsNymU5/PgobdkuzQRMwARMwARMYEII/C5ogaXs1cXhx6eh0p9MSMVdTRMwARMwARMwgTQE/hw0wL8pKv6Wwcfhb4uCNIBdigmYgAmYgAl0ncBfghj4oyq5IdIhxBR8GHYiDBxTIEpem4AJmIAJmMD0EDgXmvL7IAb+GTdpgyBg5+NAw+PhJ8sHYdkblrG/vCjUwckETMAETMAETKAZgbsh+9Ww/Ccs/wrL34IY2PDNgP8BQKksQoi1naQAAAAASUVORK5CYII=)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#添加事件-2)添加事件

`home.vue`添加`gotoSearch`方法，完成页面跳转。

```javascript
gotoSearch () {
  uni.navigateTo({
    url: '/subcom-pkg/search/search'
  })
}
```

完成效果：

![iShot2021-05-23 23.36.17](./assets/iShot2021-05-23 23.36.17.gif)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#搜索历史功能)搜索历史功能

* 本地数据缓存
* 新的搜索数据显示在最前面
* 点击垃圾桶删除所有数据，并只显示热门推荐
* 点击标签进行快速搜索

本地数据缓存：

```js
data () {
  return {
    // ...
    historyList: uni.getStorageSync('historyList') || [],
  }
},
```

添加对应的事件：

```js
// 添加本地缓存
addHis (value) {
  // 标签去重
  const index = this.historyList.indexOf(value)
  if (index !== -1) {
    this.historyList.splice(index, 1)
  }
  // 最近搜索的标签，显示在最前端
  this.historyList.unshift(value)
  // 本地缓存
  uni.setStorageSync('historyList', this.historyList)
},
clearSearch () {
  // 当用户点击搜索框右侧的清空按钮的逻辑
  this.showResult = false
  this.searchResults = []
  this.page = {
    title: '',
    page: 0,
    limit: 20
  }
  this.loading = false
},
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#搜索建议-列表)搜索建议(列表)

* 完成推荐列表样式
* 设置请求参数
* 使用统一发送文章请求
* 本地展示列表，并隐藏推荐历史与热门推荐
* 清空搜索条件时，把推荐列表清空，并展示搜索历史&热门推荐

结构部分：

```vue
<template>
  <view class="search">
    <!-- 搜索框 -->
    <view class="search-box">
      <u-search v-model="searchValue" @clear="clearSearch"></u-search>
    </view>
    <!-- 搜索建议列表 -->
    <view class="list" v-if="searchResults.length !== 0">
      <view class="item" v-for="(item) in searchResults" :key="item._id" @click="gotoDetail(item)">
        <view class="name">{{item.title}}</view>
        <u-icon name="arrow-right" size="25"></u-icon>
      </view>
    </view>
    <view class="list no-result" v-else-if="searchResults.length === 0 && showResult">
      这里空空如也~
    </view>
    <!-- 搜索历史 -->
    <view class="history-box" v-else-if="historyList.length !== 0 && searchResults.length === 0">
      <view class="history-title">
        <text>搜索历史</text>
        <u-icon name="trash" size="32" @click="clearStorage"></u-icon>
      </view>
      <!-- 标签列表区域 -->
      <view class="history-list">
        <uni-tag :text="item" v-for="(item,i) in historyList" :key="i" @click="quickSearch(item)"></uni-tag>
      </view>
    </view>
    <!-- 热门推荐 -->
    <view class="history-box" v-if="!showResult">
      <view class="history-title">
        <text>热门推荐</text>
      </view>
      <!-- 标签列表区域 -->
      <view class="history-list">
        <uni-tag :text="item" v-for="(item,i) in hotList" :key="i" @click="quickSearch(item)"></uni-tag>
      </view>
    </view>
  </view>
</template>
```

样式部分：

```scss
// ...
.list {
  padding: 0 5px;
  .item {
    font-size: 12px;
    padding: 13px 0;
    border-bottom: 1px solid #efefef;
    display: flex;
    align-items: center;
    justify-content: space-between;
    &:last-child {
      border-bottom: none;
    }
    .name {
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      margin-right: 3px;
    }
  }
  &.no-result {
    text-align: center;
    color: #666;
    font-size: 28rpx;
  }
}
```

methods部分:

```js
// 发起请求
async getList () {
  if (this.loading) return
  this.loading = true
  this.showResult = true
  const { data } = await this.$u.api.getList(this.page)
  this.searchResults = [...this.searchResults, ...data]
  this.loading = false
},
// 点击热门推荐，快速搜索
quickSearch (item) {
  this.searchValue = item
  this.page.title = item
  // 添加到搜索历史
  this.addHis(item)
  // 发送搜索请求 -> 请求文件列表
  this.getList()
},
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#搜索按钮事件绑定)搜索按钮事件绑定

```vue
<u-search v-model="searchValue" @clear="clearSearch" @search="search" @custom="search"></u-search>
```

添加对应的search方法：

```js
search (value) {
  if (value.trim() === '') {
    uni.showToast({
      icon: 'error',
      title: '关键词不得为空',
      duration: 2000
    })
    return
  }
  this.page.title = value
  this.quickSearch(value)
},
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#整体代码)整体代码

```vue
<template>
  <view class="search">
    <!-- 搜索框 -->
    <view class="search-box">
      <u-search v-model="searchValue" @clear="clearSearch" @search="search" @custom="search"></u-search>
    </view>
    <!-- 搜索建议列表 -->
    <view class="list" v-if="searchResults.length !== 0">
      <view class="item" v-for="(item) in searchResults" :key="item._id" @click="gotoDetail(item)">
        <view class="name">{{item.title}}</view>
        <u-icon name="arrow-right" size="25"></u-icon>
      </view>
    </view>
    <view class="list no-result" v-else-if="searchResults.length === 0 && showResult">
      这里空空如也~
    </view>
    <!-- 搜索历史 -->
    <view class="history-box" v-else-if="historyList.length !== 0 && searchResults.length === 0">
      <view class="history-title">
        <text>搜索历史</text>
        <u-icon name="trash" size="32" @click="clearStorage"></u-icon>
      </view>
      <!-- 标签列表区域 -->
      <view class="history-list">
        <uni-tag :text="item" v-for="(item,i) in historyList" :key="i" @click="quickSearch(item)"></uni-tag>
      </view>
    </view>
    <!-- 热门推荐 -->
    <view class="history-box" v-if="!showResult">
      <view class="history-title">
        <text>热门推荐</text>
      </view>
      <!-- 标签列表区域 -->
      <view class="history-list">
        <uni-tag :text="item" v-for="(item,i) in hotList" :key="i" @click="quickSearch(item)"></uni-tag>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data () {
    return {
      searchValue: '',
      historyList: uni.getStorageSync('historyList') || [],
      hotList: ['前端', 'vue', 'node', '面试', 'react', 'devops', 'flutter', 'MySQL', 'gitlab', 'redis', 'git', 'typescript', '升职', 'B站'],
      page: {
        title: '',
        page: 0,
        limit: 20
      },
      loading: false,
      searchResults: [],
      showResult: false
    }
  },
  methods: {
    search (value) {
      if (value.trim() === '') {
        uni.showToast({
          icon: 'error',
          title: '关键词不得为空',
          duration: 2000
        })
        return
      }
      this.page.title = value
      this.quickSearch(value)
    },
    addHis (value) {
      // 标签去重
      const index = this.historyList.indexOf(value)
      if (index !== -1) {
        this.historyList.splice(index, 1)
      }
      // 最近搜索的标签，显示在最前端
      this.historyList.unshift(value)
      // 本地缓存
      uni.setStorageSync('historyList', this.historyList)
    },
    async getList () {
      if (this.loading) return
      this.loading = true
      this.showResult = true
      const { data } = await this.$u.api.getList(this.page)
      this.searchResults = [...this.searchResults, ...data]
      this.loading = false
    },
    // 点击热门推荐，快速搜索
    quickSearch (item) {
      this.searchValue = item
      this.page.title = item
      // 添加到搜索历史
      this.addHis(item)
      // 发送搜索请求 -> 请求文件列表
      this.getList()
    },
    clearSearch () {
      // 当用户点击搜索框右侧的清空按钮的逻辑
      this.showResult = false
      this.searchResults = []
      this.page = {
        title: '',
        page: 0,
        limit: 20
      }
      this.loading = false
    },
    gotoDetail (item) {
      // 文章详情
      console.log('🚀 ~ file: search.vue ~ line 99 ~ gotoDetail ~ item', item)
    },
    clearStorage () {
      this.historyList = []
      uni.setStorageSync('historyList', [])
    }
  }
}
</script>

<style lang="scss" scoped>
.search {
  padding: 24rpx;
}

.search-box {
  padding-bottom: 50rpx;
}

.history-box {
  padding: 0 10rpx 50rpx;
  .history-title {
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 40px;
    font-size: 16px;
    font-weight: bold;
  }

  .history-list {
    display: flex;
    flex-wrap: wrap;
    ::v-deep .uni-tag {
      margin-top: 5px;
      margin-right: 5px;
      border-radius: 25rpx;
    }
  }
}

.list {
  padding: 0 5px;
  .item {
    font-size: 12px;
    padding: 13px 0;
    border-bottom: 1px solid #efefef;
    display: flex;
    align-items: center;
    justify-content: space-between;
    &:last-child {
      border-bottom: none;
    }
    .name {
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      margin-right: 3px;
    }
  }
  &.no-result {
    text-align: center;
    color: #666;
    font-size: 28rpx;
  }
}
</style>
```

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#发帖入口)发帖入口

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#布局与样式-5)布局与样式

在`home.vue`添加发帖入口

```vue
<image class="add-post" src="/static/images/add-post.png" />
```

修改`add-post`样式

```css
.add-post {
  position: fixed;
  width: 150rpx;
  height: 150rpx;
  bottom: 30rpx;
  right: 10rpx;
  z-index: 999;
}
```

完成效果

![image-20210527004752928](uniapp.assets/image-20210527004752928.affac181.png)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/02-首页模块.html#添加事件-3)添加事件

添加点击事件，点击跳转发帖页面

```javascript
newContent () {
  uni.navigateTo({
    url: '/subcom-pkg/post/post'
  })
}
```



# [#](https://front-end.toimc.com/notes-page/project/community-miniapp/03-登录鉴权.html#小程序鉴权登录)小程序鉴权登录

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/03-登录鉴权.html#需求分析)需求分析

流程分析：

![img](uniapp.assets/api-login.2fcc9f35.2fcc9f35.jpg)

说明：

* 时序图的阅读方式：
  * 搞清楚有几方，哪几个重要的交互部分，比如：上面有小程序、开发服务器、小程序官方后台；
  * 从上至下，从左至右，跟随箭头的方向走；
  * 时序图中，任何一个流程中断，后续的相关流程不会继续进行；

通过分析，我们获取用户的信息用于创建用户，并通过自己的服务器返回用户的登录态，即token信息与用户信息。

用户信息需要跨页面进行共享，同时，也需要持久化，所以惯性的思考到了 vuex + 本地缓存的方案。

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/03-登录鉴权.html#vuex集成uniapp)Vuex集成uniapp

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/03-登录鉴权.html#初始化store)初始化store

重要：uniapp中内置vuex，所以直接按照vue中使用vuex的步骤进行集成。

创建文件`store/index.js`：

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex);//vue的插件机制

//Vuex.Store 构造器选项
const store = new Vuex.Store({
    state:{
      //存放状态
      // ...
    }
})
export default store
```

在 `main.js` 中导入文件：

```js
import Vue from 'vue'
import App from './App'
import store from './store'

// 把 store 对象提供给 “store” 选项，这可以把 store 的实例注入所有的子组件
const app = new Vue({
    store,
    ...App
})
app.$mount()
```

state使用上的区别：

* 直接在template中不能使用`$store`取值；
* 在data中无法使用`this.$store.state`取对应的初始值；

正确的打开姿势：

* 辅助函数（推荐）
* computed方法

```js
// store.js
const state = {
  count: 0,
  msg: 'hello Vuex'
}


// 组件中
computed: {
  // 方法一：辅助函数
  // ...mapState(['count']),
  // ...mapState({
  //   msg2: (state) => state.msg
  // }),
  // 方法二：computed
  count1 () {
    return this.$store.state.count
  },
}
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/03-登录鉴权.html#如何进行调试)如何进行调试

```js
import Vue from 'vue'
import Vuex from 'vuex'
import createLogger from 'vuex/dist/logger'

// 获取执行环境变量
const debug = process.env.NODE_ENV === 'development'
// console.log('🚀 ~ file: index.js ~ line 5 ~ debug', debug)

Vue.use(Vuex)

const state = {
  count: 0,
  msg: 'hello Vuex'
}

const mutations = {
  // 测试Mutation
  add (state, payload) {
    state.count += payload
  }
}

const actions = {}

const getters = {}

// 添加vuex日志插件
const plugins = debug ? [createLogger()] : []

const store = new Vuex.Store({
  state,
  mutations,
  actions,
  getters,
  plugins
})

export default store
```

当我们在页面触发mutation的时候：

```js
mounted () {
  this.getList()
  setTimeout(() => {
    this.$store.commit('add', 100)
  }, 2000)
  // console.log(this.$store)
},
```

即可以收到console中的打印日志：

![image-20210723135408789](uniapp.assets/image-20210723135408789.c89a35bf.png)

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/03-登录鉴权.html#微信官方服务相关)微信官方服务相关

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/03-登录鉴权.html#获取appid与appsecret)获取AppID与AppSecret

登录小程序的后台 [https://mp.weixin.qq.com/(opens new window)](https://mp.weixin.qq.com/)

![image-20210724102639423](uniapp.assets/image-20210724102639423.71ad0747.png)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/03-登录鉴权.html#unionid与openid)unionid与openid

* 什么是unionid，openid？

  openid是用户在当前小程序的唯一标识；

  unionid是用户在开放平台的唯一标识符，若当前小程序已绑定到微信开放平台帐号下会返回；

* 两者的区别在哪里？unionid的作用是什么？

  都是唯一标识，针对的平台不一样，openid针对小程序，unionid针对开放平台；

  开放平台是指：[https://open.weixin.qq.com/(opens new window)](https://open.weixin.qq.com/)

  如果开发者拥有多个移动应用、网站应用、和公众帐号（包括小程序），可通过 UnionID 来区分用户的唯一性，因为只要是同一个微信开放平台帐号下的移动应用、网站应用和公众帐号（包括小程序），用户的 UnionID 是唯一的。

  **换句话说，同一用户，对同一个微信开放平台下的不同应用，UnionID是相同的。**

  **如果没有微信侧多应用场景（公众号、小程序、网页互通的需求），可以不用弄这个开放平台的账号。**

* unionID如何获取？

  绑定了开放平台开发者帐号的小程序，可以通过以下途径获取 UnionID。

  1. 开发者可以直接通过 [wx.login (opens new window)](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/login/wx.login.html)+ `code2Session` 获取到该用户 UnionID，无须用户授权。
  2. 小程序端调用云函数时，可在云函数中通过 [Cloud.getWXContext (opens new window)](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/reference-sdk-api/utils/Cloud.getWXContext.html)获取 UnionID。

* 开放平台如何注册与使用？

  开放平台需要在[开放平台地址 (opens new window)](https://open.weixin.qq.com/)注册，同时需要认证后才能使用。

  ![image-20210724102333624](uniapp.assets/image-20210724102333624.d28d20ad.png)

  认证费用300元（xxxx马x腾）

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/03-登录鉴权.html#获取用户opendata)获取用户OpenData

登录凭证校验，通过 [wx.login (opens new window)](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/login/wx.login.html)接口获得临时登录凭证 code 后传到开发者服务器调用此接口完成登录流程。更多使用方法详见 [小程序登录 (opens new window)](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/login.html)。

```js
// 记录微信相关的接口， API项目 -> 微信官方服务器
import axios from 'axios'
import config from '@/config'

const instance = axios.create({
  timeout: 10000
})

export const wxGetOpenData = async (code) => {
  const res = await instance.get(`https://api.weixin.qq.com/sns/jscode2session?appid=${config.AppID}&secret=${config.AppSecret}&js_code=${code}&grant_type=authorization_code`)
  console.log('🚀 ~ file: WxUtils.js ~ line 11 ~ wxGetOpenData ~ res', res)
}
```

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/03-登录鉴权.html#鉴权页面样式)鉴权页面样式

```vue
<template>
  <div class="auth">
    <view class="title">
      <image src="/static/images/logo.jpg" mode="aspectFill" />
      <text>toimc技术社区</text>
    </view>
    <view class="container">
      <u-button type="primary" @click="login" hover-class="none">
        <u-icon name="weixin-fill" size="32" color="#fff"></u-icon>
        <text>微信登录</text>
      </u-button>
      <u-button plain :custom-style="customStyle" hover-class="none" @click="goto">手机号登录</u-button>
    </view>
    <view class="forbid" @click="leave">暂不登录</view>
    <u-toast ref="uToast" />
  </div>
</template>

<script>
import { mapMutations, mapGetters } from 'vuex'
import auth from '@/mixins/auth'

export default {
  components: {},
  data: () => ({
    customStyle: {
      'margin-top': '40rpx',
      'background-color': '#fff',
      color: '#02d199'
    }
  }),
  mixins: [auth],
  computed: {
    ...mapGetters(['isLogin'])
  },
  methods: {
    ...mapMutations(['setIsLogin', 'setWxInfo', 'setToken', 'setUserInfo']),
    goto () {
      uni.navigateTo({
        url: '/subcom-pkg/auth/mobile-login'
      })
    },
    login () {
      // 获取用户信息
      // todo
    },
    leave () {
			// todo
      uni.navigateBack()
    }
  },
  watch: {}
}
</script>

<style lang="scss">
.title {
  display: flex;
  flex-flow: column nowrap;
  align-items: center;
  justify-content: center;
  height: 500rpx;
  width: 100vw;
  image {
    width: 168rpx;
    height: 168rpx;
    border-radius: 50%;
    box-shadow: 0 0 10px rgba($color: #000000, $alpha: 0.1);
  }
  text {
    margin-top: 32rpx;
    font-size: 32rpx;
    color: #333;
    font-weight: 500;
  }
}

.container {
  padding: 32rpx;
}

.forbid {
  position: fixed;
  bottom: 60px;
  left: 0;
  width: 100%;
  text-align: center;
  font-size: 28rpx;
  text-decoration: underline;
}
</style>
```

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/03-登录鉴权.html#解密微信数据)解密微信数据

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/03-登录鉴权.html#用户信息的解密)用户信息的解密

* 小程序调用`getUserProfile`，获取用户加密数据 + 证书，请求用户后台；

* 后台进行加密数据的校验，确定用户传递的数据是微信侧的数据；

  signature = sha1( rawData + session_key )算是校验成功；

* 加密数据解密，参考官方的[代码 (opens new window)](https://res.wx.qq.com/wxdoc/dist/assets/media/aes-sample.eae1f364.zip)；

官方说明[链接(opens new window)](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/signature.html#加密数据解密算法)

![img](uniapp.assets/signature.8a30a825.8a30a825.jpg)

代码示例：

微信解密工具js：

```js
import crypto from 'crypto'

function WXBizDataCrypt (appId, sessionKey) {
  this.appId = appId
  this.sessionKey = sessionKey
}

WXBizDataCrypt.prototype.decryptData = function (encryptedData, iv) {
  // base64 decode
  let sessionKey = Buffer.from(this.sessionKey, 'base64')
  encryptedData = Buffer.from(encryptedData, 'base64')
  iv = Buffer.from(iv, 'base64')
  let decoded
  try {
    // 解密
    let decipher = crypto.createDecipheriv('aes-128-cbc', sessionKey, iv)
    // 设置自动 padding 为 true，删除填充补位
    decipher.setAutoPadding(true)
    decoded = decipher.update(encryptedData, 'binary', 'utf8')
    decoded += decipher.final('utf8')

    decoded = JSON.parse(decoded)
  } catch (err) {
    throw new Error('Illegal Buffer')
  }

  if (decoded.watermark.appid !== this.appId) {
    throw new Error('Illegal Buffer')
  }

  return decoded
}

export default WXBizDataCrypt
```

微信工具js的封装：

```js
// 记录微信相关的接口， API项目 -> 微信官方服务器
import axios from 'axios'
import config from '@/config'
import crypto from 'crypto'
import WXBizDataCrypt from './WXBizDataCrypt'

const instance = axios.create({
  timeout: 10000
})

// 获取session_key，openid 等OpenData数据
export const wxGetOpenData = async (code) => {
  // sessin_key, openid, unoinid
  const res = await instance.get(`https://api.weixin.qq.com/sns/jscode2session?appid=${config.AppID}&secret=${config.AppSecret}&js_code=${code}&grant_type=authorization_code`)
  // console.log('🚀 ~ file: WxUtils.js ~ line 11 ~ wxGetOpenData ~ res', res)
  return res.data
}

// 获取解密用户的信息
export const wxGetUserInfo = async (user, code) => {
  // 1.获取用户的openData -> session_key
  const data = await wxGetOpenData(code)
  const sessionKey = data.session_key
  // 2.用户数据进行签名校验 -> sha1 -> session_key + rawData + signature
  const { rawData, signature, encryptedData, iv } = user
  const sha1 = crypto.createHash('sha1')
  sha1.update(rawData)
  sha1.update(sessionKey)
  if (sha1.digest('hex') !== signature) {
    // 校验失败
    return Promise.reject(
      new Error({
        code: 500,
        msg: '签名校验失败'
      })
    )
  }
  const wxBizDataCrypt = new WXBizDataCrypt(config.AppID, sessionKey)
  // 3.用户加密数据的解密
  const userInfo = wxBizDataCrypt.decryptData(encryptedData, iv)
  return { ...userInfo, ...data }
}
```

小程序侧：

```js
async login () {
  uni.login({
    success: (e) => {
      this.code = e.code
    }
  })
  uni.getUserProfile({
    lang: 'zh_CN',
    desc: '用于完善会员资料',
    success: async (e) => {
      console.log('🚀 ~ file: auth.vue ~ line 37 ~ test ~ e', e)
      await this.$u.api.wxLogin({
        code: this.code,
        user: e
      })
    },
    fail: (e) => {
      console.log('🚀 ~ file: auth.vue ~ line 40 ~ test ~ e', e)
    }
  })
}
```

需要注意的点：

* uni.login不需要用户感知的触发与uni.getUserProfile需要tap（用户点击）事件触发；
* uni.login与uni.getUserProfile如果需要同时使用，可以都使用callback回调的形式来使用；

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/03-登录鉴权.html#用户登录凭证维护)用户登录凭证维护

防止code失效，而请求失败。

解决办法：前端设置定时任务，每隔<5分钟的时间，请求一次`uni.login`刷新登录凭证。

```js
<script>

export default {
  components: {},
  data: () => ({
    code: '',
    ctrl: null
  }),
  computed: {},
  created () {
    this.getNewCode()
    this.setCron()
  },
  onShow () {
    this.getNewCode()
    this.setCron()
  },
  onHide () {
    clearTimeout(this.ctrl)
  },
  // 用户离开当前页面
  onUnload () {
    clearTimeout(this.ctrl)
  },
  methods: {
    // 获取code
    getNewCode () {
      uni.login({
        success: (e) => {
          this.code = e.code
        }
      })
    },
    // 设置定时任务
    setCron () {
      clearTimeout(this.ctrl)
      // 定时刷新code的方法
      this.ctrl = setTimeout(() => {
        this.getNewCode()
        // 重新进行cron，保证code的有效性
        this.setCron()
      }, 4 * 60 * 1000)
    },
  },
}
</script>
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/03-登录鉴权.html#用户登录接口)用户登录接口

**接口部分:**

产生token：

```js
// 生成 token 返回给客户端
const generateToken = (payload, expire = '1h') => {
  if (payload) {
    return jwt.sign(
      {
        ...payload,
      },
      config.JWT_SECRET,
      { expiresIn: expire }
    )
  } else {
    throw new Error('生成token失败！')
  }
}
```

随机用户名：

```js
const rand = (len = 8) => {
  const possible = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789'
  let text = ''
  for (let i = 0; i < len; i++) {
    text += possible.charAt(Math.floor(Math.random() * possible.length))
  }
  return text
}

const getTempName = () => {
  // 返回用户邮箱
  return 'toimc_' + rand() + '@toimc.com'
}
```

微信解密用户信息的`wxUtils.js`：

```js
// 获取解密用户的信息
export const wxGetUserInfo = async (user, code) => {
  // 1.获取用户的openData -> session_key
  const data = await wxGetOpenData(code)
  const { session_key: sessionKey } = data
  if (sessionKey) {
    // 2.用户数据进行签名校验 -> sha1 -> session_key + rawData + signature
    const { rawData, signature, encryptedData, iv } = user
    const sha1 = crypto.createHash('sha1')
    sha1.update(rawData)
    sha1.update(sessionKey)
    if (sha1.digest('hex') !== signature) {
      // 校验失败
      return Promise.reject(
        new Error({
          code: 500,
          msg: '签名校验失败'
        })
      )
    }
    const wxBizDataCrypt = new WXBizDataCrypt(config.AppID, sessionKey)
    // 3.用户加密数据的解密
    const userInfo = wxBizDataCrypt.decryptData(encryptedData, iv)
    return { ...userInfo, ...data, errcode: 0 }
  } else {
    // data -> errcode非0 ，请求失败
    return data
  }
}
```

查询并创建用户`User.js`

```js
findOrCreateByUnionid: function (user) {
  return this.findOne({
    unionid: user.unionid
    // openid: user.openid
  }, {
    unionid: 0, password: 0
  }).then(obj => {
    return (
      obj || this.create({
        openid: user.openid,
        unionid: user.unionid,
        username: getTempName(),
        name: user.nickName,
        roles: ['user'],
        gender: user.gender,
        pic: user.avatarUrl,
        location: user.city
      })
    )
  })
},
```

`LoginController.js`微信登录接口：

```js
// 微信登录
async wxLogin (ctx) {
  // 1.解密用户信息
  const { body } = ctx.request
  // console.log('🚀 ~ file: LoginController.js ~ line 223 ~ LoginController ~ wxLogin ~ body', body)
  const { user, code } = body
  if (!code) {
    ctx.body = {
      code: 500,
      data: '没有足够参数'
    }
    return
  }
  const res = await wxGetUserInfo(user, code)
  if (res.errcode === 0) {
    // 2.查询数据库 -> 判断用户是否存在
    // 3.如果不存在 —> 创建用户
    // 4.如果存在 -> 获取用户信息
    const tmpUser = await User.findOrCreateByUnionid(res)
    // 5.产生token，获取用户的签到状态
    const token = generateToken({ _id: tmpUser._id })
    const userInfo = await addSign(tmpUser)
    ctx.body = {
      code: 200,
      data: userInfo,
      token
    }
  } else {
    ctx.throw(501, res.errcode === 40163 ? 'code已失效，请刷新后重试' : '获取用户信息失败，请重试')
  }
}
```

获取access_token，并按两小时维护：

```js
export const wxGetAccessToken = async (flag = false) => {
  // https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid=APPID&secret=APPSECRET
  let accessToken = await getValue('accessToken')
  if (!accessToken || flag) {
    try {
      const result = await instance.get(
        `https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid=${config.AppID}&secret=${config.AppSecret}`
      )
      if (result.status === 200) {
        // 说明请求成功
        await setValue(
          'accessToken',
          result.data.access_token,
          result.data.expires_in
        )
        accessToken = result.data.access_token
        // {"errcode":40013,"errmsg":"invalid appid"}
        if (result.data.errcode && result.data.errmsg) {
          logger.error(
            `Wx-GetAccessToken Error: ${result.data.errcode} - ${result.data.errmsg}`
          )
        }
      }
    } catch (error) {
      logger.error(`GetAccessToken Error: ${error.message}`)
    }
  }
  return accessToken
}
```

维护`access_token`数据：

```js
import { CronJob } from 'cron'
import { wxGetAccessToken } from './WxUtils'

const job = new CronJob('* 55 */1 * * *', () => {
  wxGetAccessToken()
})

job.start()
```

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/03-登录鉴权.html#手机登录)手机登录

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/03-登录鉴权.html#业务流程图)业务流程图

* 获取手机号 -> 发送短信验证码页面

![img](uniapp.assets/image-20210730163452597.76d98020.png)

* 输入验证码 -> 手机号登录页面

![img](uniapp.assets/image-20210730163719652.14834a3e.png)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/03-登录鉴权.html#如何选择短信服务商)如何选择短信服务商

推荐腾讯云、阿里云，下面以腾讯云为示例：

![image-20210726203051548](uniapp.assets/image-20210726203051548.4d7929a2.png)

基本的使用步骤：

1. 创建云平台账号 -> 实名 -> 推荐公司实名
2. 够买短信包 -> 添加短信模板
3. 集成SDK发送短信

腾讯云的两种集成方式：

1. qcloudsms_js，参考[github仓库(opens new window)](https://github.com/qcloudsms/qcloudsms_js)
2. Tencentcloud-sdk-nodejs，参考[官方文档(opens new window)](https://cloud.tencent.com/document/product/382/43197)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/03-登录鉴权.html#登录页面样式)登录页面样式

`auth/mobile-login.vue`页面

```vue
<template>
  <view class="wrapper">
    <view class="title">登录注册更精彩</view>
    <view class="info">未注册的手机号验证后自动创建账号</view>
    <view class="form">
      <u-form label-width="120">
        <u-form-item label="手机号">
          <u-input v-model="mobile" placeholder="请输入手机号"></u-input>
          <template v-slot:right>
            <u-button type="primary" shape="circle" size="mini" hover-class="none">获取手机号</u-button>
          </template>
        </u-form-item>
      </u-form>
      <view class="btn" :class="{'deactive': !/^1[3-9]\d{9}$/.test(mobile)}">
        <u-button type="primary" hover-class="none" @click="getCode">获取验证码</u-button>
      </view>
    </view>
    <navigator open-type="navigateBack" hover-class="none">
      <view class="footer u-flex u-col-center u-row-center">
        <u-icon name="weixin-circle-fill" size="120" color="#1BB723"></u-icon>
        <text>微信登录</text>
      </view>
    </navigator>
  </view>
</template>

<script>
import auth from '@/mixins/auth'
export default {
  mixins: [auth],
  data: () => ({
    mobile: '',
    phoneNumber: '',
    loading: false
  }),
  methods: {
    async getCode () {
      // 发送短信验证码
    },
  }
}
</script>

<style lang="scss" scoped>
.wrapper {
  padding: 32rpx;
  .title {
    color: #333;
    font-size: 48rpx;
    font-weight: bold;
    padding-top: 50rpx;
  }
  .info {
    color: #666;
    line-height: 28rpx;
    padding-top: 24rpx;
  }
  .form {
    padding-top: 40rpx;
  }
  ::v-deep .btn {
    padding-top: 80rpx;
    &.deactive {
      .u-btn--primary {
        background-color: #ccc;
      }
    }
  }
}
.footer {
  position: absolute;
  bottom: 120rpx;
  width: 100vw;
  left: 0;
  text-align: center;
  flex-direction: column;
  text {
    color: #666;
    font-size: 28rpx;
    padding-top: 14rpx;
  }
}
</style>
```

`mobile-code.vue`页面：

```vue
<template>
  <view class="wrapper">
    <view class="title">输入验证码</view>
    <view class="info">验证码已发送至 {{mobile.substr(0,3)}}****{{mobile.substr(7,10)}} </view>
    <view class="inputs">
      <u-message-input :maxlength="6" mode="bottomLine" active-color="#02d199" inactive-color="#DDD" width="90" :bold="false" :breathe="false" :focus="true"></u-message-input>
    </view>
    <view class="resend" :class="{'disabled': sending}" @click="resend()">{{msg}}</view>
  </view>
</template>

<script>
import { mapMutations } from 'vuex'
export default {
  data: () => ({
    mobile: '',
    count: 60,
    ctrl: null,
    sending: false
  }),
  onLoad (options) {
    // console.log('🚀 ~ file: mobile-code.vue ~ line 15 ~ onLoad ~ options', options)
    this.mobile = options.mobile
    this.setCron()
  },
  methods: {
    ...mapMutations(['setToken', 'setUserInfo']),
    setCron () {
      clearInterval(this.ctrl)
      this.sending = true
      this.ctrl = setInterval(() => {
        this.count--
        if (this.count === 0) {
          this.count = 60
          this.sending = false
          clearInterval(this.ctrl)
        }
      }, 1000)
    },
    async resend () {
      if (this.sending) return
      const res = await this.$u.api.sendCode({
        phone: this.mobile
      })
      // console.log('🚀 ~ file: mobile-code.vue ~ line 75 ~ resend ~ res', res)
      this.setCron()
    }
  },
  computed: {
    msg () {
      let str = '重新获取'
      if (this.sending) {
        str += ('(' + (this.count + '').padStart(2, '0') + 's)')
      }
      return str
    }
  }
}
</script>

<style lang="scss" scoped>
.wrapper {
  padding: 32rpx;
  .title {
    color: #333;
    font-size: 48rpx;
    font-weight: bold;
    padding-top: 50rpx;
  }
  .info {
    color: #666;
    line-height: 28rpx;
    padding-top: 24rpx;
  }
  .inputs {
    padding: 60rpx 0 40rpx;
  }
  .resend {
    font-size: 26rpx;
    font-weight: 300;
    color: #02d199;
    &.disabled {
      color: #ddd;
    }
  }
}
</style>
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/03-登录鉴权.html#小程序获取手机号)小程序获取手机号

小程序侧逻辑：

设置open-type为`getPhoneNumber`：

```js
<u-button type="primary" shape="circle" size="mini" hover-class="none" open-type="getPhoneNumber" @getphonenumber="getPhoneNumber">获取手机号</u-button>
```

getPhoneNumber方法：

```js
async getPhoneNumber (value) {
  const { encryptedData, iv } = value.detail
  // value.detail + code
  const { code, data } = await this.$u.api.getMobile({
    code: this.code,
    encryptedData,
    iv
  })
  if (code === 200) {
    const { phoneNumber, purePhoneNumber, countryCode } = data
    if (countryCode !== '86') {
      uni.showToast({
        title: 'Please use a cell phone number from mainland China',
        duration: 2000
      })
      return
    }
    this.phoneNumber = phoneNumber
    this.mobile = purePhoneNumber
  } else {
    uni.showToast({
      icon: 'error',
      title: '获取手机号失败，请重试',
      duration: 2000
    })
  }
  // console.log('🚀 ~ file: mobile-login.vue ~ line 48 ~ getPhoneNumber ~ res', res)
}
```

API接口解密数据：

设置路由：

```js
router.post('/getMobile', loginController.getMobile)
```

获取手机号`LoginController.js`：

```js
// 获取用户手机号
async getMobile (ctx) {
  const { body } = ctx.request
  const { code, encryptedData, iv } = body
  if (!code) {
    ctx.body = {
      code: 500,
      data: '没有足够参数'
    }
    return
  }
  const { session_key: sessionKey } = await wxGetOpenData(code)
  const wxBizDataCrypt = new WXBizDataCrypt(config.AppID, sessionKey)
  // 3.用户加密数据的解密
  const data = wxBizDataCrypt.decryptData(encryptedData, iv)
  // console.log('🚀 ~ file: LoginController.js ~ line 274 ~ LoginController ~ getMobile ~ data', data)
  ctx.body = {
    code: 200,
    data,
    msg: '获取手机号成功'
  }
}
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/03-登录鉴权.html#前后端逻辑-联调)前后端逻辑&联调

常见问题：

* 参数传递异常：

  ![image-20210730161557953](uniapp.assets/image-20210730161557953.500dcdbf.png)

  解决办法：确保前后端传递的参数的正确性，比如，统一参数名称，统一请求methods。

* 短信服务商的问题

  ![image-20210730161718446](uniapp.assets/image-20210730161718446.59b97977.png)

  解决办法：1. 余额不足即充值；2. 传参非法，修改参数；3. 签名或者模板问题，修改对应的模板参数后再进行发送。

* 需要删除已经使用过的短信验证码数据；

* 如果给用户已经发送一次短信验证码，则不再相同的时间间隔内，再给用户发送，限制用户发送短信的频次；

小程序侧接口：

```js
// 获取用户手机号
const getMobile = (data) => axios.post('/login/getMobile', data)

// 发送短信
const sendCode = params => axios.get('/public/sendCode', params)
```

完成的手机发送验证码页面

```vue
<template>
  <view class="wrapper">
    <view class="title">登录注册更精彩</view>
    <view class="info">未注册的手机号验证后自动创建账号</view>
    <view class="form">
      <u-form label-width="120">
        <u-form-item label="手机号">
          <u-input v-model="mobile" placeholder="请输入手机号"></u-input>
          <template v-slot:right>
            <u-button type="primary" shape="circle" size="mini" hover-class="none" open-type="getPhoneNumber" @getphonenumber="getPhoneNumber">获取手机号</u-button>
          </template>
        </u-form-item>
      </u-form>
      <view class="btn" :class="{'deactive': !/^1[3-9]\d{9}$/.test(mobile)}">
        <u-button type="primary" hover-class="none" @click="getCode">获取验证码</u-button>
      </view>
    </view>
    <navigator open-type="navigateBack" hover-class="none">
      <view class="footer u-flex u-col-center u-row-center">
        <u-icon name="weixin-circle-fill" size="120" color="#1BB723"></u-icon>
        <text>微信登录</text>
      </view>
    </navigator>
  </view>
</template>

<script>
import auth from '@/mixins/auth'
export default {
  mixins: [auth],
  data: () => ({
    mobile: '',
    phoneNumber: '',
    loading: false
  }),
  methods: {
    async getCode () {
      // 发送短信验证码
      // 防止用户频繁发送
      if (this.loading) return
      this.loading = true
      try {
        const res = await this.$u.api.sendCode({
          mobile: this.mobile
        })
        this.loading = false
        if (res.code === 200) {
        // 提示用户
          uni.showToast({
            icon: 'none',
            title: '发送成功',
            duration: 2000
          })
          // 延迟跳转到输入验证码的页面
          setTimeout(() => {
            uni.navigateTo({
              url: '/subcom-pkg/auth/mobile-code?mobile=' + this.mobile
            })
          }, 2000)
        } else {
          uni.showToast({
            icon: 'error',
            title: '短信发送失败',
            duration: 2000
          })
        }
      } catch (error) {
        this.loading = false
      }
    },
    async getPhoneNumber (value) {
      const { encryptedData, iv } = value.detail
      // value.detail + code
      const { code, data } = await this.$u.api.getMobile({
        code: this.code,
        encryptedData,
        iv
      })
      if (code === 200) {
        const { phoneNumber, purePhoneNumber, countryCode } = data
        if (countryCode !== '86') {
          uni.showToast({
            title: 'Please use a cell phone number from mainland China',
            duration: 2000
          })
          return
        }
        this.phoneNumber = phoneNumber
        this.mobile = purePhoneNumber
      } else {
        uni.showToast({
          icon: 'error',
          title: '获取手机号失败，请重试',
          duration: 2000
        })
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.wrapper {
  padding: 32rpx;
  .title {
    color: #333;
    font-size: 48rpx;
    font-weight: bold;
    padding-top: 50rpx;
  }
  .info {
    color: #666;
    line-height: 28rpx;
    padding-top: 24rpx;
  }
  .form {
    padding-top: 40rpx;
  }
  ::v-deep .btn {
    padding-top: 80rpx;
    &.deactive {
      .u-btn--primary {
        background-color: #ccc;
      }
    }
  }
}
.footer {
  position: absolute;
  bottom: 120rpx;
  width: 100vw;
  left: 0;
  text-align: center;
  flex-direction: column;
  text {
    color: #666;
    font-size: 28rpx;
    padding-top: 14rpx;
  }
}
</style>
```

校验验证码的页面：

```vue
<template>
  <view class="wrapper">
    <view class="title">输入验证码</view>
    <view class="info">验证码已发送至 {{mobile.substr(0,3)}}****{{mobile.substr(7,10)}} </view>
    <view class="inputs">
      <u-message-input :maxlength="6" mode="bottomLine" active-color="#02d199" inactive-color="#DDD" width="90" :bold="false" :breathe="false" :focus="true" @change="change"></u-message-input>
    </view>
    <view class="resend" :class="{'disabled': sending}" @click="resend()">{{msg}}</view>
  </view>
</template>

<script>
import { mapMutations } from 'vuex'
export default {
  data: () => ({
    mobile: '',
    count: 60,
    ctrl: null,
    sending: false
  }),
  onLoad (options) {
    this.mobile = options.mobile
    this.setCron()
  },
  methods: {
    ...mapMutations(['setToken', 'setUserInfo']),
    setCron () {
      clearInterval(this.ctrl)
      this.sending = true
      this.ctrl = setInterval(() => {
        this.count--
        if (this.count === 0) {
          this.count = 60
          this.sending = false
          clearInterval(this.ctrl)
        }
      }, 1000)
    },
    async change (val) {
      if (/\d{6}/.test(val)) {
        const res = await this.$u.api.loginByPhone({
          mobile: this.mobile,
          code: val
        })
        if (res.code === 200) {
          uni.showToast({
            icon: 'none',
            title: '登录成功，2s后跳转',
            duration: 2000
          })
          const { token, data } = res
          this.setToken(token)
          this.setUserInfo(data)
          setTimeout(() => {
            uni.navigateBack({
              delta: 3
            })
          }, 2000)
        } else {
          uni.showToast({
            icon: 'none',
            title: res.msg,
            duration: 2000
          })
        }
      }
    },
    async resend () {
      if (this.sending) return
      const res = await this.$u.api.sendCode({
        mobile: this.mobile
      })
      if (res.code === 200) {
        this.setCron()
      } else {
        uni.showToast({
          icon: 'none',
          title: res.msg || '短信发送失败，请稍后重试',
          duration: 2000
        })
      }
    }
  },
  computed: {
    msg () {
      let str = '重新获取'
      if (this.sending) {
        str += ('(' + (this.count + '').padStart(2, '0') + 's)')
      }
      return str
    }
  }
}
</script>

<style lang="scss" scoped>
.wrapper {
  padding: 32rpx;
  .title {
    color: #333;
    font-size: 48rpx;
    font-weight: bold;
    padding-top: 50rpx;
  }
  .info {
    color: #666;
    line-height: 28rpx;
    padding-top: 24rpx;
  }
  .inputs {
    padding: 60rpx 0 40rpx;
  }
  .resend {
    font-size: 26rpx;
    font-weight: 300;
    color: #02d199;
    &.disabled {
      color: #ddd;
    }
  }
}
</style>
```

后端接口`LoginController.js`：

```js
// 手机号登录
async loginByPhone (ctx) {
  const { body } = ctx.request
  // mobile + code
  const { mobile, code } = body
  // 验证手机号与短信验证码的正确性
  const sms = await getValue(mobile)
  if (sms && sms === code) {
    await delValue(mobile)
    // 查询并创建用户
    const user = await User.findOrCreateByMobile({
      mobile
    })
    // 查看用户是否签到
    const userObj = await addSign(user)
    // 响应用户
    ctx.body = {
      code: 200,
      token: generateToken({ _id: userObj._id }),
      data: userObj
    }
  } else {
    ctx.body = {
      code: 500,
      msg: '手机号与验证码不匹配'
    }
  }
}
```

查询并创建手机用户`User.js`

```js
findOrCreateByMobile: function (user) {
  return this.findOne({ mobile: user.mobile }, {
    unionid: 0, password: 0
  }).then(res => {
    return res || this.create({
      mobile: user.mobile,
      username: getTempName(),
      name: getTempName(),
      roles: ['user']
    })
  })
},
```

发送验证码逻辑`PublicController.js`：

```js
// 发送手机验证码
async sendCode (ctx) {
  // 1.获取手机号 phone
  const { mobile } = ctx.query
  // 2.查询redis -> 判断是否验证码过期
  if (await getValue(mobile)) {
    ctx.body = {
      code: 501,
      msg: '短信正在发送中，请勿重新发送'
    }
    return
  }
  // 3.产生随机的6位数字
  const sms = String(Math.random()).slice(-6)
  // 4.发送短信 -> 设置redis -> sms, expire -> key:phone
  const res = await sendSms(mobile, sms)
  if (res.result === 0) {
    setValue(mobile, sms, 10 * 60)
    // 5.响应
    ctx.body = {
      code: 200,
      msg: '发送成功',
      data: res
    }
  } else {
    ctx.throw(500, '发送短信失败' + res.errmsg || '')
  }
}
```

需要注意的点：

* 短信发送失败时：
  * 限制用户频繁发送-> 不影响重发；
  * 用户提示 -> 短信失败原因 -> 这里大部分是第三方服务商的错误提示，所以需要专门进行处理；
  * 短信限制发送机制设计：不让成功发送短信之后，还让用户重复发送 -> redis记录短信数据；
* 手机登录接口：
  * 发送成功短信则响应，并设置redis，否则直接throw错误；
  * 创建用户需要使用findOne来进行查重；
  * 接口参数需要与前端进行一一对应，最好使用文档进行约定；

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/03-登录鉴权.html#统一的用户授权登录)统一的用户授权登录

用户授权登录是经常需要使用的功能，封装到`common/checkAuth.js`中：

* 需要判断小程序用户的登录状态是否失效 - 使用小程序API `checkSession`
* 用户登录失效，给用户一个Confirm提示框，让用户选择是否进行跳转登录

```js
import store from '@/store'

export const checkSession = async () => {
  try {
    await uni.checkSession()
    return true
  } catch (error) {
    return false
  }
}

export const checkToken = async () => {
  let flag = true
  const token = uni.getStorageSync('token')
  const checked = await checkSession()
  if (!store.state.token || !token || !checked) {
    flag = false
    uni.showModal({
      title: '您未登录',
      content: '需要登录才能操作，确定登录吗？',
      success: function (res) {
        if (res.confirm) {
          uni.navigateTo({
            url: '/subcom-pkg/auth/auth'
          })
        }
      }
    })
  }
  return flag
}
```



# [#](https://front-end.toimc.com/notes-page/project/community-miniapp/04-消息&热门&个人中心.html#消息-热门-个人中心)消息&热门&个人中心

消息、热门、个人中心这三块的内容重点：

* 灵活使用UI框架：uview内置样式的综合使用；
* 统一鉴权跳转提示：用户登录与未登录状态下，页面跳转逻辑；
* 熟悉前后端开发流程：从前到后的开发逻辑，排查问题从源头开始找问题；
* 完善登录失效，接口鉴权失败401的页面跳转逻辑；

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/04-消息&热门&个人中心.html#消息模块)消息模块

最终 完成效果：

![img](uniapp.assets/image-20210527025158228.2c02523b.png)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/04-消息&热门&个人中心.html#页面布局和样式)页面布局和样式

* 添加 tabs 切换，并设置吸顶

```vue
<template>
  <view class="msg">
    <view class="msg" v-if="isLogin">
      <u-sticky>
        <view class="tabs box-shadow">
          <!-- tabs -->
          <u-tabs :list="tabs" :name="'value'" :is-scroll="false" active-color="#02D199" inactive-color="#666" height="88" @change="tabsChange" :current="current"></u-tabs>
        </view>
      <u-sticky>
      <view>
        <!-- 评论列表 -->
        <!-- 点赞列表 -->
      </view>
    </view>
    <view class="info u-flex u-row-center u-col-center flex-column" v-else>
      <view class="center">
        登录过后查看评论&点赞消息
      </view>
      <u-button type="primary" hover-class="none">去登录</u-button>
    </view>
    <view class="bottom-line"></view>
  </view>
</template>

<script>
export default {
  props: {},
  data: () => ({
    current: 0,
    tabs: [
      {
        key: 'comments',
        value: '评论'
      },
      {
        key: 'like',
        value: '点赞'
      }
    ],
  }),
  methods: {
    tabsChange (i) {
      this.current = i
      this.$store.commit('setType', i === 0 ? 'comment' : 'hands')
      this.checkType()
    },
	}
}
</script>

<style lang="scss" scoped>
.flex-column {
  flex-flow: column nowrap;
}

.info {
  flex-flow: column nowrap;
  height: 100vh;
  width: 100vw;
  .center {
    color: #666;
    font-size: 32rpx;
    line-height: 50px;
  }
}
</style>
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/04-消息&热门&个人中心.html#自定义吸顶组件)自定义吸顶组件

吸顶效果最关键的属性：

```text
.t-sticky {
  position: sticky;
  top: 0;
}
```

可以自行创建`components/t-sticky/t-sticky.vue`组件：

```vue
<template>
  <view class="t-sticky" :style="{'top': top + 'px'}">
    <slot></slot>
  </view>
</template>

<script>

export default {
  props: {
    top: {
      default: 0,
      type: Number
    }
  },
  data: () => ({})
}
</script>

<style lang="scss" scoped>
.t-sticky {
  position: sticky;
  // top: 0;
}
</style>
```

使用方法：

```text
<t-sticky :top="距离顶部的值">
  // ... 组件 
</t-sticky>
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/04-消息&热门&个人中心.html#消息模块的样式)消息模块的样式

```vue
<template>
  <view class="msg">
    <view class="msg" v-if="isLogin">
      <u-sticky>
        <view class="tabs box-shadow">
          <!-- tabs -->
          <u-tabs :list="tabs" :name="'value'" :is-scroll="false" active-color="#02D199" inactive-color="#666" height="88" @change="tabsChange" :current="current"></u-tabs>
        </view>
      </u-sticky>
      <view>
        <!-- 评论列表 -->
        <view v-if="current === 0">
          <view v-for="(item, index) in comments" :key="index">
            <view class="box">
              <!-- 评论用户卡片 -->
              <view class="user u-flex">
                <u-image class="phone" :src="item.cuid.pic" width="72" height="72" shape="circle" error-icon="/static/images/header.jpg"></u-image>
                <view class="user-column u-flex-1 u-flex flex-column u-col-top">
                  <text class="name">{{ item.cuid.name }}</text>
                  <text class="label">{{ item.created | moment }} 回复了你</text>
                </view>
                <view class="reply u-flex u-row-center">
                  <image src="/static/images/advice.png" mode="aspectFit" />
                  回复
                </view>
              </view>
              <!-- 评论内容 -->
              <view class="comment">{{ item.content }}</view>
              <view class="post">
                <view>
                  <!-- 封面图 -->
                  <view v-if="item.tid.shotpic">
                    <view class="img">
                      <u-image :src="item.tid.shotpic" width="192" height="122"></u-image>
                    </view>
                  </view>
                  <!-- 文章标题 + 摘要 -->
                  <view class="post-content u-flex flex-column u-col-top">
                    <text class="title">{{ item.tid.title }}</text>
                    <text class="content">{{ item.tid.content }}</text>
                  </view>
                </view>
              </view>
            </view>
          </view>
        </view>
        <!-- 点赞列表 -->
        <view v-else>
          <view v-for="(item, index) in handUsers" :key="index">
            <view class="box">
              <view class="user u-flex">
                <u-image class="pic" :src="item.huid.pic" width="72" height="72" shape="circle" error-icon="/static/images/header.jpg" />
                <view class="user-column u-flex-1 u-flex flex-column u-col-top">
                  <span class="name">{{ item.huid.name }}</span>
                  <span class="label">{{ item.created | moment }}</span>
                </view>
              </view>
            </view>
            <view class="comment">赞了你的评论 {{item.cid.content}}</view>
          </view>
        </view>
      </view>
    </view>
    <view class="info u-flex u-row-center u-col-center flex-column" v-else>
      <view class="center">
        登录过后查看评论&点赞消息
      </view>
      <u-button type="primary" hover-class="none" @click="navTo">去登录</u-button>
    </view>
    <view class="bottom-line"></view>
  </view>
</template>

<script>
import { mapGetters } from 'vuex'
import { checkAuth } from '@/common/checkAuth'
export default {
  props: {},
  data: () => ({
    current: 0,
    tabs: [
      {
        key: 'comments',
        value: '评论'
      },
      {
        key: 'like',
        value: '点赞'
      }
    ],
    comments: [
    ],
    handUsers: [
    ],
    pageMsg: {
      page: 0,
      limit: 10
    },
    pageHands: {
      page: 0,
      limit: 10
    }
  }),
  computed: {
    ...mapGetters(['isLogin'])
  },
  methods: {
    navTo () {
      uni.navigateTo({
        url: '/subcom-pkg/auth/auth'
      })
    },
    tabsChange (i) {
      this.current = i
      this.$store.commit('setType', i === 0 ? 'comment' : 'hands')
      this.checkType()
    },
    async getMsg () {
      const { data, code } = await this.$u.api.getMsg(this.pageMsg)
      if (code === 200) {
        this.comments = data
      }
    },
    async getHands () {
      const { data, code } = await this.$u.api.getHands(this.pageHands)
      if (code === 200) {
        this.handUsers = data
      }
    },
    async checkType () {
      if (!this.isLogin) return
      const flag = await checkAuth()
      if (!flag) {
        return
      }
      if (this.$store.state.type === 'hands') {
        // 这里肯定已经登录
        this.current = 1
        this.getHands()
      } else {
        this.current = 0
        this.getMsg()
      }
    }
  },
  watch: {},
  // 页面周期函数--监听页面显示(not-nvue)
  onShow () {
    this.checkType()
  },
  // 页面周期函数--监听页面隐藏
  onHide () {
    this.current = 0
  }
}
</script>

<style lang="scss" scoped>
.flex-column {
  flex-flow: column nowrap;
}

.info {
  flex-flow: column nowrap;
  height: 100vh;
  width: 100vw;
  .center {
    color: #666;
    font-size: 32rpx;
    line-height: 50px;
  }
}
.user {
  margin: 20rpx;
  .name {
    margin-block: 20rpx;
    margin-bottom: 10rpx;
    font-size: 28rpx;
    font-weight: bold;
    color: rgba(51, 51, 51, 1);
  }
  .phone {
    width: 72rpx;
    height: 72rpx;
    border-radius: 50%;
  }
  .user-column {
    margin-left: 20rpx;
  }
  .label {
    font-size: 22rpx;
    font-weight: 500;
    color: rgba(153, 153, 153, 1);
  }
}

.reply {
  color: rgba(153, 153, 153, 1);
  margin-right: 40rpx;
  font-size: 24rpx;
  font-weight: 500;
  line-height: 40rpx;
  image {
    width: 30rpx;
    height: 30rpx;
    margin-right: 10rpx;
  }
}

.comment {
  margin: 0 20rpx 20rpx;
}

.post {
  margin: 0 20rpx 20rpx;
  padding: 20rpx;
  background-color: #f3f3f3;
  border-radius: 15rpx;
  .title {
    margin-bottom: 10rpx;
    font-size: 26rpx;
    font-weight: bold;
    color: rgba(51, 51, 51, 1);
    display: -webkit-box;
    -webkit-line-clamp: 1; /*这个数字是设置要显示省略号的行数*/
    -webkit-box-orient: vertical;
    overflow: hidden;
  }
  .content {
    font-size: 24rpx;
    font-weight: 400;
    color: rgba(102, 102, 102, 1);
    line-height: 30rpx;
    display: -webkit-box;
    -webkit-line-clamp: 2; /*这个数字是设置要显示省略号的行数*/
    -webkit-box-orient: vertical;
    overflow: hidden;
  }
}
</style>
```

前端接口`api/modules/user.js`：

```text
// 获取点赞数据
const getHands = params => axios.get('/user/getHands', params)

// 获取用户未读消息
const getMsg = (data) => axios.get('/user/getmsg', data)
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/04-消息&热门&个人中心.html#接口对接与联调)接口对接与联调

调整`src/model/CommentsHands.js`

```js
import mongoose from '../config/DBHelpler'

const Schema = mongoose.Schema

const CommentsSchema = new Schema({
  cid: { type: String, ref: 'comments' }, // 评论id
  huid: { type: String, ref: 'users' }, // 被点赞用户的id
  uid: { type: String, ref: 'users' }, // 点赞用户id
  created: { type: Date }
})

CommentsSchema.pre('save', function (next) {
  this.created = new Date()
  next()
})

CommentsSchema.post('save', function (error, doc, next) {
  if (error.name === 'MongoError' && error.code === 11000) {
    next(new Error('There was a duplicate key error'))
  } else {
    next(error)
  }
})

CommentsSchema.statics = {
  findByCid: function (id) {
    return this.find({ cid: id })
  },
  getHandsByUid: function (id, page, limit) {
    return this.find({ uid: id })
      .populate({
        path: 'huid',
        select: '_id name pic'
      })
      .populate({
        path: 'cid',
        select: '_id content'
      })
      .skip(page * limit)
      .limit(limit)
      .sort({ created: -1 })
  }
}

const CommentsHands = mongoose.model('comments_hands', CommentsSchema)

export default CommentsHands
```

获取历史消息`src/api/UserController.js`：

```js
  // 获取历史消息
  // 记录评论之后，给作者发送消息
  async getHands (ctx) {
    const params = ctx.query
    const page = params.page ? params.page : 0
    const limit = params.limit ? parseInt(params.limit) : 0
    // 方法一： 嵌套查询 -> aggregate
    // 方法二： 通过冗余换时间
    const obj = await getJWTPayload(ctx.header.authorization)
    const result = await CommentsHands.getHandsByUid(obj._id, page, limit)

    ctx.body = {
      code: 200,
      data: result
    }
  }
```

完成效果（点赞）：

![image-20210527031025147](uniapp.assets/image-20210527031025147.cad808cc.png)

完成效果（评论）：

![image-20210527030947602](uniapp.assets/image-20210527030947602.6589bfe7.png)

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/04-消息&热门&个人中心.html#热门模块)热门模块

这个部分利旧接口，所以只用开发前端页面即可：

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/04-消息&热门&个人中心.html#页面布局和样式-2)页面布局和样式

```vue
<template>
  <view>
    <u-sticky>
      <view class="tabs box-shadow">
        <u-tabs :list="tabs" :name="'value'" :current="current" @change="tabsChange" :is-scroll="false" active-color="#02D199" inactive-color="#666" height="88"></u-tabs>
      </view>
    </u-sticky>
    <view class="content">
      <view class="tags">
        <uni-tag :text="item.value" v-for="(item, i) in types[tabs[current].key]" :class="{ active: tagCur === i }" :key="i" @click="tagsChange(i)"></uni-tag>
      </view>
      <HotPostList :lists="lists" v-if="tabs[current].key === 'posts'" @click="postDetail"></HotPostList>
      <HotCommentsList :lists="lists" :type="types.comments[tagCur].key" v-else-if="tabs[current].key === 'comments'" @click="commentDetail"></HotCommentsList>
      <HotSignList :lists="lists" :type="types.sign[tagCur].key" v-else></HotSignList>
    </view>
    <view class="bottom-line"></view>
  </view>
</template>

<script>
import HotPostList from './components/HotPostList'
import HotCommentsList from './components/HotCommentsList'
import HotSignList from './components/HotSignList'

export default {
  components: {
    HotPostList,
    HotCommentsList,
    HotSignList
  },
  data: () => ({
    tabs: [
      {
        key: 'posts',
        value: '热门帖子'
      },
      {
        key: 'comments',
        value: '热门评论'
      },
      {
        key: 'sign',
        value: '签到排行'
      }
    ],
    types: {
      posts: [
        {
          key: '3',
          value: '全部'
        },
        {
          key: '0',
          value: '3日内'
        },
        {
          key: '1',
          value: '7日内'
        },
        {
          key: '2',
          value: '30日内'
        }
      ],
      comments: [
        {
          key: '1',
          value: '最新评论'
        },
        {
          key: '0',
          value: '热门评论'
        }
      ],
      sign: [
        {
          key: '0',
          value: '总签到榜'
        },
        {
          key: '1',
          value: '今日签到榜'
        }
      ]
    },
    current: 0,
    tagCur: 0,
    lists: [
    ],
    page: {
      page: 0,
      limit: 50
    }
  }),
  onLoad (options) {
    const { scene } = options
    if (scene) {
      this.tabsChange(scene)
    } else {
      this.getHotPost()
    }
  },
  onShow () {
    this.hanldeChange()
  },
  methods: {
    async getHotPost () {
      const { data } = await this.$u.api.getHotPost({
        ...this.page,
        index: this.types.posts[this.tagCur].key
      })
      this.lists = data
    },
    async getHotComments () {
      const { data } = await this.$u.api.getHotComments({
        ...this.page,
        index: this.types.comments[this.tagCur].key
      })
      this.lists = data
    },
    async getHotSignRecord () {
      const { data } = await this.$u.api.getHotSignRecord({
        ...this.page,
        index: this.types.sign[this.tagCur].key
      })
      this.lists = data
    },
    tabsChange (value) {
      this.current = value
      this.tagCur = 0
      this.page = {
        page: 0,
        limit: 50
      }
      this.hanldeChange()
    },
    tagsChange (value) {
      this.tagCur = value
      this.page = {
        page: 0,
        limit: 50
      }
      this.hanldeChange()
    },
    hanldeChange () {
      if (this.current === 0) {
        // 热门帖子
        this.getHotPost()
      } else if (this.current === 1) {
        // 热门评论
        this.getHotComments()
      } else {
        // 签到排行
        this.getHotSignRecord()
      }
    },
    // 跳转文章详情
    postDetail (item) {
      uni.navigateTo({
        url: '/subcom-pkg/detail/detail?tid=' + item._id
      })
    },
    // 评论详情
    commentDetail (item) {
      uni.navigateTo({
        url: `/subcom-pkg/detail/detail?tid=${item.tid}&cid=${item._id}`
      })
    }
  },
  async onPullDownRefresh () {
    this.hanldeChange()
    uni.stopPullDownRefresh()
  },
  // 页面处理函数--监听用户上拉触底
  onReachBottom () {}
  // 页面处理函数--监听页面滚动(not-nvue)
  /* onPageScroll(event) {}, */
  // 页面处理函数--用户点击右上角分享
  /* onShareAppMessage(options) {}, */
}
</script>

<style lang="scss" scoped>
.tags {
  display: flex;
  padding: 20rpx 25rpx;
  width: 100vw;
  background-color: #fff;
  z-index: 200;
  ::v-deep .uni-tag {
    // margin-top: 20rpx;
    margin-right: 25rpx;
    border-radius: 25rpx;
    text {
      color: #999;
      white-space: nowrap;
      font-size: 26rpx;
    }
  }
  .active {
    ::v-deep .uni-tag {
      background-color: #d6f8ef;
      text {
        color: #02d199;
        font-weight: bold;
      }
    }
  }
}

::v-deep .list {
  z-index: 100;
  padding: 0 30rpx 60rpx 30rpx;
  .list-item {
    display: flex;
    flex-flow: row nowrap;
    justify-content: space-between;
    align-items: center;
    border-bottom: 1px solid #ddd;
  }
  .num {
    font-size: 36rpx;
    font-weight: bold;
    &.first {
      color: #ed745e;
    }
    &.second {
      color: #e08435;
    }
    &.third {
      color: #f1ae37;
    }
    &.common {
      color: #999;
    }
  }
  .user {
    width: 90rpx;
    height: 90rpx;
    border-radius: 50%;
    margin-left: 20rpx;
  }
  .column {
    flex: 1;
    display: flex;
    flex-flow: column nowrap;
    justify-content: space-between;
    height: 186rpx;
    padding: 30rpx 24rpx;

    &.no-between {
      justify-content: center;
      .title {
        padding-bottom: 16rpx;
      }
    }
    .title {
      color: #333;
      font-size: 32rpx;
      font-weight: bold;
    }
    .read {
      font-size: 26rpx;
      color: #999;
      text {
        color: #333;
        font-weight: bold;
        padding-right: 10rpx;
      }
    }
  }
  .img {
    width: 200rpx;
    height: 125rpx;
    border-radius: 12rpx;
    overflow: hidden;
    img {
      width: 100%;
      height: 100%;
    }
  }
}
</style>
```

自定义三个组件，热门帖子`HotPostList.vue`：

```vue
<template>
  <view>
    <view class="list" v-for="(item,index) in lists" :key="index">
      <view class="list-item" @click="gotoDetail(item)">
        <view class="num first" v-if="index === 0">01</view>
        <view class="num second" v-else-if="index === 1">02</view>
        <view class="num third" v-else-if="index === 2">03</view>
        <view class="num common" v-else-if="index < 9">{{ '0' + (index+1) }}</view>
        <view class="num common" v-else-if="index < 50 && index >=9">{{ index+1 }}</view>
        <view class="num" v-else></view>
        <view class="column">
          <view class="title">{{item.title}}</view>
          <view class="read">{{parseInt(item.answer) > 1000?parseInt(item.answer/1000).toFixed(1) + 'k': item.answer}} 评论</view>
        </view>
        <view class="img" v-if="item.shotpic">
          <image :src="item.shotpic" mode="aspectFill" />
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  props: {
    lists: {
      type: Array,
      default: () => []
    }
  },
  methods: {
    gotoDetail (item) {
      this.$emit('click', item)
    }
  }
}
</script>

<style lang="scss" scoped>
</style>
```

热门评论`HotCommentsList.vue`：

```vue
<template>
  <view>
    <view class="list" v-for="(item,index) in lists" :key="index">
      <!-- 评论 -->
      <view class="list-item" @click="gotoDetail(item)">
        <view class="num first" v-if="index === 0">01</view>
        <view class="num second" v-else-if="index === 1">02</view>
        <view class="num third" v-else-if="index === 2">03</view>
        <view class="num common" v-else-if="index < 9">{{ '0' + (index+1) }}</view>
        <view class="num common" v-else-if="index < 50 && index >=9">{{ index+1 }}</view>
        <view class="num" v-else></view>
        <u-image width="88" height="88" class="user" :src="item.cuid? item.cuid.pic : ''" mode="aspectFit" shape="circle" error-icon="/static/images/header.jpg" />
        <view class="column no-between">
          <view class="title">{{item.cuid && item.cuid.name? item.cuid.name : 'imooc'}}</view>
          <view class="read" v-if="parseInt(type) === 0">
            <text>{{item.count}}</text> 条评论
          </view>
          <view class="read" v-else>{{item.created | moment}} 发表了评论</view>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  props: {
    lists: {
      type: Array,
      default: () => []
    },
    type: {
      type: [Number, String],
      default: 0
    }
  },
  methods: {
    gotoDetail (item) {
      this.$emit('click', item)
    }
  }
}
</script>

<style lang="scss" scoped>
</style>
```

签到排行`HotSignList.vue`：

```vue
<template>
  <view>
    <view class="list" v-for="(item, index) in lists" :key="index">
      <!-- 签到 -->
      <view
        class="list-item"
        v-if="item.count && item.count > 0"
        @click="gotoDetail(item)"
      >
        <view class="num first" v-if="index === 0">01</view>
        <view class="num second" v-else-if="index === 1">02</view>
        <view class="num third" v-else-if="index === 2">03</view>
        <view class="num common" v-else-if="index < 9">{{
          '0' + (index + 1)
        }}</view>
        <view class="num common" v-else-if="index < 50 && index >= 9">{{
          index + 1
        }}</view>
        <view class="num" v-else></view>
        <u-image
          width="88"
          height="88"
          class="user"
          :src="parseInt(type) === 0 ? item.pic : item.uid.pic"
          mode="aspectFit"
          shape="circle"
          error-icon="/static/images/header.jpg"
        />
        <view class="column no-between">
          <view class="title">{{
            (item.uid ? item.uid.name : item.name) || 'imooc'
          }}</view>
          <view class="read" v-if="parseInt(type) === 0">
            已经连续签到
            <span>{{ item.count }}</span> 天
          </view>
          <view class="read" v-else>{{ item.created | hours }}</view>
        </view>
      </view>
      <view class="list-item" v-else>
        <view class="num first" v-if="index === 0">01</view>
        <view class="num second" v-else-if="index === 1">02</view>
        <view class="num third" v-else-if="index === 2">03</view>
        <view class="num common" v-else-if="index < 9">{{
          '0' + (index + 1)
        }}</view>
        <view class="num common" v-else-if="index < 50 && index >= 9">{{
          index + 1
        }}</view>
        <view class="num" v-else></view>
        <u-image
          width="88"
          height="88"
          class="user"
          :src="parseInt(type) === 0 ? item.pic : item.uid.pic"
          mode="aspectFit"
          shape="circle"
          error-icon="/static/images/header.jpg"
        />
        <view class="column no-between">
          <view class="title">{{ item.uid ? item.uid.name : 'imooc' }}</view>
          <view class="read">
            今日签到时间<span class="text">{{ item.created | hours }}</span>
          </view>
          <!-- <view class="read" v-else>{{item.created | hours}}</view> -->
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  props: {
    lists: {
      type: Array,
      default: () => []
    },
    type: {
      type: [Number, String],
      default: 0
    }
  },
  methods: {
    gotoDetail (item) {
      this.$emit('click', item)
    }
  }
}
</script>

<style lang="scss" scoped>
.text {
  padding-left: 15rpx;
}
</style>
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/04-消息&热门&个人中心.html#接口对接与联调-2)接口对接与联调

`PublicController.js`文件：

```js
  async getHotPost (ctx) {
    // page limit
    // type index 0-3日内， 1-7日内， 2-30日内， 3-全部
    const params = ctx.query
    const page = params.page ? parseInt(params.page) : 0
    const limit = params.limit ? parseInt(params.limit) : 10
    const index = params.index ? params.index : '0'
    let startTime = ''
    let endTime = ''
    if (index === '0') {
      startTime = moment().subtract(2, 'day').format('YYYY-MM-DD 00:00:00')
    } else if (index === '1') {
      startTime = moment().subtract(6, 'day').format('YYYY-MM-DD 00:00:00')
    } else if (index === '2') {
      startTime = moment().subtract(29, 'day').format('YYYY-MM-DD 00:00:00')
    }
    endTime = moment().add(1, 'day').format('YYYY-MM-DD 00:00:00')
    const result = await Post.getHotPost(page, limit, startTime, endTime)
    const total = await Post.getHotPostCount(page, limit, startTime, endTime)
    ctx.body = {
      code: 200,
      total,
      data: result,
      msg: '获取热门文章成功'
    }
  }

  async getHotComments (ctx) {
    // 0-热门评论，1-最新评论
    const params = ctx.query
    const page = params.page ? parseInt(params.page) : 0
    const limit = params.limit ? parseInt(params.limit) : 10
    const index = params.index ? params.index : '0'
    const result = await Comments.getHotComments(page, limit, index)
    const total = await Comments.getHotCommentsCount(index)
    ctx.body = {
      code: 200,
      data: result,
      total,
      msg: '获取热门评论成功'
    }
  }

  async getHotSignRecord (ctx) {
    // 0-总签到榜，1-最新签到
    const params = ctx.query
    const page = params.page ? parseInt(params.page) : 0
    const limit = params.limit ? parseInt(params.limit) : 10
    const index = params.index ? params.index : '0'
    let result
    let total = 0
    if (index === '0') {
      // 总签到榜
      result = await User.getTotalSign(page, limit)
      total = await User.getTotalSignCount()
    } else if (index === '1') {
      // 今日签到
      result = await SignRecord.getTopSign(page, limit)
      total = await SignRecord.getTopSignCount()
    } else if (index === '2') {
      // 最新签到
      result = await SignRecord.getLatestSign(page, limit)
      total = await SignRecord.getSignCount()
    }
    ctx.body = {
      code: 200,
      data: result,
      total,
      msg: '获取签到排行成功'
    }
  }
```

完成效果（热门帖子）：

![image-20210527032522285](uniapp.assets/image-20210527032522285.4590bccc.png)

完成效果（热门评论）：

![image-20210527032608703](uniapp.assets/image-20210527032608703.890cffa2.png)

完成效果（签到排行）：

![image-20210527032652755](uniapp.assets/image-20210527032652755.931b7a09.png)

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/04-消息&热门&个人中心.html#个人中心模块)个人中心模块

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/04-消息&热门&个人中心.html#页面布局和样式-3)页面布局和样式

整体的页面分为：

* 背景
* 个人信息 + 统计信息
* 功能区
* 快速跳转区

```vue
<template>
  <view>
    <view class="grey">
      <view class="bg"></view>
      <view class="wrapper">
        <!-- 个人信息卡片 -->
        <view class="profile">
          <view class="info">
            <u-image class="pic" :src="isLogin ? userInfo.pic: ''" width="120" height="120" shape="circle" error-icon="/static/images/header.jpg" />
            <!-- 用户昵称 + VIP -->
            <view class="user" @click="navTo">
              <view class="name">{{isLogin ?userInfo.name : '请登录'}}</view>
              <view class="fav">
                <!-- <van-icon name="fav2" class-prefix="iconfont" size="14"></van-icon> -->
                积分：{{userInfo && userInfo.favs ? userInfo.favs:0}}
              </view>
            </view>
            <view class="link" @click="gotoGuard('/sub-pkg/user-info/user-info')">个人主页 ></view>
          </view>
          <!-- 统计信息 -->
          <view class="stats" v-if="isLogin">
            <view class="item">
              <navigator :url="'/sub-pkg/posts/posts?uid=' + uid + '&type=p'">
                <view>{{ countMyPost }}</view>
                <view class="title">我的帖子</view>
              </navigator>
            </view>
            <view class="item">
              <navigator :url="'/sub-pkg/posts/posts?uid=' + uid+ '&type=c'">
                <view>{{ countMyCollect }}</view>
                <view class="title">收藏夹</view>
              </navigator>
            </view>
            <view class="item">
              <navigator :url="'/sub-pkg/posts/posts?uid=' + uid+ '&type=h'">
                <view>{{ countMyHistory }}</view>
                <view class="title">最近浏览</view>
              </navigator>
            </view>
          </view>
        </view>
      </view>
      <!-- 功能区 -->
      <view class="center-wraper">
        <view class="center-list first">
          <li v-for="(item,index) in lists" :key="index">
            <view @click="gotoGuardHandler(item)">
              <i :class="item.icon"></i>
              <span>{{item.name}}</span>
            </view>
          </li>
        </view>
        <!-- 首页 -> 分类标签 快速跳转 -->
        <view class="center-list">
          <li v-for="(item,index) in routes" :key="index" @click="gotoHome(item.tab)">
            <i :class="item.icon"></i>
            <span>{{item.name}}</span>
          </li>
        </view>
      </view>
    </view>
    <view class="bottom-line"></view>
  </view>
</template>

<script>
import { gotoGuard } from '@/common/checkAuth'
import { mapGetters, mapState, mapMutations } from 'vuex'
export default {
  data: () => ({
    lists: [
      {
        name: '我的帖子',
        icon: 'icon-teizi',
        routeName: '/sub-pkg/posts/posts'
      },
      {
        name: '修改设置',
        icon: 'icon-setting',
        routeName: '/sub-pkg/settings/settings'
      },
      {
        name: '签到中心',
        icon: 'icon-qiandao',
        routeName: '/sub-pkg/sign/sign'
      },
      {
        name: '电子书',
        icon: 'icon-book',
        routeName: '/sub-pkg/books/books'
      },
      {
        name: '关于我们',
        icon: 'icon-about',
        routeName: '/sub-pkg/about/about'
      },
      {
        name: '人工客服',
        icon: 'icon-support',
        routeName: '/sub-pkg/suggest/suggest'
      },
      {
        name: '意见反馈',
        icon: 'icon-lock2',
        routeName: '/sub-pkg/suggest/survey'
      }
    ],
    routes: [
      {
        name: '提问',
        icon: 'icon-question',
        tab: 'ask'
      },
      {
        name: '分享',
        icon: 'icon-share',
        tab: 'share'
      },
      {
        name: '讨论',
        icon: 'icon-taolun',
        tab: 'discuss'
      },
      {
        name: '建议',
        icon: 'icon-advise',
        tab: 'advise'
      }
    ],
    countMyPost: 0,
    countMyCollect: 0,
    countMyHistory: 0
    // isLogin: true
  }),
  computed: {
    ...mapGetters(['isLogin']),
    ...mapState(['userInfo']),
    uid () {
      return this.userInfo._id
    }
  },
  onShow () {
  },
  methods: {
    ...mapMutations(['setUserInfo']),
    gotoGuard,
    gotoGuardHandler (item) {
      const { name, routeName } = item
      if (name === '我的帖子') {
        gotoGuard(routeName + `?uid=${this.uid}&type=p`)
      } else {
        gotoGuard(routeName)
      }
    },
    gotoHome (tab) {
      uni.switchTab({
        url: '/pages/home/home'
      })
    },
    navTo () {
      if (!this.isLogin) {
        uni.navigateTo({
          url: '/subcom-pkg/auth/auth'
        })
      }
    },

  }

}
</script>

<style lang="scss">
.grey {
  position: fixed;
  width: 100%;
  height: 100%;
  left: 0;
  top: 0;
  z-index: 30;
}
a {
  color: #666;
  text-decoration: none;
}
.bg {
  background-image: url("/static/images/my_bg.png");
  background-repeat: no-repeat;
  background-size: contain;
  position: relative;
  left: 0;
  top: 0;
  width: 100%;
  height: 280rpx;
  background-position: 0 0;
  z-index: 100;
}
.wrapper {
  width: 100%;
  height: 370rpx;
  padding: 25rpx;
  position: absolute;
  left: 0;
  top: 0;
  z-index: 100;
  box-sizing: border-box;
  color: #333;
  .profile {
    background: #fff;
    border-radius: 12rpx;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    width: 100%;
    height: 100%;
    padding: 30rpx;
    box-sizing: border-box;
    .name {
      font-size: 36rpx;
      font-weight: 700;
      margin-bottom: 10rpx;
      margin-top: 0;
      width: 370rpx;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
    }
    .link {
      font-size: 24rpx;
      color: #999;
    }
    .fav {
      display: inline-block;
      padding: 8px 12rpx;
      background: rgba(2, 209, 153, 0.16);
      border-radius: 12rpx;
      color: #02d199;
      margin: 0;
      font-size: 22rpx;
      .icon-fav {
        padding-right: 10rpx;
      }
    }
    .info,
    .stats {
      display: flex;
      flex-flow: row nowrap;
      justify-content: space-between;
      align-items: center;
    }
    .info {
      margin-bottom: 24rpx;
    }
    .stats {
      justify-content: space-around;
    }
    .user {
      flex: 1;
      padding-left: 20rpx;
    }
    .pic {
      width: 120rpx;
      height: 120rpx;
      border-radius: 50%;
    }
    .item {
      text-align: center;
      position: relative;
      p {
        margin-top: 14rpx;
        margin-bottom: 0;
      }
      &:after {
        width: 2rpx;
        height: 80rpx;
        background: #ddd;
        content: "";
        position: absolute;
        right: -60rpx;
        top: 20rpx;
      }
      &:last-child {
        &:after {
          width: 0;
        }
      }
      .title {
        color: #666;
      }
    }
  }
}
.center-wraper {
  background: #f6f5f8;
  position: relative;
  width: 100%;
  // height: 100%;
  z-index: 10;
  .center-list {
    background: #fff;
    margin-bottom: 30rpx;
    display: flex;
    flex-flow: wrap;
    padding-top: 40rpx;
    &.first {
      padding-top: 100rpx;
    }
    li {
      width: 25%;
      text-align: center;
      color: #666;
      margin-bottom: 40rpx;
      font-size: 26rpx;
    }
    i {
      display: block;
      margin: 0 auto;
      font-size: 40rpx;
      width: 56rpx;
      height: 56rpx;
      margin-bottom: 20rpx;
      color: #888;
      background-size: contain;
    }
    .icon-teizi {
      background-image: url("/static/images/teizi@2x.png");
    }
    .icon-setting {
      background-image: url("/static/images/setting@2x.png");
    }
    .icon-lock2 {
      background-image: url("/static/images/lock2@2x.png");
    }
    .icon-support {
      background-image: url("/static/images/support.png");
    }
    .icon-qiandao {
      background-image: url("/static/images/sign1.png");
    }
    .icon-book {
      background-image: url("/static/images/books.png");
    }
    .icon-record {
      background-image: url("/static/images/record@2x.png");
    }
    .icon-about {
      background-image: url("/static/images/about.png");
    }
    // 快捷访问
    .icon-question {
      background-image: url("/static/images/question@2x.png");
    }
    .icon-share {
      background-image: url("/static/images/share@2x.png");
    }
    .icon-taolun {
      background-image: url("/static/images/taolun@2x.png");
    }
    .icon-advise {
      background-image: url("/static/images/advice@2x.png");
    }
  }
}
</style>
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/04-消息&热门&个人中心.html#最近浏览功能)最近浏览功能

创建`PostHistory`模型：

```js
import mongoose from '../config/DBHelpler'

const Schema = mongoose.Schema

const PostHistorySchema = new Schema(
  {
    uid: { type: String, ref: 'user', required: true },
    tid: { type: String, ref: 'post', required: true }
  },
  { timestamps: { createdAt: 'created', updatedAt: 'updated' } }
)

PostHistorySchema.statics = {
  queryCount (options) {
    return this.find(options).countDocuments()
  },
  addOrUpdate (uid, tid) {
    // 增加浏览记录，如果有则更新浏览时间
    return this.findOne({ uid, tid }).exec((err, doc) => {
      if (err) {
        console.log(err)
      }
      if (doc) {
        doc.created = new Date()
        doc.save()
      } else {
        this.create({ uid, tid })
      }
    })
  },
  delOne (uid, tid) {
    return this.deleteOne({ uid, tid })
  },
  getListByUid (uid, skip, limit) {
    // 获取用户的浏览记录
    return this.find({ uid })
      .populate({
        path: 'tid',
        select: 'uid catalog title content answer created',
        populate: {
          path: 'uid',
          select: 'name pic'
        }
      })
      .sort({ created: -1 })
      .skip(skip)
      .limit(limit)
  },
  deleteByPostId: function (tid) {
    return this.deleteMany({ tid })
  }
}

const PostHistory = mongoose.model('post_history', PostHistorySchema)

export default PostHistory
```

创建`src/api/StaticsController.js`：

* 统计最近浏览、我的帖子、收藏夹、我的评论、我的点赞、我的获赞、个人积分、用户的签到日期；
* 个人积分需要单独写方法，其他的可以使用mongoose中的`countDocuments`方法；

```js
// import { getJWTPayload } from '@/common/Utils'
import Post from '@/model/Post'
import PostHistory from '@/model/PostHistory'
import User from '@/model/User'
import UserCollect from '@/model/UserCollect'
import Comments from '@/model/Comments'
import CommentsHands from '@/model/CommentsHands'
import SignRecord from '@/model/SignRecord'

/**
 * 统计相关的 api 放在这里
 */
class StatisticsController {
  // 统计数据：最近浏览、我的帖子、收藏夹、我的评论、我的点赞、获赞、个人积分
  async wxUserCount (ctx) {
    const body = ctx.query
    // const obj = await getJWTPayload(ctx.header.authorization)
    const { _id: uid } = ctx
    const { reqAll } = body
    // console.log('🚀 ~ file: StatisticsController.js ~ line 20 ~ StatisticsController ~ wxUserCount ~ reqAll', reqAll)
    if (uid) {
      const countMyHistory =
        (body.reqHistory || reqAll) && (await PostHistory.countDocuments({ uid })) // 最近浏览
      const countMyPost =
        (body.reqPost || reqAll) && (await Post.countDocuments({ uid })) // 我的帖子
      const countMyCollect =
        (body.reqCollect || reqAll) &&
        (await UserCollect.countDocuments({ uid })) // 收藏夹
      const countMyComment =
        (body.reqComment || reqAll) &&
        (await Comments.countDocuments({ cuid: uid })) // 我的评论
      const countMyHands =
        (body.reqHands || reqAll) &&
        (await CommentsHands.countDocuments({ uid })) // 我的点赞
      const countHandsOnMe =
        (body.reqHandsOnMe || reqAll) &&
        (await CommentsHands.countDocuments({ huid: uid })) // 获赞
      const countFavs = (body.reqFavs || reqAll) && (await User.getFavs(uid)) // 个人积分
      const countSign =
        (body.reqSign || reqAll) && (await SignRecord.countDocuments({ uid })) // 签到次数
      const lastSigned =
        (body.reqLastSigned || reqAll) && (await SignRecord.countDocuments({ uid })) // 获取用户最新的签到日期

      ctx.body = {
        code: 200,
        data: {
          countMyPost,
          countMyCollect,
          countMyComment,
          countMyHands,
          countHandsOnMe,
          countMyHistory,
          countFavs,
          lastSigned,
          countSign
        }
      }
    } else {
      ctx.body = {
        code: 500,
        msg: '查询失败'
      }
    }
  }
}

export default new StatisticsController()
```

更新用户获取文章详情的方法`getPostDetail`，在文件`src/api/ContentController.js`中：

```js
  // 获取文章详情
  async getPostDetail (ctx) {
    const params = ctx.query
    if (!params.tid) {
      ctx.body = {
        code: 500,
        msg: '文章id为空'
      }
      return
    }
    const post = await Post.findByTid(params.tid)
    if (!post) {
      ctx.body = {
        code: 200,
        data: {},
        msg: '查询文章详情成功'
      }
      return
    }
    let isFav = 0
    // 判断用户是否传递Authorization的数据，即是否登录
    if (
      typeof ctx.header.authorization !== 'undefined' &&
      ctx.header.authorization !== ''
    ) {
      const obj = await getJWTPayload(ctx.header.authorization)
      const userCollect = await UserCollect.findOne({
        uid: obj._id,
        tid: params.tid
      })
      if (userCollect && userCollect.tid) {
        isFav = 1
      }
      await PostHistory.addOrUpdate(ctx._id, params.tid) // 添加浏览记录
    }
    const newPost = post.toJSON()
    newPost.isFav = isFav
    // 更新文章阅读记数
    const result = await Post.updateOne(
      { _id: params.tid },
      { $inc: { reads: 1 } }
    )
    if (post._id && result.ok === 1) {
      ctx.body = {
        code: 200,
        data: newPost,
        msg: '查询文章详情成功'
      }
    } else {
      ctx.body = {
        code: 500,
        msg: '获取文章详情失败'
      }
    }
  }
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/04-消息&热门&个人中心.html#接口对接与联调-3)接口对接与联调

前端添加接口：

```js
// 个人中心的统计数字
const wxUserCount = params => axios.get('/user/wxUserCount', params)
```

前端页面添加请求：

```js
async getUserCount () {
  const { _id: uid } = this.userInfo
  if (!uid) return
  await this.getUserInfo()
  const { data, code } = await this.$u.api.wxUserCount({ uid, reqAll: 1 })
  if (code === 200) {
    const { countMyPost, countMyCollect, countMyHistory } = data
    this.countMyPost = countMyPost
    this.countMyCollect = countMyCollect
    this.countMyHistory = countMyHistory
  }
}
```

完整的页面代码：

```vue
<template>
  <view>
    <view class="grey">
      <view class="bg"></view>
      <view class="wrapper">
        <!-- 个人信息卡片 -->
        <view class="profile">
          <view class="info">
            <u-image class="pic" :src="isLogin ? userInfo.pic: ''" width="120" height="120" shape="circle" error-icon="/static/images/header.jpg" />
            <!-- 用户昵称 + VIP -->
            <view class="user" @click="navTo">
              <view class="name">{{isLogin ?userInfo.name : '请登录'}}</view>
              <view class="fav">
                <!-- <van-icon name="fav2" class-prefix="iconfont" size="14"></van-icon> -->
                积分：{{userInfo && userInfo.favs ? userInfo.favs:0}}
              </view>
            </view>
            <view class="link" @click="gotoGuard('/sub-pkg/user-info/user-info')">个人主页 ></view>
            <!-- <navigator class="link" url="/subcom-pkg/auth/auth">个人主页 ></navigator> -->
            <!-- <navigator class="link" url="/sub-pkg/user-info/user-info">个人主页 ></navigator> -->
          </view>
          <view class="stats" v-if="isLogin">
            <view class="item">
              <navigator :url="'/sub-pkg/posts/posts?uid=' + uid + '&type=p'">
                <view>{{ countMyPost }}</view>
                <view class="title">我的帖子</view>
              </navigator>
            </view>
            <view class="item">
              <navigator :url="'/sub-pkg/posts/posts?uid=' + uid+ '&type=c'">
                <view>{{ countMyCollect }}</view>
                <view class="title">收藏夹</view>
              </navigator>
            </view>
            <view class="item">
              <navigator :url="'/sub-pkg/posts/posts?uid=' + uid+ '&type=h'">
                <view>{{ countMyHistory }}</view>
                <view class="title">最近浏览</view>
              </navigator>
            </view>
          </view>
        </view>
      </view>
      <view class="center-wraper">
        <view class="center-list first">
          <li v-for="(item,index) in lists" :key="index">
            <view @click="gotoGuardHandler(item)">
              <i :class="item.icon"></i>
              <span>{{item.name}}</span>
            </view>
          </li>
        </view>
        <view class="center-list">
          <li v-for="(item,index) in routes" :key="index" @click="gotoHome(item.tab)">
            <i :class="item.icon"></i>
            <span>{{item.name}}</span>
          </li>
        </view>
      </view>
    </view>
    <view class="bottom-line"></view>
  </view>
</template>

<script>
import { gotoGuard } from '@/common/checkAuth'
import { mapGetters, mapState, mapMutations } from 'vuex'
export default {
  data: () => ({
    lists: [
      {
        name: '我的帖子',
        icon: 'icon-teizi',
        routeName: '/sub-pkg/posts/posts'
      },
      {
        name: '修改设置',
        icon: 'icon-setting',
        routeName: '/sub-pkg/settings/settings'
      },
      {
        name: '签到中心',
        icon: 'icon-qiandao',
        routeName: '/sub-pkg/sign/sign'
      },
      {
        name: '电子书',
        icon: 'icon-book',
        routeName: '/sub-pkg/books/books'
      },
      {
        name: '关于我们',
        icon: 'icon-about',
        routeName: '/sub-pkg/about/about'
      },
      {
        name: '人工客服',
        icon: 'icon-support',
        routeName: '/sub-pkg/suggest/suggest'
      },
      {
        name: '意见反馈',
        icon: 'icon-lock2',
        routeName: '/sub-pkg/suggest/survey'
      }
    ],
    routes: [
      {
        name: '提问',
        icon: 'icon-question',
        tab: 'ask'
      },
      {
        name: '分享',
        icon: 'icon-share',
        tab: 'share'
      },
      {
        name: '讨论',
        icon: 'icon-taolun',
        tab: 'discuss'
      },
      {
        name: '建议',
        icon: 'icon-advise',
        tab: 'advise'
      }
    ],
    countMyPost: 0,
    countMyCollect: 0,
    countMyHistory: 0
    // isLogin: true
  }),
  computed: {
    ...mapGetters(['isLogin']),
    ...mapState(['userInfo']),
    uid () {
      return this.userInfo._id
    }
  },
  onShow () {
    this.getUserCount()
  },
  methods: {
    ...mapMutations(['setTab', 'setUserInfo']),
    gotoGuard,
    gotoGuardHandler (item) {
      const { name, routeName } = item
      if (name === '我的帖子') {
        gotoGuard(routeName + `?uid=${this.uid}&type=p`)
      } else {
        gotoGuard(routeName)
      }
    },
    gotoHome (tab) {
      this.setTab(tab)
      uni.switchTab({
        url: '/pages/home/home'
      })
    },
    navTo () {
      if (!this.isLogin) {
        uni.navigateTo({
          url: '/subcom-pkg/auth/auth'
        })
      }
    },
    async getUserInfo () {
      const { _id: uid } = this.userInfo
      const { code, data } = await this.$u.api.getBasic({ uid })
      if (code === 200) {
        this.setUserInfo(data)
      }
    },
    async getUserCount () {
      const { _id: uid } = this.userInfo
      if (!uid) return
      await this.getUserInfo()
      const { data, code } = await this.$u.api.wxUserCount({ uid, reqAll: 1 })
      if (code === 200) {
        const { countMyPost, countMyCollect, countMyHistory } = data
        this.countMyPost = countMyPost
        this.countMyCollect = countMyCollect
        this.countMyHistory = countMyHistory
      }
    }
  }

}
</script>

<style lang="scss">
.grey {
  position: fixed;
  width: 100%;
  height: 100%;
  left: 0;
  top: 0;
  z-index: 30;
}
a {
  color: #666;
  text-decoration: none;
}
// .bg {
//   height: 260rpx;
//   // 4个参数： 左上 右上 右下 左下
//   border-radius: 0 0 50% 50%;
//   background-color: #16d1a2;
//   position: relative;
//   z-index: 50;
// }
.bg {
  background-image: url("/static/images/my_bg.png");
  background-repeat: no-repeat;
  background-size: contain;
  position: relative;
  left: 0;
  top: 0;
  width: 100%;
  height: 280rpx;
  background-position: 0 0;
  z-index: 100;
}
.wrapper {
  width: 100%;
  height: 370rpx;
  padding: 25rpx;
  position: absolute;
  left: 0;
  top: 0;
  z-index: 100;
  box-sizing: border-box;
  color: #333;
  .profile {
    background: #fff;
    border-radius: 12rpx;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    width: 100%;
    height: 100%;
    padding: 30rpx;
    box-sizing: border-box;
    .name {
      font-size: 36rpx;
      font-weight: 700;
      margin-bottom: 10rpx;
      margin-top: 0;
      width: 370rpx;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
    }
    .link {
      font-size: 24rpx;
      color: #999;
    }
    .fav {
      display: inline-block;
      padding: 8px 12rpx;
      background: rgba(2, 209, 153, 0.16);
      border-radius: 12rpx;
      color: #02d199;
      margin: 0;
      font-size: 22rpx;
      .icon-fav {
        padding-right: 10rpx;
      }
    }
    .info,
    .stats {
      display: flex;
      flex-flow: row nowrap;
      justify-content: space-between;
      align-items: center;
    }
    .info {
      margin-bottom: 24rpx;
    }
    .stats {
      justify-content: space-around;
    }
    .user {
      flex: 1;
      padding-left: 20rpx;
    }
    .pic {
      width: 120rpx;
      height: 120rpx;
      border-radius: 50%;
    }
    .item {
      text-align: center;
      position: relative;
      p {
        margin-top: 14rpx;
        margin-bottom: 0;
      }
      &:after {
        width: 2rpx;
        height: 80rpx;
        background: #ddd;
        content: "";
        position: absolute;
        right: -60rpx;
        top: 20rpx;
      }
      &:last-child {
        &:after {
          width: 0;
        }
      }
      .title {
        color: #666;
      }
    }
  }
}
.center-wraper {
  background: #f6f5f8;
  position: relative;
  width: 100%;
  // height: 100%;
  z-index: 10;
  .center-list {
    background: #fff;
    margin-bottom: 30rpx;
    display: flex;
    flex-flow: wrap;
    padding-top: 40rpx;
    &.first {
      padding-top: 100rpx;
    }
    li {
      width: 25%;
      text-align: center;
      color: #666;
      margin-bottom: 40rpx;
      font-size: 26rpx;
    }
    i {
      display: block;
      margin: 0 auto;
      font-size: 40rpx;
      width: 56rpx;
      height: 56rpx;
      margin-bottom: 20rpx;
      color: #888;
      background-size: contain;
    }
    .icon-teizi {
      background-image: url("/static/images/teizi@2x.png");
    }
    .icon-setting {
      background-image: url("/static/images/setting@2x.png");
    }
    .icon-lock2 {
      background-image: url("/static/images/lock2@2x.png");
    }
    .icon-support {
      background-image: url("/static/images/support.png");
    }
    .icon-qiandao {
      background-image: url("/static/images/sign1.png");
    }
    .icon-book {
      background-image: url("/static/images/books.png");
    }
    .icon-record {
      background-image: url("/static/images/record@2x.png");
    }
    .icon-about {
      background-image: url("/static/images/about.png");
    }
    // 快捷访问
    .icon-question {
      background-image: url("/static/images/question@2x.png");
    }
    .icon-share {
      background-image: url("/static/images/share@2x.png");
    }
    .icon-taolun {
      background-image: url("/static/images/taolun@2x.png");
    }
    .icon-advise {
      background-image: url("/static/images/advice@2x.png");
    }
  }
}
</style>
```

最终效果：

![image-20210801235142625](uniapp.assets/image-20210801235142625.48262a26.png)



# [#](https://front-end.toimc.com/notes-page/project/community-miniapp/05-RefreshToken机制.html#refreshtoken机制)RefreshToken机制

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/05-RefreshToken机制.html#需求分析)需求分析

用户token不能设置太长的有效时间，这样可以提高接口部分的安全性。

![refreshToken](uniapp.assets/refreshToken.02c26e12.gif)

说明：

* 登录请求/login成功之后会响应token&refreshToken；
* token的过期时间比较短，比如1小时到4小时，refreshToken的过期时间比token要长，比如1天或者7天；
* 用户在登录之后，所有的鉴权的接口带上token；
* 如果token过期，则使用refreshToken进行请求/login/refresh接口，请求新的token；
* 如果refreshToken未过期，则返回新token；
* 如果refreshToken过期，则返回401;

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/05-RefreshToken机制.html#创建refreshtoken接口)创建refreshToken接口

添加`src/routes/modules/loginRouter.js`路由：

```js
// refresh
router.post('/refresh', loginController.refresh)
```

调整登录接口`src/api/LoginController.js`：

```js
  // 微信登录
  async wxLogin (ctx) {
  // 1.解密用户信息
    const { body } = ctx.request
    const { user, code } = body
    if (!code) {
      ctx.body = {
        code: 500,
        data: '没有足够参数'
      }
      return
    }
    const res = await wxGetUserInfo(user, code)
    if (res.errcode === 0) {
    // 2.查询数据库 -> 判断用户是否存在
    // 3.如果不存在 —> 创建用户
    // 4.如果存在 -> 获取用户信息
      const tmpUser = await User.findOrCreateByUnionid(res)
      // 5.产生token，获取用户的签到状态
      const token = generateToken({ _id: tmpUser._id })
      const userInfo = await addSign(tmpUser)
      ctx.body = {
        code: 200,
        data: userInfo,
        token,
        // 新增refreshToken
        refreshToken: generateToken({ _id: tmpUser._id }, '7d')
      }
    } else {
      ctx.throw(501, res.errcode === 40163 ? 'code已失效，请刷新后重试' : '获取用户信息失败，请重试')
    }
  }

  // refreshToken
  async refresh (ctx) {
    ctx.body = {
      code: 200,
      token: generateToken({ _id: ctx._id }, '60m'),
      msg: '获取token成功'
    }
  }
```

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/05-RefreshToken机制.html#前端页面逻辑)前端页面逻辑

* 错误拦截中，针对 401的情况进行单独处理；
  * 返回401之后，使用新的request实例请求refreshToken的接口；
  * 请求成功之后，更新本地的token及缓存；
* 使用新的Token重新发发起旧的请求；

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/05-RefreshToken机制.html#登录失败后处理缓存)登录失败后处理缓存

在`src/store/index.js`中新增关于清除token及用户信息的`actions`：

```js
const actions = {
  logout ({ commit }) {
    commit('setToken', '')
    commit('setUserInfo', {})
    commit('setRefreshToken', '')
    uni.clearStorage()
  }
}
```

最终测试与效果：

![image-20210802192754262](uniapp.assets/image-20210802192754262.f2e58c05.png)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/05-RefreshToken机制.html#封装simple-js)封装Simple.js

simple.js封装新的request补全，用于在默认实例请求401的情况下，发送refreshToken的请求：

```js
export const simpleHttp = (options, { header = {}, callback }) => {
  const result = new Promise((resolve, reject) => {
    uni.request(Object.assign({
      timeout: 10 * 1000
    }, options, {
      header,
      success: (res) => {
        // 请求成功
        if (res.statusCode >= 200 && res.statusCode < 300) {
          resolve(res.data)
        } else {
          reject(res)
        }
      },
      fail: (err) => {
        reject(err)
      },
      complete: () => {
        callback && callback()
      }
    }))
  })
  return result
}
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/05-RefreshToken机制.html#调整errorhandle-401逻辑)调整errorHandle 401逻辑

```js
import { simpleHttp } from './request/simple'

// 调整errorHandle处理流程：
errorHandle: async (err, { options, instance }) => {
  if (err.statusCode === 401) {
    // 4.业务 —> refreshToken -> 请求响应401 -> 刷新token
    try {
      const { code, token } = await simpleHttp({
        method: 'POST',
        url: baseUrl + '/login/refresh'
      }, {
        header: {
          Authorization: 'Bearer ' + uni.getStorageSync('refreshToken')
        }
      })
      if (code === 200) {
        // refreshToken请求成功
        // 1.设置全局的token
        store.commit('setToken', token)
        // 2.重新发起请求
        const newResult = await instance.request(options)
        return newResult
      }
    } catch (error) {
      // 代表refreshToken已经失效
      // 清除本地的token
      store.dispatch('logout')
      // 导航到用户的登录页面
      authNav()
    }
    uni.showToast({
      icon: 'none',
      title: '鉴权失败，请重新登录',
      duration: 2000
    })
  } else {
    // 其他的错误
    // showToast提示用户
    // 3.对错误进行统一的处理 -> showToast
    const { data: { msg } } = err
    uni.showToast({
      icon: 'none',
      title: msg || '请求异常，请重试',
      duration: 2000
    })
  }
}
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/05-RefreshToken机制.html#请求拦截器)请求拦截器

要注意请求拦截器中token的设置方式，可以使用`config.header.Authorization = 'Bearer ' + token`进行赋值：

```js
// 请求拦截，配置Token等参数
Vue.prototype.$u.http.interceptor.request = (config) => {
  // 引用token
  // 1.在头部请求的时候，token带上 -> 请求拦截器
  const publicArr = [/\/public/, /\/login/]
  // local store -> uni.getStorageSync('token')
  let isPublic = false
  publicArr.forEach(path => {
    isPublic = isPublic || path.test(config.url)
  })
  const token = uni.getStorageSync('token')
  if (!isPublic && token) {
    config.header.Authorization = 'Bearer ' + token
  }
  // 最后需要将config进行return
  return config
  // 如果return一个false值，则会取消本次请求
  // if(config.url === '/user/rest') return false; // 取消某次请求
}
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/05-RefreshToken机制.html#调整工具类request-js)调整工具类request.js

原先Request.js封装中，errorhandle的部分只会返回错误的内容。

而我们在errorHandle中需要发送旧的请求，即需要：

* 原请求的相关配置`config`，如url、method等；
* 原请求的实例，上面有请求拦截器，使用token数据发送请求；

思路使用callback回调的方式，把参数传递出来：

```js
const errCallback = { options, instance: this }

// ...
errorHandle(response, errCallback)
```

完整的代码：

```js
import deepMerge from '../function/deepMerge'
// import validate from '../function/test'
class Request {
  // 设置全局默认配置
  setConfig (customConfig) {
    // 深度合并对象，否则会造成对象深层属性丢失
    this.config = deepMerge(this.config, customConfig)
  }

  // 主要请求部分
  request (options = {}) {
    // 检查请求拦截
    if (this.interceptor.request && typeof this.interceptor.request === 'function') {
      const interceptorRequest = this.interceptor.request(options)
      if (interceptorRequest === false) {
        // 返回一个处于pending状态中的Promise，来取消原promise，避免进入then()回调
        return new Promise(() => { })
      }
      this.options = interceptorRequest
    }
    options.dataType = options.dataType || this.config.dataType
    options.responseType = options.responseType || this.config.responseType
    options.url = options.url || ''
    options.params = options.params || {}
    options.header = Object.assign({}, this.config.header, options.header)
    options.method = options.method || this.config.method
    const errCallback = { options, instance: this }
    return new Promise((resolve, reject) => {
      options.complete = (response) => {
        const { errorHandle } = this.config
        // 请求返回后，隐藏loading(如果请求返回快的话，可能会没有loading)
        uni.hideLoading()
        // 清除定时器，如果请求回来了，就无需loading
        clearTimeout(this.config.timer)
        this.config.timer = null
        // 判断用户对拦截返回数据的要求，如果originalData为true，返回所有的数据(response)到拦截器，否则只返回response.data
        if (this.config.originalData) {
          // 判断是否存在拦截器
          if (this.interceptor.response && typeof this.interceptor.response === 'function') {
            const resInterceptors = this.interceptor.response(response)
            // 如果拦截器不返回false，就将拦截器返回的内容给this.$u.post的then回调
            if (resInterceptors !== false) {
              resolve(resInterceptors)
            } else {
              // 如果拦截器返回false，意味着拦截器定义者认为返回有问题，直接接入catch回调
              errorHandle(response, errCallback)
              reject(response)
            }
          } else {
            // 如果要求返回原始数据，就算没有拦截器，也返回最原始的数据
            resolve(response)
          }
        } else {
          if (response.statusCode === 200) {
            if (this.interceptor.response && typeof this.interceptor.response === 'function') {
              const resInterceptors = this.interceptor.response(response.data)
              if (resInterceptors !== false) {
                resolve(resInterceptors)
              } else {
                errorHandle(response, errCallback)
                reject(response.data)
              }
            } else {
              // 如果不是返回原始数据(originalData=false)，且没有拦截器的情况下，返回纯数据给then回调
              resolve(response.data)
            }
          } else {
            // 不返回原始数据的情况下，服务器状态码不为200，modal弹框提示
            // if(response.errMsg) {
            //  uni.showModal({
            //   title: response.errMsg
            //  });
            // }
            errorHandle(response, errCallback)
            reject(response)
          }
        }
      }

      // 判断用户传递的URL是否/开头,如果不是,加上/，这里使用了uView的test.js验证库的url()方法
      // 情景一：如果是使用的官方的uview组件库 -> 创建新的reg -> url
      // 情况二：如果是自己定义的request工具js -> 替换正则
      options.url = /^https?:\/\/.*/.test(options.url)
        ? options.url
        : (this.config.baseUrl + (options.url.indexOf('/') === 0
            ? options.url
            : '/' + options.url))
      // console.log('🚀 ~ file: index.js ~ line 82 ~ Request ~ returnnewPromise ~ options.url', options.url, /^https?:\/\/.*/.test(options.url))

      // 是否显示loading
      // 加一个是否已有timer定时器的判断，否则有两个同时请求的时候，后者会清除前者的定时器id
      // 而没有清除前者的定时器，导致前者超时，一直显示loading
      if (this.config.showLoading && !this.config.timer) {
        this.config.timer = setTimeout(() => {
          uni.showLoading({
            title: this.config.loadingText,
            mask: this.config.loadingMask
          })
          this.config.timer = null
        }, this.config.loadingTime)
      }
      uni.request(options)
    })
    // .catch(res => {
    //  // 如果返回reject()，不让其进入this.$u.post().then().catch()后面的catct()
    //  // 因为很多人都会忘了写后面的catch()，导致报错捕获不到catch
    //  return new Promise(()=>{});
    // })
  }

  constructor () {
    this.config = {
      baseUrl: '', // 请求的根域名
      // 默认的请求头
      header: {},
      method: 'POST',
      // 设置为json，返回后uni.request会对数据进行一次JSON.parse
      dataType: 'json',
      // 此参数无需处理，因为5+和支付宝小程序不支持，默认为text即可
      responseType: 'text',
      showLoading: true, // 是否显示请求中的loading
      loadingText: '请求中...',
      loadingTime: 800, // 在此时间内，请求还没回来的话，就显示加载中动画，单位ms
      timer: null, // 定时器
      originalData: false, // 是否在拦截器中返回服务端的原始数据，见文档说明
      loadingMask: true // 展示loading的时候，是否给一个透明的蒙层，防止触摸穿透
    }

    // 拦截器
    this.interceptor = {
      // 请求前的拦截
      request: null,
      // 请求后的拦截
      response: null
    }

    // get请求
    this.get = (url, data = {}, header = {}) => {
      return this.request({
        method: 'GET',
        url,
        header,
        data
      })
    }

    // post请求
    this.post = (url, data = {}, header = {}) => {
      return this.request({
        url,
        method: 'POST',
        header,
        data
      })
    }

    // put请求，不支持支付宝小程序(HX2.6.15)
    this.put = (url, data = {}, header = {}) => {
      return this.request({
        url,
        method: 'PUT',
        header,
        data
      })
    }

    // delete请求，不支持支付宝和头条小程序(HX2.6.15)
    this.delete = (url, data = {}, header = {}) => {
      return this.request({
        url,
        method: 'DELETE',
        header,
        data
      })
    }
  }
}
export default new Request()
```



# [#](https://front-end.toimc.com/notes-page/project/community-miniapp/06-文章详情.html#文章详情)文章详情

完成效果：

![image-20210527020457329](uniapp.assets/image-20210527020457329.01b5962e.png)

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/06-文章详情.html#页面布局与样式)页面布局与样式

基本的步骤

* 新增页面`/subcom-pkg/detail/detail`

* 配置`pages.json`
* 调整页面的样式与接口

配置`pages.json`

```json
"subPackages": [
  {
    "root": "subcom-pkg",
    "pages": [
      ...
      {
        "path": "detail/detail",
        "style": {
          "navigationBarTitleText": "文章详情"
        }
      }
    ]
  }
]
```

页面样式与基础逻辑

```vue
<template>
  <view class="detail" v-show="page._id" @click.stop="showReply = false">
    <view class="header title">
      {{page.title}}
    </view>
    <view class="content">
      <view class="user u-flex">
        <u-image class="photo" :src="page.uid.pic" error-icon="/static/images/header.jpg" width="72" height="72" />
        <view class="user-column u-flex-1">
          <span class="name">{{page.uid.name}}</span>
          <span class="label">{{ page.created | moment }}</span>
        </view>
      </view>
      <u-parse :html="page.content">
        <view class="title"></view>
      </u-parse>
    </view>
    <view class="comments" :style="{'padding-bottom': paddingHeight + 'px'}">
      <view class="title">评论</view>
      <view class="item" v-for="(item) in comments" :key="item._id">
        <view class="u-flex u-col-center">
          <view class="user u-flex-1">
            <u-image class="photo" :src="item.cuid.pic" error-icon="/static/images/header.jpg" width="72" height="72" />
            <view class="user-column u-flex-1">
              <span class="name">{{ item.cuid.name }}</span>
              <span class="label">{{ item.created | moment }} 回复了你</span>
            </view>
          </view>
          <view class="u-flex u-col-center add-hand">
            <view class="reply" :class="{'active': item.handed === '1'}" @click="hand(item)">
              <u-icon name="thumb-up-fill" size="30" v-if="item.handed === '1'"></u-icon>
              <u-icon name="thumb-up" size="30" v-else></u-icon>
              <text>{{item.hands}}</text>
            </view>
            <view v-if="isOwner">
              <view class="caina" v-if="item.isBest === '1'">
                <u-icon name="yicaina" custom-prefix="iconfont" size="70" color="#58a571"></u-icon>
              </view>
              <view class="setBest" v-else-if="parseInt(page.isEnd) === 0 && parseInt(item.isBest) === 0" @click="setBest(item)">
                <u-icon name="caina" custom-prefix="iconfont" size="32"></u-icon>
              </view>
            </view>
          </view>
        </view>
        <view class="comments-content">{{item.content}}</view>
      </view>
      <view v-if="comments.length === 0">
        <view v-if="!loading" class="info">
          暂无评论，赶紧来抢沙发吧~~~
        </view>
        <view class="flex-center-center loading" v-else>
          <u-loading class="loading-icon" mode="circle"></u-loading>
          <text class="loading-text">加载中...</text>
        </view>
      </view>
    </view>
    <view class="footer">
      <view class="box u-flex u-col-center" v-if="!showReply">
        <view class="add-comment" @click.stop="reply()">
          <u-icon name="edit-pen" size="32" color="#cccccc"></u-icon>
          <text class="text">写评论</text>
        </view>
        <view class="ctrls u-flex u-col-center u-row-between">
          <view class="comment u-flex flex-column">
            <u-icon name="chat" size="45"></u-icon>
            <text>评论{{ page.answer > 0 ? page.answer : ''}}</text>
          </view>
          <view class="fav u-flex flex-column" :class="{'active': page.isFav === 1}" @click="setCollect">
            <u-icon name="star-fill" size="45" v-if="page.isFav === 1"></u-icon>
            <u-icon name="star" size="45" v-else></u-icon>
            <text>{{page.isFav === 1 ? '已收藏': '收藏'}}</text>
          </view>
          <view class="like u-flex flex-column" :class="{'active': page.isHand === 1}" @click="handsPost">
            <u-icon name="thumb-up-fill" size="45" v-if="page.isHand === 1"></u-icon>
            <u-icon name="thumb-up" size="45" v-else></u-icon>
            <text>{{page.isHand === 1 ? '已点赞' : '点赞'}}</text>
          </view>
        </view>
      </view>
      <view class="box u-flex u-col-center" v-else>
        <u-input v-model="content" class="reply" placeholder="请输入评论内容" focus @clear="clear"></u-input>
        <button type="primary" plain size="mini" @click.stop="send">发送</button>
      </view>
    </view>
  </view>
</template>

<script>
import { mapGetters, mapState } from 'vuex'
import { checkToken } from '@/common/checkAuth'

export default {
  components: {},
  data: () => ({
    page: {},
    comments: [],
    params: {
      page: 0,
      limit: 10,
      tid: ''
    },
    content: '',
    showReply: false,
    height: 0,
    paddingHeight: 60,
    loading: false
  }),
  computed: {
    ...mapState(['userInfo']),
    ...mapGetters(['isLogin']),
    isCollect () {
      return typeof this.page.isFav !== 'undefined' && this.page.isFav === 1
    },
    isOwner () {
      let flag = false
      if (this.page.uid && typeof this.page.uid !== 'undefined' && typeof this.userInfo._id !== 'undefined') {
        flag = this.page.uid._id === this.userInfo._id
      }
      return flag
    }
  },
  methods: {
    check: checkToken,
    async getReply () {
      const { data } = await this.$u.api.getComents(this.params)
      const arr = data.reverse()
      if (this.params.page === 0) {
        this.comments = arr
      } else {
        this.comments = [...this.comments, ...arr]
      }
      this.page.answer = this.comments.length || 0
    },
    async handsPost () {
      // 文章点赞
    },
    async hand (item) {
      // 评论点赞
      if (!this.check()) return
      const { msg, data, code } = await this.$u.api.setHands({ cid: item._id })
      if (code === 200 && data) {
        item.handed = '1'
        item.hands++
      } else {
        uni.showToast({
          icon: 'none',
          title: msg,
          duration: 2000
        })
      }
    },
    async setCollect () {
      // 设置收藏
      if (!this.check()) return
      const { msg, isCollect } = await this.$u.api.addCollect({ tid: this.params.tid, isFav: this.isCollect ? 1 : 0 })
      if (isCollect) {
        this.page.isFav = 1
      } else {
        this.page.isFav = 0
      }
      uni.showToast({
        icon: 'none',
        title: msg,
        duration: 2000
      })
    },
    reply () {
      if (!this.check()) return
      this.showReply = true
    },
    async send () {
      // 微信评论
    },
    setBest (item) {
      // 设置最佳
    },
    onShareAppMessage () {
      // 微信分享
    }
  },
  watch: {},
  // 页面周期函数--监听页面加载
  async onLoad (options) {
    this.loading = true
    const { tid } = options
    this.params.tid = tid
    const { data } = await this.$u.api.getDetail({ tid })
    this.page = data
    await this.getReply()
    this.loading = false
  },
  // 页面周期函数--监听页面初次渲染完成
  onPullDownRefresh () {
    uni.stopPullDownRefresh()
  },
  // 页面处理函数--监听用户上拉触底
  onReachBottom () {}
  // 页面处理函数--监听页面滚动(not-nvue)
  /* onPageScroll(event) {}, */
  // 页面处理函数--用户点击右上角分享
  /* onShareAppMessage(options) {}, */
}
</script>

<style lang="scss">
.detail {
  background: #f4f6f8;
  min-height: 100vh;
}

.header,
.content,
.comments {
  background: #fff;
  padding: 32rpx;
}

.header,
.content {
  margin-bottom: 24rpx;
  box-shadow: 0 5rpx 5px rgba($color: black, $alpha: 0.1);
}

.add-hand {
  position: relative;
  .caina {
    position: absolute;
    right: 100rpx;
    top: -20rpx;
  }
  .setBest {
    padding-left: 25rpx;
  }
}

.footer {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100vw;
  padding: 20rpx 32rpx;
  background-color: #fff;
  // height: 100rpx;
  box-shadow: 0 -5rpx 5px rgba($color: black, $alpha: 0.1);
  .box {
    width: 100%;
  }
  .reply {
    flex: 1;
    border: 1px solid #eee;
    padding: 0 15rpx;
    margin-right: 15rpx;
  }
}

.title {
  font-size: 32rpx;
  color: #333;
  font-weight: bold;
}

.user {
  display: flex;
  align-items: center; /* 垂直居中 */
  margin-right: 20rpx;
  .name {
    margin-bottom: 10rpx;
    font-size: 28rpx;
    font-family: PingFang SC;
    font-weight: bold;
    color: rgba(51, 51, 51, 1);
    white-space: nowrap;
    max-width: 420rpx;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  .photo {
    width: 72rpx;
    height: 72rpx;
    border-radius: 50%;
  }
  .user-column {
    display: flex;
    flex-direction: column;
    margin-left: 20rpx;
  }
  .label {
    font-size: 22rpx;
    font-family: PingFang SC;
    font-weight: 500;
    color: rgba(153, 153, 153, 1);
  }
}

.comments {
  .item {
    padding: 24rpx 0;
    .comments-content {
      padding-top: 32rpx;
    }
    .reply {
      text {
        padding-left: 10rpx;
      }
      &.active {
        color: $u-type-primary;
      }
    }
  }
  .info {
    font-size: 28rpx;
    color: #666;
    line-height: 90rpx;
    text-align: center;
  }
}

.ctrls {
  color: #999;
  font-size: 22rpx;
  width: 35%;
  .fav,
  .like {
    &.active {
      color: $u-type-primary;
    }
  }
}

.add-comment {
  background: #f3f3f3;
  height: 64rpx;
  border-radius: 32rpx;
  line-height: 64rpx;
  padding: 0 32rpx;
  width: 65%;
  margin-right: 40rpx;
  color: #ccc;
  .text {
    padding-left: 10rpx;
  }
}

.loading {
  height: 50px;
  .loading-text {
    padding-left: 15rpx;
  }
}
</style>
```

App.vue页面中添加公共样式：

```scss
.flex-column {
  flex-direction: column;
}
```

调整api接口：

```js
// 获取文章中的评论列表
const getComents = (params) => {
  const token = store.state.token
  let headers = {}
  if (token !== '') {
    headers = {
      headers: {
        Authorization: 'Bearer ' + store.state.token
      }
    }
  }
  return axios.get('/public/comments', params, headers)
}

// 获取文章详情
const getDetail = (data) => {
  const token = store.state.token
  let headers = {}
  if (token !== '') {
    headers = {
      headers: {
        Authorization: 'Bearer ' + store.state.token
      }
    }
  }
  return axios.get('/public/content/detail', data, headers)
}
```

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/06-文章详情.html#长屏适配方案)长屏适配方案

安全区域指的是一个可视窗口范围，处于安全区域的内容不受圆角（corners）、齐刘海（sensor housing）、小黑条（Home Indicator）影响，如下图蓝色区域：

![img](uniapp.assets/iphonex_4.7d2b50ea.png)

也就是说，我们要做好适配，必须保证页面可视、可操作区域是在安全区域内。

解决方案env() 和 constant()：

iOS11 新增特性，Webkit 的一个 CSS 函数，用于设定安全区域与边界的距离，有四个预定义的变量：

* safe-area-inset-left：安全区域距离左边边界距离
* safe-area-inset-right：安全区域距离右边边界距离
* safe-area-inset-top：安全区域距离顶部边界距离
* safe-area-inset-bottom：安全区域距离底部边界距离

这里我们只需要关注 safe-area-inset-bottom 这个变量，因为它对应的就是小黑条的高度（横竖屏时值不一样）。

> 注意：当 viewport-fit=contain 时 env() 是不起作用的，必须要配合 viewport-fit=cover 使用。对于不支持env() 的浏览器，浏览器将会忽略它。

在这之前，笔者使用的是 constant()，后来，官方文档加了这么一段注释（坑）：

> The env() function shipped in iOS 11 with the name constant(). Beginning with Safari Technology Preview 41 and the iOS 11.2 beta, constant() has been removed and replaced with env(). You can use the CSS fallback mechanism to support both versions, if necessary, but should prefer env() going forward.

这就意味着，之前使用的 constant() 在 iOS11.2 之后就不能使用的，但我们还是需要做向后兼容，像这样：

```javascript
padding-bottom: constant(safe-area-inset-bottom); /* 兼容 iOS < 11.2 */
padding-bottom: env(safe-area-inset-bottom); /* 兼容 iOS >= 11.2 */
```

注意：env() 跟 constant() 需要同时存在，而且顺序不能换。

更详细说明，参考文档： [Designing Websites for iPhone X(opens new window)](https://webkit.org/blog/7929/designing-websites-for-iphone-x/?hmsr=funteas.com&utm_medium=funteas.com&utm_source=funteas.com)

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/06-文章详情.html#页面分享设置)页面分享设置

与页面分享相关的uniapp的API有两个：

* [uni.share(OBJECT) (opens new window)](https://uniapp.dcloud.io/api/plugins/share?id=share)—— 针对于App分享到微信、QQ、微博
* [onShareAppMessage(OBJECT) (opens new window)](https://uniapp.dcloud.io/api/plugins/share?id=onshareappmessage)—— 针对于小程序的页面的分享

小程序中用户点击分享后，在 js 中定义 onShareAppMessage 处理函数（和 onLoad 等生命周期函数同级），设置该页面的分享信息。

* 用户点击分享按钮的时候会调用。这个分享按钮可能是小程序右上角原生菜单自带的分享按钮，也可能是开发者在页面中放置的分享按钮（`<button open-type="share">`）；
* 此事件需要 return 一个Object，用于自定义分享内容。

微信小程序平台的分享管理比较严格，请参考 [小程序分享指引 (opens new window)](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/share.html)。

在详情页面中加入`onShareAppMessage`方法：

```js
onShareAppMessage () {
  // 微信分享 -> 这个分享单一的好友
  return {
    title: this.page.title,
    path: '/subcom-pkg/detail/detail?tid=' + this.params.tid
  }
}
```

> 目前，微信官方没有提供正式的朋友圈分享的功能，只是在android设备上进行beta测试，参考说明：https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onShareTimeline
>
> ![image-20210802212601310](uniapp.assets/image-20210802212601310.a2267358.png)

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/06-文章详情.html#富文本显示)富文本显示

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/06-文章详情.html#高亮方案一-自定义的highlight组件)高亮方案一：自定义的highlight组件

步骤：

* 定义highlight.vue组件；
* 安装prismjs库，并使用tokenize方法进行拆分html字符串；
* 使用normalize库进行转换为对象数组；

安装依赖：

```text
npm i prismjs
```

自定义的`highlight.vue`组件

```vue
<template>
  <view class="highlight">
    <scroll-view scroll-x>
      <view v-for="(line,i) in tokenLines" :key="i" class="lines">
        <!-- 行数 -->
        <text class="line-number">{{i+1}}</text>
        <!-- 代码块 -->
        <text v-for="(token,index) in line" :key="index" :class="'token--' + token.type">{{token.content}}</text>
      </view>
    </scroll-view>
  </view>
</template>

<script>
import Prism from 'prismjs'
import normalize from '@/common/utils/normalize'

// const code = `
// <template>
//   <view class="list-item">
//     <view class="list-head">
//       <!-- 标题部分 -->
//       <text class="type" :class="['type-'+item.catalog]">{{tabs.filter(o => o.key === item.catalog)[0].value}}</text>
//       <text class="title">{{item.title}}</text>
//     </view>
//     <!-- 用户部分 -->
//     <view class="author u-flex u-m-b-18">
//       <u-image :src="item.uid.pic" class="head" width="40" height="40" shape="circle" error-icon="/static/images/header.jpg"></u-image>
//       <text class="name u-m-l-10">{{item.uid.name}}</text>
//     </view>
//     <!-- 摘要部分 + 右侧的图片 -->
//     <view class="list-body u-m-b-30 u-flex u-col-top">
//       <view class="info u-m-r-20 u-flex-1">{{item.content}}</view>
//       <image class="fmt" :src="item.snapshot" v-if="item.snapshot" mode="aspectFill" />
//     </view>
//     <!-- 回复 + 文章发表的时间 -->
//     <view class="list-footer u-flex">
//       <view class="left">
//         <text class="reply-num u-m-r-25">{{item.answer}} 回复</text>
//         <text class="timer">{{item.created | moment}}</text>
//       </view>
//     </view>
//   </view>
// </template>
// `
export default {
  props: {
    code: {
      type: String,
      default: ''
    },
    language: {
      type: String,
      default: 'js'
    }
  },
  data: () => ({
  }),
  computed: {
    tokenLines () {
      const result = normalize(Prism.tokenize(this.code, Prism.languages[this.language]))
      // console.log('🚀 ~ file: highlight.vue ~ line 54 ~ tokenLines ~ result', result)
      return result
    }
  },
  methods: {}
}
</script>

<style lang="scss" scoped>
.intro {
  margin: 30px;
  text-align: center;
}

:host {
  text-align: left;
  font-family: consolas, monospace;
  line-height: 1.44;
  white-space: nowrap;
}

.lines {
  display: flex;
  flex-flow: row nowrap;
  width: 100%;
  align-items: center;
  justify-content: flex-start;
}

.line-number {
  display: inline-block;
  flex-shrink: 0;
  border-right: 4px solid #d8d8d8;
  min-width: 55px;
  text-align: right;
  margin-right: 10px;
  padding-right: 10px;
  color: #888;
  &.max {
    width: 110px;
  }
}

.token {
  color: #333;
  white-space: pre;
}

.token--plain,
.token--string {
  white-space: pre;
}

.token--keyword {
  color: #00f;
}

.token--number {
  color: #09885a;
}

.token--string {
  color: #a31515;
}

.token--regex {
  color: #811f3f;
}

.token--comment {
  color: #008000;
}
</style>
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/06-文章详情.html#高亮方案二-wxparse)高亮方案二：wxParse

效果展示：

![img](uniapp.assets/image-20210803112201381.b1453768.png) ![img](uniapp.assets/image-20210803112237505.ba04d822.png)

* 1.将`prism.css`重命名为`prism.wxss`
* 2.复制`prism.js`和`prism.wxss`至`utils`文件夹下
* 3.替换`prism.wxss`中的`code[class*="language-"]`为`.wxParse-code[class*="language-"]`
* 4.`wxParse.wxss`中引用`prism.wxss`

```css
@import "../utils/prism.wxss";
```

* 5.新增`highlight.js`高亮工具类（代码放置在wxParse文件夹下）

```js
var Prism = require('../utils/prism.js')
function highlight(data, that) {
  // Prism 所支持的代码语言数组
  let langArr = new Array();
  langArr = listLanguages();
  // console.log('all-language:'+langArr)
  let html = data;
  //匹配到的所有标签<\/code>
  let tagArr = data.match(/<\/?code[^>]*>/g);
  if (tagArr == null) {
    return html;// 如果没有 pre 标签就直接返回原来的内容，不做代码高亮处理
  }
  //记录每一个 code 标签在data中的索引位置
  let indexArr = [];
  //计算索引位置
  for (let i = 0; i < tagArr.length; i++) {
    //添加索引值
    if (i == 0) {
      indexArr.push(data.indexOf(tagArr[i]));
    }
    else {
      indexArr.push(data.indexOf(tagArr[i], indexArr[i - 1]));
    }
  }

  //记录基本的class信息
  let cls;

  // 开始循环处理 code 标签
  let i = 0;
  while (i < tagArr.length - 1) {// 这里减一是因为不处理最后的 code 标签
    // 调用函数来获取 class 信息
    getStartInfo(tagArr[i])
    // 获取标签的值
    var label = tagArr[i].match(/<*([^> ]*)/)[1];
    // console.log('label:'+label)
    if (tagArr[i + 1] === '</' + label + '>') {//判断紧跟它的下一个标签是否为它的闭合标签
      if (label === 'code') {
        // 代码语言判断,根据类进行判断，自定义，比如 lang-语言,language-语言。
        let lang = cls.split(' ')[0];
        if (/lang-(.*)/i.test(lang)) {// 代码语言定义是 lang-XXX 的样式
          lang = lang.replace(/lang-(.*)/i, '$1');
        }
        else if (/languages?-(.*)/i.test(lang)) {
          lang = lang.replace(/languages?-(.*)/i, '$1');// 代码语言定义是 language(s)-XXX 的样式
        }
        // 如果代码语言不在 Prism 存在的语言，或者 class 值是 null，则不执行代码高亮函数
        if (langArr.indexOf(lang) == -1 || lang == null || lang == 'none' || lang == 'null') {
        }
        else {
          // 获取代码段内容为 code
          let code = data.substring(indexArr[i], indexArr[i + 1]).replace(/<code[^>]*>/, '');

          // 执行 Prism 的代码高亮函数
          let hcode = Prism.highlight(code, Prism.languages[lang], lang);
          html = html.replace(code, hcode);
        }

      }
      // 指向下一个标签（闭合标签）索引
      i++;
    } else {
      //onsole.log('不是闭包')
    }
    // 指向下一个标签（开始标签）的索引
    i++;
  }
  return html;

  function getStartInfo(str) {
    cls = matchRule(str, 'class');
  }

  //获取部分属性的值
  function matchRule(str, rule) {
    let value = '';
    let re = new RegExp(rule + '=[\'"]?([^\'"]*)');
    //console.log('regexp:'+re)
    if (str.match(re) !== null) {
      value = str.match(re)[1];
      //console.log('value:'+value)
    }
    return value;
  }


  // 列出当前 Prism.js 中已有的代码语言，可以自己在 Prism 的下载页面选择更多的语言。
  function listLanguages() {
    var langs = new Array();
    let i = 0;
    for (let language in Prism.languages) {
      if (Object.prototype.toString.call(Prism.languages[language]) !== '[object Function]') {
        langs[i] = language;
        i++;
      }
    }
    return langs;
  }
}

module.exports = {
  highlight: highlight
};
```

* 6.在`wxParse`文件夹下的`html2json.js`中引用`highlight.js`工具类的`highligh`高亮函数

```js
//引用`highlight.js`工具类
var highlight = require('./highlight.js');

function html2json(html, bindName, that) {
    html = removeDOCTYPE(html);
    html = trimHtml(html);
    html = wxDiscode.strDiscode(html);
    //引用高亮函数
    html = highlight.highlight(html, that); 

    //省略了后续代码
    ...

}
```

参考链接 ：

https://blog.sunriseydy.top/technology/server-blog/wordpress/wordpress-miniapp-code-highlight

https://blog.csdn.net/qq_41107410/article/details/89042212



# [#](https://front-end.toimc.com/notes-page/project/community-miniapp/07-安全域名相关.html#安全域名相关)安全域名相关

微信小程序的安全域名必须是HTTPS，同时必须是在国内ICP备案的域名，本章来介绍如何使用acme.sh+nginx配置https与申请SSL证书。

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/07-安全域名相关.html#ssl证书申请)SSL证书申请

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/07-安全域名相关.html#前置准备)前置准备

* 申请域名：阿里云、腾讯云等各家云服务商；

* 有一台云主机，有公网的IP，最好进行过备案；如果没有国内的服务器，也可以注册一台国外的服务器，地址1：[搬瓦工 (opens new window)](https://bwh81.net/aff.php?aff=6389)，地址2：[vultr (opens new window)](https://www.vultr.com/?ref=6862890)；

* 云主机的操作系统：Centos 7+（以下所有演示内容，均为Centos 7）或者Ubuntu 16LTS+；

* DNS解析：DNSpod，cloudflare；

  配置域名进行解析：

  ![image-20210810145545806](uniapp.assets/image-20210810145545806.a0e07748.png)

  使用A记录，配置自己的域名 -> 自己的云主机IP

  使用ping命令检查 是否已经网络通了：

  ![image-20210810145801200](uniapp.assets/image-20210810145801200.cc140063.png)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/07-安全域名相关.html#整体架构)整体架构

配置架构图：

![image-20210810144327946](uniapp.assets/image-20210810144327946.bbb98c11.png)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/07-安全域名相关.html#基本步骤)基本步骤

借助[acme.sh (opens new window)](https://github.com/acmesh-official/acme.sh/wiki/说明)进行申请SSL证书，以便微信接口部分的使用，下面演示的是DNSpod进行证书申请的过程。

**主要步骤：**

前置：使用ssh命令连接到服务器 ->

1. 安装 **acme.sh**

2. 生成证书 ——[推荐DNS的方式(opens new window)](https://github.com/acmesh-official/acme.sh/wiki/dnsapi)

   > 支持的DNS列表：[https://github.com/acmesh-official/acme.sh/tree/master/dnsapi(opens new window)](https://github.com/acmesh-official/acme.sh/tree/master/dnsapi)

3. copy 证书到 nginx/apache 或者其他服务

4. 更新证书

5. 更新 **acme.sh**

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/07-安全域名相关.html#acme-sh安装)acme.sh安装

安装很简单, 一个命令:

```text
curl  https://get.acme.sh | sh -s email=my@example.com
```

![image-20210810145930893](uniapp.assets/image-20210810145930893.237427d0.png)

再次打开一个新的ssh终端，使用`crontab -e`(Centos)查看`acme.sh`自动创建的定时任务：

![image-20210810150236024](uniapp.assets/image-20210810150236024.e82227d2.png)

输入`i`进入编辑模式，调整如上图所示，使用`:wq`退出。

> PS：上面的配置是每天的0点23分去执行一次脚本。

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/07-安全域名相关.html#配置dns密钥)配置DNS密钥

下面介绍了DNSpod、阿里云、Cloudflare的配置过程，大家可以根据自己的云服务商选择，过程类似。

#### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/07-安全域名相关.html#dnspod)DNSpod

* 选择密钥管理：

  ![image-20210810150640094](uniapp.assets/image-20210810150640094.3d656e26.png)

* 创建密钥

  ![image-20210810150731394](uniapp.assets/image-20210810150731394.b27888e3.png)

* 记录下来，后续使用：

  ![image-20210810150751582](uniapp.assets/image-20210810150751582.43ca872b.png)

配置密钥：

```sh
export DPI_Id="1234"
export DPI_Key="sADDsdasdgdsf"
```

#### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/07-安全域名相关.html#阿里云)阿里云

点击：[https://ak-console.aliyun.com/#/accesskey (opens new window)](https://ak-console.aliyun.com/#/accesskey)，申请密钥：

![image-20210810151001761](uniapp.assets/image-20210810151001761.9040174d.png)

选择`编程访问`：

![image-20210810151252035](uniapp.assets/image-20210810151252035.d404c680.png)

生成accessKey与密钥：

![image-20210810151319187](uniapp.assets/image-20210810151319187.d113629f.png)

配置过程：

```sh
export Ali_Key="sdfsdfsdfljlbjkljlkjsdfoiwje"
export Ali_Secret="jlsdflanljkljlfdsaklkjflsa"
```

#### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/07-安全域名相关.html#cloudflare)Cloudflare

配置[页面 (opens new window)](https://dash.cloudflare.com/profile)：

![image-20210810151518194](uniapp.assets/image-20210810151518194.3045c842.png)

配置过程：

```shell
export CF_Key="sdfsdfsdfljlbjkljlkjsdfoiwje"
export CF_Email="xxxx@sss.com"
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/07-安全域名相关.html#产生证书)产生证书

* Dnspod:

  ```sh
  acme.sh --issue --dns dns_dpi -d example.com -d www.example.com
  ```

* 阿里云：

  ```sh
  acme.sh --issue --dns dns_ali -d example.com -d www.example.com
  ```

* Cloudflare：

  ```sh
  acme.sh --issue --dns dns_cf -d example.com -d www.example.com
  ```

> 推荐域名通配符的写法：example.com 与 *.example.com，那么二级子域名，全部都可以使用https。

过程1：

![image-20210810152354761](uniapp.assets/image-20210810152354761.b8f93ecb.png)

上面的图片是展示出在对域名进行校验。

过程2：

![image-20210810152415177](uniapp.assets/image-20210810152415177.a8f72743.png)

证书申请成功，证书所在位置。

```text
cer 自签
key 密钥
CA 中间证书
把CA与cert链接到一起的证书，这个是后续放在nginx上的，用于服务器之间的协商
```

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/07-安全域名相关.html#nginx配置)Nginx配置

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/07-安全域名相关.html#前置准备-2)前置准备

![image-20210810205823556](uniapp.assets/image-20210810205823556.b18f986b.png)

步骤：

* 安装docker,docker-compose -> nginx
* docker-compose来启动nginx
* 创建一个https docker网络 -> 共享给github, jenkins 等需要https的服务
* 配置`nginx.conf`配置文件，创建`conf.d`文件目录，可以创建虚拟域名`vhost.conf`文件
* 使用证书安装命令安装ssl证书 -> 到指定的目录
* `docker-compose up -d`运行nginx容器 -> 测试cron定时任务脚本

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/07-安全域名相关.html#证书安装命令)证书安装命令

Apache配置:

```sh
acme.sh --install-cert -d example.com \
--cert-file      /path/to/certfile/in/apache/cert.pem  \
--key-file       /path/to/keyfile/in/apache/key.pem  \
--fullchain-file /path/to/fullchain/certfile/apache/fullchain.pem \
--reloadcmd     "service apache2 force-reload"
```

Nginx配置:

```sh
acme.sh --install-cert -d example.com \
--key-file       /path/to/keyfile/in/nginx/key.pem  \
--fullchain-file /path/to/fullchain/nginx/cert.pem \
--reloadcmd     "service nginx force-reload"
```

> `--reloadcmd`可以指定docker容器进行重启，或者是指定运行shell脚本。
>
> 比如：
>
> ```
> --reloadcmd "docker restart some-nginx"
> ```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/07-安全域名相关.html#dhparam-pem证书)dhparam.pem证书

创建一个目录：`/home/keys`

```text
# 生成 dhparam.pem 文件, 在命令行执行任一方法:

# 方法1: 很慢
openssl dhparam -out /etc/nginx/ssl/dhparam.pem 2048

# 方法2: 较快
# 与方法1无明显区别. 2048位也足够用, 4096更强
openssl dhparam -dsaparam -out /etc/nginx/ssl/dhparam.pem 4096
```

把dhparam.pem复制到个人SSL证书安装相同的目录`/home/keys`。

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/07-安全域名相关.html#docker配置)docker配置

* 手动创建网络

  ```text
  docker network create https
  ```

  ![image-20210810210138510](uniapp.assets/image-20210810210138510.dce600c7.png)

  > 查看网络 `docker network ls`
  >
  > 检查网络的信息 `docker network inspect https`

* 创建`docker-compose.yml`文件

```yaml
version: "3"
services:
  web:
    image: nginx:latest
    container_name: "some-nginx"
    restart: always
    volumes:
      - /home/nginx/nginx.conf:/etc/nginx/nginx.conf
      - /home/nginx/conf.d:/etc/nginx/conf.d
      - /home/keys:/home/keys
      # blog
      # - /home/blog:/var/www
    ports:
      - "80:80"
      - "443:443"

# docker network create https
networks:
  default:
    external:
      name: https
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/07-安全域名相关.html#nginx-conf配置)nginx.conf配置

```shell
user nginx;
worker_processes auto;
pid /run/nginx.pid;
worker_rlimit_nofile 65535;

events {
	# 设置事件驱动模型，是内核2.6以上支持
	use epoll;
	worker_connections 65535;
	accept_mutex off;
	multi_accept off;
}

http {
	# Basic Settings
	sendfile on;
	tcp_nopush on;
	tcp_nodelay on;
	send_timeout 120;
	keepalive_timeout 300;
	client_body_timeout 300;
	client_header_timeout 120;

	proxy_read_timeout 300;
	proxy_send_timeout 300;
	#tcp_nopush on;
	types_hash_max_size 4096;
	client_header_buffer_size 16m;
	client_max_body_size 4096m;

  # 添加nginx的配置文件目录 -> 用户后期添加vhost文件
	include /etc/nginx/mime.types;
	include /etc/nginx/conf.d/*.conf;

	default_type application/octet-stream;
	# Logging Settings
	access_log /var/log/nginx/access.log;
	error_log /var/log/nginx/error.log;
	log_format main '$remote_addr - $remote_user [$time_local] "$request" '
	'$status $body_bytes_sent "$http_referer" '
	'"$http_user_agent" "$http_x_forwarded_for"';
	
	# 开启gzip
	gzip on;
	# 启用gzip压缩的最小文件，小于设置值的文件将不会压缩
	gzip_min_length 1k;
	# gzip 压缩级别，1-10，数字越大压缩的越好，也越占用CPU时间，后面会有详细说明
	gzip_comp_level 2;
	# 进行压缩的文件类型。javascript有多种形式。其中的值可以在 mime.types 文件中找到。
	gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png font/ttf font/otf image/svg+xml;
	# 是否在http header中添加Vary: Accept-Encoding，建议开启
	gzip_vary on;
	# 禁用IE 6 gzip
	gzip_disable "MSIE [1-6]\.";
}
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/07-安全域名相关.html#安装证书启动nginx)安装证书启动nginx

![image-20210810211832312](uniapp.assets/image-20210810211832312.c848f3ae.png)

测试Nignx的配置文件：

```text
docker exec -it some-nginx nginx -t
```

![image-20210810220403001](uniapp.assets/image-20210810220403001.ba89c359.png)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/07-安全域名相关.html#课程福利nginx配置)课程福利nginx配置

nginx的配置文件地址：[github(opens new window)](https://gist.github.com/toimc/24df7ea1adab57ee9560062ae9905c2a)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/07-安全域名相关.html#场景一-配置博客应用)场景一：配置博客应用

添加宿主机的docker目录映射：

```yaml
version: "3"
services:
  web:
    image: nginx:latest
    container_name: "some-nginx"
    restart: always
    volumes:
      - /home/nginx/nginx.conf:/etc/nginx/nginx.conf
      - /home/nginx/conf.d:/etc/nginx/conf.d
      - /home/keys:/home/keys
      # blog
      - /home/blog:/var/www
    ports:
      - "80:80"
      - "443:443"

# docker network create https
networks:
  default:
    external:
      name: https
```

nginx的配置文件 `conf.d/vhost.conf`：

```sh
# listen on HTTP2/SSL
server {
  listen 443 ssl http2;
  server_name www.wayearn.com;
  # ssl certs from letsencrypt
  # ssl on;
  # 这里要注意目录是docker里面的目录，所以建议大家把容器里面的目录与宿主机的目录映射一致
  ssl_certificate /home/keys/certs.pem;
  ssl_certificate_key /home/keys/key.pem;
  # dhparam.pem
  ssl_dhparam /home/keys/dhparam.pem;

  ssl_session_cache shared:SSL:50m;
  ssl_session_timeout 30m;
  ssl_session_tickets off;

  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  # ciphers chosen for forward secrecy and compatibility
  # http://blog.ivanristic.com/2013/08/configuring-apache-nginx-and-openssl-for-forward-secrecy.html
  ssl_ciphers 'ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES256-SHA:ECDHE-ECDSA-DES-CBC3-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:DES-CBC3-SHA:!DSS';

  ssl_prefer_server_ciphers on;

  add_header Strict-Transport-Security "max-age=31536000; includeSubdomains; preload";

  location / {
    root /var/www/;
    index index.html;
    proxy_set_header Host $host:$server_port;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
  }
}

# redirect HTTP and handle let's encrypt requests
server {
  listen 80;
  server_name www.wayearn.com;
  location / {
    return 301 https://$host$request_uri;
  }
}
```

检测ssl网站的评级 myssl.com：

![image-20210810220929142](uniapp.assets/image-20210810220929142.1537ad70.png)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/07-安全域名相关.html#场景二-配置小程序api接口)场景二：配置小程序API接口

步骤：

* api -> 线上 -> nginx进行代理 ；
* api部署到线上的服务器；
* api服务可以被nginx访问到；
* 域名 + https形式去访问 -> 提供api服务
  * 配置nginx的vhost配置 + 配置域名解析dns
  * wx.yourdomain.com -> 指向服务器的IP
  * 重启nginx容器让vhost的配置生效 -> 测试服务

小程序的docker-compose文件：

```yaml
version: "3"
services:
  web:
    build:
      context: .
      dockerfile: Dockerfile
      args:
        Version: ${Version}
    image: api_online:${tag}
    container_name: api_online
    restart: always
    # env_file: .env
    environment:
      - DB_USER=$DB_USER
      - DB_PASS=$DB_PASS
      - DB_HOST=$DB_HOST
      - DB_PORT=$DB_PORT
      - DB_NAME=$DB_NAME
      - REDIS_HOST=$REDIS_HOST
      - REDIS_PORT=$REDIS_PORT
      - REDIS_PASS=$REDIS_PASS
    ports:
      - "${PORT}:3000"
      - "${WS_PORT}:3001"
    volumes:
      - /home/imooc/online:/app/public

# 关键就是这里，配置API项目的网络，连接到https网络
# 然后，使用vhost配置，让Nginx进行代理
networks:
  default:
    external:
      name: https
```

可选方案：提供Mongo、redis环境的`docker-compose.yml`文件：

```yaml
version: "3"
services:
  web:
    build:
      context: .
      dockerfile: Dockerfile
      args:
        Version: 1.0
    image: api_online:1.0
    container_name: api_online
    restart: always
    # env_file: .env
    environment:
      - DB_USER=toimc
      - DB_PASS=long_random_pass_mongo
      - DB_HOST=mongo
      - DB_PORT=27017
      - DB_NAME=community
      - REDIS_HOST=redis
      - REDIS_PORT=6379
      - REDIS_PASS=long_random_pass_redis
    ports:
      - "10030:3000"
      - "10031:3001"
    volumes:
      - /home/imooc/online:/app/public

  mongo:
    image: mongo
    container_name: "mongodb"
    restart: always
    volumes:
      - /home/imooc/db:/data/db
      - /home/imooc/db/initdb.d:/docker-entrypoint-initdb.d/
      # .sh & .js
    ports:
      - "27017:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: example
      MONGO_INITDB_DATABASE: testdb
      MONGO_INITDB_USERNAME: toimc
      MONGO_INITDB_PASSWORD: long_random_pass_mongo

  redis:
    image: redis
    container_name: "redis"
    restart: always
    ports:
      - "6379:6379"
    command: redis-server --requirepass long_random_pass_redis

networks:
  default:
    external:
      name: https
```

MongoDB的初始化文件`init.d/mongo-init.sh`文件：

```sh
mongo -- "$MONGO_INITDB_DATABASE" <<EOF
var rootUser = '$MONGO_INITDB_ROOT_USERNAME';
var rootPassword = '$MONGO_INITDB_ROOT_PASSWORD';
var initDB = '$MONGO_INITDB_DATABASE'
var admin = db.getSiblingDB(initDB);
admin.auth(rootUser, rootPassword);

var user = '$MONGO_INITDB_USERNAME';
var passwd = '$MONGO_INITDB_PASSWORD';
db.createUser({ user: user, pwd: passwd, roles: ["dbOwner"] });
EOF
```

手动部署API服务：

* 使用docker命令进行docker镜像的构建

* 推送镜像或者使用手动方式导入镜像

  保存(Save)

  ```text
   # 保留原镜像的名称和标签
   docker save <IMAGE NAME>:<IMAGE TAG> > save.tar
  
   # 不保留原镜像的基本信息,加载load后需执行tag命令重命名none镜像
   docker save <IMAGE ID> > save.tar 
  ```

  示例:

  ```text
  docker save elasticsearch:7.1.1 > elasticsearch-7.1.1.tar
  # 或
  docker save b0cb1543380d > elasticsearch-7.1.1.tar
  ```

  加载(Load)

  ```text
  docker load < save.tar
  ```

  示列:

  ```text
  docker load < elasticsearch-7.1.1.tar
  ```

* 配置nginx文件`/home/nginx/conf.d/ws.conf`：

  ```sh
  upstream target-server {
    server api_online:3001 fail_timeout=0;
  }
  # listen on HTTP2/SSL
  server {
    listen 443 ssl http2;
    server_name ws.wayearn.com;
    # ssl certs from letsencrypt
    ssl_certificate /home/acme/fullchain.pem;
    ssl_certificate_key /home/acme/key.pem;
    # dhparam.pem
    ssl_dhparam /home/acme/dhparam.pem;
    ssl_session_timeout 5m;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers 'ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:ECDHE-RSA-DES-CBC3-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!MD5:!PSK:!RC4';
    ssl_prefer_server_ciphers on;
    location / {
      proxy_set_header X-Forwarded-Ssl on;
      proxy_redirect off;
      proxy_set_header Host $host:$server_port;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
      proxy_pass http://target-server;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection 'Upgrade';
    }
  }
  # redirect HTTP and handle let's encrypt requests
  server {
    listen 80;
    server_name ws.wayearn.com;
    # send everything else to HTTPS
    rewrite ^(.*)$ https://$host$1 permanent;
  }
  ```

* 添加dns解析

  ![image-20210810223431309](uniapp.assets/image-20210810223431309.368c5c94.png)

* 导入镜像，启动api服务：

  ![image-20210810223550333](uniapp.assets/image-20210810223550333.d0fedae0.png)

  使用`docker-compose up -d`启动服务：

  ![image-20210810223613863](uniapp.assets/image-20210810223613863.5fbb1359.png)

* 重启nginx服务：

  ```text
  docker restart some-nginx
  ```

  网络互访：

  ```text
  docker network inspect https
  ```

  ![image-20210810223801721](uniapp.assets/image-20210810223801721.cc7b1e2f.png)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/07-安全域名相关.html#排查问题)排查问题

* 常见的防火墙问题：

```sh
# 查看防火墙的运行状态
firewall-cmd --state

firewall-cmd --add-port=443/tcp --permanenet

firewall-cmd --reload
```

* 云服务商的放行策略：

华为云示例：

![image-20210810221145314](uniapp.assets/image-20210810221145314.e34347c6.png)

腾讯云：

![image-20210810221210594](uniapp.assets/image-20210810221210594.0b72c5d5.png)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/07-安全域名相关.html#小程序接口联调)小程序接口联调

* nginx启动之后，就可以访问`https://域名`了

* 导出测试数据库

  ```sh
  docker exec -it mongodb mongodump -u test -d testdb -o /tmp/
  ```

  使用docker cp命令来复制目录：

  ```sh
  docker cp <容器id>:/tmp/testdb /tmp/本地目录
  ```

* 导入数据库

  上传上面的本地目录的文件到线上的服务器。

  mongodb恢复数据库的命令：

  ```sh
  docker cp /home/docker/testdb <容器id>:/tmp/
  
  docker exec -it mongodb mongorestore -u test -d testdb /tmp/testdb
  ```

  ![image-20210810224430465](uniapp.assets/image-20210810224430465.e62eb13b.png)

请求测试：

![image-20210810224507919](uniapp.assets/image-20210810224507919.89fa22dc.png)

* 微信小程序配置安全域名

  开发 -> 开发设置 -> 安全域名

  ![image-20210810224610237](uniapp.assets/image-20210810224610237.4224a675.png)

  添加域名：

  ![image-20210810224636162](uniapp.assets/image-20210810224636162.9792e446.png)

  

  下面即可以在微信开发者工具中进行测试了，取消`不校验合法域名`：

  ![image-20210810224714543](uniapp.assets/image-20210810224714543.439e8328.png)



# [#](https://front-end.toimc.com/notes-page/project/community-miniapp/08-订阅消息.html#订阅消息)订阅消息

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/08-订阅消息.html#介绍)介绍

消息能力是小程序能力中的重要组成，我们为开发者提供了订阅消息能力，以便实现服务的闭环和更优的体验。

* 订阅消息推送位置：服务通知
* 订阅消息下发条件：用户自主订阅
* 订阅消息卡片跳转能力：点击查看详情可跳转至该小程序的页面

![intro](uniapp.assets/request-subscribe-message.3851318e.3851318e.jpg)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/08-订阅消息.html#消息类型)消息类型

**1. 一次性订阅消息**

一次性订阅消息用于解决用户使用小程序后，后续服务环节的通知问题。用户自主订阅后，开发者可**不限时间**地下发**一条对应的服务消息**；每条消息可单独订阅或退订。

**2. 长期订阅消息**

一次性订阅消息可满足小程序的大部分服务场景需求，但线下公共服务领域存在一次性订阅无法满足的场景，如航班延误，需根据航班实时动态来多次发送消息提醒。为便于服务，我们提供了长期性订阅消息，用户订阅一次后，开发者可长期下发多条消息。

目前长期性订阅消息仅向政务民生、医疗、交通、金融、教育等线下公共服务开放，后期将逐步支持到其他线下公共服务业务。

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/08-订阅消息.html#使用步骤)使用步骤

* 步骤一：获取模板 ID

  在微信公众平台手动配置获取模板 ID： 登录 [https://mp.weixin.qq.com (opens new window)](https://mp.weixin.qq.com/)获取模板，如果没有合适的模板，可以申请添加新模板，审核通过后可使用。

  ![intro](uniapp.assets/subscribe-message.b562750a.jpg)

* 步骤二：获取下发权限

  详见小程序端消息订阅接口 [wx.requestSubscribeMessage(opens new window)](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/subscribe-message/wx.requestSubscribeMessage.html)

* 步骤三：调用接口下发订阅消息

  详见服务端消息发送接口 [subscribeMessage.send(opens new window)](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/subscribe-message/subscribeMessage.send.html)

  > 注意事项：用户勾选 “总是保持以上选择，不再询问” 之后，下次订阅调用 wx.requestSubscribeMessage 不会弹窗，保持之前的选择，修改选择需要打开小程序设置进行修改。

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/08-订阅消息.html#封装订阅消息工具js)封装订阅消息工具js

作用：

* 发起订阅请求；
* 判断用户是否开启了通知；
* 提示未开启通知的用户，并跳转设置页面；

```js
export default (arr, cb, mute = false) => {
  uni.requestSubscribeMessage({
    tmplIds: arr.length > 3 ? arr.splice(0, 3) : arr,
    // 在调试工具中，无论订阅成功还是取消，都可以在complete取到状态
    // 调试工具中，无法直接测试关闭订阅的状态
    // 在真机上，可以获取用户拒绝订阅的状态
    // 而且在complete部分可以获取到success/fail的回调内容
    complete: (res) => {
      // 2.1 如果用户未订阅，并未拒绝，正常发起订阅
      // 2.2 如果用户拒绝了订阅，需要给用户一个轻提示 -> 手动打开订阅消息的
      // wx.openSetting
      if (arr.includes(item => res[item] === 'reject') || res.errCode === 20004) {
        uni.showModal({
          title: '您关闭了订阅通知',
          content: '需要打开设置进行手动设置吗？',
          success: function (res) {
            if (res.confirm) {
              uni.openSetting()
            } else if (res.cancel) {
              uni.showToast({
                icon: 'error',
                title: '您取消了订阅',
                duration: 2000
              })
            }
          }
        })
      } else if (!arr.some(item => res[item] === 'reject')) {
        !mute && uni.showToast({
          icon: 'none',
          title: '您已经订阅了该消息',
          duration: 1500
        })
      } else if (res.errCode === 10002 || res.errCode === 10003) {
        uni.showToast({
          title: '网络问题订阅失败，请重新订阅',
          duration: 1500
        })
      } else {
        // 其他的逻辑 https://developers.weixin.qq.com/miniprogram/dev/api/open-api/subscribe-message/wx.requestSubscribeMessage.html
      }
      cb && cb()
    }
  })
}
```

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/08-订阅消息.html#前端主动订阅)前端主动订阅

在App.vue中添加判断用户订阅逻辑：

```js
export default {
  onLaunch: function () {
    // console.warn('当前组件仅支持 uni_modules 目录结构 ，请升级 HBuilderX 到 3.1.0 版本以上！')
    // console.log('App Launch')
    uni.getSetting({
      withSubscriptions: true,
      success: async (res) => {
        const app = getApp()
        app.globalData.subscriptionsSetting = res.subscriptionsSetting
        const arr = [
          'S7zrpjN9Kq05-4ZG_nlTAYxnARMLWlSW09h54A2JCZo',
          'ANN2-LhDgrhdFjs7jHOLdTnaxWpQU1LqS3kDIMF9GDs',
          'FSQZganmBgaRRoNNlelQ1Qm2u4gx6pVSt69EJfkLbPA',
          'g9FFU43_deHRuez-2FcrASorTSITsJJPYx-GhzvHEIU'
        ]
        // 1. 获取用户已经订阅的消息
        const { itemSettings: keys, mainSwitch } = res.subscriptionsSetting
        // 相当于用户未打开订阅开关
        if (!mainSwitch) {
          return
        }
        // 用户开启订阅消息 -> 如果未设置任何消息
        if (!keys) {
          app.globalData.tmplIds = arr
        } else {
          // 用户开启了订阅消息 -> 已经有部分订阅 -> reject, accept
          const keysArr = Object.keys(keys)
          app.globalData.tmplIds = arr.filter((item) => keysArr.indexOf(item) === -1)
        }
      }
    })
  },
  onShow: function () {
    console.log('App Show')
  },
  onHide: function () {
    console.log('App Hide')
  }
}
```

在需要订阅的位置，加入订阅逻辑，比如在个人中心中，点击去登录时：

```js
import sub from '@/common/utils/subscribe'

// ...
const app = getApp()
const tmplIds = app.globalData.tmplIds || []
sub(tmplIds.splice(0, 3), () => {
  if (!this.isLogin) {
    uni.navigateTo({
      url: '/subcom-pkg/auth/auth'
    })
  }
}, true)
```

> 注意：订阅消息一次最多只可以订阅3条，所以需要加入一个splice方法截断未订阅的消息。
>
> 也可以自己在指定的业务部分，给`sub`方法，传递指定的模板ID。

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/08-订阅消息.html#订阅消息模板api)订阅消息模板API

应用场景：

* 后台控制哪些消息可以被订阅；
* 当模板消息的id发生了变化时，如果保证订阅的准确性；

前端请求模板API：

```js
// 获取模板id
const getSubIds = () => axios.get('/public/subids')
```

调整App.vue中的模板IDs的判断逻辑，处理：

```js
// 模板ID
// 改装成一个API接口 -> key:value -> value, key -> 业务场景
const { code, data } = await this.$u.api.getSubIds()
let arr
if (code === 200) {
  // {key: value, key1: value1} => [value, value1]
  arr = Object.entries(data).map(o => o[1])
} else {
  // 默认的前端模板数据
  arr = [
    'S7zrpjN9Kq05-4ZG_nlTAYxnARMLWlSW09h54A2JCZo',
    'ANN2-LhDgrhdFjs7jHOLdTnaxWpQU1LqS3kDIMF9GDs',
    'FSQZganmBgaRRoNNlelQ1Qm2u4gx6pVSt69EJfkLbPA',
    'g9FFU43_deHRuez-2FcrASorTSITsJJPYx-GhzvHEIU'
  ]
}
```

后端创建路由与接口：

路由：

```js
// 获取微信模板id
router.get('/subids', publicController.getSubIds)
```

接口：

```js
async getSubIds (ctx) {
  ctx.body = {
    code: 200,
    data: config.subIds
  }
}
```

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/08-订阅消息.html#维护accesstoken)维护accessToken

后端发送订阅消息需要accessToken：

```js
POST https://api.weixin.qq.com/cgi-bin/message/subscribe/send?access_token=ACCESS_TOKEN
```

见官方文档：[链接(opens new window)](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/subscribe-message/subscribeMessage.send.html)

由于acessToken的有效时间是2小时，而且每天微信请求的限制为2000次（[链接 (opens new window)](https://developers.weixin.qq.com/doc/offiaccount/Message_Management/API_Call_Limits.html)），如下图：

![image-20210812102933432](uniapp.assets/image-20210812102933432.90192975.png)

所以，需要自己定时维护accessToken：

* 创建`wxGetAccessToken`方法：

  ```js
  import log4js from '@/config/Log4j'
  const logger = log4js.getLogger('error')
  import { getValue, setValue } from '@/config/RedisConfig'
  
  // flag 强制刷新，默认false - 不强制刷新
  export const wxGetAccessToken = async (flag = false) => {
    // https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid=APPID&secret=APPSECRET
    // 1.判断redis中是否有accessToken
    // 2.有 & flag -> 则直接返回
    // 3.没有 -> 请求新的token
    let accessToken = await getValue('accessToken')
    if (!accessToken || flag) {
      try {
        const result = await instance.get(`https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid=${config.AppID}&secret=${config.AppSecret}`)
        // console.log('🚀 ~ file: WxUtils.js ~ line 60 ~ wxGetAccessToken ~ result', result)
        if (result.status === 200) {
          await setValue('accessToken', result.data.access_token, result.data.expires_in)
          accessToken = result.data.access_token
          if (result.data.errcode && result.data.errmsg) {
            logger.error(`wxGetAccessToken error${result.data.errcode} - ${result.data.errmsg}`)
          }
        }
      } catch (error) {
        logger.error(`wxGetAccessToken error: ${error.message}`)
      }
    }
    return accessToken
  }
  ```

* 安装依赖`npm i cron`

* 配置定时任务：

  ```js
  // 每隔7200秒执行一次 刷新accessToken
  import { CronJob } from 'cron'
  import { wxGetAccessToken } from './WxUtils'
  
  // Seconds: 0-59
  // Minutes: 0-59
  // Hours: 0-23
  // Day of Month: 1-31
  // Months: 0-11 (Jan-Dec)
  // Day of Week: 0-6 (Sun-Sat)
  
  const job = new CronJob('* * */1 * * *', () => {
    wxGetAccessToken(true)
  })
  
  job.start()
  ```

* 在入口文件处理，添加该文件：

  ```js
  import './common/Cron'
  ```

**常见问题，如果不小心accessToken请求超限怎么办：**

![img](uniapp.assets/0.a36c9ed1.png)

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/08-订阅消息.html#后台发送订阅消息)后台发送订阅消息

发送订阅消息：[官方文档(opens new window)](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/subscribe-message/subscribeMessage.send.html)

调用方式：

* [HTTPS 调用(opens new window)](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/subscribe-message/subscribeMessage.send.html#method-http)
* [云调用(opens new window)](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/subscribe-message/subscribeMessage.send.html#method-cloud)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/08-订阅消息.html#https调用接口说明)HTTPS调用接口说明

```text
POST https://api.weixin.qq.com/cgi-bin/message/subscribe/send?access_token=ACCESS_TOKEN
```

**请求参数：**

| 属性                                  | 类型   | 默认值 | 必填 | 说明                                                         |
| :------------------------------------ | :----- | :----- | :--- | :----------------------------------------------------------- |
| access_token / cloudbase_access_token | string |        | 是   | [接口调用凭证(opens new window)](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/access-token/auth.getAccessToken.html) |
| touser                                | string |        | 是   | 接收者（用户）的 openid                                      |
| template_id                           | string |        | 是   | 所需下发的订阅模板id                                         |
| page                                  | string |        | 否   | 点击模板卡片后的跳转页面，仅限本小程序内的页面。支持带参数,（示例index?foo=bar）。该字段不填则模板无跳转。 |
| data                                  | Object |        | 是   | 模板内容，格式形如 { "key1": { "value": any }, "key2": { "value": any } } |
| miniprogram_state                     | string |        | 否   | 跳转小程序类型：developer为开发版；trial为体验版；formal为正式版；默认为正式版 |
| lang                                  | string |        | 否   | 进入小程序查看”的语言类型，支持zh_CN(简体中文)、en_US(英文)、zh_HK(繁体中文)、zh_TW(繁体中文)，默认为zh_CN |

**说明一下关于`data`：**

例如，模板的内容为

```text
姓名: {{name01.DATA}}
金额: {{amount01.DATA}}
行程: {{thing01.DATA}}
日期: {{date01.DATA}}
```

则对应的json为

```json
{
  "touser": "OPENID",
  "template_id": "TEMPLATE_ID",
  "page": "index",
  "data": {
      "name01": {
          "value": "某某"
      },
      "amount01": {
          "value": "￥100"
      },
      "thing01": {
          "value": "广州至北京"
      } ,
      "date01": {
          "value": "2018-01-01"
      }
  }
}
```

这里的data有内容限制（举例如下表），更多查看官方文档详情：

| 参数类别              | 参数说明 | 参数值限制                                                   | 说明                                                         |
| :-------------------- | :------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| thing.DATA            | 事物     | 20个以内字符                                                 | 可汉字、数字、字母或符号组合                                 |
| number.DATA           | 数字     | 32位以内数字                                                 | 只能数字，可带小数                                           |
| letter.DATA           | 字母     | 32位以内字母                                                 | 只能字母                                                     |
| symbol.DATA           | 符号     | 5位以内符号                                                  | 只能符号                                                     |
| character_string.DATA | 字符串   | 32位以内数字、字母或符号                                     | 可数字、字母或符号组合                                       |
| time.DATA             | 时间     | 24小时制时间格式（支持+年月日），支持填时间段，两个时间点之间用“~”符号连接 | 例如：15:01，或：2019年10月1日 15:01                         |
| date.DATA             | 日期     | 年月日格式（支持+24小时制时间），支持填时间段，两个时间点之间用“~”符号连接 | 例如：2019年10月1日，或：2019年10月1日 15:01                 |
| amount.DATA           | 金额     | 1个币种符号+10位以内纯数字，可带小数，结尾可带“元”           | 可带小数                                                     |
| phone_number.DATA     | 电话     | 17位以内，数字、符号                                         | 电话号码，例：+86-0766-66888866                              |
| car_number.DATA       | 车牌     | 8位以内，第一位与最后一位可为汉字，其余为字母或数字          | 车牌号码：粤A8Z888挂                                         |
| name.DATA             | 姓名     | 10个以内纯汉字或20个以内纯字母或符号                         | 中文名10个汉字内；纯英文名20个字母内；中文和字母混合按中文名算，10个字内 |
| phrase.DATA           | 汉字     | 5个以内汉字                                                  | 5个以内纯汉字，例如：配送中                                  |

返回的 JSON 数据包

| 属性    | 类型   | 说明     |
| :------ | :----- | :------- |
| errcode | number | 错误码   |
| errmsg  | string | 错误信息 |

**errcode 的合法值**

| 值    | 说明                                                         |
| :---- | :----------------------------------------------------------- |
| 40003 | touser字段openid为空或者不正确                               |
| 40037 | 订阅模板id为空不正确                                         |
| 43101 | 用户拒绝接受消息，如果用户之前曾经订阅过，则表示用户取消了订阅关系 |
| 47003 | 模板参数不准确，可能为空或者不满足规则，errmsg会提示具体是哪个字段出错 |
| 41030 | page路径不正确，需要保证在现网版本小程序中存在，与app.json保持一致 |

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/08-订阅消息.html#创建发送消息工具js)创建发送消息工具js

```js
export const wxSendMessage = async (options) => {
  // POST https://api.weixin.qq.com/cgi-bin/message/subscribe/send?access_token=ACCESS_TOKEN
  let accessToken = await wxGetAccessToken()
  try {
    const { data } = await instance.post(`https://api.weixin.qq.com/cgi-bin/message/subscribe/send?access_token=${accessToken}`, options)
    return data
  } catch (error) {
    logger.error(`wxSendMessage error: ${error.message}`)
  }
}
```

举例：

在用户登录之后，发送订阅消息通知：

```js
// 推送消息
// 字段限制：https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/subscribe-message/subscribeMessage.send.html
const notify = await wxSendMessage({
  touser: tmpUser.openid,
  template_id: 'FSQZganmBgaRRoNNlelQ1Qm2u4gx6pVSt69EJfkLbPA',
  data: {
    phrase1: {
      value: '登录安全'
    },
    date2: {
      value: moment().format('YYYY年MM月DD HH:mm')
    },
    thing4: {
      value: '通过微信授权登录成功，请注意信息安全'
    }
  },
  miniprogram_state: process.env.NODE_ENV === 'development' ? 'developer' : 'formal'
})
```

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/08-订阅消息.html#前后端联调)前后端联调

联调需要注意的点：

* 设置`miniprogram_state`属性；
* 使用真机进行调试 -> 配置局域网的访问的IP -> 让电脑与手机处于同一个网段；
* 检查发送订阅消息的数据格式、模板id、用户openid数据是否正确；
* 使用单步调试：发送订阅消息的接口，查看返回的errcode如果是0，说明发送成功；如果失败，则根据errcode来调整程序代码；

![image-20210812111502555](uniapp.assets/image-20210812111502555.6ac23418.png)



# [#](https://front-end.toimc.com/notes-page/project/community-miniapp/09-内容安全.html#内容安全)内容安全

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/09-内容安全.html#微信安全检测)微信安全检测

微信官方提供了可疑用户与危险接口的检查：

![image-20210815144637652](uniapp.assets/image-20210815144637652.605489b5.png)

接口安全扫描：

![image-20210815144649621](uniapp.assets/image-20210815144649621.30fc62ba.png)

这两个功能在后台即可操作，平坦可以交由运营小姐姐来进行操作一下。

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/09-内容安全.html#第三方内容安全推荐)第三方内容安全推荐

* 各家云服务商：[云盾 (opens new window)](https://www.aliyun.com/product/lvwang)(阿里)，[易盾 (opens new window)](https://dun.163.com/)(网易)，[天御 (opens new window)](https://cloud.tencent.com/product/tms)(腾讯)
* 第三方：[图普 (opens new window)](https://www.tuputech.com/text_moderation)、国信网安、[梵为科技(opens new window)](https://www.vanwei.com.cn/main/tq)

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/09-内容安全.html#文本内容安全)文本内容安全

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/09-内容安全.html#整体思路)整体思路

* 下载网络上的一些敏感词的数据库，使用正则进行匹配过滤一次，减少成本；
* 使用官方的限额的api对文本进行查验；
* 使用第三方的api对内容进行查验；

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/09-内容安全.html#官方介绍)官方介绍

检查一段文本是否含有违法违规内容，下面介绍的接口：[官方链接(opens new window)](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/sec-check/security.msgSecCheck.html)

1.0 版本接口文档[【点击查看】(opens new window)](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/sec-check/security.msgSecCheck-v1.html)

应用场景举例：

1. 用户个人资料违规文字检测；
2. 媒体新闻类用户发表文章，评论内容检测；
3. 游戏类用户编辑上传的素材(如答题类小游戏用户上传的问题及答案)检测等。 *频率限制：单个 appId 调用上限为 4000 次/分钟，2,000,000 次/天**

调用方式：

* [HTTPS 调用(opens new window)](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/sec-check/security.msgSecCheck.html#method-http)
* [云调用(opens new window)](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/sec-check/security.msgSecCheck.html#method-cloud)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/09-内容安全.html#工具js封装)工具js封装

请求地址

```text
POST https://api.weixin.qq.com/wxa/msg_sec_check?access_token=ACCESS_TOKEN
```

请求参数

| 属性                                  | 类型   | 默认值 | 必填 | 说明                                                         |
| :------------------------------------ | :----- | :----- | :--- | :----------------------------------------------------------- |
| access_token / cloudbase_access_token | string |        | 是   | [接口调用凭证(opens new window)](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/access-token/auth.getAccessToken.html) |
| version                               | string |        | 是   | 接口版本号，2.0版本为固定值2                                 |
| openid                                | string |        | 是   | 用户的openid（用户需在近两小时访问过小程序）                 |
| scene                                 | number |        | 是   | 场景枚举值（1 资料；2 评论；3 论坛；4 社交日志）             |
| content                               | string |        | 是   | 需检测的文本内容，文本字数的上限为2500字                     |
| nickname                              | string |        | 否   | 用户昵称                                                     |
| title                                 | string |        | 否   | 文本标题                                                     |
| signature                             | string |        | 否   | 个性签名，该参数仅在资料类场景有效(scene=1)                  |

返回的 JSON 数据包

| 属性     | 类型   | 说明                       |
| :------- | :----- | :------------------------- |
| errcode  | number | 错误码                     |
| errmsg   | string | 错误信息                   |
| trace_id | string | 唯一请求标识，标记单次请求 |
| result   | object | 综合结果                   |
| detail   | array  | 详细检测结果               |

逻辑：

* 用户只用传文本内容，其他的openid等信息可以不传，设置一个默认值，方便其他的平台使用；
* 使用正则匹配掉一些无用的内容；
* 长度判断，分2500词进行多次判断；
* 判断结果非pass的全部进入risky部分；

```js
// 文本内容安全
// content - 内容
// title - 标题 可选
// signature - 签名 也是可选
export const wxMsgCheck = async (content, {
  user: {
    openid,
    name: nickname,
    remark: signature
  },
  scene,
  title
} = {
  user: {},
  scene: 3,
  title: ''
}) => {
  // POST https://api.weixin.qq.com/wxa/msg_sec_check?access_token=ACCESS_TOKEN
  let accessToken = await wxGetAccessToken()
  let res
  try {
    // 1.过滤掉一些如Html，自定义的标签内容
    content = content.replace(/<[^>]+>/g, '').replace(/\sface\[\S{1,}]/g, '').replace(/img\[\S+\]/g, '').replace(/\sa\(\S+\]/g, '').replace(/\[\/?quote\]/g, '').replace(/\[\/?pre\]/g, '').replace(/\[\/?hr\]/g, '').replace(/[\r\n|\n|\s]/g, '')
    // 2.如果content内容超过了2500词，需要进行分段处理
    if (content.length > 2500) {
      // 分段 —> arr -> method1: for , method2: reg
      let arr = content.match(/[\s\S]{1,2500}/g) || []
      // 多次请求接口
      let mulResult = []
      for (let i = 0; i < arr.length; i++) {
        // 获取所有接口的返回结果 -> 结果判断 -> 返回
        res = await instance.post(`https://api.weixin.qq.com/wxa/msg_sec_check?access_token=${accessToken}`, {
          version: 2,
          openid: openid || 'ooTjn5YPpogMWLtEQ_PxyUJkIp2I',
          scene,
          content: arr[i],
          nickname: nickname,
          title,
          signature: scene === 1 ? signature : null
        })
        mulResult.push(res)
      }
      // 判断mulResult
      console.log(mulResult)
      const arrTemp = mulResult.filter(item => {
        const { status, data: { errcode, result } } = item
        return status !== 200 || errcode !== 0 || (result && result.suggest !== 'pass')
      })
      return !(arrTemp.length > 0)
    } else {
      res = await instance.post(`https://api.weixin.qq.com/wxa/msg_sec_check?access_token=${accessToken}`, {
        version: 2,
        openid: openid || 'ooTjn5YPpogMWLtEQ_PxyUJkIp2I',
        scene,
        content,
        nickname: nickname,
        title,
        signature: scene === 1 ? signature : null
      })
      const { status, data: { errcode, result } } = res
      return status === 200 && errcode === 0 && result && result.suggest === 'pass'
    }
  } catch (error) {
    logger.error(`wxMsgCheck error: ${error.message}`)
  }
}
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/09-内容安全.html#自定义安全敏感词汇)自定义安全敏感词汇

在小程序管理后台，开发管理 -> 安全中心 -> 内容风控：

![image-20210815144815314](uniapp.assets/image-20210815144815314.0bc0ee08.png)

> 在这里添加的分值越高，越可能被屏蔽。

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/09-内容安全.html#测试接口工具js)测试接口工具js

* 加入accessToken失效判断

  ```js
  instance.interceptors.response.use(async (res) => {
    const { data } = res
    if (data.errcode === 40001) {
      // 重新获取新的accessToken
      const accessToken = await wxGetAccessToken(true)
      const { url } = res.config
      // 重新发起请求 -> res
      if (url.indexOf('access_token') !== -1) {
        const arr = url.split('?') // ?key=value&key1=value1... -> ['域名', 'key=value&key1=value1...']
        const params = qs.parse(arr[1])
        const newParams = {
          ...params,
          access_token: accessToken
        }
        const newUrl = arr[0] + '?' + qs.stringify(newParams)
        const config = { ...res.config, url: newUrl }
        const result = await axios(config)
        return result
      }
    }
    return res
  })
  ```

* 有风险的词汇：

  ![image-20210815151921140](uniapp.assets/image-20210815151921140.02b4ac8e.png)

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/09-内容安全.html#图片内容安全)图片内容安全

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/09-内容安全.html#整体思路-2)整体思路

* 图片上传之后，创建tmp目录（使用[make-dir (opens new window)](https://www.npmjs.com/package/make-dir)），获取图片的基本信息；
* 判断图片的尺寸，如果超过了微信接口的分辨率要求750x1336，那需要使用[sharp (opens new window)](https://www.npmjs.com/package/sharp)来进行压缩;
* nodejs侧对接微信接口需要使用formData数据类型，所以还需要安装[form-data(opens new window)](https://www.npmjs.com/package/form-data)
* 最后还需要删除临时文件，使用[del (opens new window)](https://www.npmjs.com/package/del)库，删除之前使用[fs.access (opens new window)](https://nodejs.org/api/fs.html#fs_fs_accesssync_path_mode)进行判断

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/09-内容安全.html#官方介绍-2)官方介绍

请求地址

```text
POST https://api.weixin.qq.com/wxa/img_sec_check?access_token=ACCESS_TOKEN
```

请求参数

| 属性                                  | 类型     | 默认值 | 必填 | 说明                                                         |
| :------------------------------------ | :------- | :----- | :--- | :----------------------------------------------------------- |
| access_token / cloudbase_access_token | string   |        | 是   | [接口调用凭证(opens new window)](https://developers.weixin.qq.com/miniprogram/dev/api-backend/open-api/access-token/auth.getAccessToken.html) |
| media                                 | FormData |        | 是   | 要检测的图片文件，格式支持PNG、JPEG、JPG、GIF，图片尺寸不超过 750px x 1334px |

返回的 JSON 数据包

| 属性    | 类型   | 说明     |
| :------ | :----- | :------- |
| errcode | number | 错误码   |
| errmsg  | string | 错误信息 |

**errcode 的合法值**

| 值    | 说明             | 最低版本 |
| :---- | :--------------- | :------- |
| 0     | 内容正常         |          |
| 87014 | 内容可能潜在风险 |          |

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/09-内容安全.html#工具js封装-2)工具js封装

安装依赖：

```sh
npm i make-dir form-date sharp del
```

其中[sharp (opens new window)](https://sharp.pixelplumbing.com/)需要配置加速：

```sh
npm config set sharp_binary_host "https://npm.taobao.org/mirrors/sharp"
npm config set sharp_libvips_binary_host "https://npm.taobao.org/mirrors/sharp-libvips"
```

工具js：

```js
// 获取头部属性
export const getHeaders = (form) => {
  return new Promise((resolve, reject) => {
    form.getLength((err, length) => {
      if (err) {
        reject(err)
      }
      const headers = Object.assign({
        'Content-Length': length
      }, form.getHeaders())
      resolve(headers)
    })
  })
}

// 删除文件
export const checkAndDelFile = async (path) => {
  try {
    accessSync(path, constants.R_OK | constants.W_OK)
    await del(path)
  } catch (err) {
    // console.error('no access!')
  }
}

// 图片内容安全
export const wxImgCheck = async (file) => {
  // POST https://api.weixin.qq.com/wxa/img_sec_check?access_token=ACCESS_TOKEN
  const accessToken = await wxGetAccessToken()
  // 1.保证图片 -> 判断分辨率 -> sharp 750 * 1334
  let newPath = file.path
  const tmpPath = path.resolve('./tmp')
  try {
    const img = sharp(newPath)
    const meta = await img.metadata()
    if (meta.width > 750 || meta.height > 1334) {
      // 判断临时路径是否存在，并创建
      await mkdir(tmpPath)
      // uuid -> 指定临时的文件名称
      newPath = path.join(tmpPath, uuidv4() + path.extname(newPath) || '.jpg')
      await img.resize(750, 1334, {
        fit: 'inside'
      }).toFile(newPath)
    }
    const stream = fs.createReadStream(newPath)
    // 2.FormData类型的数据准备
    const form = new FormData()
    form.append('media', stream)
    const headers = await getHeaders(form)
    // 3.请求接口 -> 返回结果
    const result = await instance.post(`https://api.weixin.qq.com/wxa/img_sec_check?access_token=${accessToken}`, form, { headers })
    // 校验成功 -> 删除tmp数据 -> 判断路径中的文件是否存在
    console.log('🚀 ~ file: WxUtils.js ~ line 232 ~ wxImgCheck ~ result', result)
    await checkAndDelFile(newPath)
    return result.status === 200 && result.data && result.data.errcode === 0
    // if (result.status === 200 && result.data && result.data.errcode === 0) {
    //   // errcode 0 - 内容正常，否则 - 异常
    //   return true
    // } else {
    //   return false
    // }
  } catch (error) {
    await checkAndDelFile(newPath)
    logger.error(`wxImgCheck error: ${error.message}`)
  }
}
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/09-内容安全.html#测试接口工具js-2)测试接口工具js

注意：

* 自行找一下网上可疑的图片进行测试；
* errcode为0说明校验通过，反之不通过；
* 通过之后，删除图片临时目录中的文件；
* 一般上传图片的接口需要对接云上的存储，所以采用了本地缓存的方式校验图片；

![image-20210815154746020](uniapp.assets/image-20210815154746020.dd53a2fe.png)



# [#](https://front-end.toimc.com/notes-page/project/community-miniapp/10-发贴&评论功能.html#发贴评论功能)发贴评论功能

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/10-发贴&评论功能.html#创建分包)创建分包

在uniapp中添加分包，减少主包体积，提升页面加载的性能：

* 新增页面，`/subcom-pkg/post/post`加入基本的vue页面的结构

* 配置`pages.json`

`pages.json`中配置分包：

```json
"subPackages": [
  {
    "root": "subcom-pkg",
    "pages": [
      ...
      {
        "path": "post/post",
        "style": {
          "navigationBarTitleText": "发贴"
        }
      }
    ]
  }
]
```

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/10-发贴&评论功能.html#表单校验)表单校验

使用uview中的[form (opens new window)](https://www.uviewui.com/components/form.html)表单组件：

```vue
<template>
  <view class="container">
    <u-form :model="form" ref="uForm" label-width="0">
      <u-form-item prop="title">
        <u-input v-model="form.title" placeholder="请输入帖子标题" clearable></u-input>
      </u-form-item>
      <u-form-item prop="content">
        <u-input v-model="form.content" placeholder="请输入帖子内容" type="textarea"></u-input>
      </u-form-item>

      <view class="upload-img">
        <view class="prev">设置封面图片:</view>
        <!-- 上传图片 -->
      </view>
      <u-form-item :label-position="labelPosition" label="发贴类型" label-width="140" prop="catalog">
        <u-input class="right-text" type="select" :select-open="show1" v-model="form.catalog" placeholder="请选择发贴类型" @click="show1=true"></u-input>
        <u-select v-model="show1" :list="list" @confirm="confirmType"></u-select>
      </u-form-item>
      <!-- </view> -->
      <u-form-item label="奖励积分" label-width="140" prop="fav">
        <u-input class="right-text" type="select" :select-open="show" v-model="form.fav" placeholder="请选择奖励积分" @click="show=true"></u-input>
        <u-select v-model="show" :list="tempFavs" @confirm="confirmFav"></u-select>
      </u-form-item>
    </u-form>
    <view class="btn">
      <u-button size="default" type="primary" hover-class="none">发布</u-button>
    </view>
  </view>
</template>
```

加入表单校验：

* 配置`u-form`中的`model`属性，`ref`用于校验整个表单；
* 配置`u-form-item`中的`prop`属性，配置对应的rules规则
* 在script的`onReady`回调中配置`this.$refs.uForm.setRules(this.rules)`

```js
<script>
export default {
  components: {},
  data: () => ({
    show: false,
    show1: false,
    form: {
      title: '',
      content: '',
      catalog: '',
      fav: '',
      snapshot: ''
    },
    rules: {
      title: [
        {
          required: true,
          message: '请输入标题',
          // 可以单个或者同时写两个触发验证方式
          trigger: ['blur']
        }
      ],
      content: [
        {
          required: true,
          message: '请输入文章内容',
          trigger: 'blur'
        }
      ],
      catalog: [
        {
          required: true,
          message: '请选择发贴类型',
          // 触发器可以同时用blur和change
          trigger: ['change', 'blur']
        }
      ],
      fav: [
        {
          required: true,
          message: '请选择积分',
          trigger: ['change', 'blur']
        }
      ]
    },
    list: [
      {
        value: '',
        label: '请选择'
      },
      {
        value: 'ask',
        label: '提问'
      },
      {
        value: 'share',
        label: '分享'
      },
      {
        value: 'discuss',
        label: '讨论'
      },
      {
        value: 'advise',
        label: '建议'
      }
    ],
    listIndex: 0,
    tempFavs: [
      {
        label: '请选择',
        value: ''
      },
      {
        label: '20',
        value: 20
      },
      {
        label: '30',
        value: 30
      },
      {
        label: '50',
        value: 50
      },
      {
        label: '100',
        value: 100
      }
    ],
    favIndex: 0,
    fileList: [],
    disabledButton: true
  }),
  methods: {
    confirmType (e) {
      const index = this.list.findIndex(item => item.value === e[0].value)
      this.listIndex = index
      this.form.catalog = e[0].label
    },
    confirmFav (e) {
      const index = this.tempFavs.findIndex(item => item.value === e[0].value)
      this.favIndex = index
      this.form.fav = e[0].label
    },
    addPost () {
      this.$refs.uForm.validate(async valid => {
        if (valid) {
          // to do
        } else {
          console.log('验证失败')
        }
      })
    }
  },
  onReady () {
    this.$refs.uForm.setRules(this.rules)
  }
}
</script>
```

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/10-发贴&评论功能.html#图片上传)图片上传

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/10-发贴&评论功能.html#上传接口)上传接口

上传formdata类型的数据，需要使用uni.uploadFile API，封装成一个promise谅，在complete中加入callback，方便后续的处理：

```js
// 图片上传接口
const uploadImg = (params, callback) => {
  return new Promise((resolve, reject) => {
    uni.uploadFile({
      url: baseUrl + '/content/upload', // 仅为示例，非真实的接口地址
      filePath: params,
      name: 'file',
      header: {
        'Content-Type': 'multipart/form-data',
        authorization: 'Bearer ' + store.state.token
      },
      formData: {
        // 'user': 'test'
      },
      success: (uploadFileRes) => {
        resolve(uploadFileRes)
      },
      fail: (err) => {
        reject(err)
      },
      complete: (res) => {
        callback && callback(res)
      }
    })
  })
}

// 发贴接口
const addPost = (data) => axios.post('/content/wxAdd', data)
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/10-发贴&评论功能.html#页面结构)页面结构

可以使用[u-upload (opens new window)](https://www.uviewui.com/components/upload.html)组件来轻松实现图片上传：

```vue
<template>
  <view class="container">
    <u-form :model="form" ref="uForm" label-width="0">
      <u-form-item prop="title">
        <u-input v-model="form.title" placeholder="请输入帖子标题" clearable></u-input>
      </u-form-item>
      <u-form-item prop="content">
        <u-input v-model="form.content" placeholder="请输入帖子内容" type="textarea"></u-input>
      </u-form-item>

      <view class="upload-img">
        <view class="prev">设置封面图片:</view>
        <u-upload ref="uUpload" :file-list="fileList" action="#" :auto-upload="false" @on-list-change="uploadImg" multiple :max-size="5 * 1024 * 1024" max-count="1" />
      </view>
      <u-form-item :label-position="labelPosition" label="发贴类型" label-width="140" prop="catalog">
        <u-input class="right-text" type="select" :select-open="show1" v-model="form.catalog" placeholder="请选择发贴类型" @click="show1=true"></u-input>
        <u-select v-model="show1" :list="list" @confirm="confirmType"></u-select>
      </u-form-item>
      <!-- </view> -->
      <u-form-item label="奖励积分" label-width="140" prop="fav">
        <u-input class="right-text" type="select" :select-open="show" v-model="form.fav" placeholder="请选择奖励积分" @click="show=true"></u-input>
        <u-select v-model="show" :list="tempFavs" @confirm="confirmFav"></u-select>
      </u-form-item>
    </u-form>
    <view class="btn">
      <u-button size="default" type="primary" @click="addPost" hover-class="none">发布</u-button>
    </view>
  </view>
</template>

<script>
import { authNav } from '@/common/checkAuth'
export default {
  data: () => ({
    // ...
    fileList: [],
  }),
  methods: {
    // ....
    async uploadImg (lists, name) {
      if (lists.length > 0) {
        const res = await this.$u.api.uploadImg(lists[0].url)
        if (res.statusCode === 401) {
          await authNav('登录已失效，图片上传失败，请登录后重传！')
          this.$refs.uUpload.clear()
        }
        if (res.statusCode === 200) {
          const { data } = res
          const { code, msg, data: url } = JSON.parse(data)
          if (code === 200) {
            this.form.snapshot = url
          }
          uni.showToast({
            icon: 'none',
            title: msg,
            duration: 2000
          })
        }
      }
    },
    // ...
  },
}
</script>
```

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/10-发贴&评论功能.html#完成效果)完成效果

```vue
<template>
  <view class="container">
    <u-form :model="form" ref="uForm" label-width="0">
      <u-form-item prop="title">
        <u-input v-model="form.title" placeholder="请输入帖子标题" clearable></u-input>
      </u-form-item>
      <u-form-item prop="content">
        <u-input v-model="form.content" placeholder="请输入帖子内容" type="textarea"></u-input>
      </u-form-item>

      <view class="upload-img">
        <view class="prev">设置封面图片:</view>
        <u-upload ref="uUpload" :file-list="fileList" action="#" :auto-upload="false" @on-list-change="uploadImg" multiple :max-size="5 * 1024 * 1024" max-count="1" />
      </view>
      <u-form-item :label-position="labelPosition" label="发贴类型" label-width="140" prop="catalog">
        <u-input class="right-text" type="select" :select-open="show1" v-model="form.catalog" placeholder="请选择发贴类型" @click="show1=true"></u-input>
        <u-select v-model="show1" :list="list" @confirm="confirmType"></u-select>
      </u-form-item>
      <!-- </view> -->
      <u-form-item label="奖励积分" label-width="140" prop="fav">
        <u-input class="right-text" type="select" :select-open="show" v-model="form.fav" placeholder="请选择奖励积分" @click="show=true"></u-input>
        <u-select v-model="show" :list="tempFavs" @confirm="confirmFav"></u-select>
      </u-form-item>
    </u-form>
    <view class="btn">
      <u-button size="default" type="primary" @click="addPost" hover-class="none">发布</u-button>
    </view>
  </view>
</template>

<script>
import { mapMutations } from 'vuex'
import { authNav } from '@/common/checkAuth'
export default {
  components: {},
  data: () => ({
    show: false,
    show1: false,
    form: {
      title: '',
      content: '',
      catalog: '',
      fav: '',
      snapshot: ''
    },
    rules: {
      title: [
        {
          required: true,
          message: '请输入标题',
          // 可以单个或者同时写两个触发验证方式
          trigger: ['blur']
        }
      ],
      content: [
        {
          required: true,
          message: '请输入文章内容',
          trigger: 'blur'
        }
      ],
      catalog: [
        {
          required: true,
          message: '请选择发贴类型',
          // 触发器可以同时用blur和change
          trigger: ['change', 'blur']
        }
      ],
      fav: [
        {
          required: true,
          message: '请选择积分',
          trigger: ['change', 'blur']
        }
      ]
    },
    list: [
      {
        value: '',
        label: '请选择'
      },
      {
        value: 'ask',
        label: '提问'
      },
      {
        value: 'share',
        label: '分享'
      },
      {
        value: 'discuss',
        label: '讨论'
      },
      {
        value: 'advise',
        label: '建议'
      }
    ],
    listIndex: 0,
    tempFavs: [
      {
        label: '请选择',
        value: ''
      },
      {
        label: '20',
        value: 20
      },
      {
        label: '30',
        value: 30
      },
      {
        label: '50',
        value: 50
      },
      {
        label: '100',
        value: 100
      }
    ],
    favIndex: 0,
    fileList: [],
    disabledButton: true
  }),
  computed: {
  },
  methods: {
    ...mapMutations(['setPage']),
    confirmType (e) {
      const index = this.list.findIndex(item => item.value === e[0].value)
      this.listIndex = index
      this.form.catalog = e[0].label
    },
    confirmFav (e) {
      const index = this.tempFavs.findIndex(item => item.value === e[0].value)
      this.favIndex = index
      this.form.fav = e[0].label
    },
    async uploadImg (lists, name) {
      if (lists.length > 0) {
        const res = await this.$u.api.uploadImg(lists[0].url)
        if (res.statusCode === 401) {
          await authNav('登录已失效，图片上传失败，请登录后重传！')
          this.$refs.uUpload.clear()
        }
        if (res.statusCode === 200) {
          const { data } = res
          const { code, msg, data: url } = JSON.parse(data)
          if (code === 200) {
            this.form.snapshot = url
          }
          uni.showToast({
            icon: 'none',
            title: msg,
            duration: 2000
          })
        }
      }
    },
    addPost () {
      this.$refs.uForm.validate(async valid => {
        if (valid) {
          const data = {
            ...this.form,
            catalog: this.list[this.listIndex].value,
            fav: this.tempFavs[this.favIndex].value
          }
          const { code, msg, data: res } = await this.$u.api.addPost(data)
          // console.log('🚀 ~ file: post.vue ~ line 157 ~ addPost ~ res', res)
          if (code === 200 && res._id) {
            uni.showToast({
              icon: 'none',
              title: msg,
              duration: 2000
            })
            uni.navigateBack()
          } else {
            // 内容审核提示
            if (code === 500 && /内容安全/.test(msg)) {
              uni.showModal({
                title: '注意文明用语',
                content: '发布内容没有通过内容审核，请检查后重新提效',
                showCancel: false,
                success: function (res) {
                  console.log(res)
                }
              })
              return
            }
            uni.showToast({
              icon: 'none',
              title: msg,
              duration: 2000
            })
          }
        } else {
          console.log('验证失败')
        }
      })
    }
  },
  onReady () {
    this.$refs.uForm.setRules(this.rules)
  }
}
</script>

<style lang="scss" scoped>
.container {
  padding: 32rpx;
}

::v-deep .edit-post {
  position: relative;
  textarea {
    max-height: 400rpx;
  }
  .u-clear-icon {
    position: absolute;
    right: 10rpx;
    bottom: 30rpx;
  }
}

.btn {
  margin-top: 60rpx;
}

::v-deep .right-text {
  .u-input__input {
    text-align: end;
    padding-right: 15rpx;
  }
}

.prev {
  padding: 15rpx 0 30rpx;
}
</style>
```

完成效果：

![image-20210527014805079](uniapp.assets/image-20210527014805079.5b658899.png)

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/10-发贴&评论功能.html#发帖服务端逻辑)发帖服务端逻辑

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/10-发贴&评论功能.html#图片上传-2)图片上传

```js
// 上传图片
async uploadImg (ctx) {
  const file = ctx.request.files.file
  // 这里加入内容安全
  const flag = await wxImgCheck(file)
  if (!flag) {
    ctx.body = {
      code: 500,
      msg: '内容安全校验失败，请检查'
    }
    return
  }
  // 图片名称、图片格式、存储的位置，返回前台一可以读取的路径
  const ext = file.name.split('.').pop()
  const dir = `${config.uploadPath}/${moment().format('YYYYMMDD')}`
  // 判断路径是否存在，不存在则创建
  await mkdir(dir)
  // 存储文件到指定的路径
  // 给文件一个唯一的名称
  const picname = uuidv4()
  const destPath = `${dir}/${picname}.${ext}`
  const reader = fs.createReadStream(file.path)
  const upStream = fs.createWriteStream(destPath)
  const filePath = `/${moment().format('YYYYMMDD')}/${picname}.${ext}`
  // method 1
  reader.pipe(upStream)

  ctx.body = {
    code: 200,
    msg: '图片上传成功',
    data: filePath
  }
}
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/10-发贴&评论功能.html#发帖接口)发帖接口

调整整体的逻辑，删除验证码的逻辑部分，并添加内容安全校验：

```js
async addWxPost (ctx) {
  const { body } = ctx.request
  // 校验用户传递的参数与内容是否通过内容安全的校验
  // 判断用户的积分数是否 > fav，否则，提示用户积分不足发贴
  // 用户积分足够的时候，新建Post，减除用户对应的积分
  const user = await User.findByID({ _id: ctx._id })
  const flag = await wxMsgCheck(body.content || '', { user: user, title: body.title })
  if (!flag) {
    ctx.body = {
      code: 500,
      msg: '内容安全校验失败，请检查'
    }
    return
  }
  if (user.favs < body.fav) {
    ctx.body = {
      code: 501,
      msg: '积分不足'
    }
    return
  } else {
    await User.updateOne({ _id: ctx._id }, { $inc: { favs: -body.fav } })
  }
  const newPost = new Post(body)
  newPost.uid = ctx._id
  const result = await newPost.save()
  ctx.body = {
    code: 200,
    msg: '成功的保存的文章',
    data: result
  }
}
```

添加路由：

```js
// 小程序发表新贴
router.post('/wxAdd', contentController.addWxPost)
```

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/10-发贴&评论功能.html#评论功能)评论功能

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/10-发贴&评论功能.html#创建接口)创建接口

```js
// 评论接口
const addComment = (data) => axios.post('/comments/reply', data)
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/10-发贴&评论功能.html#fixed底部定位问题)fixed底部定位问题

在小程序中，使用了fixed定位的底部元素可能被遮挡：

解决方案：

```vue
<!-- fixed: 1.static/absolute 2.点击跳转新页面 3.cursor-spacing -->
<view class="box u-flex u-col-center" v-else>
  <u-input v-model="content" class="reply" placeholder="请输入评论内容" focus @clear="clear" :cursor-spacing="10"></u-input>
  <button type="primary" plain size="mini" @click.stop="send">发送</button>
</view>
```

推荐：使用`cursor-spacing`，[官方链接(opens new window)](https://developers.weixin.qq.com/miniprogram/dev/component/input.html)

![image-20210817235100442](uniapp.assets/image-20210817235100442.52657281.png)

如果不设置，默认是0，也是为什么会浮动不准确的原因。

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/10-发贴&评论功能.html#页面样式与事件)页面样式与事件

```vue
<template>
  <view class="detail" :class="{'ipx': ipxFlag}" v-show="page._id" @click.stop="showReply = false">
    // .....
    <view class="footer">
      <view class="box u-flex u-col-center" v-if="!showReply">
        <view class="add-comment" @click.stop="reply()">
          <u-icon name="edit-pen" size="32" color="#cccccc"></u-icon>
          <text class="text">写评论</text>
        </view>
        <view class="ctrls u-flex u-col-center u-row-between">
          <view class="comment u-flex flex-column">
            <u-icon name="chat" size="45"></u-icon>
            <text>评论{{ page.answer > 0 ? page.answer : ''}}</text>
          </view>
          <view class="fav u-flex flex-column" :class="{'active': page.isFav === 1}" @click="setCollect">
            <u-icon name="star-fill" size="45" v-if="page.isFav === 1"></u-icon>
            <u-icon name="star" size="45" v-else></u-icon>
            <text>{{page.isFav === 1 ? '已收藏': '收藏'}}</text>
          </view>
          <view class="like u-flex flex-column" :class="{'active': page.isHand === 1}" @click="handsPost">
            <u-icon name="thumb-up-fill" size="45" v-if="page.isHand === 1"></u-icon>
            <u-icon name="thumb-up" size="45" v-else></u-icon>
            <text>{{page.isHand === 1 ? '已点赞' : '点赞'}}</text>
          </view>
        </view>
      </view>
      <!-- fixed: 1.static/absolute 2.点击跳转新页面 3.cursor-spacing -->
      <view class="box u-flex u-col-center" v-else>
        <u-input v-model="content" class="reply" placeholder="请输入评论内容" focus @clear="clear" :cursor-spacing="10"></u-input>
        <button type="primary" plain size="mini" @click.stop="send">发送</button>
      </view>
    </view>
  </view>
</template>

<script>
import { mapGetters, mapState } from 'vuex'
import { checkToken } from '@/common/checkAuth'
import formatHTML from '@/common/utils/formatHTML'

export default {
  components: {},
  data: () => ({
    // ....
    content: '',
    showReply: false,
		// ...
  }),
  // ....
  methods: {
    // ....
    reply () {
      if (!this.check()) return
      this.showReply = true
    },
    async send () {
      const { code, msg } = await this.$u.api.addReply({ tid: this.params.tid, content: this.content })
      if (code === 200) {
        uni.$success(msg)
      } else {
        if (code === 500 && /内容安全/.test(msg)) {
          uni.showModal({
            title: '注意文明用语',
            content: '发布内容没有通过内容审核，请检查后重新提效',
            showCancel: false
          })
          return
        }
        uni.$error(msg)
      }
      await this.getReply()
      this.content = ''
      this.showReply = false
    },
  },
  // ...
}
</script>

<style lang="scss">
.detail {
  background: #f4f6f8;
  height: 100vh;
  &.ipx {
    .comments,
    .footer {
      padding-bottom: constant(safe-area-inset-bottom); // 兼容iOS < 11.2
      padding-bottom: env(safe-area-inset-bottom);
    }
  }
}

.header,
.content,
.comments {
  background: #fff;
  padding: 32rpx;
}

.header,
.content {
  margin-bottom: 24rpx;
  box-shadow: 0 5rpx 5px rgba($color: black, $alpha: 0.1);
}

.add-hand {
  position: relative;
  .caina {
    position: absolute;
    right: 100rpx;
    top: -20rpx;
  }
  .setBest {
    padding-left: 25rpx;
  }
}

.footer {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100vw;
  padding: 10px 32rpx;
  background-color: #fff;
  // height: 100rpx;
  box-shadow: 0 -5rpx 5px rgba($color: black, $alpha: 0.1);
  .box {
    width: 100%;
  }
  .reply {
    flex: 1;
    border: 1px solid #eee;
    padding: 0 15rpx;
    margin-right: 15rpx;
  }
}

.title {
  font-size: 32rpx;
  color: #333;
  font-weight: bold;
}
.add-comment {
  background: #f3f3f3;
  height: 64rpx;
  border-radius: 32rpx;
  line-height: 64rpx;
  padding: 0 32rpx;
  width: 65%;
  margin-right: 40rpx;
  color: #ccc;
  .text {
    padding-left: 10rpx;
  }
}

.loading {
  height: 50px;
  .loading-text {
    padding-left: 15rpx;
  }
}

.layui-elem-quote {
  margin-bottom: 10rpx;
  padding: 15rpx;
  line-height: 14px;
  border-left: 2px solid #009688;
  border-radius: 0 2px 2px 0;
  background-color: #f2f2f2;
}
</style>
```

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/10-发贴&评论功能.html#评论服务端逻辑)评论服务端逻辑

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/10-发贴&评论功能.html#需求分析)需求分析

* 用户是否被禁言
* 内容安全检查
* 评论 -> 发送订阅消息给用户

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/10-发贴&评论功能.html#评论接口)评论接口

路由：

```js
// 微信评论回复
router.post('/wxreply', commentsController.wxAddComment)
```

逻辑代码：

```js
const canReply = async (ctx) => {
  let result = false
  // const obj = await getJWTPayload(ctx.header.authorization)
  if (typeof ctx._id === 'undefined') {
    return result
  } else {
    const user = await User.findByID(ctx._id)
    if (user && user.status === '0') {
      result = true
    }
    return result
  }
}

async wxAddComment(ctx) {
    // 查看用户是否被禁言
    const check = await canReply(ctx)
    if (!check) {
      ctx.body = {
        code: 500,
        msg: '用户已被禁言！',
      }
      return
    }
    const { body } = ctx.request
    const result = await wxMsgCheck(body.content)
    if (result && ctx._id) {
      // const obj = await getJWTPayload(ctx.header.authorization)
      const user = await User.findByID(ctx._id)
      const newComment = new Comments(body)
      newComment.cuid = ctx._id
      // 添加文章评论记数
      await Post.updateOne({ _id: body.tid }, { $inc: { answer: 1 } })
      const post = await Post.findByPostId(body.tid)
      newComment.uid = post.uid._id // 保存帖子作者的id
      const comment = await newComment.save()

      // 调用微信订阅消息api
      // 1、发帖的作者必须是用微信登录的才可以接收订阅消息
      // 2、自己不能给自己发订阅消息
      // if (post.uid.openid) {
      if (ctx._id !== post.uid._id.toString() && post.uid.openid) {
        const notice = await wxSendMessage({
          touser: post.uid.openid,
          template_id: 'ANN2-LhDgrhdFjs7jHOLdTnaxWpQU1LqS3kDIMF9GDs',
          data: {
            thing1: {
              value: getShort(post.title),
            },
            thing4: {
              value: getCatalog(post.catalog),
            },
            thing2: {
              value: getShort(comment.content),
            },
            name6: {
              value: user.name.substr(0, 10),
            },
            date3: {
              value: moment().format('YYYY年MM月DD HH:mm'),
            },
          },
          page: '/subcom-pkg/detail/detail?tid=' + post._id,
        })
        console.log(notice)
      }

      ctx.body = {
        msg: '回帖成功',
        code: 200,
        data: comment,
      }
    } else {
      ctx.body = {
        code: 500,
        msg: '内容安全：' + result.errmsg,
      }
    }
  }
```



# [#](https://front-end.toimc.com/notes-page/project/community-miniapp/11-发布上线.html#发布上线)发布上线

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/11-发布上线.html#分包机制)分包机制

某些情况下，开发者需要将小程序划分成不同的子包，在构建时打包成不同的分包，用户在使用时按需进行加载。

![image-20210818000627795](uniapp.assets/image-20210818000627795.e1b5dd91.png)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/11-发布上线.html#普通分包)普通分包

在构建小程序分包项目时，构建会输出一个或多个分包。

每个使用分包小程序必定含有一个**主包**。

所谓的主包，即放置默认启动页面/TabBar 页面，以及一些所有分包都需用到公共资源/JS 脚本；而**分包**则是根据开发者的配置进行划分。

在小程序启动时，默认会下载主包并启动主包内页面，当用户进入分包内某个页面时，客户端会把对应分包下载下来，下载完成后再进行展示。

目前小程序分包大小有以下限制：

* 整个小程序所有分包大小不超过 20M
* 单个分包/主包大小不能超过 2M

对小程序进行分包，可以优化小程序首次启动的下载时间，以及在多团队共同开发时可以更好的解耦协作。

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/11-发布上线.html#独立分包)独立分包

独立分包是小程序中一种特殊类型的分包，可以独立于主包和其他分包运行。从独立分包中页面进入小程序时，不需要下载主包。当用户进入普通分包或主包内页面时，主包才会被下载。

独立分包属于分包的一种，普通分包的所有限制都对独立分包有效（2M大小）。独立分包中插件、自定义组件的处理方式同普通分包。

此外，使用独立分包时要注意：

* **独立分包中不能依赖主包和其他分包中的内容**，包括 js 文件、template、wxss、自定义组件、插件等。

* 主包中的 `app.wxss` 对独立分包无效，应避免在独立分包页面中使用 `app.wxss` 中的样式；

* `App` 只能在主包内定义，独立分包中不能定义 `App`，会造成无法预期的行为；

  与普通分包不同，独立分包运行时，`App` 并不一定被注册，因此 `getApp()` 也不一定可以获得 `App` 对象；

* 独立分包中暂时不支持使用插件。

* 由于独立分包中无法定义 `App`，小程序生命周期的监听可以使用 [wx.onAppShow (opens new window)](https://developers.weixin.qq.com/miniprogram/dev/api/base/app/app-event/wx.onAppShow.html)，[wx.onAppHide (opens new window)](https://developers.weixin.qq.com/miniprogram/dev/api/base/app/app-event/wx.onAppHide.html)完成。

  `App` 上的其他事件可以使用 [wx.onError (opens new window)](https://developers.weixin.qq.com/miniprogram/dev/api/base/app/app-event/wx.onError.html)，[wx.onPageNotFound (opens new window)](https://developers.weixin.qq.com/miniprogram/dev/api/base/app/app-event/wx.onPageNotFound.html)监听。

> 应用场景：活动页面、登录注册相关页面...
>
> 大多数独立分包的场景，使用分包也可以很好的完成对应的功能，而且可以共享主包的样式。

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/11-发布上线.html#分包预加载)分包预加载

开发者可以通过配置，在进入小程序某个页面时，由框架自动预下载可能需要的分包，提升进入后续分包页面时的启动速度。

预下载分包行为在进入某个页面时触发，通过在 `app.json` 增加 `preloadRule` 配置来控制。

```js
"preloadRule": {
    "进入的页面": {
      "network": "all",
      "packages": ["加载的包中的页面", "或者加载整个目录"]
    },
    "sub1/index": {
      "packages": ["hello", "sub3"]
    },
    "sub3/index": {
      "packages": ["path/to"]
    },
    "indep/index": {
      "packages": ["__APP__"]
    }
  }
```

`preloadRule` 中，`key` 是页面路径，`value` 是进入此页面的预下载配置，每个配置有以下几项：

| 字段     | 类型        | 必填 | 默认值 | 说明                                                         |
| :------- | :---------- | :--- | :----- | :----------------------------------------------------------- |
| packages | StringArray | 是   | 无     | 进入页面后预下载分包的 `root` 或 `name`。`__APP__` 表示主包。 |
| network  | String      | 否   | wifi   | 在指定网络下预下载，可选值为： `all`: 不限网络 `wifi`: 仅wifi下预下载 |

说明：

* `network`建议使用`all`；
* 所有预下载的分包的总体积要小于2m，限额会打包自动校验；

> 不是必要，不是非常影响用户的交互体验，不要占用预下载分包的份额，不要使用大于40k的图片与资源。

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/11-发布上线.html#检查分包大小)检查分包大小

* 使用HBuilder中的发布方式打包；

  

* 在详情中进行查看：

  

  点击查看详情，用于排查哪些比较大的资源：

  ![image-20210818001614038](uniapp.assets/image-20210818001614038.b58d4f6c.png)

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/11-发布上线.html#基本流程)基本流程

![img](uniapp.assets/5.2.ac870e6c.ac870e6c.png)

* 测试-检查-打包-配置
* 在开发工具中上传
* 管理后台中，提交审核，通过后发布

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/11-发布上线.html#成员说明)成员说明

小程序成员管理包括对小程序项目成员及体验成员的管理。

* 项目成员：表示参与小程序开发、运营的成员，可登录小程序管理后台，包括运营者、开发者及数据分析者。管理员可在“成员管理”中添加、删除项目成员，并设置项目成员的角色。
* 体验成员：表示参与小程序内测体验的成员，可使用体验版小程序，但不属于项目成员。管理员及项目成员均可添加、删除体验成员。

不同项目成员拥有不同的权限，从而保证小程序开发安全有序。

| 权限           | 运营者 | 开发者 | 数据分析者 |
| :------------- | :----- | :----- | :--------- |
| 开发者权限     |        | √      |            |
| 体验者权限     | √      | √      | √          |
| 登录           | √      | √      | √          |
| 数据分析       |        |        | √          |
| 微信支付       | √      |        |            |
| 推广           | √      |        |            |
| 开发管理       | √      |        |            |
| 开发设置       |        | √      |            |
| 暂停服务       | √      |        |            |
| 解除关联公众号 | √      |        |            |
| 腾讯云管理     |        | √      |            |
| 小程序插件     | √      |        |            |
| 游戏运营管理   | √      |        |            |

配置路径：登录https://mp.weixin.qq.com/，选择管理 -> 成员管理 -> 添加项目成员，最多100个；体验成员->最多100个（已认证）；

![image-20210818123401415](uniapp.assets/image-20210818123401415.973c514f.png)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/11-发布上线.html#版本说明)版本说明

| **权限**   | **说明**                                                     |
| :--------- | :----------------------------------------------------------- |
| 开发版本   | 使用开发者工具，可将代码上传到开发版本中。 开发版本只保留每人最新的一份上传的代码。 点击提交审核，可将代码提交审核。开发版本可删除，不影响线上版本和审核中版本的代码。 |
| 体验版本   | 可以选择某个开发版本作为体验版，并且选取一份体验版。         |
| 审核中版本 | 只能有一份代码处于审核中。有审核结果后可以发布到线上，也可直接重新提交审核，覆盖原审核版本。 |
| 线上版本   | 线上所有用户使用的代码版本，该版本代码在新版本代码发布后被覆盖更新。 |

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/11-发布上线.html#注意事项)注意事项

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/11-发布上线.html#配置baseurl)配置BaseURL

```js
export const baseUrl = process.env.NODE_ENV === 'development' ? 'http://192.168.31.132:3000' : 'https://yourdomain.com'
```

> uniapp内置的打包工具即是webpack

说明：

* uniapp中，使用发布方式打包，即`process.env.NODE_ENV`会自动设置成`production`；

* 在上线之前，请确保配置了api后台项目，并申请了域名，配置好了HTTPS服务；

* 在小程序后台中，添加安全域名：

  

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/11-发布上线.html#使用hbuilder生产打包方式)使用HBuilder生产打包方式

一定要注意，在uniapp中开发的时候，启动的是调试进程，这时的代码中有很多调试代码，也未进行压缩。

步骤：

* 打开项目的`pages.json`文件；

* 点击顶部的菜单：`发行`-> `小程序-微信`

  

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/11-发布上线.html#打包上线)打包上线

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/11-发布上线.html#小程序打包)小程序打包

见 [检查分包大小](https://front-end.toimc.com/notes-page/project/community-miniapp/11-发布上线.html#检查分包大小)，注意这里的项目名称并非小程序的项目名称。

> 小程序的名称在注册小程序的时候，就已经确定下来了，这里只是一个项目别名。

请检查小程序的AppID。

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/11-发布上线.html#上传代码)上传代码

![image-20210818123801710](uniapp.assets/image-20210818123801710.34fb94d4.png)

上传代码是用于提交体验或者审核使用的。

点击开发者工具顶部操作栏的上传按钮，填写版本号以及项目备注，需要注意的是，这里版本号以及项目备注是为了方便管理员检查版本使用的，开发者可以根据自己的实际要求来填写这两个字段。

![image-20210818124047238](uniapp.assets/image-20210818124047238.169f3490.png)

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/11-发布上线.html#审核与发布)审核与发布

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/11-发布上线.html#审核版本)审核版本

上传成功之后，登录[小程序管理后台 (opens new window)](https://mp.weixin.qq.com/)- 开发管理 - 开发版本 就可以找到刚提交上传的版本了。

可以将这个版本设置 体验版 或者是 提交审核。

![image-20210818124719121](uniapp.assets/image-20210818124719121.d35f55fa.png)

然后点击下一步：

![image-20210818124806867](uniapp.assets/image-20210818124806867.cce75ad9.png)

说明：

* 加急的情况：与用户的钱有关，与平台的稳定有关，与自身的利益有关，才加急——慎用；
* 上面一般要写一个注释，说明版本的升级情况；

审核中：

![image-20210818125910968](uniapp.assets/image-20210818125910968.f62dbdcc.png)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/11-发布上线.html#关于灰度发布)关于灰度发布

审核通过后，需要在页面中点击发布，选择全量发布或者指定用户进行发布（灰度测试）。

点击发布后，即可发布小程序。小程序提供了两种发布模式：全量发布和分阶段发布。全量发布是指当点击发布之后，所有用户访问小程序时都会使用当前最新的发布版本。

分阶段发布是指分不同时间段来控制部分用户使用最新的发布版本，分阶段发布我们也称为灰度发布。一般来说，普通小程序发布时采用全量发布即可，当小程序承载的功能越来越多，使用的用户数越来越多时，采用分阶段发布是一个非常好的控制风险的办法。



# [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#支付专题)支付专题

支付作为工作中最常见的一个核心业务场景，前端同学其实需要做的东西有限，基本的流程、逻辑与难点都是在服务端。为了让前端同学，也能自己后续开发支付功能（Node.js），在本篇介绍了详细的从企业主体 -> 支付必要条件 ->微信小程序支付完整的闭环。

支付业务支撑：

* 商城应用
* 金融产品
* 充值应用（会员、点券）
* ...

支付应用场景：

* H5支付 -> 扫码、跳转App
* 小程序
* 移动App

支付的技术难点：

* 多平台（微信、支付宝、银联）
* 安全性（HTTPS、全流程日志、交易备案、数据安全性如灾备...）

常见的问题：

* 支付的开发流程，是不是前端不用管？只用调接口——是的，举例：小程序前端，其实上只是wx.requestPayment调起支付即可；
* 支付功能的开通容易吗？——容易，但是针对于个人，无法开通微信支付；个人能开通吗？——不行，但是可以使用第三方服务，比如JSAPI；
* 服务端对接常见的支付功能的流程是怎样的？——基本步骤如下：
  1. 申请微信小程序账号
  2. 微信小程序认证
  3. 申请商户平台账号
  4. 信小程序关联商户号
  5. 接入微信支付

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#企业注册与税务)企业注册与税务

开办企业需要注意：

* 财税问题，如果零报税，则需要企业年报每年需要申报。

  开办企业不是玩，一定要负责任。而且，不良的企业负债，可能会影响到个人的征信，最终无法贷款、乘车等。

* 股份问题：如果多人开办，千万不要对半

  多人入股不能均分，不能0资金入股，分股用期权；

  设计退股机制，分红机制，以及债务问题解决办法；

  股权主要的目的是激励；

* 利益分配问题：天下熙熙皆为利来，天下攘攘皆为利往；

无论是支付宝还是微信，能够支持的支付主体只有企业、个体工商户和政府及事业单位等，共同点是**非个人**。

> 个人开发者，如果需要接入支付功能，也可以选择第三方服务商：
>
> * PayJS：开通费用300+手续费0.38%+服务费2%
> * PaysApi：月付手续费30-199+单笔费率从0.3~0.1%不等
> * PayBob：开通费用300+手续费0.38%+服务费1-2%
> * xorpay：月付手续费0-60+手续费0.38%+单笔手续费1.2%~0.5%不等

了解企业的注册流程及税务相关的知识，有助于学习支付相关的内容，扩展知识面。

企业注册比较麻烦的地方：

* 设立登记可能要跑几躺
* 税务登记 + 银行开户 折腾个10几天
* 报税：有季报+年报（工商年报次年6月前、汇缴清算不定）

但是，开办企业也是有好处的，举例：

* 作为团队出去接项目
* 大多数网上的服务针对的是企业
* 国家政府对于小微企业现在有大力的扶持

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#注册企业流程)注册企业流程

![图片 1](./assets/图片 1.png)

企业注册流程中，需要注意的点：

* 准备工作（场地、人员、名称）

  公司查名各地有各地的查询网址或者在工商管理部门办事大厅进行现场查重（因为可能会重名，所以需要多准备几个）

  湖北企业查名查重：http://scjg.hubei.gov.cn/ICPSP/newNamecheck/nameCheck.action

  企业经营范围查询网址：https://jyfwyun.com/

* 提交资料（章程、住所证明）

  可以在当地的区（县）政务中心领取材料 或者在其网站上下载对应的模板文件，自行打印与复印。

* 办理税务+银行开户

  当办理完企业登记，并核发营业执照之后，可以输税务与企业银行开户。

  各家银行的开户费用大体相同，只是服务可能不一样，大家可以选择几家对比一下。推荐：招商与中兴。

  选择银行需要注意：

  * 服务问题；
  * 便利性——不要选择一家离自己办公点或者家很远的地方；

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#个人工商户与企业区别)个人工商户与企业区别

共同点：

* 流程：个体要记账、交税、年检、审批，也会被抽查
* 税费：与小微企业无异

不同点：

* 债务：个体负责到底-无限，公司申请破产保护-有限责任
* 规模：个体8人不能上市，公司可以设立分机构、上市
* 业务：个体不能做进出口业务
* 经营 ：个体限制经营范围
* 税费：税种不一样，征收方式不一样

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#自己注册vs代注册)自己注册vs代注册

**自己注册：**

优点：

* 没有费用

缺点：

* 需要花时间
* 需要了解整体流程
* 需要去税务、银行

**代注册：**

优点：

* 省时间
* 省事（办证、税务、企业银行、注册地、资料准备一条龙）

缺点：

* 费用不等，从3000-6000，甚至更多；
* 后续可能会有代账、年租费用等；
* 开票&办税也会收费；

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#报税-开票)报税&开票

大多数企业会找一个代账会计或者找一个代账公司，而企业前期是无需会计的，完全可以零报税，操作流程非常的简单。

如果不清楚报税的流程，可以在税务机关处进行现场报税。

代账费用：从200元到300元/月不等。

找了代账的企业需要会看如下的几个表格，了解概念：

* 利润表：你赚了多少钱！
* 资产负债表：有没有欠你的钱、你欠的钱，余下的钱

除了找代账公司以外，Brain更推荐自己在前期进行记账与报税，流程不复杂，而且软件非常的智能，只用填入自己的收入与支出，财务软件可以自动形成报表。

财务软件推荐：

* 金碟
* 用友

云平台推荐：

* 柠檬云

> 季度+年度报税：3个月报季报，照着软件填

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#企业日常的开销)企业日常的开销

主要分为如下几类：

* 资产：办公设备如打印机、办公桌椅、空调等
* 费用：租赁场地、水+电+网
* 费用：人员（会计、开发、产品等）
* ...

从上面的分类来看，注册企业没有什么费用，反而是维持一个企业的运转是需要大量费用的。大家在准备企业开办之前，需要有一定的准备。

> 不打没有准备的仗，也不要什么都准备好了，才开始！！

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#支付前置必要条件)支付前置必要条件

下面以微信支付为例，来介绍，开发支付功能需要准备的前置条件：

* 选择合适的主体&场景（企业服务号）微信（支付宝）认证；
* 域名+公网IP+HTTPS；
* 云服务器ICP备案；

微信支付：

* 主体问题：订阅号非媒体号无法支付，推荐企业服务号；
* 小程序：必须HTTPS + ICP备案；
* 开通商户号，也需要企业主体，与公众号&小程序同一主体；

支付宝支付：

* 主体问题：企业主体；

* 行业类目及资质要求，[文档 (opens new window)](https://opendocs.alipay.com/iot/multi-platform/material)；

  而且对于实体店铺审核要严格一些。

![image-20210830134004706](uniapp.assets/image-20210830134004706.f4a99b49.png)

说明：

* 关于ICP备案，可以参考：[阿里云 (opens new window)](https://beian.aliyun.com/)、[腾讯云 (opens new window)](https://cloud.tencent.com/product/ba)（前置条件：域名、云服务器）；
* HTTPS：必须要有域名，必须要有一台云服务器；
* 微信商户号，对应的网址：https://pay.weixin.qq.com/

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#小程序支付流程)小程序支付流程

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#开发技巧)开发技巧

* 技术问题，先读文档，查找官方的社区；
* 比较主流的支付方案，可以搜索一下有没有Node.js侧的npm包，例如：[wechatpay-axios-plugin (opens new window)](https://www.npmjs.com/package/wechatpay-axios-plugin)是一个非常不错的支持v2/v3的npm包，ts风格；
* 问stackoverflow、知乎、csdn等；
* 最后，才考虑自己开发轮子；

如果是学习，也可以从0到1的开发，了解整个支付的流程。

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#流程图)流程图

学会看时序图：

* 最上面的是角色
* 从左往右看，按照箭头的方向走
* 从上往下看，是时间关系流程的流转

![img](uniapp.assets/6_2.39c8c442.png)

重点步骤说明：

步骤3：用户下单发起支付，商户可通过[JSAPI下单 (opens new window)](https://pay.weixin.qq.com/wiki/doc/apiv3/apis/chapter3_5_1.shtml)创建支付订单。

步骤8： 用户可通过[小程序调起支付API (opens new window)](https://pay.weixin.qq.com/wiki/doc/apiv3/apis/chapter3_5_4.shtml)调起微信支付，发起支付请求。

步骤15：用户支付成功后，商户可接收到微信支付支付结果通知[支付通知API (opens new window)](https://pay.weixin.qq.com/wiki/doc/apiv3/apis/chapter3_5_5.shtml)。

步骤20：商户在没有接收到微信支付结果通知的情况下需要主动调用[查询订单API (opens new window)](https://pay.weixin.qq.com/wiki/doc/apiv3/apis/chapter3_5_2.shtml)查询支付结果。

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#搭建开发环境)搭建开发环境

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#支付准备)支付准备

![image-20210830135731466](uniapp.assets/image-20210830135731466.7d34db68.png)

按照上面的流程，准备相关的开发环境，参考指引：[官方链接(opens new window)](https://pay.weixin.qq.com/wiki/doc/apiv3/open/pay/chapter2_8_1.shtml)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#配置https-域名解析)配置Https+域名解析

说明：

* 前置的章节有介绍HTTPS介绍，[文章](https://front-end.toimc.com/notes-page/project/community-miniapp/07-安全域名相关.html#ssl证书申请)

* 域名解析以阿里云为例：

  ![image-20210830140428900](uniapp.assets/image-20210830140428900.83fd5ad0.png)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#配置api密钥)配置API密钥

[商户平台 (opens new window)](https://pay.weixin.qq.com/)-> 账户中心 -> API安全，分别配置API商户证书 + APIv3密钥

![image-20210830140522534](uniapp.assets/image-20210830140522534.b857eaea.png)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#配置frp内网穿透)配置frp内网穿透

为了方便测试微信支付通知，有两种方案：

* 上传API服务到测试服务器：

  ![image-20210830140648414](uniapp.assets/image-20210830140648414.f3ff8b55.png)

* 使用本地的API服务，需要使用frp工具把远程的通知转发到本地：

  ![image-20210830140722039](uniapp.assets/image-20210830140722039.310b80f5.png)

配置过程如下：

* 下载frp包到本地，见[release (opens new window)](https://github.com/fatedier/frp/releases)（darwin是mac系统、windows、Linux要分arm与amd）；

* SSH到远程服务端，新建配置文件`/home/frp/frps.ini`：

  ```nginx
  [common]
  bind_port = 10010
  vhost_https_port = 443
  ```

* 在服务器上运行frps，使用docker镜像：

  ```text
  docker run --restart=always --network host -d -v /home/frp/frps.ini:/etc/frp/frps.ini --name frps snowdreamtech/frps
  ```

* 下载let's encrypt创建的SSL证书 -> 一般在acme生成的目录中->放置到本地解压的frp目录中；

  ![image-20210830141415349](uniapp.assets/image-20210830141415349.46683708.png)

* 在本地创建`frpc.ini`文件

  ```nginx
  [common]
  server_addr = 121.36.194.226
  server_port = 10010
  
  [test_htts2http]
  type = https
  custom_domains = test1.toimc.com
  
  plugin = https2http
  plugin_local_addr = 127.0.0.1:3000
  
  # HTTPS 证书相关的配置
  plugin_crt_path = ./fullchain.pem
  plugin_key_path = ./key.pem
  plugin_host_header_rewrite = 127.0.0.1
  plugin_header_X-From-Where = frp
  ; test1.toimc.com -> vhost_https_port 443
  ; test1.toimc.com:443 -> frp -> 127.0.0.1:3000
  ```

* 使用frpc运行该配置文件：

  ```text
  chmod +x frpc
  ./frpc -c ./frpc.ini
  ```

  运行成功的提示：

  ![image-20210830141622200](uniapp.assets/image-20210830141622200.32b8bdce.png)

如果运行失败，可以在[官方网站 (opens new window)](https://gofrp.org/)上查询失败的原因：

* 官方FAQ：https://gofrp.org/docs/faq/
* 官方Issues：https://github.com/fatedier/frp/issues

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#apiv3-vs-apiv2)APIv3 vs APIv2

为了在 保证支付 安全的前提下，带给商户 简单、一致且易用的开发体验，我们推出了全新的微信支付API v3。

相较于之前的微信支付API，主要区别是：

* 遵循统一的REST 的设计风格
* 使用JSON作为数据交互的格式，不再使用XML
* 使用基于非对称密钥的SHA256-RSA的数字签名算法，不再使用MD5或HMAC-SHA256
* 不再要求HTTPS客户端证书
* 使用AES-256-GCM，对回调中的关键信息进行加密保护

APIv3的文档：[官方链接(opens new window)](https://pay.weixin.qq.com/wiki/doc/apiv3/wechatpay/wechatpay-1.shtml)

两个接口的对接图：

| **V3**               | **规则差异** | **V2**             |
| -------------------- | ------------ | ------------------ |
| JSON                 | 参数格式     | XML                |
| POST、GET 或 DELETE  | 提交方式     | POST               |
| AES-256-GCM加密      | 回调加密     | 无需加密           |
| RSA 加密             | 敏感加密     | 无需加密           |
| UTF-8                | 编码方式     | UTF-8              |
| 非对称密钥SHA256-RSA | 签名方式     | MD5 或 HMAC-SHA256 |

**推荐：在没有接触v2的情况下，直接上手v3；如果有老旧业务，也可以对接到v3，或者不动原有的业务，直至v2的证书快过期，需要更换时，再切换到v3。**

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#jsapi统一下单)JSAPI统一下单

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#签名生成)签名生成

[官方文档(opens new window)](https://pay.weixin.qq.com/wiki/doc/apiv3/wechatpay/wechatpay4_0.shtml)

商户可以按照下述步骤生成请求的签名，微信支付API v3 要求商户对请求进行签名，微信支付会在收到请求后进行签名的验证。

**如果签名验证不通过，微信支付API v3将会拒绝处理请求，并返回`401 Unauthorized`。**

签名生成的步骤有：

* 构造签名串

  签名串一共有五行，每一行为一个参数。行尾以 `\n`（换行符，ASCII编码值为0x0A）结束，包括最后一行。如果参数本身以`\n`结束，也需要附加一个`\n`。

  ```js
  HTTP请求方法\n
  URL\n
  请求时间戳\n
  请求随机串\n
  请求报文主体\n 
  ```

* 计算签名值

  使用商户私钥对待签名串进行SHA256 with RSA签名，并对签名结果进行Base64编码得到签名值，示例：

  ```bash
  $ echo -n -e \
  "GET\n/v3/certificates\n1554208460\n593BEC0C930BF1AFEB40B4A08C8FB242\n\n" \
    | openssl dgst -sha256 -sign apiclient_key.pem \
    | openssl base64 -A
    uOVRnA4qG/MNnYzdQxJanN+zU+lTgIcnU9BxGw5dKjK+VdEUz2FeIoC+D5sB/LN+nGzX3hfZg6r5wT1pl2ZobmIc6p0ldN7J6yDgUzbX8Uk3sD4a4eZVPTBvqNDoUqcYMlZ9uuDdCvNv4TM3c1WzsXUrExwVkI1XO5jCNbgDJ25nkT/c1gIFvqoogl7MdSFGc4W4xZsqCItnqbypR3RuGIlR9h9vlRsy7zJR9PBI83X8alLDIfR1ukt1P7tMnmogZ0cuDY8cZsd8ZlCgLadmvej58SLsIkVxFJ8XyUgx9FmutKSYTmYtWBZ0+tNvfGmbXU7cob8H/4nLBiCwIUFluw==
  ```

第一步比较好实现：

```js
// HTTP请求方法\n
// URL\n  https://www.imooc.com/path1/path2/?query1=value
// 请求时间戳\n
// 请求随机串\n
// 请求报文主体\n
const tmpUrl = new URL(url)
const nonceStr = rand.generate(16)
const pathname = /http/.test(url) ? tmpUrl.pathname : url
const timestamp = Math.floor(Date.now() / 1000)
const message = `${method.toUpperCase()}\n${pathname + tmpUrl.search
  }\n${timestamp}\n${nonceStr}\n${body ? JSON.stringify(body) : ''}\n`
```

第二步的RSA的算法实现思路：

* 找一下微信社区有没有类似实现
* 找一下网上有没有类似实现
* 找一下有没有npm开源包
* 找一下stackoverflow或者百度
* ...

如果 以上都没有，那么就要自己造轮子了。但是这样的场景少之有少，哈哈，还轮不上大家自己上。

```js
export const rsaSign = (message) => {
  const keyPem = fs.readFileSync(
    path.join(__dirname, 'keys/apiclient_key.pem'),
    'utf-8'
  )
  const signature = crypto
    .createSign('RSA-SHA256')
    .update(message, 'utf-8')
    .sign(keyPem, 'base64')
  return signature
}
```

然后使用上面合成的message进行签名即可：

```js
const signature = rsaSign(message)
```

**如何验证呢？**

方案一：

Linux或者mac直接使用openssl来进行验证

```text
echo -n -e \
"GET\n/v3/certificates\n1554208460\n593BEC0C930BF1AFEB40B4A08C8FB242\n\n" \
  | openssl dgst -sha256 -sign apiclient_key.pem \
  | openssl base64 -A
```

方案二：

使用老师给大家准备的docker镜像进行验证`lw96/libressl`：

```bash
# 1.创建容器
docker run -itd --name ssl lw96/libressl

# 2.拷贝证书
docker cp 证书目录 容器id:/tmp

# 3.进入容器
docker exec -it ssl sh

# 4.使用上面的一样的命令
echo -n -e \
"GET\n/v3/certificates\n1554208460\n593BEC0C930BF1AFEB40B4A08C8FB242\n\n" \
  | openssl dgst -sha256 -sign /tmp/apiclient_key.pem \
  | openssl base64 -A
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#authentication头部密钥串)Authentication头部密钥串

微信支付商户API v3要求请求通过`HTTP Authorization`头来传递签名。`Authorization`由*认证类型*和*签名信息*两个部分组成。

下面我们使用命令行演示如何生成签名。

```text
Authorization: 认证类型 签名信息
```

具体组成为：

1.认证类型，目前为WECHATPAY2-SHA256-RSA2048

2.签名信息

* 发起请求的商户（包括直连商户、服务商或渠道商）的商户号`mchid`
* [商户API证书 (opens new window)](https://pay.weixin.qq.com/wiki/doc/apiv3/wechatpay/wechatpay3_1.shtml)`serial_no`，用于[声明所使用的证书 (opens new window)](https://pay.weixin.qq.com/wiki/doc/apiv3/wechatpay/wechatpay3_1.shtml#part-3)（管理员账号登录微信商户管理后台，在API安全里面点击查看证书可以获取。）
* 请求随机串`nonce_str`
* 时间戳`timestamp`
* 签名值`signature`
* 注：以上五项签名信息，无顺序要求。

`Authorization` 头的示例如下：（注意，示例因为排版可能存在换行，实际数据应在一行）

```bash
Authorization: WECHATPAY2-SHA256-RSA2048 mchid="1900009191",nonce_str="593BEC0C930B
```

最终我们可以组一个包含了签名的HTTP请求了。

```bash
$ curl https://api.mch.weixin.qq.com/v3/certificates -H 'Authorization: WECHATPAY2-SHA256-RSA2048 mchid="1900009191",nonce_str="593BEC0C930BF1AFEB40B4A08C8FB242",signature="uOVRnA4qG/MNnYzdQxJanN+zU+lTgIcnU9BxGw5dKjK+VdEUz2FeIoC+D5sB/LN+nGzX3hfZg6r5wT1pl2ZobmIc6p0ldN7J6yDgUzbX8Uk3sD4a4eZVPTBvqNDoUqcYMlZ9uuDdCvNv4TM3c1WzsXUrExwVkI1XO5jCNbgDJ25nkT/c1gIFvqoogl7MdSFGc4W4xZsqCItnqbypR3RuGIlR9h9vlRsy7zJR9PBI83X8alLDIfR1ukt1P7tMnmogZ0cuDY8cZsd8ZlCgLadmvej58SLsIkVxFJ8XyUgx9FmutKSYTmYtWBZ0+tNvfGmbXU7cob8H/4nLBiCwIUFluw==",timestamp="1554208460",serial_no="1DDE55AD98ED71D6EDD4A4A16996DE7B47773A8C"'
```

代码示例：

```js
// config.js
const mchid = 'xxxxx'

const serialNo = 'xxxxx'

export default {
  // ...
  mchid,
  serialNo
}

// WxPay.js
export const getSignHeaders = (url, method, body) => {
  // HTTP请求方法\n
  // URL\n  https://www.imooc.com/path1/path2/?query1=value
  // 请求时间戳\n
  // 请求随机串\n
  // 请求报文主体\n
  const tmpUrl = new URL(url)
  const nonceStr = rand.generate(16)
  const pathname = /http/.test(url) ? tmpUrl.pathname : url
  const timestamp = Math.floor(Date.now() / 1000)
  const message = `${method.toUpperCase()}\n${pathname + tmpUrl.search
    }\n${timestamp}\n${nonceStr}\n${body ? JSON.stringify(body) : ''}\n`
  // const keyPem = fs.readFileSync(path.join(__dirname, 'keys/apiclient_key.pem'), 'utf-8')
  // const signature = crypto.createSign('RSA-SHA256').update(message, 'utf-8').sign(keyPem, 'base64')
  const signature = rsaSign(message)
  // 1.解决问题：windows上无openssl -> lw96/libressl
  // 2.需要传递apiclient_key.pem给镜像 -> 因为只有在容器里面才能执行openssl
  // 3.method1: docker cp  method2: -v
  // 4.使用openssl进行签名 -> 对比crypto产生的base64串

  return {
    headers: `WECHATPAY2-SHA256-RSA2048 mchid="${config.mchid}",nonce_str="${nonceStr}",signature="${signature}",timestamp="${timestamp}",serial_no="${config.serialNo}"'`,
    nonceStr,
    timestamp
  }
}
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#接口说明)接口说明

接口文档：[JSAPI(opens new window)](https://pay.weixin.qq.com/wiki/doc/apiv3/apis/chapter3_5_1.shtml)

**请求URL：**https://api.mch.weixin.qq.com/v3/pay/transactions/jsapi

**请求方式：**POST

形成随机的商户订单号：

```js
export const getTradeNo = () => {
  // 服务端侧：out_trade_no -> 订单号 -> timestamp + type + id
  // 1.Date.now()  2.Moment/dayjs
  // 2. 01-小程序
  return (
    dayjs().format('YYYYMMDDHHmmssSSS') +
    '01' +
    Math.random().toString().substr(-10)
  )
}
```

最终，统一订单的接口：

```js
export const wxJSPAY = async (params) => {
  const {
    description,
    goodsTag,
    total,
    user: { openid },
    detail,
    sceneInfo,
    settleInfo
  } = params
  // https://api.mch.weixin.qq.com/v3/pay/transactions/jsapi
  // 小程序用户侧： description，amount:{total} -> token -> id -> openid

  // 参数准备
  const wxParams = {
    appid: config.AppID,
    mchid: config.mchid,
    description,
    out_trade_no: getTradeNo(),
    time_expire: dayjs().add(30, 'm').format(),
    attach: '',
    notify_url: 'https://test1.toimc.com/public/notify',
    goods_tag: goodsTag,
    amount: {
      total: parseInt(total),
      currency: 'CNY'
    },
    payer: {
      openid
    },
    detail,
    scene_info: sceneInfo,
    settle_info: settleInfo
  }
  const url = 'https://api.mch.weixin.qq.com/v3/pay/transactions/jsapi'
	// 头部签名  
  const { headers, nonceStr, timestamp } = getSignHeaders(
    url,
    'post',
    wxParams
  )
  try {
    const result = await instance.post(url, wxParams, {
      headers: {
        Authorization: headers
      }
    })
    console.log('🚀 ~ file: WxPay.js ~ line 53 ~ wxJSPAY ~ result', result)
    const { status, data } = result
    if (status === 200) {
      return { prepayId: data.prepay_id, nonceStr, timestamp }
    } else {
      logger.error(`wxJSPAY error: ${result}`)
    }
  } catch (error) {
    logger.error(`wxJSPAY error: ${error.message}`)
  }
}
```

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#用户支付)用户支付

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#支付步骤)支付步骤

前端步骤：

* 用户下单 -> 发起请求，让后端生成支付参数
* 用户支付 -> wx.requestPayment调起支付

后端步骤：

* 使用JSAPI统一下单，产生的prepay_id（预支付id）
* 构造签名串 -> 计算签名
* 返回前端支付参数

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#准备支付参数)准备支付参数

第一步：

**造签名串**

```text
签名串一共有四行，每一行为一个参数。行尾以\n（换行符，ASCII编码值为0x0A）结束，包括最后一行。
如果参数本身以\n结束，也需要附加一个\n
```

**参与签名字段及格式：**

```text
小程序appId
时间戳
随机字符串
订单详情扩展字符串
```

第二步：

**计算签名：**

```bash
echo -n -e \
"wx8888888888888888\n1414561699\n5K8264ILTKCH16CQ2502SI8ZNMTM67VS\nprepay_id=wx201410272009395522657a690389285100\n" \
  | openssl dgst -sha256 -sign apiclient_key.pem \
  | openssl base64 -A
```

参数：

| 参数名             | 变量      | 类型[长度限制] | 必填 | 描述                                                         |
| :----------------- | :-------- | :------------- | :--- | :----------------------------------------------------------- |
| 时间戳             | timeStamp | string[1,32]   | 是   | 当前的时间，其他详见[时间戳规则 (opens new window)](https://pay.weixin.qq.com/wiki/doc/api/wxpay_v2/jiekouguize/chapter1_2.shtml#part-5)。 示例值：1414561699 |
| 随机字符串         | nonceStr  | string[1,32]   | 是   | 随机字符串，不长于32位。 示例值：5K8264ILTKCH16CQ2502SI8ZNMTM67VS |
| 订单详情扩展字符串 | package   | string[1,128]  | 是   | 小程序下单接口返回的prepay_id参数值，提交格式如：prepay_id=*** 示例值：prepay_id=wx201410272009395522657a690389285100 |
| 签名方式           | signType  | string[1,32]   | 是   | 签名类型，默认为RSA，仅支持RSA。 示例值：RSA                 |
| 签名               | paySign   | string[1,512]  | 是   | 签名，使用字段appId、timeStamp、nonceStr、package计算得出的签名值 示例值 |

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#后端接口开发)后端接口开发

创建路由：

```js
// 微信用户下单
router.post('/wxOrder', userController.wxOrder)
```

创建`wxOrder`下单方法：

```js
async wxOrder (ctx) {
  const { body } = ctx.request
  // 为什么订单的total即商品的金额信息不能从前端传？
  // 从前端传商品的id -> 在后端查询对应id的商品价格
  const { description, total } = body
  const user = await User.findByID(ctx._id)
  const params = {
    description,
    total,
    user
  }
  // 1. 发起wxPay -> prepay_id
  const { prepayId, nonceStr, timestamp } = await wxJSPAY(params)
  // 小程序appId
  // 时间戳
  // 随机字符串
  // 订单详情扩展字符串
  const paySign = rsaSign(`${config.AppID}\n${timestamp}\n${nonceStr}\nprepay_id=${prepayId}\n`)
  // 2. 拼接数据返回前端
  ctx.body = {
    code: 200,
    data: {
      appId: config.AppID,
      timestamp,
      nonceStr,
      package: `prepay_id=${prepayId}`,
      signType: 'RSA',
      paySign
    }
  }
}
```

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#前端模拟下单-支付)前端模拟下单&支付

```js
async order () {
  const res = await this.$u.api.orderGoods({
    description: 'toimc测试商品',
    total: 1 // 单位分
  })
  // console.log('🚀 ~ file: order.vue ~ line 23 ~ order ~ res', res)
  const { code, data } = res
  if (code === 200) {
    this.orderParams = data
    uni.showToast({
      icon: 'none',
      title: '下单成功',
      duration: 2000
    })
  }
},
pay () {
  uni.requestPayment({
    provider: 'weixin',
    orderInfo: {
      description: 'toimc测试商品',
      total: 1 // 单位分
    },
    timeStamp: this.orderParams.timestamp + '',
    nonceStr: this.orderParams.nonceStr,
    package: this.orderParams.package,
    signType: this.orderParams.signType,
    paySign: this.orderParams.paySign,
    complete: function (res) {
      console.log('🚀 ~ file: order.vue ~ line 47 ~ pay ~ res', res)
      // errMsg: "requestPayment:ok" -> 支付成功
      // errMsg: "requestPayment:fail cancel" -> 取消
    }
  })
}
```

## [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#订单查询)订单查询

用户支付成功后，需要接受微信平台的被动通知或者是主动查询订单的支付状态。

原因：可能用户支付成功后，微信后台已经给了用户侧反馈，但是由于网络问题，商户平台可能未收到通知。

![image-20210830173043224](uniapp.assets/image-20210830173043224.358bbadb.png)

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#微信主动通知)微信主动通知

frp转发通知，只需要创建对应的接口，然后在JSAPI统一下单的接口中指定回调的域名即可。

**请求方式：**POST

**回调URL：**该链接是通过基础下单接口中的请求参数“notify_url”来设置的，要求必须为https地址。请确保回调URL是外部可正常访问的，且不能携带后缀参数，否则可能导致商户无法接收到微信的回调通知信息。回调URL示例： “https://pay.weixin.qq.com/wxpay/pay.action”

**通知规则**

用户支付完成后，微信会把相关支付结果和用户信息发送给商户，商户需要接收处理该消息，并返回应答。

对后台通知交互时，如果微信收到商户的应答不符合规范或超时，微信认为通知失败，微信会通过一定的策略定期重新发起通知，尽可能提高通知的成功率，但微信不保证通知最终能成功。（通知频率为15s/15s/30s/3m/10m/20m/30m/30m/30m/60m/3h/3h/3h/6h/6h - 总计 24h4m）

**通知报文**

支付结果通知是以POST 方法访问商户设置的通知url，通知的数据以JSON 格式通过请求主体（BODY）传输。通知的数据包括了加密的支付结果详情。

下面详细描述对通知数据进行解密的流程：

1. 用商户平台上设置的APIv3密钥【[微信商户平台 (opens new window)](https://pay.weixin.qq.com/)—>账户设置—>API安全—>设置APIv3密钥】，记为key；
2. 针对resource.algorithm中描述的算法（目前为AEAD_AES_256_GCM），取得对应的参数nonce和associated_data；
3. 使用key、nonce和associated_data，对数据密文resource.ciphertext进行解密，得到JSON形式的资源对象；

**注：** AEAD_AES_256_GCM算法的接口细节，请参考[rfc5116 (opens new window)](https://tools.ietf.org/html/rfc5116)。微信支付使用的密钥key长度为32个字节，随机串nonce长度12个字节，associated_data长度小于16个字节并可能为空。

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#主动通知后端接口)主动通知后端接口

解密方法：

```js
export const decryptByApiV3 = ({
  associate, // 加密参数 - 类型
  nonce, // 加密参数 - 随机数
  ciphertext // 加密密文
} = {}) => {
  ciphertext = decodeURIComponent(ciphertext)
  ciphertext = Buffer.from(ciphertext, 'base64')

  const authTag = ciphertext.slice(ciphertext.length - 16)
  const data = ciphertext.slice(0, ciphertext.length - 16)

  const decipher = crypto.createDecipheriv(
    'aes-256-gcm',
    config.apiV3Key,
    nonce
  )
  decipher.setAuthTag(authTag)
  decipher.setAAD(Buffer.from(associate))

  let decryptedText = decipher.update(data, null, 'utf8')
  decryptedText += decipher.final()
  return decryptedText
}
```

创建接口：

```text
// 获取支付通知
router.post('/notify', adminController.wxNotify)
```

接口详情`wxNotify`：

```js
// 微信支付回调通知
async wxNotify (ctx) {
  const { body } = ctx.request
  const { resource_type: type, resource } = body
  if (type === 'encrypt-resource') {
    const { ciphertext, associated_data: associate, nonce } = resource
    const str = decryptByApiV3({
      associate,
      nonce,
      ciphertext
    })
    console.log('🚀 ~ file: AdminController.js ~ line 326 ~ AdminController ~ wxNotify ~ str', str)
    // todo 入库，并修改订单的支付成功的状态
  }
  console.log(
    '🚀 ~ file: AdminController.js ~ line 294 ~ AdminController ~ wxNotify ~ body',
    body
  )
  ctx.body = {
    code: 200
  }
}
```

> 收到订单通知后，需要保存通知中的状态与数据（微信订单号）。

### [#](https://front-end.toimc.com/notes-page/project/community-miniapp/12-支付专题.html#订单查询接口)订单查询接口

[官方文档(opens new window)](https://pay.weixin.qq.com/wiki/doc/apiv3/apis/chapter3_5_2.shtml)

有两种方案：

* 微信支付订单号查询
* 商户订单号查询（一般采用这种）

**请求URL：** `https://api.mch.weixin.qq.com/v3/pay/transactions/out-trade-no/{out_trade_no}`

**请求方式：**GET

**请求参数**

| 参数名     | 变量         | 类型[长度限制] | 必填 | 描述                                                         |
| :--------- | :----------- | :------------- | :--- | :----------------------------------------------------------- |
| 直连商户号 | mchid        | string[1,32]   | 是   | query 直连商户的商户号，由微信支付生成并下发。 示例值：1230000109 |
| 商户订单号 | out_trade_no | string[6,32]   | 是   | path 商户系统内部订单号，只能是数字、大小写字母_-*且在同一个商户号下唯一。 特殊规则：最小字符长度为6 示例值：1217752501201407033233368018 |

示例：

```text
https://api.mch.weixin.qq.com/v3/pay/transactions/out-trade-no/1217752501201407033233368018?mchid=1230000109
```

> 这里要特别注意，url中有路径参数与query参数

后端代码：

```js
import qs from 'qs'

// 微信支付订单查询 out_trade_no
//  https://api.mch.weixin.qq.com/v3/pay/transactions/out-trade-no/{out_trade_no}
export const getNofityByTradeNo = async (id) => {
  try {
    let url = `https://api.mch.weixin.qq.com/v3/pay/transactions/out-trade-no/${id}?`
    const params = {
      mchid: config.mchid
    }
    url += qs.stringify(params)
    const { headers, nonceStr, timestamp } = getSignHeaders(url, 'get')
    const result = await instance.get(url, {
      headers: {
        Authorization: headers
      }
    })
    console.log(
      '🚀 ~ file: WxPay.js ~ line 147 ~ getNofityByTradeNo ~ timestamp',
      timestamp
    )
    console.log(
      '🚀 ~ file: WxPay.js ~ line 147 ~ getNofityByTradeNo ~ nonceStr',
      nonceStr
    )
    console.log('🚀 ~ file: WxPay.js ~ line 53 ~ wxJSPAY ~ result', result)
    // todo result.data -> trade_state trade_type -> 存储订单的其他信息
  } catch (error) {
    logger.error(`getNofityByTradeNo error: ${error.message}`)
  }
}
```

至此，完成的小程序支付的一个完整的闭环。

> 关于退款与退款通知与下单&订单通知是类似，不再提供示例。