---
layout:     post
title:      Static& final关键字陷阱
subtitle:   java Static& final
date:       2017-10-27
author:     CoderFrank	
header-img: img/post-bg-unix-linux.jpg
catalog: true
tags:
    - Java
---



1. static代码块，在java代码加载到jvm上就已经执行了，先于类构造方法执行，并且只执行一次。 在继承体系中，也是先把所有的静态代码块执行完了后再由上往下执行构造方法。


2. 不能在静态方法中访问非静态成员变量
3. static方法在继承中隐含问题

```java
package com.coreJavaSe;

class M{
    public static void s(){
        System.out.println("M");
    }
}

class N extends M{
    public static void s(){
        System.out.println("N");
    }
}
public class StaticTest1 {

    public static void main(String args[]) {
        M s = new N();
        s.s();
    }
}

```

在这个例子中，s调用的是M中的s方法，static静态方法是可以被继承的，但不可以被*重写,override*但是这里得记清楚，对于方法重载，调用时的标准是：

> the version of the hidden method that gets invoked depends on whether it is invoked from the superclass of the subclass.

即：*谁来调用的*

4. final修饰的类不能被修改，不能被_继承_。final修饰的方法不能被重写

5. final类修饰的类虽然不能修改，但是该类中的非final属性和方法是可以修改的

   ```java
   package com.coreJavaSe;

   class People{
   	final Address address = new Address();
   }

   class Address{
   	String name = "beijing";
   }

   public class FinalTest1{
   	public static void main(String args[]){
   		People people = new People();
         	people.address.name = "shanghai";
       }
   }
   ```

   但是改变类本身，比如`people = new People();`和改变类的成员变量有什么区别呢？

   > 改变类本身，改变了people本身引用的值，所以不被允许，改变成员变量，只是改变了people引用指向的内容，但people本身的值并没有被改变

