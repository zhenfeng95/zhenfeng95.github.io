---
title: git 安装与使用
type: git
order: 1
---

## 安装

### 1、Windows 安装 Git

1. 打开官网  https://git-scm.com/install/windows

2. 点击主页上的 **“Download for Windows”**，下载 `.exe` 安装包。

3. 双击安装程序，保持默认选项即可（一般一路「Next」）。

   > ⚙️ 建议在选项中勾选：
   >
   > * “Git Bash Here”
   > * “Use Git from Windows Command Prompt”
   > * “Checkout Windows-style, commit Unix-style line endings”

4. 安装完成后，打开命令行（CMD 或 PowerShell），输入：

   ```
   git --version
   ```

   如果输出类似：

   ```
   git version 2.47.0.windows.1
   ```

   说明安装成功。

### 2、macOS 安装 Git

方式一：通过 Homebrew 安装（推荐）

如果你已安装 **Homebrew**，直接运行：

```
brew install git
```

方式二：通过 Xcode Command Line Tools 安装

系统自带方式：

```
xcode-select --install
```

安装后运行：

```
git --version
```

验证成功。

### 3、Linux 安装 Git

根据不同发行版执行命令：

| 系统            | 命令                                         |
| --------------- | -------------------------------------------- |
| Ubuntu / Debian | `sudo apt update && sudo apt install git -y` |
| CentOS / RHEL   | `sudo yum install git -y`                    |
| Fedora          | `sudo dnf install git -y`                    |
| Arch Linux      | `sudo pacman -S git`                         |

安装完成后验证：

```
git --version
```

### 4、安装后基础配置

首次使用 Git，建议配置用户名与邮箱（用于提交记录）：

```
git config --global user.name "Your Name"             
git config --global user.email "email@example.com"
```

验证配置：

```
git config --global --list
```

### 5、生成 SSH 密钥（连接 GitHub / GitLab）

**检查是否已有 SSH Key**

```bash
ls -al ~/.ssh	
```

**生成密钥**

新的GitHub 默认使用的是 ED25519 类型的 SSH key，因为它比传统的 RSA 更安全、性能更高，文件更小

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

