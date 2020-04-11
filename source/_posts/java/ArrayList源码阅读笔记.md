---
title: ArrayList源码笔记
date: 2020-04-11 12:53:58
tags: java
categories: java
discription: java集合框架整理
---

#### ArrayList简介

> ArrayList是一个以数组实现的List，可以简单的理解为一个动态数组，相比于数组可以动态扩容。

#### 类图

![](https://raw.githubusercontent.com/zjajxzg/figure_bed_public/master/github_img/arraylist_uml.png)

从类图我们可以看到这些信息：

1. 直接继承了AbstractList，实现了List，表明拥有一个集合的基本操作，增加、删除、修改、遍历等等。
2. 实现了Serializable接口，表明ArrayList支持序列化。
3. 实现了Cloneable接口，表明ArrayList可以被克隆。
4. 实现了RandomAccess接口，表明ArrayList支持快速随机访问，关于这个接口的简单介绍有另一篇文章。

#### 源码解析

##### 属性

```java
    /**
     * 默认初始容量 数组的长度
     */
    private static final int DEFAULT_CAPACITY = 10;

    /**
     * 空数组
     */
    private static final Object[] EMPTY_ELEMENTDATA = {};

    /**
     * 空数组，传入容量时使用，添加第一个元素的时候会重新初始为默认容量大小
     */
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

    /**
     * 存储元素的数组 不参与
     */
    transient Object[] elementData;

    /**
     * 元素个数
     */
    private int size;
```



##### 构造方法

- 无参构造方法

```java
    /**
     * 无参构造方法 使用DEFAULTCAPACITY_EMPTY_ELEMENTDATA
     */
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }
```

- 参数为容量大小（即数组长度）的构造方法

```java
public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
            // 如果容量>0  创建一个长度为容量大小的数组
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            // 如果=0 使用空数组EMPTY_ELEMENTDATA
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            // 如果<0 抛出异常
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        }
    }
```

- 参数为集合的构造方法

```java
public ArrayList(Collection<? extends E> c) {
    	// 将集合转成数组
        elementData = c.toArray();
        if ((size = elementData.length) != 0) {
            // 如果不是Object数组，重新拷贝成Object[]类型
            if (elementData.getClass() != Object[].class)
                elementData = Arrays.copyOf(elementData, size, Object[].class);
        } else {
            // 如果集合的长度=0，则初始化为空数组
            this.elementData = EMPTY_ELEMENTDATA;
        }
    }
```



##### 核心方法

- add(E e) 添加元素至末尾 平均时间复杂度为O(1)

```java
// 添加元素至末尾
public boolean add(E e) {
        // 检查是否需要扩容 当前元素个数+1
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        // 将元素添加到末尾
        elementData[size++] = e;
        return true;
    }


private void ensureCapacityInternal(int minCapacity) {
    ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
}

// 计算容量
private static int calculateCapacity(Object[] elementData, int minCapacity) {
    // 如果是空数组 返回默认容量10和需要扩容容量比较大的值 这里必然是初始容量 minCapacity = 1
    if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
        return Math.max(DEFAULT_CAPACITY, minCapacity);
    }
    // 否则返回size+1
    return minCapacity;
}

private void ensureExplicitCapacity(int minCapacity) {
    // 记录修改次数
    modCount++;

    // overflow-conscious code
    if (minCapacity - elementData.length > 0)
        grow(minCapacity);
}

private void grow(int minCapacity) {
    // overflow-conscious code
    // 有溢出意识的代码
    // 扩容之前的容量 = 数组的长度
    int oldCapacity = elementData.length;
    // 新的容量 = 原容量 + 原容量右移一位（即原容量/2 位运算效率更高） 即扩容1.5倍
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    // 如果扩容1.5倍之后还是不够 则直接将容量变为所需的容量
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
    // 如果扩容之后的容量>最大容量限制（Integer.MAX_VALUE - 8 某些虚拟机可能会保留 header words）
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    // minCapacity is usually close to size, so this is a win:
    // 以新容量拷贝出一个新的数组
    elementData = Arrays.copyOf(elementData, newCapacity);
}

private static int hugeCapacity(int minCapacity) {
    // 溢出	
    if (minCapacity < 0) // overflow
        throw new OutOfMemoryError();
    return (minCapacity > MAX_ARRAY_SIZE) ?
        Integer.MAX_VALUE :
    MAX_ARRAY_SIZE;
}
```



- 待续。。。