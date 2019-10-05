---
layout:     post
title:      "【Java核心技术卷I笔记】第9章 集合"
date:       2019-10-5 15:44:04
author:     "dxc"
header-img: "img/post-bg-malaban.jpg"
tags:
    - Java
---
 
**1、 LinkedList：**  
LinkedList可以使用ListIterator类从前后两个方向遍历链表中的元素，可以添加、删除元素。  
linkedList.get(n)的效率极低。  
```java
for(int i = 0; i < n; i++)
    do someting with list.get(i)    //效率低到不行  
```
get做了微小的优化，如果索引大于size()/2，就从列表的尾端开始搜索。  
总结：当需要插入删除的时候用LinkedList，当需要随机访问的时候用ArrayList。  

**2、 ArrayList：**  
Vector类的所有方法都是同步的，可以由两个线程安全地访问一个Vector对象。但是，由一个线程访问Vector，代码要在同步操作上消耗大量时间。而ArrayList不是同步的。因此，在不需要同步时使用ArrayList，需要时使用Vector。  

**3、 TreeSet：**  
要使用树集，必须能够比较元素，这些元素必须实现Comparable接口。  

**4、 算法：**  
```java
//排序
List<String> staff = new LinkedList<>():
Collections.sort(staff);
staff.sort(Comparator.reverseOrder());
```
资料：<https://www.cnblogs.com/yoyohong/p/7644650.html>