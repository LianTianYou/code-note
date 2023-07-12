# Git

## 目录

-   [1. 初始化仓库](#1-初始化仓库)
-   [2. 暂存区](#2-暂存区)
-   [3. 提交](#3-提交)
    -   [3.1 修改提交](#31-修改提交)
-   [4. 远程仓库](#4-远程仓库)
    -   [4.1 关联仓库](#41-关联仓库)
    -   [4.2 推送到远程](#42-推送到远程)
-   [5. 分支](#5-分支)
-   [6. 撤消和恢复](#6-撤消和恢复)
-   [7. 配置](#7-配置)

[GitHub](https://www.wolai.com/3SFJ2jPhY4xFDs3iXteFBF "GitHub")

# 1. 初始化仓库

初始化一个仓库（创建 .git 目录）：

```.properties
git init
```

配置用户信息：

```.properties
git config --global user.name <用户名>
git config --global user.email <邮箱>
```

# 2. 暂存区

添加指定文件到暂存区：

```.properties
git add <文件路径>
```

添加所有文件到暂存区：

```.properties
git add .
```

显示文件状态：

```.properties
git status
```

# 3. 提交

提交：

```.properties
git commit -m "提交信息"
```

添加并提交（不需要 add）：

```.properties
git commit -a -m "提交信息"
```

## 3.1 修改提交

提交历史

```.properties
git log
```

修改上一次的提交信息：

```.properties
git commit --amend -m '提交信息'
```

提交时忽略的文件:

-   在项目中创建一个`.gitignore`文件，将文件列表填写在里面。

# 4. 远程仓库

克隆远程仓库到本地：

> 包含`.git`文件夹

```.properties
git clone <URL>
```

## 4.1 关联仓库

关联远程仓库：

> 将本地仓库关联到远程

```.properties
git remote add origin <URL>
```

查看关联的远程仓库：

```git
git remote -v
```

删除关联的仓库：

```git
git remote remove origin
```

## 4.2 推送到远程

推送到远程仓库：

```.properties
git push origin master
```

覆盖推送到远程：

```.properties
git push origin master -f
```

从远程仓库拉取：

```.properties
git pull origin master
```

# 5. 分支

查看分支列表：

```.properties
git branch
```

新建分支：

```.properties
git branch <分支名>
```

切换分支：

```.properties
git checkout <分支名>
```

新建分支并切换：

```.properties
git checkout -b <分支名>
```

删除分支：

```.properties
git branch -d <分支名>
```

强制删除：

```.properties
git branch -D <分支名>
```

# 6. 撤消和恢复

对比更改内容：

```.properties
git diff
```

恢复文件（回退到最后一次提交）：

```.properties
git checkout <目标文件>
```

将文件移出暂存区：

```.properties
git reset <文件路径>
```

# 7. 配置

设置代理：

```.properties
git config --global http.proxy http://127.0.0.1:1080
```

取消代理：

```.properties
git config --global --unset http.proxy

```

只对 github 设置代理：

```.properties
git config --global http.https://github.com.proxy http://127.0.0.1:1080
```

取消github代理：

```.properties
git config --global --unset http.https://github.com.proxy
```
