---
layout:     post
title:      "【Java核心技术卷I笔记】第4章 对象与类"
date:       2019-9-20 15:00:45
author:     "dxc"
header-img: "img/post-bg-malaban.jpg"
tags:
    - Java
---

**1、 对象之间最常见的关系：**  
依赖（“uses-a”）  
聚合（“has-a”）  
继承（“is-a”）  

**2、 对象的引用：**
一个对象变量并没有实际包含一个对象，而仅仅引用一个对象。  
在Java中，任何对象变量的值都是对存储在另外一个地方的一个对象的引用。new操作符的返回值也是一个引用。

**3、 LocalDate的使用举例：**
``` java
LocalDate newYearsEve = LocalDate.of(1999, 12, 31);

int year = newYearsEve.getYear();   //1999
int month = newYearsEve.getMonthValue();    //12
int day = newYearsEve.getDayOfMonth();  //31

LocalDate aThousandDaysLater = newYearsEve.plusDays(1000);
```

**4、 方法参数：**  
Java是传值调用。    
基本数据类型的参数值不可改变，对象引用的参数不可改变其引用。

如下方法不能交换x和y：
``` java
public static void swap(Employee x, Employee y){    //doesn't work
    Employee temp = x;
    x = y;
    y = temp;
}
```

**5、 重载**  
相同的方法名，不同的参数叫重载。  
编译器通过参数自动挑选相应的方法。

方法的签名（signature）：方法名及参数类型。  
返回值不属于方法签名。也就是说，不能有两个名字相同、参数类型也相同却返回值不同类型值得方法。

**6、 无参数的构造器**  
如果没有编写构造器，系统就会自动提供一个无参数的构造器，这个构造器将所有的实例域设置为默认值：0、false、null。  
如果编写了构造器，调用时不加参数，就是不合法。

**7、 显示域初始化**  
实例域可以设置一个有意义的初值，这是一种很好的设计习惯。如：
``` java
class Employee{
    private String name = "";
    ...
}
```

**8、 初始化块**  
初始化块（initialization block），只要构造类的对象，这些块就会被执行。
``` java
class Employee{
    private static int nextId;
    
    private int id;
    
    {
        id = nextId;
        nextId ++;
    }
}
```

如果对类的静态域进行初始化的代码比较复杂，那么可以使用静态的初始化块。在类第一次加载的时候，会进行静态域的初始化。
``` java
class Employee{
    private static int nextId;
    
    static
    {
        Random generator = new Random();
        nextId = generator.nextInt(10000);
    }
}
```

**9、对象析构**  
Java不支持析构器。