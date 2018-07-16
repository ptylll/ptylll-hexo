---
title: git常用命令
date: 2018-04-08 11:21:30
tags: git
---

git 提交仓库常用命令总结

<!--more-->
```
    //初始化目录
    git init 

    //把所有内容添加到索引库中（将目录中所有文件添加）
    $git add ./* 

    //同步线上库到本地（新添加项目）
    $git pull --rebase origin master

    //提交到本地仓库，分号中是针对要提交文件的描述
    $git commit -m "随便写"

    //如果远程origin已经存在，先删除远程origin，或者改用别的名字
    $git remote rm origin

    //关联远程仓库git
    $git remote add origin git@github.com:名字/库名.git

    //提交本地仓库到远程仓库主干中
    $git push -u origin master

```

```
 $git remote -v //查看远程仓库

 $git fetch origin masterFrom https://github.com/ptylll/ptylll-hexo.git //更新仓库

 $git merge origin/master //远程仓库合并到本地

 
git remote add origin git@github.com:ptylll/ptylll-hexo.git//本地git仓库关联GitHub仓库 

```
```

// 创建分支（创建分支时名字不能与原有分支名冲突）
$git branch newBranch 

// 查看全部分支
$git branch -a 

// 切换分支
$git checkout newbranch

// 切回master主分支
$git checkout master

```
