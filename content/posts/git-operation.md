+++
title = "git常用操作"
date =  2014-04-30T10:41:35+08:00
draft = true
tags = ["shell","git"]
categories = ["git"]
+++
这里是一些自己经常用到的git命令

## git 全局配置
#### 配置全局user
```
git config --global user.name "Your Name"

```
  
#### 配置全局Email
```
git config --global user.email your.email@example.com 

```

#### 配置别名
```
git config --global alias.co checkout   
这里co 代替字数较多的 checkout 命令

```

#### 配置默认编辑器
```
git config --global core.editor "subl -w"  
这里设置的是sublime,
如果使用其他编辑器，请使用以下代码替换 subl -w：TextMate 用 mate -w，gVim 用 gvim -f，MacVim 用 mvim -f

```

## git 本地操作
  
#### 初始化项目
```
git init

```

#### 加入文件
```
git add

```
#### 加入所有文件 
```
git add --all

```

#### 提交变更
```
git commit -a  -m "comments"   
-a表示提交所有更改

```

#### 创建并且切换到一个分支
```
git checkout -b modify-README  
创建一个新分支modify-README，然后切换到这个分支 

```

#### 切换到分支
```
git checkout master   
切换到分支master

```

#### revert文件
```
git checkout -- file  
把文件file恢复到当前head版本

```

#### 撤销所有更改
```
git checkout -f  
-f 意思是覆盖当前的改动

```

#### 显示所有分支
```
git branch

```

#### 删除分支
```
git branch -d modify-README    
 删除分支modify-README

```
 
#### 删除所有分支
```
 git branch -D

```
 
#### 显示比较差异
```
git diff

```

#### 显示当前状态
```
git status

```

#### 显示日志
```
git log  
q退出

```

## git remote仓库操作

#### 加入远程仓库
```
git remote add origin git@github.com:username/first_app.git     
origin为自定义的远程仓库名称   
git@github.com:username/first_app.git 为远程git地址 

```

#### 提交远程仓库
```
git push origin master  
origin为远程仓库名称   
master为项目分支

```

#### 显示当前remote仓库列表
```
git remote 

```

#### 删除一个remote仓库列表
```
git remote remove name

```

#### 从远程仓库更新并且合并
```
git pull

```

#### 从远程仓库更新
```
git fetch

```
 

 