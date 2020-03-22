---
title: HashMap
date: 2019-09-08 23:26:34
tags: Java面试
categories: java
---

- HashMap（非线程安全）
  - Java8以前
    -  数组+链表实现 （默认数组长度为16）最坏情况O(1)->O(n)
  - Java8以后
    -  数组+链表+红黑树 O(lgn)

<!-- more -->

- 源码分析
  - 成员变量
    - 数据结构，树化阈值
  - 构造函数
    - 延迟创建
  - put方法
    - hashmap未被初始化 则初始化（延迟加载）
    - 对key求hash值
  - get方法 
  - hash方法
    - hashCode()
  - resize()
    - 默认负载因子0.75  
    - 扩容的问题
      - 多线程环境下， 调整大小会存在条件竞争，容易出现死锁
      - rehashing比较耗时
  
- 如何有效减少碰撞
  - 扰动函数
  - 使用final对象

