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

    //把所有内容添加到索引库中
    $git add .

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
