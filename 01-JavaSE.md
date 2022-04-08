---
title: 01-JavaSE
date: 2022-01-24 09:51:58
tags: [Java]
---

# 0 导言

*导言：尚硅谷宋红康PDF——数组定义、面向对象中继承多态封装的概念、集合是重中之重（主要是ArrayList和HashMap）、接口的理解、异常的两种处理机制*

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220124104014052.png" alt="image-20220124104014052" style="zoom:50%;" />



# 第1章  Java语言概述

## 一、Java两种核心机制

### 1.  Java虚拟机——JVM

1. JVM是一个虚拟的计算机，具有指令集并使用不同的存储区域。负责执行指令，管理数据、内存、寄存器。
2. 对于不同的平台，有不同的虚拟机。
3. 只有某平台提供了对应的java虚拟机，java程序才可在此平台运行。
4. Java虚拟机机制屏蔽了底层运行平台的差别，实现了“一次编译，到处运行”。

### 2. 垃圾回收

1. 不再使用的内存空间应回收—— 垃圾回收。
2. 在C/C++等语言中，由程序员负责回收无用内存。
3. Java 语言消除了程序员回收无用内存空间的责任：它提供一种系统级线程跟踪存储空间的分配情况。并在JVM空闲时，检查并释放那些可被释放的存储空间。
4. 垃圾回收在Java程序运行过程中自动进行，程序员无法精确控制和干预。
5. Java程序还会出现内存泄漏和内存溢出问题吗？Yes!

## 二、Java语言环境

### 1.  JDK

（Java Development  Kit ）——Java开发工具包

​	JDK是提供给Java开发人员使用的，其中包含了java的开发工具，也包括了JRE。所以安装了JDK，就不用在单独安装JRE了。	其中的开发工具：编译工具(javac.exe) 打包工具(jar.exe)等

### 2.  JRE

（Java Runtime Environment）——Java运行环境

​	包括Java虚拟机(JVM Java Virtual Machine)和Java程序所需的核心类库等，如果想要运行一个开发好的Java程序，计算机中只需要安装JRE即可。

​	简单而言，使用JDK的开发工具完成的java程序，交给JRE去运行。

### 3.  JDK、JRE、JVM关系

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220124114909574.png" alt="image-20220124114909574" style="zoom:50%;" />

` JDK = JRE + 开发工具集（如javac编译工具）`

`JRE = JVM + Java SE标准类库 `



# 第2章 Java基本语法

## 一、类型转换

### 1. 自动类型转换

容量小的类型自动转换为容量大的数据类型。

数据类型按容量大小排序为：

![image-20220124122304732](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220124122304732.png)

+ byte,short,char之间不会相互转换，他们三者在计算时首先转换为int类型。 
+ boolean类型不能与其它数据类型运算。
+ 当把任何基本数据类型的值和字符串(String)进行连接运算时(+)，基本数据类

型的值将自动转化为字符串(String)类型。

### 2. 强制类型转换

强制转换符: ()

```java
int i = Integer.parseInt(a);

byte b = 3;
b = (byte)(b+4);
```

## 二、运算符

### 1.逻辑运算符

 “&”和“&&”的区别

&：左边无论真假，右边都进行运算；

&&：如果左边为真，右边参与运算，如果左边为假，那么右边不参与运算

### 2. 位运算符

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220124150036669.png)

![<!---->](https://gitee.com/doubledogss/image/raw/master/img/image-20220124150104946.png)

## 三、break、continue、return

**break** 

```java
//1. break语句用于终止某个语句块的执行
{ 	……
	break;
	……
} 
//2. break语句出现在多层嵌套的语句块中时，可以通过标签指明要终止的是哪一层语句块
label1: { ……
label2: 	{ ……
label3: 		{ ……
				break label2;
				……
				} 
            }
        }
```

**continue**

```java
// continue只能使用在循环结构中
// continue语句用于跳过其所在循环语句块的一次执行，继续下一次循环
// continue语句出现在多层嵌套的循环语句体中时，可以通过标签指明要跳过的是哪一层循环
```

**return**

```java
// return：并非专门用于结束循环的，它的功能是结束一个方法。
// 当一个方法执行到一个return语句时，这个方法将被结束。
// 与break和continue不同的是，return直接结束整个方法，不管这个return处于多少层循环之内
```

小结：

+ break只能用于switch语句和循环语句中。
+ continue 只能用于循环语句中。
+ 二者功能类似，但continue是终止**本次**循环，break是终止**本层**循环。

# 第3章 数组

## 一、概述

+ 数组(Array)，是多个相同类型数据按一定顺序排列的集合，并使用一个名字命名，并通过编号的方式对这些数据进行统一管理。 
+ 数组本身是引用数据类型，而数组中的元素可以是任何数据类型，包括基本数据类型和引用数据类型。 
+  创建数组对象会在内存中开辟一整块连续的空间，而数组名中引用的是这块连续空间的首地址。 
+  数组的长度一旦确定，就不能修改。 
+  我们可以直接通过下标(或索引)的方式调用指定位置的元素，速度很快。 

## 二、一维数组

### 1. 声明

```java
int a[];
int[] a1;
String[] c;
int a[5]; //非法,Java语言中声明数组时不能指定其长度(数组中元素的数)
```

### 2. 初始化

1. 动态初始化：数组声明且为数组元素分配空间与赋值的操作分开进行。

```java
int[] arr = new int[3];
arr[0] = 3;
arr[1] = 9;
arr[2] = 8;
```

2. 静态初始化：在定义数组的同时就为数组元素分配空间并赋值。

```java
int arr[] = new int[]{ 3, 9, 8};
int[] arr1 = {3,9,8};
```

### 3. 数值元素默认初始化值

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220124174428629.png)

### 4. 一维数组内存解析

​	当定义一个数组时，会先在栈中建立一个数组名，然后根据定义的数组的大小在堆建立一个内存空间，并给该内存空间第一个空间命名一个地址，将该地址赋给栈里面的数组名。

​	当要操作到该数组时，会先找到栈中的变量名，再根据该变量名在堆中指定的地址值找到数组在堆中的第一个内存空间开始寻找，找到想做操作的值。

​	当堆中的内存空间在栈中没有对应的值时，堆的内存空间则会被自动回收（JVM的垃圾自动回收机制），当main方法执行完成后，数组的变量名也会自动弹出栈，此时的堆的内存空间也会被自动回收。

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220126185940213.png)

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220126190010636.png)

## 三、多维数组

​	对于二维数组的理解，我们可以看成是一维数组array1又作为另一个一维数组array2的元素而存在。其实，从数组底层的运行机制来看，其实没有多维数组。

### 1.初始化

1. 动态初始化

