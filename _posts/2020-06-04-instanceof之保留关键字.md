---
layout:     post
title:      instanceof 和保留关键字
subtitle:   
date:       2020-06-04
author:     298
header-img: img/instanceof.jpg
catalog: 	 true
tags:
    - java
    - java基础之关键字
    - instanceof

---



### instanceof 是java的保留关键字。它的作用是来测试左边的对象是否是它右边的实例，返回boolean的数据类型。



```java
Object testObject = new ArrayList();

o instanceof ArrayList //true
```



**注意：**

- 类的实例包含本身的实例，以及所有直接或间接子类的实例。
- instanceof左边显式声明的类型与右边操作元必须是同种类或存在继承关系，也就是说需要位于同一个继承树，否则会编译错误。
- 左边的对象实例不能是基础数据类型。
- null用instanceof跟任何类型比较时都是false





**保留字是以后可能用。**

**关键字是现在就已经使用。**

java总共有51个保留关键字，其中`const`和`goto`虽然被保留，但未被使用。你不能使用保留关键字来命名类，方法或者变量。

>**保留关键字**
>
>数据类型：
>
>Boolean   int   long   short   byte   float   double   char   class   interface
>
>
>
>流程控制：
>
>if    else   do   while   for   switch   case   default   break   continue   return   try   catch   finally
>
>
>
>修饰符：      
>
>public   protected   private   final   void    static   strictfp    abstract    transient
>
>synchronized    volatile   native
>
>
>
>动作：          
>
>package   import    throw   throws    extends   implements   this   Super   instanceof   new
>
>
>
>**保留字：**      
>
>true    false   null   goto   const
>