老旧版本如何生成：

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
ssh-keygen -t rsa -C "youremail@example.com"
```

连续回车（默认路径 `~/.ssh/id_rsa`）。

**查看公钥**

```
cat ~/.ssh/id_rsa.pub
```

centos系统查看公钥，然后配置在github上

```bash
cat /root/.ssh/id_ed25519.pub
```

在centos系统中，尽量通过ssh拉取代码

```bash
git clone ssh://git@ssh.github.com:443/zhenfeng95/mysite.git
```

**查看是否配置成功**

将输出内容复制到 GitHub / GitLab 的 “SSH Keys” 设置中。

测试是否连接成功：

```
ssh -T git@github.com
```

```bash
ssh -T -p 443 git@ssh.github.com
```

成功会提示：

```
输出：Hi zhenfeng95! You've successfully authenticated, but GitHub does not provide shell access.
说明：
你的 SSH 公钥已经成功配置在 GitHub 上；
服务器可以通过 ssh.github.com:443 成功访问 GitHub；
可以安全地使用 SSH 拉取代码（使用 git@ssh.github.com:443 方式）
```

### 6、图形界面工具（可选）

* **GitHub Desktop**（简单易用）
   👉 https://desktop.github.com/
* **SourceTree**（适合团队协作）
   👉 https://www.sourcetreeapp.com/

## 常用命令

### 创建/拉取仓库

在当前目录新建一个Git代码库

```shell
git init
```

新建一个目录，将其初始化为Git代码库

```shell
git init [project-name]
```

下载一个项目和它的整个代码历史

```shell
git clone [url]
```

### 增加/删除文件

添加指定文件到暂存区

```shell
git add [file1] [file2] ...
```

添加指定目录到暂存区，包括子目录

```shell
git add [dir]
```

添加当前目录的所有文件到暂存区

```shell
git add .
```

添加每个变化前，都会要求确认
对于同一个文件的多处变化，可以实现分次提交

```shell
git add -p
```

删除工作区文件，并且将这次删除放入暂存区

```shell
git rm [file1] [file2] ...
```

停止追踪指定文件，但该文件会保留在工作区

```shell
git rm --cached [file]
git rm --cached -r . # 用于.gitignore不生效的情况
```

改名文件，并且将这个改名放入暂存区

```shell
git mv [file-original] [file-renamed]
```

### 代码提交

提交暂存区到仓库区

```shell
git commit -m [message]
```

提交工作区自上次commit之后的变化，直接到仓库区

```shell
git commit -a
```

提交时显示所有diff信息

```shell
git commit -v
```

使用一次新的commit，替代上一次提交
如果代码没有任何新变化，则用来改写上一次commit的提交信息

```shell
git commit --amend -m [message]
```

重做上一次commit，并包括指定文件的新变化

```shell
git commit --amend [file1] [file2] ...
```

### 查看信息

显示有变更的文件

```shell
git status
```

显示当前分支的版本历史

```shell
git log
```

显示commit历史，以及每次commit发生变更的文件

```shell
git log --stat
```

搜索提交历史，根据关键词

```shell
git log -S [keyword]
```

显示某个commit之后的所有变动，每个commit占据一行

```shell
git log [tag] HEAD --pretty=format:%s
```

显示某个commit之后的所有变动，其"提交说明"必须符合搜索条件

```shell
git log [tag] HEAD --grep feature
```

显示某个文件的版本历史，包括文件改名

```shell
git log --follow [file]
```

显示指定文件相关的每一次diff

```shell
git log -p [file]
```

显示过去5次提交

```shell
git log -5 --pretty --oneline
```

显示所有提交过的用户，按提交次数排序

```shell
git shortlog -sn
```

显示指定文件是什么人在什么时间修改过

```shell
git blame [file]
```

显示暂存区和工作区的差异

```shell
git diff
```

显示暂存区和上一个commit的差异

```shell
git diff --cached [file]
```

显示工作区与当前分支最新commit之间的差异

```shell
git diff HEAD
```

显示两次提交之间的差异

```shell
git diff [first-branch]...[second-branch]
```

显示今天你写了多少行代码

```shell
git diff --shortstat "@{0 day ago}"
```

显示某次提交的元数据和内容变化

```shell
git show [commit]
```

显示某次提交发生变化的文件

```shell
git show --name-only [commit]
```

显示某次提交时，某个文件的内容

```shell
git show [commit]:[filename]
```

显示当前分支的最近几次提交，记录着本地所有的提交以及分支的切换，包括删除类型的操作，reset操作等等

```shell
git reflog
```

### 查看分支

列出所有本地分支

```shell
git branch
```

列出所有远程分支

```shell
git branch -r
```

列出所有本地分支和远程分支

```shell
git branch -a
```

新建一个分支，但依然停留在当前分支

```shell
git branch [branch-name]
```

新建一个分支，并切换到该分支

```shell
git checkout -b [branch]
git checkout -b [branch] orgin/master
```

新建一个与远端同名分支，并切换到该分支

```shell
git checkout -t origin/[branch]
```

新建一个分支，指向指定commit

```shell
git branch [branch] [commit]
```

新建一个分支，与指定的远程分支建立追踪关系

```shell
git branch --track [branch] [remote-branch]
```

切换到指定分支，并更新工作区

```shell
git checkout [branch-name]
```

切换到上一个分支

```shell
git checkout -
```

建立追踪关系，在现有分支与指定的远程分支之间

```shell
git branch --set-upstream [branch] [remote-branch]
```

合并指定分支到当前分支

```shell
git merge [branch]
```

选择一个commit，合并进当前分支

```shell
git cherry-pick [commit]
```

删除分支

```shell
git branch -d [branch-name]
```

删除远程分支

```shell
git push origin --delete [branch-name]
```

### 查看标签

列出所有tag

```shell
git tag
```

新建一个tag在当前commit

```shell
git tag [tag]
```

新建一个tag在指定commit

```shell
git tag [tag] [commit]
```

删除本地tag

```shell
git tag -d [tag]
```

删除远程tag

```shell
git push origin :refs/tags/[tagName]
```

查看tag信息

```shell
git show [tag]
```

提交指定tag

```shell
git push [remote] [tag]
```

提交所有tag

```shell
git push [remote] --tags
```

新建一个分支，指向某个tag

```shell
git checkout -b [branch] [tag]
```

### 远程同步

下载远程仓库的所有变动

```shell
git fetch [remote]
```

显示所有远程仓库

```shell
git remote -v
```

显示某个远程仓库的信息

```shell
git remote show [remote]
```

增加一个新的远程仓库，并命名

```shell
git remote add [shortname] [url]
```

取回远程仓库的变化，并与本地分支合并

```shell
git pull [remote] [branch]
```

允许不相关历史提交,并强制合并

```shell
git pull origin master --allow-unrelated-histories
```

上传本地指定分支到远程仓库

```shell
git push [remote] [branch]
```

强行推送当前分支到远程仓库，即使有冲突

```shell
git push [remote] --force
```

推送所有分支到远程仓库

```shell
git push [remote] --all
```

### 撤销

恢复暂存区的指定文件到工作区

```shell
git checkout [file]
```

恢复某个commit的指定文件到暂存区和工作区

```shell
git checkout [commit] [file]
```

恢复暂存区的所有文件到工作区

```shell
git checkout .
```

重置暂存区的指定文件，与上一次commit保持一致，但工作区不变

```shell
git reset [file]
```

重置暂存区与工作区，与上一次commit保持一致

```shell
git reset --hard 
```

重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变

```shell
git reset [commit]
```

重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致

```shell
git reset --hard [commit]
```

回退到上一个版本

```shell
git reset --hard head^
git push origin master -f // 强制推到远端
```

重置当前HEAD为指定commit，但保持暂存区和工作区不变

```shell
git reset --keep [commit]
```

新建一个commit，用来撤销指定commit
后者的所有变化都将被前者抵消，并且应用到当前分支

```shell
git revert [commit]
```

暂时将未提交的变化移除，稍后再移入

```shell
git stash
会将当前分支的最后一次缓存的内容释放出来，但是刚才的记录不存在list中
git stash pop 
会将当前分支的最后一次缓存的内容释放出来，但是刚才的记录还存在list中
git stash apply 
```

## 忽略文件配置（.gitignore)

1、配置语法:

> 以斜杠“/”开头表示目录；
> 以星号“\*”通配多个字符；
> 以问号“?”通配单个字符
> 以方括号“[]”包含单个字符的匹配列表；
> 以叹号“!”表示不忽略(跟踪)匹配到的文件或目录；

此外，git 对于 .ignore 配置文件是按行从上到下进行规则匹配的，意味着如果前面的规则匹配的范围更大，则后面的规则将不会生效；
2、示例：
（1）规则：fd1/\*
　　  说明：忽略目录 fd1 下的全部内容；注意，不管是根目录下的 /fd1/ 目录，还是某个子目录 /child/fd1/ 目录，都会被忽略；
（2）规则：/fd1/\*
　　  说明：忽略根目录下的 /fd1/ 目录的全部内容；
（3）规则：
			/\*
			!.gitignore
			!/fw/bin/
			!/fw/sf/
			说明：忽略全部内容，但是不忽略 .gitignore 文件、根目录下的 /fw/bin/ 和 /fw/sf/ 目录；

## github代码如何同步到gitee

1. 生成一对公私钥对，github往gitee同步代码，github需要钥匙，就是私钥，而gitee需要公钥，相当于大门

   ```shell
   ssh-keygen -t ed25519 -C "285273676@qq.com"
   ```

   输入上方命令回车，然后在Enter file in which to save the key：中指定一个新的文件地址保存，不要影响现有项目的ssh，如：/Users/zhangzhenfeng/Downloads/demo_gitee，然后一路回车

2. 进入Downloads，查看公私钥

   ```shell
   cat demo_gitee.pub
   cat demo_gitee
   ```

 3. 在gitee平台配置公钥，设置-->SSH公钥中添加
 4. 在github平台配置公钥和私钥，进入具体的项目仓库，Settings-->Deploy keys中添加公钥，Settings-->Secrets and variables-->Repository secrets添加私钥，这个名称在后面的配置文件中会用到
 5. 开始配置github的Actions，搜索git-mirror-action插件，找到写好的脚本文件，在.github/workflows中新建一个sync-gitee.yml文件
 ```yml
name: Mirror to Gitee Repo

on: [ push ]

# Ensures that only one mirror task will run at a time.
concurrency:
  group: git-mirror

jobs:
  git-mirror:
    runs-on: ubuntu-latest
    steps:
      - uses: wearerequired/git-mirror-action@v1
        env:
          SSH_PRIVATE_KEY: ${{ secrets.GITEE_DEPLOY_KEY }}
        with:
          source-repo: "git@github.com:zhenfeng95/community-pc.git"
          destination-repo: "git@gitee.com:zhenfeng95/community-pc.git"
 ```