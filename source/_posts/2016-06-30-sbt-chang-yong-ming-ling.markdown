---
layout: post
title: "sbt 常用命令"
date: 2016-06-30 09:19:27 +0800
comments: true
categories: sbt 命令
---
## sbt 命令 ##
#### 常用命令 ####
1. actions - 显示当前工程可用的命令
2. update - 下载依赖
3. compile - 编译代码
4. test - 运行测试代码
5. package - 创建一个可以发布的jar包
6. publish-local - 把构建出来的jar包包装到本地的ivy缓存
7. publish - 把jar包发布到远程仓库
8. run - 执行

#### 更多命令 ####
1. test-failed - 运行失败的spec
2. test-quick - 运行所有失败的以及/或者是由依赖更新的spec
3. clean-cache - 清除所有的sbt缓存，类似于sbt的clean命令
4. clean-lib - 删除lib_managed下的所有内容