```java
//格式1：二维数组中有3个一维数组，每一个一维数组中有2个元素
int[][] arr = new int[3][2]; 

//格式2：二维数组中有3个一维数组。每个一维数组都是默认初始化值null。可以对这个三个一维数组分别进行初始化arr[0] = new int[3]; arr[1] = new int[1]; arr[2] = new int[2];
int[][] arr1 = new int[3][];

int[][]arr = new int[][3]; //非法
```

2. 静态初始化

```java
int[][] = new int[][]{{3,8,2},{2,7},{9,0,1,6}};
/*
定义一个名称为arr的二维数组，二维数组中有三个一维数组
每一个一维数组中具体元素也都已初始化
第一个一维数组 arr[0] = {3,8,2};
第二个一维数组 arr[1] = {2,7};
第三个一维数组 arr[2] = {9,0,1,6};
第三个一维数组的长度表示方式：arr[2].length;
*/
```

注意特殊写法情况：int[] x , y[]; x是一维数组，y是二维数组。

Java中多维数组不必都是规则矩阵形式

### 2. 二维数组内存解析

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220126191345416.png)

## 四、数组常见算法TODO

## 五、Arrays工具类

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220126200120588.png)

<hr>

# 第4章 面向对象

## 一、内存解析

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220127085628284.png)

+ 堆（Heap），此内存区域的唯一目的就是存放对象实例，几乎所有的对象实例都在这里分配内存。这一点在Java虚拟机规范中的描述是：所有的对象实例以及数组都要在堆上分配。 
+  通常所说的栈（Stack），是指虚拟机栈。虚拟机栈用于存储局部变量等。局部变量表存放了编译期可知长度的各种基本数据类型（boolean、byte、char 、 short 、 int 、 float 、 long 、double）、对象引用（reference类型，它不等同于对象本身，是对象在堆内存的首地址）。 方法执行完，自动释放。 
+  方法区（Method Area），用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。



```java
//匿名对象
new Person().shout();
//使用场景：对一个对象只进行一次方法调用；将匿名对象作为实参传递给一个方法调用。
```

## 二、类的成员

### 1. 属性（成员变量）

1. 修饰符

   + 常用的权限修饰符有：private、缺省、protected、public

   + 其他修饰符：static、final (暂不考虑)

2. 成员变量（属性）与局部变量

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220127090557137.png)

​		**同：**都有生命周期

​		**异：**局部变量除形参外，均需显式初始化。当一个对象被创建时，会对其中各种类型的成员变量自动进行初始化赋值。

​		区别：

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220127091013737.png)

### 2.方法

1. 修饰符

​		public，缺省，private，protected等

2. 没有具体返回值的情况，返回值类型用关键字void表示，那么方法体中可以不必使用return语句。如果使用，仅用来结束方法。
3. 方法中只能调用方法或属性，不可以在方法内部定义方法。
4. 重载

```java
//重载的概念:在同一个类中，允许存在一个以上的同名方法，只要它们的参数个数或者参数类型不同即可。
//重载的特点：与返回值类型无关，只看参数列表，且参数列表必须不同。(参数个数或参数类型)。调用时，根据方法参数列表的不同来区别。

//重载示例：
//返回两个整数的和
int add(int x,int y){return x+y;}
//返回三个整数的和
int add(int x,int y,int z){return x+y+z;}
//返回两个小数的和
double add(double x,double y){return x+y;}
```

5. 可变个数的形参

+ 声明格式：方法名(参数的类型名 ...参数名)
+ 可变参数：方法参数部分指定类型的参数个数是可变多个：0个，1个或多个
+ 可变个数形参的方法与同名的方法之间，彼此构成重载
+ 可变参数方法的使用与方法参数部分使用数组是一致的
+ 方法的参数部分有可变形参，需要放在形参声明的最后
+ 在一个方法的形参位置，最多只能声明一个可变个数形参

```java
public void test(String[] msg){
	System.out.println(“含字符串数组参数的test方法 ");
}
public void test1(String book){
	System.out.println(“****与可变形参方法构成重载的test1方法****");
}
public void test1(String ... books){
	System.out.println("****形参长度可变的test1方法****");
}
public static void main(String[] args){
	TestOverload to = new TestOverload();
	//下面两次调用将执行第二个test方法
	to.test1();
	to.test1("aa" , "bb");
	//下面将执行第一个test方法
	to.test(new String[]{"aa"});
}
```

6. 方法参数的值传递机制

​		Java里方法的参数传递方式只有一种：值传递。 即将实际参数值的副本（复制品）传入方法内，而参数本身不受影响。 

+ 形参是基本数据类型：将实参基本数据类型变量的“数据值”传递给形参
+ 形参是引用数据类型：将实参引用数据类型变量的“地址值”传递给形参

```java
//基本数据类型的参数传递
public static void main(String[] args) {
	int x = 5;
	System.out.println("修改之前x = " + x);// 5
	// x是实参
	change(x);
	System.out.println("修改之后x = " + x);// 5
}
public static void change(int x) {
	System.out.println("change:修改之前x = " + x);
	x = 3;
	System.out.println("change:修改之后x = " + x);
}
```

```JAVA
//引用数据类型的参数传递
public static void main(String[] args) {
	Person obj = new Person();
	obj.age = 5;
	System.out.println("修改之前age = " + obj.age);// 5
	// x是实参
	change(obj);
	System.out.println("修改之后age = " + obj.age);// 3
}
public static void change(Person obj) {
	System.out.println("change:修改之前age = " + obj.age);
	obj.age = 3;
	System.out.println("change:修改之后age = " + obj.age);
}

//其中Person类定义为：
class Person{
	int age; 
} 
```

```java
//引用数据类型的参数传递
public static void main(String[] args) {
	Person obj = new Person();
	obj.age = 5;
	System.out.println("修改之前age = " + obj.age);// 5
	// x是实参
	change(obj);
	System.out.println("修改之后age = " + obj.age);// 5
}
public static void change(Person obj) {
    obj = new Person();
	System.out.println("change:修改之前age = " + obj.age);
	obj.age = 3;
	System.out.println("change:修改之后age = " + obj.age);
}

//其中Person类定义为：
class Person{
	int age; 
} 
```

### 3.构造器（构造方法）

+ 它具有与类相同的名称
+ 它不声明返回值类型。（与声明为void不同）
+ 不能被static、final、synchronized、abstract、native修饰，不能有return语句返回值
+ 分类：隐式无参构造器（系统默认提供）；显式定义一个或多个构造器（无参、有参），Java语言中，每个类都至少有一个构造器。
+ 默认构造器的修饰符与所属类的修饰符一致，一旦显式定义了构造器，则系统不再提供默认构造器。
+ 一个类可以创建多个重载的构造器
+ 父类的构造器不可被子类继承

