---
title: classLoader
date: 2018-06-11 09:30:21
tags: work
---

### java类加载

#### 加载

加载是类装载的第一步，首先通过class文件的路径读取到二进制流，并解析二进制流将里面的元数据（类型、常量等）载入到方法区，在java堆中生成对应的java.lang.Class对象。

#### 连接

连接过程又分为3步，验证、准备、解析

##### 验证

验证的主要目的就是判断class文件的合法性，比如class文件一定是以0xCAFEBABE开头的，另外对版本号也会做验证，例如如果使用java1.8编译后的class文件要再java1.6虚拟机上运行，因为版本问题就会验证不通过。除此之外还会对元数据、字节码进行验证，具体的验证过程就复杂的多了，可以专门查看相关资料去了解。

##### 准备

准备过程就是分配内存，给类的一些字段设置初始值，例如：

> public static int v=1;

这段代码在准备阶段v的值就会被初始化为0，只有到后面类初始化阶段时才会被设置为1。

但是对于static final（常量），在准备阶段就会被设置成指定的值，例如：

> public static final int v=1;

这段代码在准备阶段v的值就是1。

##### 解析

解析过程就是将符号引用替换为直接引用，例如某个类继承java.lang.object，原来的符号引用记录的是“java.lang.object”这个符号，凭借这个符号并不能找到java.lang.object这个对象在哪里？而直接引用就是要找到java.lang.object所在的内存地址，建立直接引用关系，这样就方便查询到具体对象。

#### 初始化

初始化过程，主要包括执行类构造方法、static变量赋值语句，staic{}语句块，需要注意的是如果一个子类进行初始化，那么它会事先初始化其父类，保证父类在子类之前被初始化。所以其实在java中初始化一个类，那么必然是先初始化java.lang.Object，因为所有的java类都继承自java.lang.Object。