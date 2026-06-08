---
title: Python自动化测试
tags:
  - testing
---

## 相关库及使用


### request库


请求语法：
![[Pasted image 20250826180013.png]]

**例子**：

>GET
![[Pasted image 20250826181031.png]]

>POST
![[Pasted image 20250826181056.png]]

>PUT/DELETE
![[Pasted image 20250826181157.png]]

#### 获取响应结果的具体内容

~~~
## 获取url

res.url

## 获取响应状态码

res.status_code

## 获取Cookie

res.cookies

## 获取响应头

res.headers

## 获取响应体

res.text 或者是 res.json()
~~~


### cookie+session
因为http协议是无连接状态的，所以无法保持信息，所以信息需要存储在浏览器端，存储信息的技术就是Cookie，cOOKIE中的信息可以随意访问，cookie中存储的数据类型受浏览器限制

**基于**cookie+session的认证技术：








### pytest框架
这是一个专门用来测试的python框架能够进行web.app接口测试

```
python install pytest
```




###  unittest框架

用于组织和管理测试用例

#### TestCase（测试用例）


是一个代码文件，在代码文件当中来书写测试用例

1. 导入unittest包
2. 自定义测试类
3. 书写测试方法
4. 执行用例


定义测试类：

继承unitest.TestCase类，表明这个是个测试类

![[Pasted image 20250827165517.png]]


测试方法必须以test开头，一般实际使用中以test_开头书写

![[Pasted image 20250827165641.png]]





#### TestRunner和TestSuite

TestSuit是测试套件，用来管理组装打包TestCase测试用例的

TestRunner用来执行打包好的TestSuit

**使用方法**：

1. 导入unittest包
2. 实例化套件对象
3. 使用套件对象添加用例方法
4. 实例化运行对象
5. 使用运行对象执行套件对象

![[Pasted image 20250827170301.png]]

**添加测试方法的方式：**

第一种是如上图所示的.addTest(测试类名（‘方法名’）)

第二种方式将类中的所有测试方法添加
![[Pasted image 20250827170453.png]]


![[Pasted image 20250827170604.png]]



#### TestLoader

作用和TestSuit相同作为TestSuit功能补充

使用场景：当测试用例的代码文件很多的时候，使用TestLoader可以发现并加载一个目录下的测试用例文件



![[Pasted image 20250827170852.png]]



#### Fixture（测试夹具）的使用

Fixture是一种代码结构在特定情况下会自动执行

对于unittest来说Fixture的级别有两个一个是方法级别上的一个是类级别上的


对于方法级别来说Fixture会在**每个**测试方法执行之前和之后自动执行

![[Pasted image 20250827171248.png]]


对于类级别来说，在类调用测试方法之前执行一次，所有方法执行完毕后执行一次，执行两次


![[Pasted image 20250827171743.png]]


#### unittest断言

常用断言方法，expr为预期结果

![[Pasted image 20250827171926.png]]
![[Pasted image 20250827171951.png]]


需要注意的是由于断言是属于unittest的所以只能在继承了TestCase的类中使用，使用方式是self.方法名


#### 参数化

参数化就是使用变量来代替方法中具体的测试数据，用传参的方式将数据传入测试方法，从而减少代码量

工作场景中，测试数据一般存放在json文件当中，需要将json文件提取列表的形式


需要安装插件

~~~
pip intall parameterized


from parameterized import parameterized
~~~

**具体使用**：

![[Pasted image 20250827172738.png]]

注解当中传递参数是一个列表，列表中每一个元素是一个元组或者一个列表，每一个元素就对应测试方法中的形参，加上了注解的方法会自动迭代列表当中的元素，将每一个元素的值赋给对应参数

![[Pasted image 20250827173149.png]]


#### PyMySQL操作数据库


![[Pasted image 20250828171558.png]]


![[Pasted image 20250828171546.png]]


**使用方法**：

~~~

import pymysql
## 获取连接对象
conn = pymysql.connect()
## 获取游标对象
cursor = conn.Cursor()
## 利用游标对象来执行sql语句
cursor.execute("sql语句")

## 提交事务
conn.commit()

## 回滚事务
conn.rollback()

cursor.close()
conn.rollback()


~~~

连接参数：
![[Pasted image 20250828174704.png]]


**游标介绍（cursor）**：

![[Pasted image 20250828190037.png]]

cursor是实际执行sql操作的对象



![[Pasted image 20250828190403.png]]

游标就是在客户端和数据库服务器之间传送数据的对象，当执行查询操作时，提取游标所在位置的下一行数据

**返回结果的常用方法**：

- fetchone（）：从结果集中提取一行，也就是提取游标所在位置的下一行
- fetchmany(size)：从结果集当中提取size行
- fetchall():提取所有结果
- 属性rownumber

如何设置游标的位置？
使用rownumber属性，cursor.rownumber = 0 归零

返回结果的类型是元组类型


更新操作的实现
![[Pasted image 20250828191541.png]]

**try-except封装数据库操作：**

因为对数据库进行更新操作时有可能会出错，所以需要捕获当中的异常

~~~
##显示更新的行数
conn.afect_rows()
~~~


~~~
try :
	获取数据库连接和更新操作，提交事务
except:
	显示错误信息和回滚事务
finally:
	关闭连接
~~~

![[Pasted image 20250829091619.png]]


**数据库工具类封装**：
将常用的数据库操作封装到方法当中，后续直接调用方法实现即可，提高代码复用率

![[Pasted image 20250829095927.png]]



#### 接口对象封装



![[Pasted image 20250901175736.png]]

这类似于后端当中的分层思想，目的是为了减少代码耦合度，减少代码量方便维护。

具体思想就是将向接口发送请求获取返回结果封装为单独的类，而测试用例断言单独一个类，这样我们只需要关注向接口发送的数据和返回的数据就行不用在意如何发送请求

**例子**：
登录接口对象层
![[Pasted image 20250901175957.png]]
测试用例层封装测试用例


当需要用例中断言的数据较多时可以封装一个断言方法，将预期结果转换为参数，这样可以减少代码量，**相同的数据抽象为参数**

![[Pasted image 20250901180802.png]]

登录封装，将可变的写入参数，不变的保留在方法当中
![[Pasted image 20250918170302.png]]

## 相关笔记
- [[测试相关/接口测试]]
- [[测试相关/pytest]]
- [[测试相关/Selenium/操作行为]]
- [[测试相关/Selenium/元素定位]]
- [[测试相关/Selenium/等待机制]]


