---
title: Collection集合
tags:
  - java/collection
---

![[Pasted image 20250428162308.png]]


![[Pasted image 20250428162357.png]]


Collection是所有集合的父类，所以里面有一些共性方法

## 集合存储数据类型的特点

![[Pasted image 20250510194245.png]]

### 基础数据类型包装类

需要在集合中用到基本类的时候可以转换为对应包装类

![[Pasted image 20250510194350.png]]

## 迭代器

在没有索引可以使用的时候就需要迭代器

![[Pasted image 20250428162757.png]]



### 利用增强FOR遍历集合中的元素


```
for (String item : list) {
    System.out.println(item);
}
// 等价于：
Iterator<String> it = list.iterator();
while (it.hasNext()) {
    String item = it.next();
    System.out.println(item);
}
```
这个就是增强for循环，E代表从集合中取出元素，collection是需要遍历的集合

**需要注意的是，能够使用增强for进行遍历，需要该集合能够返回迭代器，有Iterator方法就可以进行返回**

如果是自定义的集合类那么需要在其中实现迭代器才能进行增强for的遍历

如果是自定义的某个集合需要使用增强for循环的话需要返回一个迭代器

### forEach遍历

![[Pasted image 20250511130557.png]]



#### 1. **步骤概览**

1. 让自定义类实现 `Iterable` 接口。（该接口需要实现一个iterrator方法返回自定义迭代器）
    
2. 定义一个内部类实现 `Iterator` 接口。这个类就是实现的迭代器
    
3. 实现 `hasNext()` 和 `next()` 方法。
    
4. （可选）实现 `remove()` 方法。


## 相关集合

### ArrayList

![[Pasted image 20250510193457.png]]


### Set集合

**Set系列集合的特点是没有索引，所以无法直接找到元素**，但是可以使用迭代器来进行遍历


![[Pasted image 20250511140851.png]]


![[Pasted image 20250511141023.png]]
相当于是在HashSet的基础上给每个元素添加了链表的索引，所以遍历的时候不需要遍历整个数组

![[Pasted image 20250511141929.png]]

## 集合底层实现原理

### ArrayList实现


![[Pasted image 20250428164040.png]]

## 相关笔记
- [[Java/泛型类]]
- [[Java/函数式接口]]
- [[Java/多线程]]









