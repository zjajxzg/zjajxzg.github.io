---
title: java面试准备
date: 2019-09-15 22:40:20
tags: Java面试
categories: 面试
---

### 面试准备

------

### 一、计算机网络相关

- OSI七层模型（概念、参考模型）
  - 物理层
  - 数据链路层
  - 网络层
    - IP协议
  - 传输层
    - TCP协议
    - UDP协议
  - 会话层
  - 表示层
  - 应用层
- TCP/IP概念层模型（4层，OSI具体实现）
  - 应用层
  - 传输层
  - 网络层
  - 网络接口层（数据链路层和物理层）

### 二、数据库

##### 主要考点

设计一个关系型数据库

- 存储数据（类似文件系统）  机械硬盘  SSD
- 程序实例 
  - 存储管理
  - 缓存机制
  - SQL解析
  - 日志管理
  - 权限划分
  - 容灾机制
  - 索引管理
  - 锁管理
- 架构
- 索引
  - 为什么要使用索引？
    - 快速查询数据
  - 什么样的信息能成为索引？
    - 主键
    - 唯一键
    - 普通键
  - 索引的数据结构
    - 生成索引，建立二叉查找树进行二分查找
      - O（logn）
      - 缺点 每个节点的容量太小  io瓶颈
    - B-Tree
      - 定义
        - 根节点至少包括两个孩子
        - 树中每个节点最多包含m个孩子
        - 除根节点和叶节点外，其他每个节点至少有ceil(m/2)个孩子
        - 所有叶子节点都位于同一层
    - B+-Tree
    - Hash
- 锁
- 语法
- 理论范式

### 三、redis

- 缓存中间件 Memcache和Redis的区别
  - Memcache:
    - 代码层次类似hash
    - 支持简单数据类型
    - 不支持数据持久化存储
    - 不支持主从
    - 不支持分片
  - redis
    - 数据类型分布
    - 支持吃数据磁盘持久化存储
    - 支持主从
    - 支持分片
- 为什么redis这么快  100000+QPS（每秒查询次数）
  - 完全基于内存
  - 数据结构简单 对数据操作简单
  - 采用单线程
  - 使用多路I/O复用 非阻塞
- 多路I/O复用模型
- Redis数据类型
  - String： 最基本的数据类型 二进制安全     sds
  - Hash：String元素组成的字典 适合用于存储对象
  - List：列表，按照String元素插入顺序排序
  - Set：String元素组成的无序集合，通过哈希表实现，不允许重复
  - Sorted Set：通过分数来为集合中的成员进行从小到大的排序
  - 用于计数的HyperLogLog   用于支持存储的地理位置信息的Geo
- 底层数据类型基础：
  - 简单动态字符串
  - 链表
  - 字典
  - 跳跃表
  - 整数集合
  - 压缩列表
  - 对象
- 海量key里查询某一固定前缀的key（先问数据量）
  - keys pattern
    - keys指令一次性返回所有匹配的key
    - 键的数量过大会使服务卡顿
  - scan cursor [match pattern] [count count]
    - 基于游标的迭代器  需要基于上一次的游标延续之前的迭代过程
    - 以0作为游标开始一次新的迭代，知道命令返回游标0完成一次遍历
    - 不保证每次执行都返回某个给定数量的元素，支持模糊查询
    - 一次返回的数量不可控，只能是大概率符合count参数
- 如何实现分布式锁
- 消息队列
- 持久化
  - RDB（快照）无法保存最近一次快照之后的数据
    - redis.conf
    - save    阻塞
    - bgsave     fork()子进程
  - AOF（指令）
  - RDB-AOF混合持久化方式
    - BGSAVE做镜像全量持久化 AOF做增量持久化
  - redis数据恢复
- Pipeline（管道）
  - 与Linux的管道类似
  - Pipelibe批量执行指令  节省io
- Redis的同步机制
  - 主从同步原理
  - Redis Sentinel  解决主从同步Master宕机后的主从切换问题
  - 流言协议Gossip
- Redis集群
  - 如何从海量数据里快速找到所需
    - 分片
    - 一致性hash算法  对2^32取模

### 五、Java底层知识

- 谈谈你对Java的理解
  - 平台无关性（一次编译 到处运行）
    - Java源码先被编译成字节码（.java -> javac -> .class），再由不同平台的JVM进行解析，Java在不同平台上运行时不需要重新进行编译，Java虚拟机在执行字节码的时候，把字节码转换成具体平台上的机器指令
    - 为什么要先编译成字节码再解析成机器码？
      - 准备工作：每次执行都需要各种检查
      - 兼容性：也可以将别的语言解析成字节码
  - GC
  - 语言特性
  - 面向对象
  - 类库
  - 异常处理

### 七、Java线程知识

- 进程和线程的区别
- Java进程和线程的关系
  - Java对操作系统提供的功能进行封装，包括进程和线程
  - 运行一个程序会产生一个进程，进程包含至少一个线程
  - 每个进程对应一个Jvm实例，多个线程共享JVM里的堆
  - Java采用单线程编程模型，程序会自动创建主线程
  - 主线程可以创建子线程，原则上要后于子线程完成执行
