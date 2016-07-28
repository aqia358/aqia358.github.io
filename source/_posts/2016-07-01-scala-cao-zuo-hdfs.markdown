---
layout: post
title: "scala 操作 hdfs"
date: 2016-07-01 15:14:37 +0800
comments: true
categories: scala hdfs crud
---
### scala 操作HDFS ###
```
import org.apache.hadoop.conf.Configuration
import org.apache.hadoop.fs.{FileSystem, Path}
 

object Hdfs extends App{
 
  def write(uri: String, filePath: String, data: Array[Byte]) = {
    System.setProperty("HADOOP_USER_NAME", "ead")
    val conf = new Configuration()
    conf.set("fs.defaultFS", uri)
    val fs = FileSystem.get(conf)
    val path = new Path(filePath)
    val os = fs.create(path)
    os.write(data)
    fs.close()
  }
 
}
```
### usage ###
```
Hdfs.write("hdfs://host:8000", "{hdfs_path}", "hello".getBytes())
```