```java
public class Person { 
	private String name;
	private int age;
	private Date birthDate;
	public Person(String n, int a, Date d) {
		name = n;
		age = a;
		birthDate = d; 
    }
	public Person(String n, int a) {
		name = n;
		age = a; 
    }
	public Person(String n, Date d) {
		name = n;
		birthDate = d; 
    }
	public Person(String n) {
		name = n;
		age = 30;
    }
}
```



### 4.属性赋值顺序

<img src="https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220127152022037.png" style="zoom:67%;" />



## 三、封装

```JAVA
//将数据声明为私有的(private)，再提供公共的（public）方法:getXxx()和setXxx()实现对该属性的操作
class Animal {
	private int legs;// 将属性legs定义为private，只能被Animal类内部访问
	public void setLegs(int i) { // 在这里定义方法 eat() 和 move()
		if (i != 0 && i != 2 && i != 4) {
			System.out.println("Wrong number of legs!");
			return; 
        }
		legs = i; 
    }
	public int getLegs() {
		return legs; 
    } 
}

public class Zoo {
	public static void main(String args[]) {
	Animal xb = new Animal();
	xb.setLegs(4); // xb.setLegs(-1000);
	//xb.legs = -1000; // 非法
	System.out.println(xb.getLegs());
    }
}
```

### 1.四种访问权限修饰符

​	Java权限修饰符public、protected、(缺省)、private置于**类的成员**定义前，用来限定对象对该类成员的访问权限。

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220127100217721.png)

​	对于class的权限修饰只可以用public和default(缺省)。 

+ public类可以在任意地方被访问。
+ default类只可以被同一个包内部的类访问。

<hr>

### 2.JavaBean

+ JavaBean是一种Java语言写成的可重用组件。
+ 所谓javaBean，是指符合如下标准的Java类：
  + 类是公共的
  + 有一个无参的公共的构造器
  + 有属性，且有对应的get、set方法
+  用户可以使用JavaBean将功能、处理、值、数据库访问和其他任何可以用Java代码创造的对象进行打包，并且其他的开发者可以通过内部的JSP页面、Servlet、其他JavaBean、applet程序或者应用来使用这些对象。用户可以认为JavaBean提供了一种随时随地的复制和粘贴的功能，而不用关心任何改变。

```java
public class JavaBean {
	private String name; // 属性一般定义为private
	private int age;
	public JavaBean() {
        
    }
	public int getAge() {
		return age; 
    }
	public void setAge(int a) {
        age = a; 
    }
    public String getName() {
        return name; 
    }
    public void setName(String n) {
        name = n; 
    } 
}
```

<hr>

### 3.this

```java
//this可以作为一个类中构造器相互调用的特殊格式
class Person{ // 定义Person类
	private String name ;
	private int age ;
	public Person(){ // 无参构造器
		System.out.println("新对象实例化") ;
        }
	public Person(String name){
		this(); // 调用本类中的无参构造器
		this.name = name ;
        }
	public Person(String name,int age){
        this(name) ; // 调用有一个参数的构造器
        this.age = age;
        }
	public String getInfo(){
		return "姓名：" + name + "，年龄：" + age ;
        } 
}

/*
	可以在类的构造器中使用"this(形参列表)"的方式，调用本类中重载的其他的构造器！
	明确：构造器中不能通过"this(形参列表)"的方式调用自身构造器
	如果一个类中声明了n个构造器，则最多有 n - 1个构造器中使用了"this(形参列表)"
	"this(形参列表)"必须声明在类的构造器的首行！
	在类的一个构造器中，最多只能声明一个"this(形参列表)"
*/
```

<hr>

## 四、继承

`class Subclass extends SuperClass{ }`

子类不能直接访问父类中私有的(private)的成员变量和方法

### 1.方法的重写

​	在子类中可以根据需要对从父类中继承来的方法进行改造，也称为方法的重置、覆盖。在程序执行时，子类的方法将覆盖父类的方法。

+ 子类重写的方法必须和父类被重写的方法具有相同的方法名称、参数列表
+ 子类重写的方法的返回值类型不能大于父类被重写的方法的返回值类型
+ 子类重写的方法使用的访问权限不能小于父类被重写的方法的访问权限
+ 子类不能重写父类中声明为private权限的方法
+ 子类方法抛出的异常不能大于父类被重写方法的异常
+ 子类与父类中同名同参数的方法必须同时声明为非static的(即为重写)，或者同时声明为static的（不是重写）。因为static方法是属于类的，子类无法覆盖父类的方法。

<hr>

### 2.super

```java
//调用父类的构造器
/* 
	子类中所有的构造器默认都会访问父类中空参数的构造器 
 	当父类中没有空参数的构造器时，子类的构造器必须通过this(参数列表)或者super(参数列表)语句指定调用本类或者父类中相应的构造器。同时，只能”二选一”，且必须放在构造器的首行 
 	如果子类构造器中既未显式调用父类或本类的构造器，且父类中又没有无参的构造器，则编译出错
 */
```

<hr>

## 五、多态

### 1.对象的多态性

​	**父类的引用指向子类的对象,子类的对象可以替代父类的对象使用**

​	Java引用变量有两个类型：**编译时类型**和**运行时类型**。编译时类型由声明该变量时使用的类型决定，运行时类型由实际赋给该变量的对象决定。简称：编译看左边，运行看右边。 

+ 若编译时类型和运行时类型不一致，就出现了对象的多态性(Polymorphism)

+ 多态情况下，“看左边”：看的是父类的引用（父类中不具备子类特有的方法） ；

  ​                       “看右边”：看的是子类的对象（实际运行的是子类重写父类的方法）

```java
Person p = new Student();
Object o = new Person();//Object类型的变量o，指向Person类型的对象
o = new Student(); //Object类型的变量o，指向Student类型的对象
```

​	子类可看做是特殊的父类，所以父类类型的引用可以指向子类的对象：向上转型(upcasting)

<hr>

**一个引用类型变量如果声明为父类的类型，但实际引用的是子类对象，那么该变量就不能再访问子类中添加的属性和方法**

```java
Student m = new Student();
m.school = “pku”;//合法,Student类有school成员变量

Person e = new Student();
e.school = “pku”;//非法,Person类没有school成员变量
//属性是在编译时确定的，编译时e为Person类型，没有school成员变量，因而编译错误
```



### 2.虚拟方法调用

​	子类中定义了与父类同名同参数的方法，在多态情况下，将此时父类的方法称为虚拟方法，父类根据赋给它的不同子类对象，动态调用属于子类的该方法。这样的方法调用在编译期是无法确定的。

```java
Person e = new Student();
e.getInfo(); //调用Student类的getInfo()方法
//编译时e为Person类型，而方法的调用是在运行时确定的，所以调用的是Student类的getInfo()方法。——动态绑定
```



### 3.小结

​	**多态作用：**提高了代码的通用性，常称作接口重用

​	**前提：**需要存在继承或者实现关系；有方法的重写

​	**成员方法：**

