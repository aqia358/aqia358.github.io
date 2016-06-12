---
layout: post
title: "mysql自增主键不连续导致的storm重复消费问题"
date: 2016-06-08 08:52:35 +0800
comments: true
categories: storm mysql
---
storm TransactionalSpout使用mysql作为数据源，数据实时的插入到mysql 数据表中，表中的id字段为自增。spout每次从当前id读取id+a个的数据，next batch的起点为id+a.
当用两个进程去写mysql时，会发现有数据被消费了多次。
这是由于mysql自增主键不连续导致的，mysql 5.1.22后默认主键的自增策略为innodb_autoinc_lock_mode=1,这种模式下每次会“预申请”多余的id(handler.cc:compute_next_insert_id)。这个预留的策略是“不够时多申请几个”， 实际执行中是分步申请。至于申请几个，是由当时“已经插入了几条数据N”决定的。当auto_increment_offset＝1时，预申请的个数是 N-1。
这样做的好处是：一次性分配足够的auto_increment id，只会将整个分配的过程锁住。但却可能会导致自增id的不连续。