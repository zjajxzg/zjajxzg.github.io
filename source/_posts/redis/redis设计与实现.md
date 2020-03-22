---
title: redis设计与实现-数据结构与对象
date: 2020-03-14 18:11:21
categories: 数据库
tags: redis
---

#### 第一部分-数据结构与对象

##### 一、简单动态字符串（SDS）

```c
struct sdshde {
	// 记录buf数组中已经使用字节的数量
    // 等于SDS所保存字符串的长度
    int len;
    // 记录buf数组中未使用字节的数量
    int free;
    // 字节数组，用于保存字符串
    char buf[];

}
```

<!-- more -->

相比与C字符串的优势：

- 常数复杂度获取字符串长度
- 杜绝缓冲区溢出
- 减少修改字符串时带来的内存重分配次数
  - 空间预分配（len小于1m: free=len，len>1m: free=1m）
  - 惰性空间释放
- 二进制安全（使用len来判断字符串结束，而不是结尾的空字符串）
- 兼容部分C字符串函数

##### 二、链表

> 每个链表节点使用一个adlist.h/listNode结构来表示

```c
typedef struct listNode {
	// 前置节点
	struct listNode *prev;
	// 后置节点
	struct listNode *next;
	// 节点的值
	void *value;
} listNode;
```

> 多个listNode可以组成链表

```c
typedef struct list {
	// 表头节点
	listNode *head;
	// 表尾节点
	listNode *tail;
	// 链表所包含的节点数量
	unsigned long len;
	// 节点值复制函数
	void *(*dup) (void *ptr);
	// 节点值释放函数
	void (*free) (void *ptr);
	// 节点值对比函数
	void (*match) (void *ptr, void *key);
} list;
```

**redis的链表实现的特性可以总结如下：**

1. 双端：链表节点带有prev和next指针，获取某个节点的前置节点和后置节点的复杂度都是O(1)。
2. 无环：表头节点的prev指针和表尾节点的next指针都指向null，对链表的访问以null为终点。
3. 带表头指针和表尾指针：通过list结构的head指针和tail指针，程序获取链表的表头节点和表尾节点的复杂度为O(1)。
4. 带链表长度计数器：程序使用list结构的len属性来对list特有的链表节点进行计数，程序获取链表中节点数量的复杂度为O(1)。
5. 多态：链表节点使用void*指针来保存节点值，并且可以通过list结构的dup、free、match三个属性为节点值设置类型特定函数，所以链表可以用于保存各种不同类型的值。

##### 三、字典

> 字典的实现：redis的字典使用哈希表作为底层实现，一个哈希表里面可以有多个哈希表节点，而每个哈希表节点就保存了字典中的一个键值对