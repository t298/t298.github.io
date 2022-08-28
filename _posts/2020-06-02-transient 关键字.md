---
layout:     post   				    # 使用的布局（不需要改）
title: transient 关键字			# 标题 
subtitle:   #副标题
date:       2020-06-02		# 时间
author:     t298						# 作者
header-img: img/transient.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - java
    - transient
    - java基础之关键字


---

### transient关键字

简单来说：**就是让某些被修饰的成员属性变量不被序列化。**

- 一旦变量被transient修饰，变量不再是持久话的一部分，该变量内容在序列化后无法获得访问。
- transient只能修饰成员变量，不能修饰方法和类。
- 被transient关键字修饰的变量不能被序列化，一个静态变量不管是否被transient修饰，均不能被序列化。



### 那么什么是序列化呢？

- 序列化（Serialization）：将对象写到IO流中。
- 反序列化：从IO流中恢复对象。
- 意义：序列化机制允许将实现序列化的java对象转换位字节序列。这些字节序列可以保存在磁盘上，或通过网络传输，以达到以后恢复成原来的对象。序列化机制使得对象可以运行而独立存在。
- 使用场景：所有可在网络上传输的对象都必须是可序列化，比如RMI（remote method invoke,即远程方法调用），传入的参数或者返回的对象都是可序列化的，否则会出错；所有需要保存到磁盘的java对象都必须是可序列化的。**通常建议：程序创建的每个JavaBean类都实现Serializeable接口。**



### 你确定？

   我们知道在Java中，对象的序列化可以通过实现两种接口来实现：

- 若实现的是Serializable接口，则所有的序列化将会自动进行；
- 若实现的是Externalizable接口，则没有任何东西可以自动序列化，需要在writeExternal方法中进行手工指定所要序列化的变量，这与是否被transient修饰无关。







