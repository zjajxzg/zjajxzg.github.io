---
title: RandomAccess接口
date: 2020-04-11 12:53:58
tags: java
categories: java
discription: java集合框架整理
---

#### RandomAccess有什么意义？

最近在看java集合框架源码，在看ArrayList的时候发现了RandomAccess接口，点进源码发现只是一个空的接口，那这个接口究竟有什么用呢？仔细看看接口上面的一大串注释，其实仔细看完可以发现官方已经对这个接口有了很详细的说明了。

##### 源码

```java
/**
 * Marker interface used by <tt>List</tt> implementations to indicate that
 * they support fast (generally constant time) random access.  The primary
 * purpose of this interface is to allow generic algorithms to alter their
 * behavior to provide good performance when applied to either random or
 * sequential access lists.
 *
 * <p>The best algorithms for manipulating random access lists (such as
 * <tt>ArrayList</tt>) can produce quadratic behavior when applied to
 * sequential access lists (such as <tt>LinkedList</tt>).  Generic list
 * algorithms are encouraged to check whether the given list is an
 * <tt>instanceof</tt> this interface before applying an algorithm that would
 * provide poor performance if it were applied to a sequential access list,
 * and to alter their behavior if necessary to guarantee acceptable
 * performance.
 *
 * <p>It is recognized that the distinction between random and sequential
 * access is often fuzzy.  For example, some <tt>List</tt> implementations
 * provide asymptotically linear access times if they get huge, but constant
 * access times in practice.  Such a <tt>List</tt> implementation
 * should generally implement this interface.  As a rule of thumb, a
 * <tt>List</tt> implementation should implement this interface if,
 * for typical instances of the class, this loop:
 * <pre>
 *     for (int i=0, n=list.size(); i &lt; n; i++)
 *         list.get(i);
 * </pre>
 * runs faster than this loop:
 * <pre>
 *     for (Iterator i=list.iterator(); i.hasNext(); )
 *         i.next();
 * </pre>
 *
 * <p>This interface is a member of the
 * <a href="{@docRoot}/../technotes/guides/collections/index.html">
 * Java Collections Framework</a>.
 *
 * @since 1.4
 */
public interface RandomAccess {
}

```

---

- 个人理解

简单来说就是对List的一种标记，如果实现了RandomAccess则表明这个List支持快速随机访问，即通过for循环来遍历list会比通过迭代器来遍历来的更快。

##### 简单测试

以下是对ArrayList和LinkedList通过for循环和迭代器两种方式进行遍历的简单测试代码

```java
public class RandomAccessTest {
    public static void main(String[] args) {
        List<String> arrayList = new ArrayList<>();
        List<String> linkedList = new LinkedList<>();
        for (int i = 0; i < 10000; i++) {
            arrayList.add("" + i);
            linkedList.add("" + i);
        }

        RandomAccessTest randomAccessTest = new RandomAccessTest();
        System.out.println("ArrayList:");
        randomAccessTest.traverseListByLoop(arrayList);
        randomAccessTest.traverseListByIterator(arrayList);
        System.out.println("LinkedList:");
        randomAccessTest.traverseListByLoop(linkedList);
        randomAccessTest.traverseListByIterator(linkedList);

    }

    private void traverseListByLoop(List list) {
        long startTime = System.currentTimeMillis();
        for (int i = 0; i < list.size(); i++) {
            list.get(i);
        }
        long endTime = System.currentTimeMillis();
        System.out.println("for: " + (endTime - startTime) + "ms");
    }

    private void traverseListByIterator(List list) {
        Iterator iterator = list.iterator();
        long startTime = System.currentTimeMillis();
        while (iterator.hasNext()) {
            iterator.next();
        }
        long endTime = System.currentTimeMillis();
        System.out.println("迭代器: " +(endTime - startTime) + "ms");
    }
}

```

**测试结果**

ArrayList:
for: 2ms
迭代器: 1ms
LinkedList:
for: 328ms
迭代器: 1ms

##### 结论

通过测试结果还是可以很直观看到差距的   所以通过RandomAccess我们在遍历List的时候可以通过如下代码进行优化

```java
private void traverseList(List list) {
        Iterator iterator = list.iterator();
        long startTime = System.currentTimeMillis();
        // 如果支持快速随机访问  使用for循环遍历
        if (list instanceof RandomAccess) {
            for (int i = 0; i < list.size(); i++) {
                list.get(i);
            }
            long endTime = System.currentTimeMillis();
            System.out.println("RandomAccess: " + (endTime - startTime) + "ms");
        } else {
            while (iterator.hasNext()) {
                iterator.next();
            }
            long endTime = System.currentTimeMillis();
            System.out.println("迭代器: " +(endTime - startTime) + "ms");
        }
    }
```

当然这只是从RandomAccess接口的作用来思考，置于为什么ArrayList支持快速随机访问，LinkedList不支持那就要看这两者的底层实现来看了（数组和链表的差异）

多了解原理可以给我们提供更多的思路定位问题，优化代码。