+++
date = '2025-07-08T02:15:15+08:00'
title = 'Git_use'
draft = false
description = '在学习Git过程中的记录'    
categories = ["项目管理"]
+++


**命令**

```c
# 初始化一个本地仓库
git init
# 添加所有文件到暂存区
git add .
# 提交暂存区的文件到本地仓库，并添加一条提交信息
git commit -m "first commit"
# 添加一个远程仓库的地址，命名为origin
git remote add origin https://github.com/user/repo.git
# 将本地仓库的内容推送到远程仓库的master分支
git push -u origin master
#查看当前缓存区的all内容
git ls-files   
#delete a cangku 的所有提交记录
***
- 创建一个新的孤立分支，并切换到该分支：`git checkout --orphan latest_branch`
- 删除原来的master分支：`git branch -D master`
- 重命名当前分支为master：`git branch -m master`
- 强制推送当前分支到远程仓库：`git push -f origin master`
- **该操作不可逆
***
#查看当前分支数量以及名称
git branch
#怎么上传一个.git所在文件中的文件夹里面的所有文件
**添加子文件夹及其内容至暂存区**： 
使用`git add`命令，配合递归通配符`**`来添加子文件夹及其内部的所有文件。假设子文件夹名为`subfolder`，执行：
git add subfolder/**
```

```c
//显示差异摘要（每个提交修改的文件及行数统计）：
git log --author="当前用户名" --stat
//以图形化方式展示分支历史
git log --author="当前用户名" --graph
//- 显示完整的提交消息：
git log --author="当前用户名" --pretty=format:"%h %s"


```

## git文件恢复
```c
//没有add的文件恢复，一个一个恢复
git checkout <文件名>

//有add，无commit
git checkout <commit id> 文件名
#id使用git log查看

//已经commit
git checkout <commit id> 文件名

```
![](https://raw.githubusercontent.com/etherealzoom/blog_img/main/Pic/Git_Flow.png)


知识点
1）
```c
f7cafda (HEAD -> master) 重新提交有关tc264开发的相关资料 
d01e0e1 提交有关tc264开发的相关资料 
前面的一串数字是什么意思
答：
前面的一串数字是哈希值，它是一个由 40 个十六进制字符组成的字符串，用来唯一标识每一次提交。Git 通过计算提交的内容、作者、日期等信息的 SHA-1 散列值来生成这个哈希值
```
2)
```c
(HEAD -> master)是什么
答：
(HEAD -> master)是Git中的一种表示方式，它表示HEAD指向master分支，而master分支指向最新的提交。HEAD是一个特殊的引用，它总是指向当前的提交，而master是一个普通的分支，它可以指向任何一个提交。当你在master分支上进行提交时，HEAD和master都会移动到最新的提交上(https://zhuanlan.zhihu.com/p/40001702)
[1]关于Git分支中HEAD和Master的理解(https://zhuanlan.zhihu.com/p/40001702)[: 关于Git分支中HEAD和Master的理解 - 知乎](https://www.jianshu.com/p/4219b6f62ce3)
```
3）解释git push -u origin master
```c
- git是一个分布式版本控制系统，可以用来管理和协作项目的源代码。
- push是一个git子命令，它的作用是将本地仓库的内容推送到远程仓库的指定分支。
- -u是一个参数，它的全称是–set-upstream，它的作用是设置默认的上游分支，也就是当你在本地仓库执行git pull或git push时，不需要指定远程仓库和分支名，git会自动使用你设置的上游分支。
- origin是一个远程仓库的名称，它是git默认给你添加的远程仓库的别名，你可以使用git remote命令来查看或修改你的远程仓库的名称。
- master是一个分支的名称，它是git默认给你创建的本地分支的名称，你可以使用git branch命令来查看或修改你的本地分支的名称。
```
4）上传文件到gitee
https://blog.csdn.net/fayoung3568/article/details/119488325

5）git pull and git pull --rebase
https://www.bilibili.com/video/BV1dH4y1g7tn/?vd_source=d7d8561214ca72d815e8ee788ea8c86b