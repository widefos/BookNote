---
title: String相关
tags:
  - java/string
---

## String 构造函数


![[Pasted image 20250427162146.png]]

这里注意char类型和string 类型的字符串可以可以在构造函数内进行转换


### 内存中

![[Pasted image 20250427163151.png]]

对string进行直接赋值，赋值的常量字符串存放在串池当中，如果后续有相同的赋值那会直接从串池中寻找，如果有相同的则赋值为相同的值

## 字符串比较

这里需要分类来看


对于基本数据类型来说，== 比较的是数据值

而对于引用类型来说， == 比较的是地址值

**而对于String 来说比较的是引用类型所以比价的是地址值**

所以对于String来说不同的赋值，比较的结果也不同

![[Pasted image 20250427163745.png]]



根据在内存中的存储，如果比较的是两个直接赋值的对象，那么这两个其实都指向串池当中的内容，比较结果相同

但如果一个采用构造函数赋值而一个还是直接赋值那么，最后的比较结果不同


### 比较方法


![[Pasted image 20250427164039.png]]

equals这是是String内部的方法，s1.equals(s2);比较的是两个字符串对象的内容是否相同而不是地址值

这个可以扩展到任意两个对象的比较，对于java内部提供的对象都提供了equals方法进行两个对象内容的，而对于自定义的对象可以进行重写quals方法进行内容比较


## StringBuilder 

![[Pasted image 20250427173722.png]]

这个类相当于一个存放字符串的容器，内容可以改变

![[Pasted image 20250427173802.png]]


## StringJoiner

这个类也可以看做是一个容器，不过主要功能是用来S给字符串做拼接


![[Pasted image 20250427174330.png]]

每添加一个数据，就会在其中加入指定的连接符

![[Pasted image 20250427174409.png]]



## String.Split(char)

## 相关笔记
- [[Java/IO]]
- [[Java/异常]]

按照某个字符将字符串进行拆分

```
public String[] split(String regex)
public String[] split(String regex, int limit)
```


有两个重载形式，第二个的第二个参数规定了数组元素的长度


