- thread中start和run方法的区别
  - start()会创建一个新的子线程并启动
  - run方法只是Thread的一个普通方法的调用
- Thread和Runnable的关系
  - Thread是实现了Runnable接口的类，使得run支持多线程
  - 因类的单一继承原则，推荐多使用Runnable接口
- 如何给run()传参
  - 构造函数传参
  - 成员变量传参
  - 回调函数传参
- 如何处理线程的返回值
  - 主线程等待法
  - 使用Thread的join()阻塞当前线程以等待子线程处理完毕
  - 通过Callable接口实现：通过FutureTask(isDone() get()) or 线程池获取
- 线程的状态
  - NEW  新建 创建后尚未启动的线程的状态
  - Runnale 饱和Running和Ready
  - WAITING 无线期等待 不会被分配CPU执行时间 需要显示被唤醒 
    - 没有设置Timeout参数的Object.wait()方法
    - 没有设置Timeout参数的Thread.join()方法
    - LockSupport.park()方法
  - Timed waiting  限期等待 在 一定时间后会由系统自动唤醒
    - Thread.sleep()方法
    - 设置了Timeout参数的Object.wait()方法
    - 设置了Timeout参数的Thread.join()方法
    - LockSupport.parkNanos()方法
    - LockSupport.parkUntil()方法
  - Blocked 阻塞 等待获取排他锁
  - Terminated 结束状态 已终止线程的状态 线程已经执行结束
- sleep()和wait()的区别
  - sleep是Thread类的方法    wait 是Object类中定义的方法   都是native方法
  - sleep方法可以在任何地方使用
  - wait只能在synchronized方法或者synchronized块中使用
  - 本质区别 sleep只会让出cpu 不会导致锁行为的改变 wait不仅会让出cpu 还会释放已经占有的同步资源锁
- notify()和notifyall()的区别
  - 锁池    等待锁释放的线程
  - 等待池 wait 释放锁    不会去竞争锁  
  - notifyAll会让所有属于等待池的线程全部进入锁池去竞争获取锁的机会
  - notify随机选取一个处于等待池中的线程进入锁池去竞争获取锁的机会
- yield()
  - 当调用yield()函数时，会给线程调度器一个当前线程愿意让出CPU使用的暗示，但是线程调度器可能会忽略这个暗示    不会让线程让出锁
- interrupt()
  - stop() 中断线程   已抛弃
  - 通知线程应该中断了
    - 如果线程处于被阻塞状态 那么线程将立即退出被阻塞状态 并抛出一个InterruptedException
    - 如果线程处于正常活动状态 那么会将该线程的中断标志设置为true 被设置中断标志的线程将继续正常运行 不受影响
  - 需要被调用的线程配合中断
    - 在正常运行任务时，经常检查本线程中断标志位，如果被设置了中断标志 就自行停止线程
    - 如果线程处于正常活动状态，那么会将该线程的中断标志设置为true
- 线程状态以及状态之间的转换（补张图）

### 八、Java多线程与并发

- synchronized
  - 线程安全问题的主要诱因
    - 存在共享数据（也称临界资源）
    - 存在多条线程共同操作这些共享数据
    - 解决问题的根本方法： 同一时刻有且只有一个线程在操作共享数据，其他线程必须等到该线程处理完数据后再对共享数据进行操作
  - 互斥锁的特性
    - 互斥性
    - 可见性
  - 根据获取的锁的分类：获取对象锁和获取类锁
- synchronized原理
  - Java对象头
    - 对象在内存中的布局（hotspot虚拟机）   对象头    实例数据    对齐填充
  - Monitor
    - 内部锁（管程）
- synchronized和ReentrantLock（再入锁）
- JMM
  - Java内存模型  抽象概念 描述的时一组规则或规范，通过这组规范定义了程序中各个变量（包括实例字段，静态字段和构成数组对象的元素）的访问方式
  - 主内存
    - Java实例对象
    - 成员变量
  - 工作内存
    - 当前方法的所有本地变量信息 本地变量对其他线程不可见s
    - 字节码



### 十、Spring

- IOC(控制反转)  思想   基于依赖倒置原则
  - spring core 核心
  - DI(依赖注入)    把底层类作为参数传递给上层类，实现上层对下层的控制
    - setter注入
    - 接口注入
    - 注解注入
    - 构造器注入
  - DL 依赖查找 （有侵入性  已被抛弃）
- IOC容器的优势
  - 避免在各处使用new来创建类，并且可以做到统一维护
  - 创建实例时不需要了解具体细节
- BeanFactory与ApplicationContext
  - BeanFactory是Spring框架的基础设施，面向spring
  - ApplicationContext面向使用Spring框架的开发者
- AOP
  - pointcut
  - before
  - around
  - after
  - afterreturning
  - afterThrowing
- AOP原理
  - jdkProxy和Cglib

### 十一、消息队列（RabbitMq、kafka。。。）

- 消息队列的作用
  - 异步化
  - 削峰
  - 解耦      

