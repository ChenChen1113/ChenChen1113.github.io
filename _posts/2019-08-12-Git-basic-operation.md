---
layout:     post
title:      "Git基本操作 to xl"
date:       2019-8-12 19:10:58
author:     "dxc"
header-img: "img/post-bg-malaban.jpg"
tags:
    - Git
---


### ❤ 随手命令 ###
``` shell
git status    //可以查看分支、查看文件状态，这里会显示跟踪的文件、已add未commit的文件、已commit的文件等
```
### ❤ 克隆仓库 ###
``` shell
git clone [url]
```
如：
``` shell
git clone https://code.aliyun.com/623084761/adaptive-loadbalance.git
```

### ❤ 日常提交 ###
``` shell
git add [filename]    //跟踪or添加修改的文件暂存区
git rm [filename]    //将文件删除并将这一操作放到暂存区
git commit -m "commit message"    //将暂存区的东西提交并附带提交信息。若使用git commit则进入vim界面编辑提交信息
git push [remote name] [branch name]    //将提交推送到远端的某分支
```
如：
```shell
git add ruleEngine.py	//git add --all 添加所有
git commit -m "实现xxx"
git push origin master  
```
### ❤ 若想一并恢复某文件的修改 （未暂存） ###
``` shell
git checkout [filename]
```
### ❤ 若add错了文件  （已暂存未提交） ###
``` shell
git reset [filename]   
```
### ❤ 查看提交日志 ###
``` shell
git log
```
### ❤ 如果多人同时维护这个库###
需要实时拉取别人的修改
```shell
git pull 拉取+合并    //不建议使用，自动合并还是有可能出错的
```
建议使用：
```shell
1、git fetch    //拉取
2、打开代码手动merge
3、执行“日常提交”的步骤
```
### ❤ 分支的作用###
来了一个新需求--建一个分支专门完成这个需求（主分支上该干啥干啥）--测试好了之后合并到主分支上mentor微软的未来pl排了一个项目中的10个活--建10个不同的分支
### ❤ 新建分支###
``` shell
git checkout -b [branch name]
```
### ❤ 切换分支###
``` shell
git checkout [branch name]    //切换的时候可能会提示把你原本分支上的东西保存了
```
### ❤ 合并分支###
写完需求1了，需要合并到主分支上
```shell
git checkout master    //先切到主分支上git merge xuqiu1   
```
合并然后会提示一些冲突，手动修改，然后执行“日常提交”的步骤冲突在代码里长这样:
```shell
<<<<<<< HEAD
某段代码版本1
=======
某段代码版本2
>>>>>>> xuqiu1
```
删除一个版本，然后删除<<<===符号
### ❤ 删除分支###
合并完了之后没用了
```shell
git branch -d xuqiu1
```
### ❤ 变基、疯狂变基###
超出我的能力范围，详见带图的文档:
```shell
https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%8F%98%E5%9F%BA
```