​		 编译时：要查看引用变量所声明的类中是否有所调用的方法。

​		 运行时：调用实际new的对象所属的类中的重写方法。 

​	 **成员变量：**

​		 不具备多态性，只看引用变量所声明的类

<hr>

**instanceof**

x instanceof A：检验x是否为类A的对象，返回值为boolean型。

+ 要求x所属的类与类A必须是子类和父类的关系，否则编译错误。
+ 如果x属于类A的子类B，x instanceof A值也为true。

<hr>

**对象类型转换**

对Java对象的强制类型转换称为造型

+ 从子类到父类的类型转换可以自动进行
+ 从父类到子类的类型转换必须通过造型实现
+ 无继承关系的引用类型间的转换是非法的
+ 在造型前可以使用 instanceof 操作符测试一个对象的类型

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220127135554949.png)

<hr>

```java
//继承成员变量和继承方法的区别
public class Demo {
    public static void main(String[] args) {
        Sub s = new Sub();
        System.out.println(s.count); //20
        s.display();  //20
        
        Base b = s;
        System.out.println(b == s); //true
        System.out.println(b.count); //10  ——成员变量没有多态性
        b.display(); //20 ——成员方法多态性
    }
}

class Base {
    int count = 10;
    public void display() {
        System.out.println(this.count);
    }
}

class Sub extends Base {
    int count = 20;
    public void display() {
        System.out.println(this.count);
    }
}
```



## 六、Object类

### 1.主要结构

<img src="https://gitee.com/doubledogss/image/raw/master/img/image-20220127142840099.png" style="zoom:50%;" />

### 2. == 和 equals()

1. **==**

```java
int it = 65;
float fl = 65.0f;
System.out.println(it == fl); //true
```

​		引用类型比较引用(是否指向同一个对象)：只有指向同一个对象时，==才返回true。

​		用“==”进行比较时，符号两边的**数据类型必须兼容**(可自动转换的基本数据类型除外)，否则编译出错

2. **equals()**
   + 所有类都继承了Object，也就获得了equals()方法。还可以重写。
   + 只能比较引用类型，其作用与“==”相同,比较是否指向同一个对象。
   + 特例：当用equals()方法进行比较时，对类**File、String、Date及包装类（Wrapper Class）**来说，是比较类型及内容而不考虑引用的是否是同一个对象；原因：在这些类中重写了Object类的equals()方法。
   + 当自定义使用equals()时，可以重写。用于比较两个对象的“内容”是否都相等。

## 七、包装类

