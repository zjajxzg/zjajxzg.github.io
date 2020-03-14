---
title: Java异常
date: 2019-09-08 22:25:30
tags: 异常
categories: java
---

### Java异常

- Java异常层次结构图

  ![](https://raw.githubusercontent.com/zjajxzg/figure_bed_public/master/github_img/1354020417_5176.jpg)

- 由图可以看出在Java中所有异常均继承自Throwable（可以抛出），Throawble下有两个重要的子类，Exception和Error
- Error（程序无法处理的错误）
  
  - 一般是Jvm级别的问题，比如OOM，StackOverflow等
- Exception（程序本身可以处理的异常）
  - RuntimeException
  - 受检异常
- 处理异常机制
  - 抛出异常
    - throw new Exception
  - 捕获异常
    - try...catch...finally...   捕获异常前必有异常的抛出   try中是可以出现异常的代码块   catch捕获可能出现的各种异常  finally
- 参考链接
  
  - <https://blog.csdn.net/hguisu/article/details/6155636>