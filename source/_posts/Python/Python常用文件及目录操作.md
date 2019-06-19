---
title: Python常用文件及目录操作
categories: Python
tags: Python
icon: code
---

## 1 目录操作
1. 遍历文件夹：`os.listdir()`
2. 得到当前工作目录：`os.getcwd()`
3. 新建文件夹：`os.makedirs()`


## 2 删除相关
1. 删除文件：`os.remove()`
2. 删除空文件夹：`os.removedirs()`
3. 删除空/非空文件夹：`shutil.rmtree()`

## 3 检验相关
1. 检验路径是否为文件：`os.path.isfile()`
2. 检验路径是否为文件夹：`os.path.isdir()`
3. 检验路径是否为快捷方式：`os.path.islink()`
4. 检验路径是否存在：`os.path.exists()`

## 4 分离目录、文件相关
1. 分离路径的目录名和文件名：`os.path.split()`
2. 分离扩展名：`os.path.splitext()`。注意：分离出的扩展名带`.`
3. 分离扩展名2：`path.split('.')`。分离出的扩展名不带`.`

## 5 重命名
1. 重命名文件/文件夹：`os.rename(old, new)`

## 6 复制相关
1. 复制文件：`shutil.copyfile(old, new)`。注意：old和new必须是文件
2. 复制文件：`shutil.copy(old, new)`。注意：old必须是文件，new可以是文件/文件夹。如果是文件夹，执行的操作是复制文件到指定文件夹
3. 复制文件夹：`shutil.copytree(olddir, newdir)`。注意：olddir和newdir都必须是文件夹，且newdir必须不存在

## 7 移动相关
1. 移动文件/文件夹：`shutil.move(old, new)`
件夹：`shutil.move(old, new)`
