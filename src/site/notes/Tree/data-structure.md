---
{"dg-publish":true,"dg-path":"  data-structure.md","permalink":"/data-structure/","tags":["CS/algorithms"],"created":"2022-09-11T16:45:18.887+08:00","updated":"2023-08-27T04:58:02.245+08:00"}
---


# List 


## LinkedList

单链表的节点结构
```java
Class Node <V> {
	V value;
	Node next;
}
```
由以上结构的节点依次连接起来所形成的链叫单链表结构

双链表的节点结构
```java
Class Node <V>{
	V value;
	Node next;
	Node last;
}
```
由以上结构的节点依次连接起来所形成的链叫双链表结构

**单链表和双链表结构只需要给定一个头部节点head,就可以找到剩下的所有的节点**

---


面试时链表解题的方法论
1. 对于笔试，不用太在乎空间复杂度，一切为了时间复杂度
2. 对于面试，时间复杂度依然放在第一位，但是一定要找到空间最省的方法

重要技巧：

1. 额外数据结构记录（哈希表等）
2. 快慢指针 


# Hash 

## HashMap & HashSet 
哈希表的简单介绍

1. 哈希表在使用层面上可以理解为一种集合结构
2. 如果只有key,没有伴随数据value,可以使用HashSet结构(C++中叫UnOrderedSet)
3. 如果既有key,又有伴随数据value,可以使用HashMap结构(C+中叫UnOrderedMap)
4. 有无伴随数据，是HashMap和HashSet唯一的区别，底层的实际结构是一回事
5. 使用哈希表增(put)、删(remove)、改(put)和查(get)的操作，可以认为时间复杂度为O(1),但是常数时间比较大
6. 放入哈希表的东西，如果是基础类型，内部按值传递，内存占用就是这个东西的大小
7. 放入哈希表的东西，如果不是基础类型，内部按引用传递，内存占用是这个东西内存地址的大小

# Tree

## TreeMap & TreeSet 

有序表的简单介绍
1. 有序表在使用层面上可以理解为一种集合结构
2. 如果只有key,没有伴随数据value,可以使用TreeSet结构(C++中叫0rderedSet)
3. 如果既有key,又有伴随数据value,可以使用TreeMap:结构(C+中叫OrderedMap)
4. 有无伴随数据，是TreeSet和TreeMap唯一的区别，底层的实际结构是一回事
5. 有序表和哈希表的区别是，有序表把ky按照顺序组织起来，而哈希表完全不组织
6. 红黑树、AVL树、size-balance-tree和跳表等都属于有序表结构，只是底层具体实现不同
7. 放入有序表的东西，如果是基础类型，内部按值传递，内存占用就是这个东西的大小
8. 放入有序表的东西，如果不是基础类型，必须提供比较器，内部按引用传递，内存占用是这个东西内存地址的大小
9. 不管是什么底层具体实现，只要是有序表，都有以下固定的基本功能和固定的时间复杂度  

---

有序表的固定操作
1. void put(K key,V value):将一个(key,value)记录加入到表中，或者将key的记录更新成value.
2. V get(K key):根据给定的key,查询value并返回。
3. void remove(Kkey):移除key的记录。
4. boolean containsKey(Kkey):询问是否有关于key的记录。
5. K firstKey():返回所有键值的排序结果中，最左（最小）的那个
6. K lastKey():返回所有键值的排序结果中，最右（最大）的那个。
7. K floorKey(Kkey):如果表中存入过key,返回key;否则返回所有键值的排序结果中，key的前一个。
8. K ceilingKey(Kkey):如果表中存入过key,返回key;否则返回所有键值的排序结果中，key的后一个。

以上所有操作时间复杂度都是0(IogN),N为有序表含有的记录数

# Heap 
1. 堆结构就是用数组实现的完全二叉树结构
2. 完全二叉树中如果每棵子树的最大值都在顶部就是大根堆
3. 完全二叉树中如果每棵子树的最小值都在顶部就是小根堆
4. 堆结构的heapInsert与heapify操作
5. 堆结构的增大和减少
6. 优先级队列结构，就是堆结构
