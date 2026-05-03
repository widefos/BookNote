---
title: Spring
tags:
  - framework/spring
---

## 简介

Spring也是一个框架整合和扩展了市面上很多的框架，从而简化的项目的开发流程，


## 系统架构


这是基于spring4.0版本的系统架构图
![[Pasted image 20250607133925.png]]

其中主要分为四个主要部分：

* 容器，spring中的对象通过容器进行管理
* AOP面向切面编程，Aspects是面向切面编程思想的实现，可以利用它在spring当中进行切面编程
*  Web部分，也就是管理网络通信的建立和数据收发
* Data Access部分，负责管理数据包括数据库的连接访问等等
	* 当中的Transactions也就是事务管理是spring的重要特色，提高了事务的效率

---
## 基本概念解释

- IOC/DI
- IOC容器
- Bean

![[Pasted image 20250607140006.png]]

由于在开发过程中业务在代码上是分层实现的，所以避免不了一个类中经常需要调用其他类或是创建其他类的对象，这就造成了代码的**耦合**

而我们要求代码是低耦合的方便后续的维护和拓展功能，例如上图的需要将new之后的对象换一个

所以关键问题在于在类中所创建的其他类的对象，如果所有的类的对象的创建都交给一个外部容器管理那么我们就能解决这个问题

这就是IOC的思想

**IOC的目的就是为了解决代码耦合问题**

spring当中体现了这一点而我们提供的这个容器就是IOC容器，IOC容器管理着类的创建初始化等一些列工作，在IOC内创建和管理的对象叫做Bean

同时为了确定对象和对象之间的分配关系，对象和对象之间是有依赖的比如上图中的两个左边的类依赖于右边的类，所以IOC中也有这样的依赖关系，这叫做DI（依赖注入）

### 使用

#### IOC

使用IOC的基本思路

- 管理什么？（管理对象）
- 如何将管理的对象告诉IOC（写入配置文件）
- 怎么获取到IOC容器？（初始化）
- 怎么从IOC容器当中获取bean对象（调用容器函数返回对象）


在pom文件中导入相关依赖：
这是IOC的相关依赖
```
<dependency>  
    <groupId>org.springframework</groupId>  
    <artifactId>spring-context</artifactId>  
    <version>5.3.22</version>  
</dependency>
```

创建配置文件：并在配置文件中写入需要管理的对象

```
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">  
  
    <bean class="org.example.Data.BookDataImplement" id = "bookdata" />  
    <bean class="org.example.Service.BookServiceIm" id =  "bookServiceIm"/>  
  
  
</beans>
```

初始化并调用对象

```
ApplicationContext ctx = new 

//参数填入配置文件路径
ClassPathXmlApplicationContext("applicationContext.xml");

//获取Bean对象
BookData bookData = (BookData) ctx.getBean("bookdata");  
  
BookService bookService = (BookService) ctx.getBean("bookServiceIm");  
bookService.save();
```

这样就完成了将对象交由IOC容器管理


#### DI的实现

将对象交给IOC管理之后，IOC还得知道将这个对象用在哪，这就需要依赖注入




首先需要注入的类当中注入的对象需要有set方法，也就是IOC容器需要有一个入口来注入
```
BookData bookData ;  
  
@Override  
public void save() {  
    System.out.println("service sace");  
    bookData.save();  
}  
  
public void setBookData(BookData bookData) {  
    this.bookData = bookData;  
}
```


在配置文件当中配置依赖关系
需要在bean当中配置property属性，property可以配置这个bean当中的某一个属性，name表示属性,ref表示参照哪一个Bean进行配置或者说指向谁

这里将在某个类当中的对象变量视作这个类的一个属性，那这个变量就指向实现类

```
<bean class="org.example.Data.BookDataImplement" id = "bookdata" />  
<bean class="org.example.Service.BookServiceIm" id =  "bookServiceIm">  
        <property name="bookData" ref="bookdata"></property>  
</bean>
```


这样就完成了一个对象完全由IOC容器进行管理，或者说完成了对代码的解耦


#### Bean类的配置

**Bean的名称**


Bean可以有多个别名
通过标签当中的name属性来进行配置，多个别名的时候使用空格来进行分隔
```
<bean class="org.example.Data.BookDataImplement" name="bookDataImplement asdf asdsdf" id = "bookdata" />
```



**Bean的作用范围**

在以上配置文件中默认创建的Bean对象都是同一个对象或者说是一个单例对象，它们的地址都相同，而这个性质也可以在配置文件中更改
![[Pasted image 20250607153737.png]]

通过scope属性可以进行改变，prototype表示的就是创建的Bean对象是多个不同的对象

![[Pasted image 20250607153904.png]]


从这可以看出Bean的默认就是单例对象

这是因为正是很多对象需要进行复用我们才交给Bean进行管理的，才能体现效率，而一些用于存储数据的类则不适合Bean进行管理，比如用于封装的用于传递数据的类

![[Pasted image 20250607154331.png]]


#### Bean的构造创建

IOC在创建Bean的对象的时候是通过调用无参构造函数进行的，而不论构造函数的修饰符权限是什么，只要有无参构造函数就能创建Bean对象，这是通过反射进行的


下面这种配置class和id的就是通过构造函数来创建Bean对象的
```
<bean class="org.example.Data.BookDataImplement" id = "bookdata" />  
```

而还有一种方法是**工厂方式**，工厂方式同样可以产生出对象，具体来说就是一个类中有一个特定方法可以返回目标类的对象，这种特殊的方法就是工厂方法，这种方法也可以降低代码的耦合度
例如下面这样

![[Pasted image 20250607201520.png]]

而spring也是支持这样创建IOC对象的

![[Pasted image 20250607201634.png]]

其中factory-method指明了具体是哪个方法创建了对象，交给工厂创建的好处就是可以在工厂方法中做其他事，而用构造函数创建的方式只能在构造函数中完成，这反而会增加代码的耦合度，所以用工厂+IOC管理的方式相当于解耦了两次，从而在调用的时候不用去管具体的实现方式

**实例工厂和FactoryBean**

实例工厂就是先创建工厂的Bean类然后再创建具体工厂方法创建目标对象的Bean类
![[Pasted image 20250607211110.png]]

这其实就是将工厂也进行实例化交给IOC容器进行管理


而FactoryBean是基于实例工厂的改进而来的，是spring的原生模式后续的springBoot也是用这种方式来创建的，实际上是用对应接口简化了实例工厂的配置，只要实现了对应接口spring就能知道这是个实例工厂
![[Pasted image 20250607212028.png]]

#### Bean的生命周期

- 1.创建对象(分配内存)
- 2.执行构造方法
- 3.执行属性注入（这里的属性指的就是在类中依赖的其他对象）
- 4.执行初始化方法

- 使用Bean
- 销毁Bean

**初始化和销毁**

spring的配置文件当中是可以指定Bean创建之前的初始化方法和销毁之前的方法的

方法需要写在指定的Bean类内部，用init-method和destroy-method两个属性指定

![[Pasted image 20250608124527.png]]

还有一种方式是使用spring的特定接口，使用特定接口的时候就不需要写这两个属性

![[Pasted image 20250608125604.png]]

#### 依赖注入方式

因为向一个类中传递数据的方式可以分为以下两种，所以注入方式也可以分为两种

- 普通方式（利用对应的set方法进行注入）
- 构造方式（利用构造函数进行注入）

而因为注入的数据类型有基础数据类型和引用数据类型，所以我们也有

## 相关笔记
- [[数据库相关/Mybatis]]
- [[Java/类相关]]
- [[Java/多线程]]
- [[计算机网络/网络安全/网络安全]]

