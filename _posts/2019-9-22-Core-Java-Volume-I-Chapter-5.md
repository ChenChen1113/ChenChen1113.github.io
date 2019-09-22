---
layout:     post
title:      "【Java核心技术卷I笔记】第5章 继承"
date:       2019-9-22 09:55:06
author:     "dxc"
header-img: "img/post-bg-malaban.jpg"
tags:
    - Java
---

**1、 公有继承：**  
在Java中，所有的继承都是公有继承。  
子类用super来访问超类的私有域。  

**2、 多态：**  
一个对象变量可以指示多种实际类型的现象被称为多态。在运行时能够自动地选择调用哪个方法的现象称为动态绑定。  
一个超类变量可以引用任何一个子类的对象，但不可以使用子类的方法。（强制转化后可以使用）  
子类变量不可以引用超类对象。  

在覆盖一个方法时，子类方法不能低于超类方法的可见性。如果超类方法是public，子类方法一定要声明为public。  
将方法或类声明为final，可以确保他们不会在子类中改变语义。  

**3、 类型强制转换：**  
超类变量引用子类后，强制转换可以使用子类方法：  
``` java
Employee[] staff = new Employee[3];
staff[0] = new Manager("Carl Cracker", 80000, 1987, 12, 15);
staff[1] = new Employee("Harry Hacker", 50000, 1989, 10, 1);
staff[2] = new Employee("Tony Tester", 40000, 1990, 3, 15);

Manager boss = (Manager)staff[0];   //此时可以使用Manager子类方法
Manager boss1 = (Manager)staff[1];   //error
```
在将超类转换为子类之前，应该使用instanceof进行检查：  
```java
if(staff[1] instanceof Manager){
    Manager boss = (Manager)staff[1];
    ...
}
```

**4、 抽象类：**  
抽象方法充当着占位的角色，它们的具体实现在子类中。扩展抽象类可以有两种选择：一种是在抽象类中定义部分抽象类方法或不定义抽象类方法，这样就必须将子类也标记为抽象类；另一种是定义全部的抽象方法，这样一来，子类就不是抽象的了。  

**5、 protected：**  
声明为protected的方法和成员变量能被同一个包中的所有类访问。能被子类访问（可不在一个包中）。  
可访问性：public > protected > package > private  
使用情况：  
1）在子类中直接访问父类的protected变量  
2）在子类中通过子类的对象访问父类中的protected变量  
3）子类中父类对象不可以访问父类中的protected变量  
4）子类中另外一个子类对象不可以访问父类中的protected变量  
资料：<https://blog.csdn.net/tianyafeng123xin/article/details/52275302>  

**6、 Object类：**  
可以使用Object类的变量引用任何类型的对象。  
但只能用于对各种值得持有，想要对内容进行具体操作，还需要清楚对象的原始类型，并进行相应的类型转换。  

**7、 equals方法：**  
Object的equals方法用于判断两个对象是否具有相同的引用。  
通常被重写，检测两个对象状态是否相等，因为有时判断引用是否相同没有意义。  

**8、 hashCode方法：**  
Object的hashCode方法获取的散列码为对象的存储地址。  
String的散列码是由内容导出的，相同内容的String散列码相同。  
而StringBuffer类没有定义hashCode方法，使用Object中默认的存储地址方式，所以相同内容的StringBuffer散列码不一定相同。  

**9、 toString方法：**  
Object的toString方法打印对象所属的类名和散列码：  
```shell
java.io.PrintStream@2f6684
```
数组继承了Object的toString方法，打印出：
```shell
[I@1a46e30
```
若想打印数组的样子，需要调用Arrays.toString()方法，若要打印多维数组，需要调用Arrays.deepToString()方法。  
强烈建议为自定义的每一个类增加toString方法。  

**10、 泛型数组列表：**  
ArrayList是一个采用类型参数（type parameter）的泛型表（generic class）。为了指定数组列表保存的元素对象类型，需要用一对尖括号将类名括起来加在后面：  
```java
ArrayList<Employee> staff = new ArrayList<Employee>();
```
Java SE 7中，可以省去右边的类型参数：  
```java
ArrayList<Employee> staff = new ArrayList<>();
```
可以预估列表数组的元素数量（可重新分配空间）：  
```java
staff.ensureCapacity(100);
ArrayList<Employee> staff = new ArrayList<>(100);   //效果相同
```

一旦能够确认数组列表的大小不再发生变化，就可以调用trimToSize方法。这个方法将存储区域的大小调整为当前元素数量所需要的存储空间数目。垃圾回收器将回收多余的存储空间。  
一旦整理了数组列表的大小，添加新元素就需要花时间再次移动存储块，所以应该确认不会添加任何元素时，再调用trimToSize。  

**11、 对象包装器与自动装箱：**  
所有的基本类型都有一个与之对应的类。  
由于每个值分别包装在对象中，所以ArrayList<Integer>的效率远远低于int[]数组。因此，应该用它构造小型集合，其原因是此时程序员操作的方便性要比执行效率更加重要。  
==运算符比较的是对象的存储地址，equals方法比较的是值。  

如果在一个条件表达式中混合使用Integer和Double类型，Integer值就会拆箱，提升为double，再装箱为Double：
```java
Integer n = 1;
Double x = 2.0;
System.out.println(true ? n : x);   //Prints 1.0
```
装箱和拆箱是编译器认可的，而不是虚拟机。编译器在生成类的字节码时，插入必要的方法调用。虚拟机只是执行这些字节码。  

**12、 枚举类：**  
```java
public enum Size{SMALL, MEDIUM, LARGE, EXTRA_LARGE};
```
这个声明定义的是一个类，刚好有4个实例，尽量不要构造新对象。  
在比较两个枚举类型的值时，不要equals，直接==就可以了。  

**13、 反射：**  
虚拟机为每个类型管理一个Class对象，因此，可以利用==运算符实现两个类对象比较的操作。  
```java
getName()   //获得类名
getPackage()    //获得包
getSuperclass() //获得父类对象
getField(String name)   //获得属性
getMethods()    //获得public方法
getMethod(String name, class[] args)    //获得指定方法
```
资料：<https://blog.csdn.net/ly_xiamu/article/details/81943226>  

查看对象域的关键方法是Field类中的get方法。如果f是一个Field类型的对象（例如，通过getDeclaredFields得到的对象），obj是某个包含f域的类的对象，f.get(obj)将返回一个对象，其值为obj域的当前值：  
```java
Employee harry = new Employee("Harry Hacker", 35000, 10, 1, 1999);
Class cl = harry.getClass();    //the class object representing Employee
Filed f = cl.getDeclaredField("name");  //the name field of the Employee class
Object v = f.get(harry);    //the value of the name field of the harry object, i.e. "Harry Hacker"
```
如果查看double类型的域，反射机制将会自动地将这个域值打包到相应的对象包装器中，这里打包成Double。  


