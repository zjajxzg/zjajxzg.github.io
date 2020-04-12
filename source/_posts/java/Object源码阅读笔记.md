---
title: Object源码笔记
date: 2020-04-12 16:53:58
tags: java
categories: java
discription: java集合框架整理
---

#### Object

#### 前言

- 去年面试的时候曾被问到过一个问题，说说Object类中有哪些方法，一时很懵逼，想想很是惭愧，写了这么久java，连java最基本的父类都不了解，有点像写代码的机器，木有感情，趁着最近阅读java源码的时候整理一下这个Java中的顶级父类，不涉及底层实现，只是通过源码的注释来了解一下，java的一些规范。

#### Object的方法

- **getClass()** 获取对象的运行时类  该方法不能被重写

```java
public final native Class<?> getClass();
```

- **hashCode()** 获取对象的散列值  之前一直以为hashcode是通过内存地址来计算的 但是源码注释中明确表示Java不是通过内存地址来实现的  看了网上的博客java8默认采用的是结合当前线程和xorshift算法实现的，有时间再去考证一下。

> http://www.majiang.life/blog/deep-dive-on-java-hashcode/

```java
 /**
  * <p>
  * As much as is reasonably practical, the hashCode method defined by
  * class {@code Object} does return distinct integers for distinct
  * objects. (This is typically implemented by converting the internal
  * address of the object into an integer, but this implementation
  * technique is not required by the
  * Java&trade; programming language.)
  */
public native int hashCode();
```

- **equals()** 比较两个对象是否相等  从源码可以看出不重写equals默认比较的是对象的hashCode值 注释中明确说明了equals的实现要求
  - 自反性。对任意x，x.equals(x)一定返回true。
  - 对称性。对任意x和y，如果y.equals(x)返回true，则x.equals(y)也返回true。
  - 传递性。对任意的x、y、z，如果x.equals(y)返回true，y.equals(z)返回true，那么x.equals(z)也一定返回true
  - 一致性。对任意x和y，如果对象中用于等价比较的信息没有改变，那么无论调用x.equals(y)多少次，返回的结果应该保持一致，要么一直是true，要么一直是false。
  - 对任何不是null的x，x.equals(null)一定返回false。

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

- **clone()**

```java
protected native Object clone() throws CloneNotSupportedException;
```

- **toString()** 类名@hashcode的十六进制     推荐我们重写这个方法来表示有意义的内容

```java
public String toString() {
    return getClass().getName() + "@" + Integer.toHexString(hashCode());
}

```

- **notify()**

```java
public final native void notify();
```

- **notifyAll()**

```java
public final native void notifyAll();
```

- **wait()**

```java
public final native void wait(long timeout) throws InterruptedException;
```

- **finalize()**

```java
protected void finalize() throws Throwable { }
```





未完待续。。。等看JUC的时候再来补充线程相关的几个方法。