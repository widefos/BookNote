---
title: Mybatis
tags:
  - database/mybatis
---

![[Pasted image 20250513185901.png]]

Mybatis是一个持久层框架，也就是在架构中的持久层起作用。主要是为了简化JDBC的操作，简化开发

![[Pasted image 20250513190133.png]]

这里的流程和JDBC的流程是相似的，SQL映射文件时一个xml文件，对应一个JAVA的POJO类

在 Java 中，**POJO**（Plain Old Java Object，普通 Java 对象）指的是一种**简单、无框架依赖的 Java 类**。它的核心特点是**不强制遵循任何特定框架的规范或接口**，仅通过最基本的 Java 语法定义，常用于表示数据或封装业务逻辑。通俗点说就是JAVA中最普通的自定义的类


## Mybatis入门

### 配置

要使用 MyBatis， 只需将 [mybatis-x.x.x.jar](https://github.com/mybatis/mybatis-3/releases) 文件置于类路径（classpath）中即可。

如果使用 **Maven** 来构建项目，则需将下面的依赖代码置于 pom.xml 文件中：
导入依赖：

```
<dependency>  
    <groupId>org.mybatis</groupId>  
    <artifactId>mybatis</artifactId>  
    <version>3.5.7</version>  
</dependency>
```

此外可能还需要数据库驱动依赖，有这个依赖才能连接到数据库

```
<dependency>  
    <groupId>com.mysql</groupId>  
    <artifactId>mysql-connector-j</artifactId>  
    <version>8.0.33</version>  
</dependency>
```


### 配置文件

如果是本地开发则需要单独一个xml配置文件
```
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE configuration  
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"  
        "http://mybatis.org/dtd/mybatis-3-config.dtd">  
<configuration>  
    <environments default="development">  
        <environment id="development">  
            <transactionManager type="JDBC"/>  
            <dataSource type="POOLED">  
  
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>  
                <property name="url" value="jdbc:mysql://localhost:3306/test "/>  
  
  
            <property name="username" value="root"/>  
            <property name="password" value="838168"/>  
  
        </dataSource>    </environment></environments>  
  
  //这里的package下的Mapper包中存放的就是对应的Mapper类
<mappers >  
    <package name="org.example.Mapper"/>  
</mappers>  
  
  
  
</configuration>
```

![[Pasted image 20250513203936.png]]

这里的enviroment可以配置多个，也就是可以配置多个数据库，然后default可以选择默认需要连接的数据库

#### 加载配置文件

获取一个SqlSessionFactory对象，这个对象是用来管理和数据库连接的

```
String resource = "org/mybatis/example/mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
```

通过该对象可以获取SqlSession对象，该对象就是和数据库的实例连接对象，通过该对象可以直接操作数据库

```
//获取SqlSession对象,用它来执行sql
SqlSession sqlSession = sqlSessionFactory. openSession();

//执行sql
List<User> users = sqlSession. selectList( statement: "test.selectAll");

System.out.println(users);

//释放资源
sqlSession.close();
```

这种操作数据库的方式是硬编码的方法，将sql语句写在代码中，缺点是不好维护和扩展
所以一般不用这种方式，通常使用Mapper映射的方式实现，下面介绍

如果使用springBoot开发直接在application.properties配置文件中进行配置即可不需要单独的配置文件

```
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver  
  
spring.datasource.url=jdbc:mysql://localhost:3306/test  
  
spring.datasource.username=root  
  
spring.datasource.password=838168
```

### XML映射文件

映射文件中写对应的sql语句，每个映射文件和一个JAVA的POJO类对应,namespace就相当于某个接口名，id就是方法名

```
<?xml version="1.0" encoding="UTF-8" ?>  
        <!DOCTYPE mapper  
                PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  
                "http://mybatis.org/dtd/mybatis-3-mapper.dtd">  
  
<mapper namespace="test">  
        <select id="selectAll" resultType="org.example.Pojo.User">  
                select  * from user;  
        </select>  
  
</mapper>
```


#### POJO对象

**每个POJO对象对应数据库当中的一张表，是表的抽象表示。**


![[Pasted image 20250513203502.png]]

这里的id的值对应Mapper接口当中的方法名，resultType代表sql语句的返回值类型，
$<mapper>$标签当中的namespace名称标签代表映射类也就是Mapper类的名称


### Mapper代理

获取sqlsession对象后，通过class文件获取接口代理对象，接口代理对象中可以直接调用已经写好的相关方法完成数据库的操作
```
//3. 获取接口代理对象
UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
//4. 执行方法,其实就是执行sql语句
List<User> users = userMapper.selectAlL();
```

![[Pasted image 20250513202014.png]]

基本思想就是将sql语句写在xml映射文件当中,然后用一个接口当中的方法来表示映射文件当中的sql语句，或者说将sql语句表示为接口当中的方法，之后只要通过SQLSession的gertMapper方法返回Mapper代理对象（这里的对象是自己定义的对应数据库当中的对象）调用当中的方法即可实现操作


#### 基本类型的映射

mybatis中因为每一张表都可以用类来表示，表中的字段在类中就是成员变量，所以sql当中的数据类型在Java中也有对应的类型具体如下：

| 别名         | 映射的类型      |
| ---------- | ---------- |
| _byte      | byte       |
| _long      | long       |
| _short     | short      |
| _int       | int        |
| _integer   | int        |
| _double    | double     |
| _float     | float      |
| _boolean   | boolean    |
| string     | String     |
| byte       | Byte       |
| long       | Long       |
| short      | Short      |
| int        | Integer    |
| integer    | Integer    |
| double     | Double     |
| float      | Float      |
| boolean    | Boolean    |
| date       | Date       |
| decimal    | BigDecimal |
| bigdecimal | BigDecimal |
| object     | Object     |
| map        | Map        |
| hashmap    | HashMap    |
| list       | List       |
| arraylist  | ArrayList  |
| collection | Collection |
| iterator   | Iterator   |

**需要注意的是，如果要正确实现字段和成员变量之间的映射，需要==名称相同==**，如果名称不同，以下是解决方案：

![[Pasted image 20250514173353.png]]

重新定义一个标签$<resultMap>$，这个标签的作用是将表中的字段名和类当中的成员变量名对应起来，从而能够完成映射，column代表表中字段名，property代表成员变量的名字，之后在语句标签的属性当中将结果属性替换为为resultMap，就能实现映射


### 如何通过接口方法向xml文件中的sql语句传递参数


Mybatis当中可以用参数占位符

* #{ }
* ${ }
![[Pasted image 20250522205728.png]]

如图中#{ }所示是将传入的参数id替换为？，从而防止sql注入,而${}只是单纯将参数拼接为sql语句

所以传入参数的时候使用#{ }


### 条件查询和动态查询


#### 参数传递

![[Pasted image 20250522210627.png]]
当接口方法中需要传入多个参数的时候，需要在每个参数前面加上@Param标识，Param当中的就是参数在xml文件当中所用的占位符，也就是写在#{ }当中的字符

第二种也可以将多个参数封装为一个类用对象进行传递，占位符和对象当中的成员变量名称相对应

第三种将参数封装为Map集合，key对应占位符名称


**这里需要注意一下，使用like的时候用户输入的名称不完全所以需要对原始输入处理后加入 % %  **

---

#### 动态条件查询

查询的时候有时候条件是不确定的，有时候需要一个字段进行查询，有时候需要多个，mybatis提供了动态条件查询来方便我们解决这个问题

![[Pasted image 20250523180359.png]]


通过在每一个条件之前加入if标签，test属性值内填写表达式，按照表达式条件来确定是否执行该sql语句，这样就实现了动态查询

**需要注意的是，这里有个小问题，如果第一个and前的条件为假而后一个为真，那么and就会报错因为只有一个运算值**
解决方法：

![[Pasted image 20250523180730.png]]

可以给每一个条件都加上and然后在所有条件之前加入1=1恒等式，这样and之前和之后都有值

![[Pasted image 20250523181107.png]]

也可以将where换成mybatis提供的where标签，使用改标签当中的sql语句就不会有这个问题


有时候，我们不想使用所有的条件，而只是想从多个条件中选择一个使用。针对这种情况，MyBatis 提供了 choose 元素，它有点像 Java 中的 switch 语句。

![[Pasted image 20250523181616.png]]

这对于只传入单个参数的来说可以使用



### 增加和修改

![[Pasted image 20250523182915.png]]

需要注意事务的手动提交，如果添加成功后没有手动提交会自动回滚事务


#### 主键返回

有时候需要获取添加项的主键，例如id等字段，

![[Pasted image 20250523183308.png]]

那么可以用useGeneratedKeys  和 keyProperty等字段来获取id，keyProperty指向对象中属性的名称，这样添加后就会自动返回主键的值


![[Pasted image 20250523195008.png]]

### 删除

![[Pasted image 20250523195340.png]]

当传递的参数有集合的时候,mybatis提供了诶foreach标签来遍历集合当中的元素，foreach相当于是将集合当中的元素取出然后拼接到sql语句当中



### 注解开发
![[Pasted image 20250523195719.png]]

## 相关笔记
- [[数据库相关/MySQL/简介]]
- [[数据库相关/MySQL/基础]]
- [[数据库相关/MySQL/进阶]]
- [[ssm相关/spring/Spring]]






















