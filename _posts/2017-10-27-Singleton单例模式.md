---
layout:     post
title:      Singleton单例模式
subtitle:   设计模式-单例模式
date:       2017-10-27
author:     CoderFrank	
header-img: img/post-bg-unix-linux.jpg
catalog: true
tags:
    - Java
    - 设计模式
---



### Singleton concept

一个类只生成一个实例，这个类可以保证没有其他实例被创建，并且可以提供一个访问该实例的方法，java实现方法有两种：

第一种：

```java
package com.coreJavaSe;

public class SingleTonTest {
    public static void main(String args[]) {
        Singleton.getInstanceOfSingleton().output();
    }
}

class Singleton{
    private static Singleton singleton = new Singleton();
    private Singleton(){

    }

    public static Singleton getInstanceOfSingleton(){
        return singleton;
    }

    public void output(){
        System.out.println("SingleTon");
    }
}
```

**key**: *使用static和private控制权限来实现单例模式*

第二种：

```java
package com.coreJavaSe;

public class SingleTonTest {
    public static void main(String args[]) {
        Singleton.getInstanceOfSingleton().output();
    }
}

class Singleton{
    private static Singleton singleton = new Singleton();
    private Singleton(){
    }

    public static Singleton getInstanceOfSingleton(){
      if(singleton == null){
          singleton = new Singleton();
      }
        return singleton;
    }

    public void output(){
        System.out.println("SingleTon");
    }
}



```

**从多线程角度讲，第一种方法更好，因为第二种方法在多线程环境中可能不是单例模式，也就是说不能保证只生成一个对象。**