![](https://gitee.com/doubledogss/image/raw/master/img/image-20220127145448835.png)

**基本类型、包装类与String类间的转换：**

![](https://gitee.com/doubledogss/image/raw/master/img/image-20220127145744455.png)

## 八、static

### 1.使用范围

​	在Java类中，可用static修饰属性、方法、代码块、内部类

### 2.类变量内存&静态方法

![](https://gitee.com/doubledogss/image/raw/master/img/image-20220127150525812.png)

<hr>

+ 因为不需要实例就可以访问static方法，因此static方法内部不能有this。(也不能有super)

+ static修饰的方法不能被重写

<hr>

### 3.单例设计模式

+ 对某个类只能存在一个对象实例，并且该类只提供一个取得其对象实例的方法。

+ 必须将类的构造器的访问权限设置为private，不能用new操作符在类的外部产生类的对象，但在类内部仍可以产生该类的对象。

+ 调用该类的某个静态方法以返回类内部创建的对象，静态方法只能访问类中的静态成员变量，所以，指向类内部产生的该类对象

  的变量也必须定义成静态的。

1. 饿汉式

```java
class Singleton {
	// 1.私有化构造器
	private Singleton() {
	}
	// 2.内部提供一个当前类的实例
	// 4.此实例也必须静态化
	private static Singleton single = new Singleton();
	// 3.提供公共的静态的方法，返回当前类的对象
	public static Singleton getInstance() {
		return single; 
    } 
}
```

2. 懒汉式

```java
class Singleton {
	// 1.私有化构造器
	private Singleton() {
	}
	// 2.内部提供一个当前类的实例
	// 4.此实例也必须静态化
	private static Singleton single;
	// 3.提供公共的静态的方法，返回当前类的对象
	public static Singleton getInstance() {
		if(single == null) {
			single = new Singleton();
	}
		return single; 
    } 
}
```

## 九、final、抽象类

### 1.final

​	在Java中声明**类、变量和方法**时，可使用关键字final来修饰。 

+ final标记的类不能被继承。String类、System类、StringBuffer类 。
+ final标记的方法不能被子类重写。比如：Object类中的getClass()。 
+ final标记的变量(成员变量或局部变量)即称为常量。名称大写，且只能被赋值一次。 
  + final标记的成员变量必须在声明时或在每个构造器中或代码块中显式赋值，然后才能使用。 
  + final double MY_PI = 3.14;
  + static final ：全局常量

<hr>

### 2.抽象类

+  含有抽象方法的类必须被声明为抽象类。
+  抽象类不能被实例化。抽象类是用来被继承的，抽象类的子类必须重写父类的抽象方法，并提供方法体。若没有重写全部的抽象方法，仍为抽象类。
+ 不能用abstract修饰变量、代码块、构造器；
+ 不能用abstract修饰私有方法、静态方法、final的方法、final的类。



## 十、接口

+ 接口(interface)是抽象方法和常量值定义的集合。 
  + 接口中的所有成员变量都默认是由public static final修饰的。 
  + 接口中的所有抽象方法都默认是由public abstract修饰的。 

+ 接口中没有构造器。  

+ 定义Java类的语法格式：先写extends，后写implements

​		` class SubClass extends SuperClass implements InterfaceA{ } `

+ 一个类可以实现多个接口，接口也可以继承其它接口。 

+ 实现接口的类中必须提供接口中所有方法的具体实现内容，方可实例化。否则，仍为抽象类。
+ 与继承关系类似，接口与实现类之间存在多态性。

<hr>

**Java 8中关于接口的改进**

Java 8中，你可以为接口添加静态方法和默认方法。

**静态方法：**使用 static 关键字修饰。可以通过接口直接调用静态方法，并执行其方法体。我们经常在相互一起使用的类中使用静态方法。你可以在标准库中找到像Collection/Collections或者Path/Paths这样成对的接口和类。



**默认方法：**默认方法使用 default 关键字修饰。可以通过实现类对象来调用。我们在已有的接口中提供新方法的同时，还保持了与旧版本代码的兼容性。比如：java 8 API中对Collection、List、Comparator等接口提供了丰富的默认方法。

```java
public interface AA {
	double PI = 3.14;
	public default void method() {
		System.out.println("北京");
	}
    //接口的默认方法
	default String method1() {
		return "上海";
	}
    //接口的静态方法
	public static void method2() {
		System.out.println("hello lambda!");
	} 
}
```

+ 若一个接口中定义了一个默认方法，而另外一个接口中也定义了一个同名同参数的方法（不管此方法是否是默认方法），在实现类同时实现了这两个接口时，会出现：**接口冲突。**
  + 解决办法：实现类必须覆盖接口中同名同参数的方法，来解决冲突。
+ 若一个接口中定义了一个默认方法，而父类中也定义了一个同名同参数的非抽象方法，则不会出现冲突问题。因为此时遵守：**类优先原则。**接口中具有相同名称和参数的默认方法会被忽略。

```java
//解决接口冲突
interface Filial {// 孝顺的
	default void help() {
		System.out.println("老妈，我来救你了");
    } 
}
interface Spoony {// 痴情的
	default void help() {
		System.out.println("媳妇，别怕，我来了");
    } 
}
class Man implements Filial, Spoony { 
    @Override
	public void help() {
		System.out.println("我该怎么办呢？");
		Filial.super.help();
		Spoony.super.help();
    } 
}
```

## 十一、内部类

Inner class的名字不能与包含它的外部类类名相同

### 1.成员内部类

static成员内部类和非static成员内部类

1. 成员内部类作为**类的成员**

+ 和外部类不同，Inner class还可以声明为**private**或**protected**； 
+ 可以调用外部类的结构
+ Inner class 可以声明为**static**的，但此时就不能再使用外层类的非static的成员变量；



2. 成员内部类作为**类**

+ 可以在内部定义属性、方法、构造器等结构
+ 可以声明为**abstract**类 ，因此可以被其它的内部类继承
+ 可以声明为**final**的 
+ 编译以后生成OuterClass$InnerClass.class字节码文件（也适用于局部内部类）



3. 注意

+ 非static的成员内部类中的成员不能声明为static的，只有在外部类或static的成员内部类中才可声明static成员。
+ 外部类访问成员内部类的成员，需要“内部类.成员”或“内部类对象.成员”的方式。
+ 成员内部类可以直接使用外部类的所有成员，包括私有的数据。
+ 当想要在外部类的静态成员部分使用内部类时，可以考虑内部类声明为静态的。

```java
class Outer {
	private int s;
	public class Inner {
		public void mb() {
			s = 100;
			System.out.println("在内部类Inner中s=" + s); 
            } 
    }
	public void ma() {
        Inner i = new Inner();
        i.mb();
    } 
}
public class InnerTest {
	public static void main(String args[]) {
		Outer o = new Outer();
		o.ma(); //在内部类Inner中s=100
    } 
}
```

```java
public class Outer {
    private int s = 111;
    public class Inner {
        private int s = 222;

        public void mb(int s) {
            System.out.println(s); // 局部变量s
            System.out.println(this.s); // 内部类对象的属性s
            System.out.println(Outer.this.s); // 外部类对象属性s
        }
    }
    public static void main(String args[]) {
        Outer a = new Outer();
        Outer.Inner b = a.new Inner();
        b.mb(333);
    }
}

//输出
333
222
111
```



### 2.局部内部类（不谈修饰符）

```java
class 外部类{
  	//只能在声明它的方法或代码块中使用，而且是先声明后使用。除此之外的任何地方都不能使用该类
    //但是它的对象可以通过外部方法的返回值返回使用，返回值类型只能是局部内部类的父类或父接口类型
	方法(){
		class 局部内部类{ 
        } 
    }
    
    {
		class 局部内部类{ 
        } 
    } 
}
```

+ 内部类仍然是一个独立的类，在编译之后内部类会被编译成独立的.class文件，但是前面冠以外部类的类名和$符号，以及数字编号。
+ 局部内部类可以使用外部类的成员，包括私有的。 
+ 局部内部类可以使用外部方法的局部变量，但是必须是final的。由局部内部类和局部变量的声明周期不同所致。
+ 局部内部类和局部变量地位类似，不能使用public,protected,缺省,private
+ 局部内部类不能使用static修饰，因此也不能包含静态成员



### 3.匿名内部类

匿名内部类不能定义任何静态成员、方法和类，只能创建匿名内部类的一个实例。一个匿名内部类一定是在new的后面，用其隐含实现一个接口或实现一个类。

```java
new 父类构造器（实参列表）|实现接口(){
	//匿名内部类的类体部分
}
```

 **匿名内部类的特点**

+ 匿名内部类必须继承父类或实现接口
+ 匿名内部类只能有一个对象
+ 匿名内部类对象只能使用多态形式引用

```java
interface A{
	public abstract void fun1();
}

public class Outer{
	public static void main(String[] args) {
		new Outer().callInner(new A(){
			//接口是不能new,但此处比较特殊,是子类对象实现接口，只不过没有为对象取名
			public void fun1() {
				System.out.println("implement for fun1");
            }
        });// 两步写成一步了            
    }            
    public void callInner(A a) {                
        a.fun1();                
    }
}
```



# 第5章 异常

## 一、异常处理机制

### 1.try-catch-finally

```java
try{
	...... //可能产生异常的代码
}
catch( ExceptionName1 e ){
	...... //当产生ExceptionName1型异常时的处置措施
}
catch( ExceptionName2 e ){
	...... //当产生ExceptionName2型异常时的处置措施
}
[ finally{
	...... //无论是否发生异常，catch语句中是否有return，都无条件执行的语句
} ]
```

捕获异常的有关信息：与其它对象一样，可以访问一个异常对象的成员变量或调用它的方法。

+ getMessage() 获取异常信息，返回字符串
+ printStackTrace() 获取异常类名和异常信息，以及异常出现在程序中的位置。返回值void。



### 2.throws+异常类型

声明抛出异常

+ 如果一个方法(中的语句执行时)可能生成某种异常，但是并不能确定如何处理这种异常，则此方法应显示地声明抛出异常，表明该方法将不对这些异常进行处理，而由该方法的调用者负责处理。 
+ 在方法声明中用throws语句可以声明抛出异常的列表，throws后面的异常类型可以是方法中产生的异常类型，也可以是它的父类。

```java
public void readFile(String file) throws FileNotFoundException {
	……
	// 读文件的操作可能产生FileNotFoundException类型的异常
	FileInputStream fis = new FileInputStream(file);
	……
}
```

<hr>

**重写方法声明抛出异常的原则**

重写方法不能抛出比被重写方法范围更大的异常类型。在多态的情况下， 对methodA()方法的调用，异常的捕获按父类声明的异常处理。

```java
public class A {
	public void methodA() throws IOException {
		……
    } 
}
public class B1 extends A {
	public void methodA() throws FileNotFoundException {
		……
    } 
}
public class B2 extends A {
	public void methodA() throws Exception { //报错
		……
	} 
}
```



## 二、throw

手动抛出异常

```java
//可以抛出的异常必须是Throwable或其子类的实例。
IOException e = new IOException();
throw e;
```



## 三、自定义异常

-  一般地，用户自定义异常类都是RuntimeException的子类。
-  自定义异常类通常需要编写几个重载的构造器。 
-  自定义异常需要提供serialVersionUID
-  自定义的异常通过throw抛出。 
-  自定义异常最重要的是异常类的名字，当异常出现时，可以根据名字判断异常类型。

```java
//用户自定义异常类MyException，用于描述数据取值范围错误信息。用户自己的异常类必须继承现有的异常类。
class MyException extends Exception {
	static final long serialVersionUID = 13465653435L;
	private int idnumber;
	public MyException(String message, int id) {
		super(message);
		this.idnumber = id; 
    }
	public int getId() {
		return idnumber; 
    } 
}

public class MyExpTest {
	public void regist(int num) throws MyException {
		if (num < 0)
			throw new MyException("人数为负值，不合理", 3);
		else
			System.out.println("登记人数" + num);
	}
	public void manager() {
		try {
			regist(100);
		} catch (MyException e) {
			System.out.print("登记失败，出错种类" + e.getId());
        }
        System.out.print("本次登记操作结束");
    }
    public static void main(String args[]) {
        MyExpTest t = new MyExpTest();
        t.manager();
    } 
}
```



# 第6章 多线程TODO



# 第7章 Java常用类TODO



# 第8章 枚举类与注解TODO

## 一、枚举类



## 二、注解Annotation

> - 从 JDK 5.0 开始, Java 增加了对元数据(MetaData) 的支持, 也就是Annotation(注解) 
> - Annotation 其实就是代码里的**特殊标记**, 这些标记可以在编译, 类加载, 运行时被读取, 并执行相应的处理。通过使用 Annotation, 程序员可以在不改变原有逻辑的情况下, 在源文件中嵌入一些补充信息。代码分析工具、开发工具和部署工具可以通过这些补充信息进行验证或者进行部署。
> - Annotation 可以像修饰符一样被使用, 可用于修饰包，类，构造器，方法，成员变量，参数， 局部变量的声明，这些信息被保存在 Annotation 的 “name=value” 对中。
> - 在JavaSE中，注解的使用目的比较简单，例如标记过时的功能，忽略警告等。在JavaEE/Android中注解占据了更重要的角色，例如用来配置应用程序的任何切面，代替JavaEE旧版中所遗留的繁冗代码和XML配置等。 
> - 未来的开发模式都是基于注解的，JPA是基于注解的，Spring2.5以上都是基于注解的，Hibernate3.x以后也是基于注解的，现在的Struts2有一部分也是基于注解的了，注解是一种趋势，一定程度上可以说：框架 = 注解 + 反射 + 设计模式。



自定义Annotation

> - 定义新的 Annotation 类型使用 @interface 关键字
> - 自定义注解自动继承了java.lang.annotation.Annotation接口
> - Annotation 的成员变量在 Annotation 定义中以无参数方法的形式来声明。其方法名和返回值定义了该成员的名字和类型。我们称为配置参数。类型只能是八种基本数据类型、String类型、Class类型、enum类型、Annotation类型、以上所有类型的数组。 
> - 可以在定义 Annotation 的成员变量时为其指定初始值, 指定成员变量的初始值可使用 default 关键字
> - 如果只有一个参数成员，建议使用参数名为value
> - 如果定义的注解含有配置参数，那么使用时必须指定参数值，除非它有默认值。格式是“参数名 = 参数值”，如果只有一个参数成员，且名称为value，可以省略“value=”
> -  没有成员定义的 Annotation 称为标记; 包含成员变量的 Annotation 称为元数据 Annotation
> - 注意：自定义注解必须配上注解的信息处理流程才有意义。



 JDK中的元注解

> JDK 的元 Annotation 用于修饰其他 Annotation 定义
>
> JDK5.0提供了4个标准的meta-annotation类型，分别是：
>
> - Retention
> - Target
> - Documented
> - Inherited

元数据的理解：String name = “atguigu”;



@Retention: 只能用于修饰一个 Annotation 定义, 用于指定该 Annotation 的生命周期, @Rentention 包含一个 RetentionPolicy 类型的成员变量，使用@Rentention 时必须为该 value 成员变量指定值: 

- RetentionPolicy.SOURCE:在源文件中有效（即源文件保留），编译器直接丢弃这种策略的注释
- RetentionPolicy.CLASS:在class文件中有效（即class保留） ， 当运行 Java 程序时, JVM 不会保留注解。 这是默认值
- RetentionPolicy.RUNTIME:在运行时有效（即运行时保留），当运行 Java 程序时, JVM 会保留注释。程序可以通过反射获取该注释。

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220301100531395.png)

<hr>

@Target: 用于修饰 Annotation 定义, 用于指定被修饰的 Annotation 能用于修饰哪些程序元素。 @Target 也包含一个名为 value 的成员变量。

![](https://doubledogs.oss-cn-beijing.aliyuncs.com/2022_images/image-20220301100808241.png)

<hr>

@Documented: 用于指定被该元 Annotation 修饰的 Annotation 类将被javadoc 工具提取成文档。默认情况下，javadoc是不包括注解的。 

定义为Documented的注解必须设置Retention值为RUNTIME。 

<hr>

@Inherited: 被它修饰的 Annotation 将具有继承性。如果某个类使用了被@Inherited 修饰的 Annotation, 则其子类将自动具有该注解。
 比如：如果把标有@Inherited注解的自定义的注解标注在类级别上，子类则可以继承父类类级别的注解
 实际应用中，使用较少









# 第9章 集合

## 一、Java集合框架

Java 集合可分为 Collection 和 Map 两种体系

**Collection接口：**单列数据，定义了存取一组对象的方法的集合

+ **List：**元素有序、可重复的集合

+ **Set：**元素无序、不可重复的集合

 **Map接口：**双列数据，保存具有映射关系“key-value对”的集合

## 二、Collection接口

单列数据，定义了存取一组对象的方法的集合

![](https://gitee.com/doubledogss/image/raw/master/img/image-20220129094659031.png)

<hr>

### 1. Collection接口

- Collection 接口是 List、Set 和 Queue 接口的父接口，该接口里定义的方法既可用于操作 Set 集合，也可用于操作 List 和 Queue 集合。 
- JDK不提供此接口的任何直接实现，而是提供更具体的子接口(如：Set和List)实现。 
- 从 JDK 5.0 增加了泛型以后，Java 集合可以记住容器中对象的数据类型。

​	**接口方法**

(1) 添加

+ add(Object obj)
+ addAll(Collection coll) 

(2) 获取有效元素的个数

+ int size()

(3) 清空集合

+ void clear()

4、是否是空集合 

+ boolean isEmpty()

5、是否包含某个元素

+ boolean contains(Object obj)：是通过元素的equals方法来判断是否是同一个对象

+ boolean containsAll(Collection c)：也是调用元素的equals方法来比较的。拿两个集合的元素挨个比较。

6、删除

+ boolean remove(Object obj) ：通过元素的equals方法判断是否是要删除的那个元素。只会删除找到的第一个元素

+ boolean removeAll(Collection coll)：取当前集合的差集

7、取两个集合的交集

+ boolean retainAll(Collection c)：把交集的结果存在当前集合中，不影响c 

8、集合是否相等

+ boolean equals(Object obj)

9、转成对象数组

+ Object[] toArray()

10、获取集合对象的哈希值

+ hashCode()

11、遍历

+ iterator()：返回迭代器对象，用于集合遍历

<hr>

​	**Iterator迭代器接口**

​	Iterator对象称为迭代器(设计模式的一种)，主要用于遍历 Collection 集合中的元素。

+ Collection接口继承了java.lang.Iterable接口，该接口有一个iterator()方法，那么所有实现了Collection接口的集合类都有一个iterator()方法，用以返回一个实现了Iterator接口的对象。 

+ Iterator 仅用于遍历集合，Iterator 本身并不提供承装对象的能力。如果需要创建Iterator 对象，则必须有一个被迭代的集合。

+ 集合对象每次调用iterator()方法都得到一个全新的迭代器对象，默认游标都在集合的第一个元素之前。

​	**Iterator接口的方法**

+ hasNext()

+ next()

  在调用it.next()方法之前必须要调用it.hasNext()进行检测。若不调用，且下一条记录无效，直接调用it.next()会抛出NoSuchElementException异常。

  ```java
  //hasNext():判断是否还有下一个元素
  while(iterator.hasNext()){
  	//next():①指针下移 ②将下移以后集合位置上的元素返回
  	System.out.println(iterator.next());
  }
  ```

+ remove()

```java
Iterator iter = coll.iterator();//回到起点
while(iter.hasNext()){
	Object obj = iter.next();
	if(obj.equals("Tom")){
		iter.remove();
	} 
}
//Iterator可以删除集合的元素，但是是遍历过程中通过迭代器对象的remove方法，不是集合对象的remove方法。 
//如果还未调用next()或在上一次调用next方法之后已经调用了remove方法，再调用remove都会报IllegalStateException。
```

​	**foreach循环遍历集合元素**

+ Java 5.0 提供了 foreach 循环迭代访问 Collection和数组。

+ 遍历操作不需获取Collection或数组的长度，无需使用索引访问元素。

+ 遍历集合的底层调用Iterator完成操作。

+ foreach还可以用来遍历数组。

```java
//遍历persons
for(Person person : persons){
    System.out.println(person.getName());
}
```

<hr>

### 2. List接口

元素有序、可重复的集合。 List容器中的元素都对应一个整数型的序号记载其在容器中的位置，可以根据序号存取容器中的元素。

**List接口方法**

+ void add(int index, Object ele)：在index位置插入ele元素
+ boolean addAll(int index, Collection eles)：从index位置开始将eles中的所有元素添加进来
+ Object get(int index)：获取指定index位置的元素
+ int indexOf(Object obj)：返回obj在集合中首次出现的位置
+ int lastIndexOf(Object obj)：返回obj在当前集合中末次出现的位置
+ Object remove(int index)：移除指定index位置的元素，并返回此元素
+ Object set(int index, Object ele)：设置指定index位置的元素为ele
+ List subList(int fromIndex, int toIndex)：返回从fromIndex到toIndex位置的子集合

<hr>

#### 2.1 ArrayList类

+ ArrayList 是 List 接口的典型实现类、主要实现类

+ 本质上，ArrayList是对象引用的一个<font color='red'>”变长”数组</font>
+ ArrayLis的JDK1.8之前与之后的实现区别？
  +  JDK1.7：ArrayList像饿汉式，直接创建一个初始容量为10的数组
  +  JDK1.8：ArrayList像懒汉式，一开始创建一个长度为0的数组，当添加第一个元素时再创建一个始容量为10的数组
+ Arrays.asList(…) 方法返回的 List 集合，既不是 ArrayList 实例，也不是Vector 实例。 Arrays.asList(…) 返回值是一个固定长度的 List 集合

<hr>

#### 2.2 LinkedList类

​	<font color='red'>双向链表</font>，内部没有声明数组，而是定义了Node类型的first和last，用于记录首末元素。同时，定义内部类Node，作为LinkedList中保存数据的基本结构。Node除了保存数据，还定义了两个变量：

+ prev变量记录前一个元素的位置

+ next变量记录下一个元素的位置

​	对于**频繁的插入或删除元素**的操作，建议使用LinkedList类，效率较高

​	 新增方法：

- void addFirst(Object obj) 
- void addLast(Object obj)
- Object getFirst()
- Object getLast()
- Object removeFirst()
- Object removeLast()

#### 2.3 Vector类

​	Vector 是一个古老的集合，JDK1.0就有了。大多数操作与ArrayList相同，区别之处在于Vector是线程安全的。在各种list中，最好把ArrayList作为缺省选择。当插入、删除频繁时，使用LinkedList；Vector总是比ArrayList慢，所以尽量避免使用。
​	新增方法：

+ void addElement(Object obj) 

-  void insertElementAt(Object obj,int index)
-  void setElementAt(Object obj,int index)
-  void removeElement(Object obj)
- void removeAllElements()

<hr>

**ArrayList和LinkedList的异同**

+ 二者都线程不安全，相对线程安全的Vector，执行效率高。

+ ArrayList是实现了基于<font color='red'>动态数组</font>的数据结构，LinkedList基于<font color='red'>链表</font>的数据结构。

+ 对于随机访问get和set，ArrayList优于LinkedList，因为LinkedList要移动指针。
+ 对于新增和删除操作add(特指插入)和remove，LinkedList比较占优势，因为ArrayList要移动数据。

**ArrayList和Vector的区别**

+ Vector和ArrayList几乎是完全相同的,唯一的区别在于Vector是同步类(synchronized)，属于强同步类。因此开销就比ArrayList要大，访问要慢。

+ 正常情况下，大多数的Java程序员使用ArrayList而不是Vector，因为同步完全可以由程序员自己来控制。

+ Vector每次扩容请求其大小的2倍空间，而ArrayList是1.5倍。Vector还有一个子类Stack。

<hr>

### 3. Set接口

​	元素无序、不可重复的集合

+ Set接口是Collection的子接口，set接口没有提供额外的方法

+ Set 集合不允许包含相同的元素，如果试把两个相同的元素加入同一个Set 集合中，则添加操作失败。

+  Set 判断两个对象是否相同不是使用 == 运算符，而是根据 equals() 方法

<hr>

#### 3.1 HashSet类

+ HashSet 是 Set 接口的典型实现，大多数时候使用 Set 集合时都使用这个实现类。
+ HashSet 按 Hash 算法来存储集合中的元素，因此具有很好的存取、查找、删除性能。 



​	**HashSet具有以下特点：**

+ 不能保证元素的排列顺序
+ HashSet 不是线程安全的
+ 集合元素可以是 null



​	**HashSet** **集合判断两个元素相等的标准**：

​	两个对象通过 hashCode() 方法比较相等，并且两个对象的 equals() 方法返回值也相等。 

​	对于存放在Set容器中的对象，对应的类一定要重写equals()和hashCode(Objectobj)方法，以实现对象相等规则。即：“相等的对象必须具有相等的散列码”。



​	**向HashSet中添加元素的过程：**

+ 当向 HashSet 集合中存入一个元素时，HashSet 会调用该对象的 hashCode() 方法来得到该对象的 hashCode 值，然后根据 hashCode 值，通过某种散列函数决定该对象在 HashSet 底层数组中的存储位置。（这个散列函数会与底层数组的长度相计算得到在数组中的下标，并且这种散列函数计算还尽可能保证能均匀存储元素，越是散列分布，该散列函数设计的越好） 
+ 如果两个元素的hashCode()值相等，会再继续调用equals方法，如果equals方法结果为true，添加失败；如果为false，那么会保存该元素，但是该数组的位置已经有元素了，那么会通过链表的方式继续链接。 
+ 如果两个元素的 equals() 方法返回 true，但它们的 hashCode() 返回值不相等，hashSet 将会把它们存储在不同的位置，但依然可以添加成功。

<img src="https://gitee.com/doubledogss/image/raw/master/img/image-20220129121642703.png" style="zoom:80%;" />

<hr>

#### 3.2 LinkedHashSet类TODO

#### 3.3 TreeSet类TODO



## 三、Map接口

双列数据，保存具有映射关系“key-value对”的集合

![](https://gitee.com/doubledogss/image/raw/master/img/image-20220129094746246.png)

+ Map 中的 key 和 value 都可以是任何引用类型的数据
+ Map 中的 key 用Set来存放，**不允许重复**，即同一个 Map 对象所对应的类，须重写hashCode()和equals()方法
+ 常用String类作为Map的“键”
+ key 和 value 之间存在单向一对一关系，即通过指定的 key 总能找到唯一的、确定的 value

**接口方法**

 添加、删除、修改操作：

-  Object put(Object key,Object value)：将指定key-value添加到(或修改)当前map对象中
-  void putAll(Map m):将m中的所有key-value对存放到当前map中 
-  Object remove(Object key)：移除指定key的key-value对，并返回value
-  void clear()：清空当前map中的所有数据

 **元素查询的操作：**

-  Object get(Object key)：获取指定key对应的value
-  boolean containsKey(Object key)：是否包含指定的key
-  boolean containsValue(Object value)：是否包含指定的value
-  int size()：返回map中key-value对的个数
-  boolean isEmpty()：判断当前map是否为空
-  boolean equals(Object obj)：判断当前map和参数对象obj是否相等

 **元视图操作的方法：**

-  Set keySet()：返回所有key构成的Set集合
-  Collection values()：返回所有value构成的Collection集合
-  Set entrySet()：返回所有key-value对构成的Set集合。映射关系的类型是Map.Entry类型，它是Map接口的内部接口

<hr>

### 1. HashMap类

- HashMap是 Map 接口**使用频率最高**的实现类。
- 允许使用null键和null值，与HashSet一样，不保证映射的顺序。
- 所有的key构成的集合是Set:无序的、不可重复的。所以，key所在的类要重写：equals()和hashCode()
- 所有的value构成的集合是Collection:无序的、可以重复的。所以，value所在的类要重写：equals()
- 一个key-value构成一个entry
- 所有的entry构成的集合是Set:无序的、不可重复的
- HashMap 判断两个key相等的标准是：两个 key 通过 equals() 方法返回 true，hashCode 值也相等。
- HashMap 判断两个value相等的标准是：两个 value 通过 equals() 方法返回 true。

​	**存储结构**：数组+链表+红黑树



### 2.LinkedHashMap类TODO

### 3.TreeMap类TODO

### 4.Hashtable类TODO

### 5.Properties类TODO



## 四、Collections工具类TODO





# 第10章 泛型TODO



# 第11章 IO流TODO



# 第12章 网络编程TODO



# 第13章 反射TODO



# 第14章 Java8-11 TODO

## 一、Java8的其他特性





### 4. Stream API

> - 实际开发中，项目中多数数据源都来自于Mysql，Oracle等。但现在数据源可以更多了，有MongDB，Radis等，而这些NoSQL的数据就需要Java层面去处理。 
>
> - Stream 和 Collection 集合的区别：Collection 是一种静态的内存数据结构，而 Stream 是有关计算的。前者是主要面向内存，存储在内存中，后者主要是面向 CPU，通过 CPU 实现计算。
>
> - Stream是数据渠道，用于操作数据源（集合、数组等）所生成的元素序列。
>
>   集合讲的是数据，Stream讲的是计算！
>
>   **注意：**
>
>   ①Stream 自己不会存储元素。
>
>   ②Stream 不会改变源对象。相反，他们会返回一个持有结果的新Stream。 
>
>   ③Stream 操作是延迟执行的。这意味着他们会等到需要结果的时候才执行。

#### 4.1 Stream的操作步骤

1. 创建Stream：一个数据源（如：集合、数组），获取一个流

2. 中间操作：一个中间操作链，对数据源的数据进行处理

3. 终止操作（终端操作）：一旦执行终止操作，就执行中间操作链，并产生结果。之后，不会再被使用



#### 4.2 创建Stream的方式

**1. 通过集合**

Java8 中的 Collection 接口被扩展，提供了两个获取流的方法： 

- default Stream<E> stream() : 返回一个顺序流

- default Stream<E> parallelStream() : 返回一个并行流



**2. 通过数组**

Java8 中的 Arrays 的静态方法 stream() 可以获取数组流：

- static <T> Stream<T> stream(T[] array): 返回一个流

重载形式，能够处理对应基本类型的数组：

- public static IntStream stream(int[] array)

- public static LongStream stream(long[] array)

- public static DoubleStream stream(double[] array)



**3. 通过Stream的of()**

可以调用Stream类静态方法 of(), 通过显示值创建一个流。它可以接收任意数量的参数。

- public static<T> Stream<T> of(T... values) :返回一个流



**4. 创建无限流**

可以使用静态方法 Stream.iterate() 和 Stream.generate(), 创建无限流。

- 迭代

public static<T> Stream<T> iterate(final T seed, final UnaryOperator<T> f)

- 生成

public static<T> Stream<T> generate(Supplier<T> s)



#### 4.3 Stream的中间操作

