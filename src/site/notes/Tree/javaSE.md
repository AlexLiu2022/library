---
{"dg-publish":true,"dg-path":"  javaSE.md","permalink":"/java-se/","tags":["CS/programming-languages/java"],"created":"2022-08-02T11:14:51.000+08:00","updated":"2023-08-27T13:29:42.232+08:00"}
---


# Java基础

## Java概述

### JDK
 
名词解释：

- JDK : Java Development Kit

- JRE : Java Runtime Environment

- JVM: Java Virtual Machine 

包含关系：

- JDK= JRE + development tools (java javac etc.)

- JRE= JVM + standard classes, packages

## 转义字符

## 开发规范

### 注释

#### 单行注释

对多行进行注释对快捷键：

command + /

#### 多行注释

Format:
```java
/*
     comment content
															 */															 																	
```
#### 文档注释

Format:

```java
/**
*  @author Alex Liu
*  @verson 1.0
*  @label  content
**/ 
```
show comments using command line

```shell
javadoc -d foldername  -labelA -labelB sourcecode
```

 ### 代码规范

1. 类、方法的注释，要以**javadoc**的方式来写
2.  非Java Doc的注释是给代码的维护者看的，着重告诉读者为什么这样写，如何修改，有何注意事项... 
3.  使用tab操作实现缩进，默认整体右移，有时用shift+tab整体左移
4.  运算符和 = 两边习惯性各加一个空格
5.  源文件使用==utf-8==编码
6.  行宽度不要超过80字符
7.  代码编写次行风格或行尾风格

### Java API

API（Application Programming Interface, 应用程序编程接口）

#### 查阅方法
按照 软件包 >> 接口/类/异常/... >>字段/ 成员方法（方法）/构造器（构造方法)/...  的层次查找
或直接索引

## 变量

### Java数据类型

#### 基本数据类型

##### 数值型

- 整数类型：`byte`[1] `short`[2] `int`[4] `long`[8]

	Java整型常量默认为int型，==声明long型常量需后加'l'或'L'==
	eg. long a = 1L (不加也可以是因为发生了自动类型转换)

- 浮点类型：`float`[4] `double`[8]

	Java的浮点型常量默认为`double`型，==声明`float`型常量，须后加'f'或'F'==

##### 字符型

`char`[2]

##### 布尔型

`boolean`[1]

#### 引用数据类型

##### 类（class）
##### 接口（interface）
#####  [ ] 数组

\[ \](array)

###  变量基本使用

### 数据类型转换

#### 自动类型转换
	
==有多种类型的数据混合运算时，系统首先自动将所有数据转换成容量最大的那种数据类型，然后再进行计算。==

把精度（容量）大的数据类型赋值给精度（容量）小的数据类型时会报错，反之会进行自动类型转换。

`byte` , `short`和`char`之间不会相互自动转换

把具体数赋给`byte` 先判断范围 符合范围即可  如 `byte = 10` 正确

`byte`,`short`,`char`三者可以计算，在计算时首先转换为`int`类型。

`boolean`不参与转换

自动提升原则：表达式结果的类型自动提升为操作数中最大的类型


#### 强制类型转换

## 运算符
## 控制结构
##  数组、排序和查找

# 面向对象编程

（Object Oriented Programming) 简称OOP

## 类与对象


类 `class` 是一种抽象的==数据类型== 包含了**属性**与**方法**（行为）

如人类 包含了名字属性 身高属性 吃、睡等行为 

对象object 是类的具体实例 instance 如男孩是一个类 ,而一群男孩中每一个男孩都是男孩类具体的实例对象

### 定义class

在Java中，创建一个类，例如，给这个类命名为Person，就是定义一个class：

```java
class Person {
    public String name;
    public int age;
}
```

一个`class`可以包含多个字段（`field`），字段用来描述一个类的特征。上面的`Person`类，
定义了两个字段，一个是`String`类型的字段，命名为name，一个是`int`类型的字段，命名为age。因此，通过class，把一组数据汇集到一个对象上，实现了数据封装。

`public` 是用来修饰字段的，它表示这个字段可以被外部访问。

### 创建实例

`new`操作符可以创建一个实例，然后需要定义一个引用类型的变量来指向这个实例

```java
Person ming = new Person();
```

有了指向实例的变量,就可以通过这个变量来操作实例。访问实例变量可以用`变量.字段`

### 对象在内存中存在形式

不同的instance拥有相同的字段，并各自都有一份独立的数据，互不干扰。

## 方法

直接操作`field`，容易造成逻辑混乱。为了避免外部代码直接访问`field`，可以用`private`修饰`field`，拒绝外部访问。

要利用无法被被外部访问的`field`，给它们赋值并读取它们的值，我们需要使用方法（`method`）来让外部代码可以间接修改`field`.

```java
public class Main {
    public static void main(String[] args) {
        Person ming = new Person();
        ming.setName("Xiao Ming"); // 设置name
        ming.setAge(12); // 设置age
        System.out.println(ming.getName() + ", " + ming.getAge());
    }
}

class Person {
    private String name;
    private int age;

    public String getName() {
        return this.name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return this.age;
    }

    public void setAge(int age) {
        if (age < 0 || age > 100) {
            throw new IllegalArgumentException("invalid age value");
        }
        this.age = age;
    }
}
```

虽然外部代码不能直接修改private字段，但是，外部代码可以调用方法setName()和setAge()来间接修改private字段。在方法内部，就有机会检查参数对不对。比如，setAge()就会检查传入的参数，参数超出了范围，直接报错。这样，外部代码就没有任何机会把age设置成不合理的值。

对setName()方法同样可以做检查，例如，不允许传入null和空字符串： 

```java
public void setName(String name) {
    if (name == null || name.isBlank()) {
        throw new IllegalArgumentException("invalid name");
    }
    this.name = name.strip(); // 去掉首尾空格
}
```

同样，外部代码不能直接读取`private`字段，但可以通过`getName()`和`getAge()`间接获取private字段的值。

所以，一个类通过定义方法，就可以给外部代码暴露一些操作的接口，同时，内部自己保证逻辑一致性。

调用方法的语法是`实例变量.方法名(参数);` 一个方法调用就是一个语句，所以不要忘了在末尾加;。例如：`ming.setName("Xiao Ming");`。

### 定义方法

从上面的代码可以看出，定义方法的语法是：

```java
修饰符 方法返回类型 方法名(方法参数列表) {
    若干方法语句;
    return 方法返回值;
}
```

方法返回值通过`return`语句实现，如果没有返回值，返回类型设置为`void`，可以省略`return`。

### private method 

和`private`字段一样，`private`方法不允许外部调用，定义`private`方法的理由是内部方法是可以调用private方法.

方法可以封装一个类的对外接口，调用方不需要知道也不关心某实例在内部到底有没有某字段

### this变量
在方法内部，可以使用一个隐含的变量`this`，它始终指向当前实例。因此，通过`this.field`就可以访问当前实例的字段。

如果没有命名冲突，可以省略`this` 但是，如果有局部变量和字段重名，那么局部变量优先级更高，就必须加上`this`

### method arguments

方法可以包含0个或任意个参数。方法参数用于接收传递给方法的变量值。调用方法时，必须严格按照参数的定义一一传递

### 可变参数

可变参数用`类型...`定义，可变参数相当于数组类型
```java
class Group {
    private String[] names;

    public void setNames(String... names) {
        this.names = names;
    }
}
```
上面的setNames()就定义了一个可变参数。调用时，可以这么写：

```java
Group g = new Group();
g.setNames("Xiao Ming", "Xiao Hong", "Xiao Jun"); // 传入3个String
g.setNames("Xiao Ming", "Xiao Hong"); // 传入2个String
g.setNames("Xiao Ming"); // 传入1个String
g.setNames(); // 传入0个String
```

完全可以把可变参数改写为`String[]`类型：
```java
class Group {
    private String[] names;

    public void setNames(String[] names) {
        this.names = names;
    }
}
```
但是，调用方需要自己先构造String[]，比较麻烦。例如：
```java
Group g = new Group();
g.setNames(new String[] {"Xiao Ming", "Xiao Hong", "Xiao Jun"}); // 传入1个String[]
```
另一个问题是，调用方可以传入`null`：
```java
Group g = new Group();
g.setNames(null);
```
而可变参数可以保证无法传入`null`，因为传入0个参数时，接收到的实际值是一个空数组而不是null  ( It is a value of the reference variable ,not a String)

### 参数绑定

调用方把参数传递给实例方法时，调用时传递的值会按参数位置一一绑定。

基本类型参数的传递，是调用方值的复制。==双方各自的后续修改，互不影响。==

引用类型参数的传递，调用方的变量，和接收方的参数变量，指向的是同一个对象。==双方任意一方对这个对象的修改，都会影响对方==

==NOTE!== String是不变类型，赋值后重新开辟内存空间存储新值 所以后续修改也不会影响

## 构造方法

为了创建对象实例时就把内部字段全部初始化为合适的值，我们需要构造方法。

构造方法的名称就是类名。构造方法的参数没有限制，在方法内部，也可以编写任意语句。但是，和普通方法相比，构造方法没有返回值（也没有`void`），调用构造方法，必须用`new`操作符。

### 默认构造方法

如果一个类没有定义构造方法，编译器会自动生成一个默认构造方法，它没有参数，也没有执行语句，类似这样：
```java
class Person {
    public Person() {
    }
}
```

==如果自定义了一个构造方法，编译器就不再自动创建默认构造方法==

如果既要能使用带参数的构造方法，又想保留不带参数的构造方法，那么只能把两个构造方法都定义出来：
```java
public class Main {
    public static void main(String[] args) {
        Person p1 = new Person("Xiao Ming", 15); // 既可以调用带参数的构造方法
        Person p2 = new Person(); // 也可以调用无参数构造方法
    }
}

class Person {
    private String name;
    private int age;

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public String getName() {
        return this.name;
    }

    public int getAge() {
        return this.age;
    }
}

// 构造方法
```

也可以对字段直接进行初化

在Java中，创建对象实例的时候，按照如下顺序进行初始化：

1. 先初始化字段，例如，`int age = 10`;表示字段初始化为10，`double salary`;表示字段默认初始化为0，`String name`;表示引用类型字段默认初始化为null；

2. 执行构造方法的代码进行初始化。

因此，构造方法的代码由于后运行，实例的字段值最终由构造方法的代码确定。

### 多构造方法

可以定义多个构造方法，在通过`new`操作符调用的时候，编译器通过构造方法的参数数量、位置和类型自动区分

一个构造方法可以调用其他构造方法，这样做的目的是便于代码复用。调用其他构造方法的语法是`this(…);`

## 方法重载

在一个类中，我们可以定义多个方法。如果有一系列方法，它们的功能都是类似的，只有参数有所不同，那么，可以把这一组方法名做成同名方法。例如，在Hello类中，定义多个hello()方法：
```java
class Hello {
	public void hello() {
		System.out.println("Hello, world!");
	}

	public void hello(String name) {
		System.out.println("Hello, " + name + "!");
	}

	public void hello(String name, int age) {
		if (age < 18) {
				System.out.println("Hi, " + name + "!");
		} else {
				System.out.println("Hello, " + name + "!");
		}
	}
}
```
这种==方法名相同，但各自的参数不同==，称为**方法重载（Overload）**

注意：==方法重载的返回值类型通常都是相同的。==

方法重载的目的是，功能类似的方法使用同一名字，更容易记住，因此，调用起来更简单。

举个例子，String类提供了多个重载方法indexOf()，可以查找子串：

- `int indexOf(int ch)`：根据字符的Unicode码查找；

- ` int indexOf(String str)`：根据字符串查找；

- `int indexOf(int ch, int fromIndex)`：根据字符查找，但指定起始位置；

- ` int indexOf(String str, int fromIndex)`根据字符串查找，但指定起始位置。



## 继承

继承是面向对象编程中非常强大的一种机制，它首先可以复用代码。当我们让`Student`从`Person`继承时，`Student`就获得了`Person`的所有功能，我们只需要为`Student`编写新增的功能。

Java使用`extends`关键字来实现继承：
```java
class Person {
    private String name;
    private int age;

    public String getName() {...}
    public void setName(String name) {...}
    public int getAge() {...}
    public void setAge(int age) {...}
}

class Student extends Person {
    // 不要重复name和age字段/方法,
    // 只需要定义新增score字段/方法:
    private int score;

    public int getScore() { … }
    public void setScore(int score) { … }
}
```
可见，通过继承，`Student`只需要编写额外的功能，不再需要重复代码。

==注意：子类自动获得了父类的所有字段，严禁定义与父类重名的字段==

在OOP的术语中，我们把`Person`称为超类（super class），父类（parent class），基类（base class），把`Student`称为子类（sub class），扩展类（extended class)

### 继承树

在Java中，没有明确写extends的类，编译器会自动加上extends Object。所以，任何类，除了Object，都会继承自某个类。下图是Person、Student的继承树：
```
┌───────────┐
|	 Object   |                               
└───────────┘
      ▲
      │
┌───────────┐
|  Person   |                                
└───────────┘
      ▲
      │
┌───────────┐
|  Student  |                             
└───────────┘
```
Java只允许一个`class`继承自一个类，因此，一个类有且仅有一个父类。只有`Object`特殊，它没有父类。

###  protected

子类无法访问父类的`private`字段或者`private`方法,为了让子类可以访问父类的字段，我们需要把`private`改为`protected`。用`protected`修饰的字段可以被子类访问

`protected`关键字可以==把字段和方法的访问权限控制在继承树内部==，一个`protected`字段和方法可以被其子类，以及子类的子类所访问

### super
`super`关键字表示父类（超类）。子类引用父类的字段时，可以用`super.fieldName`。例如：
```java
class Student extends Person {
    public String hello() {
        return "Hello, " + super.name;
    }
}
```
实际上，这里使用`super.name`，或者`this.name`，或者`name`，效果都是一样的。编译器会自动定位到父类的`name`字段。

但是，在某些时候，就必须使用`super`,即：

如果父类没有默认的构造方法，子类就必须显式调用`super()`并给出参数以便让编译器定位到父类的一个合适的构造方法。(在Java中，任何class的构造方法，第一行语句必须是调用父类的构造方法。)

==子类不会继承任何父类的构造方法。子类默认的构造方法是编译器自动生成的，不是继承的==

### 阻止继承

正常情况下，只要某个`class`没有`final`修饰符，那么任何类都可以从该`class`继承。

从Java 15开始，允许使用`sealed`修饰`class`，并通过`permits`明确写出能够从该class继承的子类名称。

例如，定义一个Shape类：

```java
public sealed class Shape permits Rect, Circle, Triangle {
    ...
}
```
上述Shape类就是一个`sealed`类，它只允许指定的3个类继承它。如果写：
```java
public final class Rect extends Shape {...}
```
是没问题的，因为`Rect`出现在`Shape`的`permits`列表中。但是，如果定义一个`Ellipse`就会报错：
```java
public final class Ellipse extends Shape {...}
// Compile error: class is not allowed to extend sealed class: Shape
```
原因是`Ellipse`并未出现在`Shape`的`permits`列表中。这种`sealed`类主要用于一些框架，防止继承被滥用。

`sealed`类在Java 15中目前是预览状态，要启用它，必须使用参数`--enable-preview`和`--source 15`。

### 向上转型

把一个子类类型安全地变为父类类型的赋值，被称为向上转型（upcasting）。

向上转型实际上是把一个子类型安全地变为更加抽象的父类型：
```java
Student s = new Student();
Person p = s; // upcasting, ok
Object o1 = p; // upcasting, ok
Object o2 = s; // upcasting, ok
```
注意到继承树是Student > Person > Object，所以，可以把Student类型转型为Person，或者更高层次的Object。

### 向下转型

和向上转型相反，如果把一个父类类型强制转型为子类类型，就是向下转型（downcasting）。例如：
```java
Person p1 = new Student(); // upcasting, ok
Person p2 = new Person();
Student s1 = (Student) p1; // ok
Student s2 = (Student) p2; // runtime error! ClassCastException!
```
如果测试上面的代码，可以发现：

Person类型p1实际指向Student实例，Person类型变量p2实际指向Person实例。在向下转型的时候，==把p1转型为Student会成功，因为p1确实指向Student实例，把p2转型为Student会失败，因为p2的实际类型是Person，不能把父类变为子类，因为子类功能比父类多，多的功能无法凭空变出来。==

因此，向下转型很可能会失败。失败的时候，Java虚拟机会报`ClassCastException`

为了避免向下转型出错，Java提供了`instanceof`操作符，可以先判断一个实例究竟是不是某种类型：
```java
Person p = new Person();
System.out.println(p instanceof Person); // true
System.out.println(p instanceof Student); // false

Student s = new Student();
System.out.println(s instanceof Person); // true
System.out.println(s instanceof Student); // true

Student n = null;
System.out.println(n instanceof Student); // false
```
`instanceof`实际上判断一个变量所指向的实例是否是指定类型，==或者这个类型的子类==。如果一个引用变量为`null`，那么对任何`instanceof`的判断都为`false`。

利用`instanceof`，在向下转型前可以先判断：
```java
Person p = new Student();
if (p instanceof Student) {
    // 只有判断成功才会向下转型:
    Student s = (Student) p; // 一定会成功
}
```
从Java 14开始，判断`instanceof`后，可以直接转型为指定变量，避免再次强制转型。例如，对于以下代码：
```java
Object obj = "hello";
if (obj instanceof String) {
    String s = (String) obj;
    System.out.println(s.toUpperCase());
}
```
可以改写如下：
```java
// instanceof variable:
public class Main {
    public static void main(String[] args) {
        Object obj = "hello";
        if (obj instanceof String s) {
            // 可以直接使用变量s:
            System.out.println(s.toUpperCase());
        }
    }
}
```
这种使用`instanceof`的写法更加简洁。

### 区分继承和组合

`Book`类也有`name`字段，但让`Student`继承自`Book`从逻辑上讲不合理，`Student`不应该从`Book`继承，而应该从`Person`继承。

究其原因，是因为`Student`是`Person`的一种，它们是is关系，而`Student`并不是`Book`。实际上`Student`和`Book`的关系是has关系。

具有has关系不应该使用继承，而是使用组合，即`Student`可以持有一个Book实例：
```java
class Student extends Person {
    protected Book book;
    protected int score;
}
```
因此，继承是is关系，组合是has关系。


## 多态

### 覆写

在继承关系中，子类如果定义了一个与父类方法签名完全相同的方法，被称为覆写（`Override`）

例如，在`Person`类中，我们定义了`run()`方法：
```java
class Person {
    public void run() {
        System.out.println("Person.run");
    }
}
```
在子类Student中，覆写这个run()方法：
```java
class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}
```
`Override`和`Overload`不同的是，如果方法签名（方法的名称和参数列表）不同，就是`Overload`，`Overload`方法是一个新方法；如果方法签名相同，并且返回值也相同，就是`Override`。

==注意：方法名相同，方法参数相同，但方法返回值不同，也是不同的方法。在Java程序中，出现这种情况，编译器会报错==

加上`@Override`可以让编译器帮助检查是否进行了正确的覆写。希望进行覆写，但是不小心写错了方法签名，编译器会报错。

---

引用变量的声明类型可能与其实际类型不符，例如：
```java
Person p = new Student();
```
那么，一个实际类型为`Student`，引用类型为`Person`的变量，调用其`run()`方法，实际上调用的方法是`Student`的`run()`方法。因此可得出结论：

==Java的实例方法调用是基于运行时的实际类型的动态调用，而非变量的声明类型。==

这个特性在面向对象编程中称之为多态（Polymorphic）

### 多态

多态是指，针对某个类型的方法调用，其真正执行的方法取决于运行时期实际类型的方法。例如：

```java
Person p = new Student();

p.run(); // 无法确定运行时究竟调用哪个run()方法 (本行联系上下文是Student中的)
```
假设我们编写这样一个方法：
```java
public void runTwice(Person p) {
    p.run();
    p.run();
}
```
它传入的参数类型是`Person`，我们是无法知道传入的参数==实际类型==究竟是Person，还是Student，还是Person的其他子类，因此，也无法确定调用的是不是Person类定义的run()方法。

所以，多态的特性就是，运行期才能动态决定调用的子类方法。==对某个类型调用某个方法，执行的实际方法可能是某个子类的[[Tree/javaSE#覆写\|覆写]]方法==

多态的作用：

假设定义一种收入，需要给它报税，那么先定义一个Income类：
```java
class Income {
    protected double income;
    public double getTax() {
        return income * 0.1; // 税率10%
    }
}
```
对于工资收入，可以减去一个基数，那么从`Income`派生出`SalaryIncome`，并覆写`getTax()`：
```java
class Salary extends Income {
    @Override
    public double getTax() {
        if (income <= 5000) {
            return 0;
        }
        return (income - 5000) * 0.2;
    }
}
```
如果享受国务院特殊津贴，那么可以全部免税：
```java
class StateCouncilSpecialAllowance extends Income {
    @Override
    public double getTax() {
        return 0;
    }
}
```
现在，编写一个报税的财务软件，对于一个人的所有收入进行报税，可以这么写：
```java
public double totalTax(Income... incomes) {
    double total = 0;
    for (Income income: incomes) {
        total = total + income.getTax();
    }
    return total;
}
```

多态允许添加更多类型的子类实现功能扩展，却不需要修改基于父类的代码。

### 覆写Object方法

因为所有的`class`最终都继承自`Object`，而`Object`定义了几个重要的方法：

- `toString()`：把instance输出为String；
- `equals()`：判断两个instance是否逻辑相等；
- `hashCode()`：计算一个instance的哈希值。
在必要的情况下，可以覆写Object的这几个方法。例如：
```java
class Person {
...
// 显示更有意义的字符串:
  @Override
	public String toString() {
			return "Person:name=" + name;
	}
	
	// 比较是否相等:
	@Override
	public boolean equals(Object o) {
			// 当且仅当o为Person类型:
			if (o instanceof Person) {
					Person p = (Person) o;
					// 并且name字段相同时，返回true:
					return this.name.equals(p.name);
			}
			return false;
	}
	
	// 计算hash:
	@Override
	public int hashCode() {
			return this.name.hashCode();
	}
}
```
### 调用super

在子类的覆写方法中，如果要调用父类的被覆写的方法，可以通过super来调用。例如：
```java
class Person {
    protected String name;
    public String hello() {
        return "Hello, " + name;
    }
}

Student extends Person {
    @Override
    public String hello() {
        // 调用父类的hello()方法:
        return super.hello() + "!";
    }
}
```

### final

[[Tree/javaSE#继承\|继承]]可以允许子类覆写父类的方法。如果一个父类不允许子类对它的某个方法进行覆写，可以把该方法标记为`final`。用`final`修饰的方法不能被`Override`：
```java
class Person {
    protected String name;
    public final String hello() {
        return "Hello, " + name;
    }
}

Student extends Person {
    // compile error: 不允许覆写
    @Override
    public String hello() {
    }
}
```
如果一个类不希望任何其他类继承自它，那么可以把这个类本身标记为`final`用`final`修饰的类不能被继承：
```java
final class Person {
    protected String name;
}

// compile error: 不允许继承自Person
Student extends Person {
}
```
对于一个类的实例字段，同样可以用`final`修饰。用`final`修饰的字段在初始化后不能被修改。例如：
```java
class Person {
    public final String name = "Unamed";
}
```
对final字段重新赋值会报错：
```java
Person p = new Person();
p.name = "New Name"; // compile error!
````
可以在构造方法中初始化final字段：
```java
class Person {
    public final String name;
    public Person(String name) {
        this.name = name;
    }
}
```
这种方法更为常用，因为可以保证实例一旦创建，其`final`字段就不可修改。

## 抽象类

如果父类的方法本身不需要实现任何功能，仅仅是为了定义方法签名，目的是让子类去[[Tree/javaSE#覆写\|覆写]]它，那么，可以把父类的方法声明为抽象方法：
```java
class Person {
    public abstract void run();
}
```
把一个方法声明为`abstract`，表示它是一个抽象方法，本身没有实现任何方法语句。因为这个抽象方法本身是无法执行的，所以，Person类也无法被实例化。编译器无法编译Person类，因为它包含抽象方法。

必须把`Person`类本身也声明为`abstract`，才能正确编译它：
```java
abstract class Person {
    public abstract void run();
}
```

### 抽象类

使用`abstract`修饰的类就是抽象类。我们无法实例化一个抽象类：

因为抽象类本身被设计成只能用于被继承，因此，抽象类可以强迫子类实现其定义的抽象方法，否则编译会报错。==因此，抽象类实际上相当于定义了“规范”==

例如，Person类定义了抽象方法`run()`，那么，在实现子类Student的时候，就必须覆写`run()`方法：
```java
// abstract class
public class Main {
    public static void main(String[] args) {
        Person p = new Student();
        p.run();
    }
}

abstract class Person {
    public abstract void run();
}

class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}
```

### 面向抽象编程

定义了抽象类`Person`，以及具体的`Student`、`Teacher`子类的时候，可以通过抽象类`Person`类型去引用具体的子类的实例：
```java
Person s = new Student();
Person t = new Teacher();
```
这种引用抽象类的好处在于，我们对其进行方法调用，并不关心Person类型变量的具体子类型：
```java
// 不关心Person变量的具体子类型:
s.run();
t.run();
```
同样的代码，如果引用的是一个新的子类，我们仍然不关心具体类型：
```java
// 同样不关心新的子类是如何实现run()方法的：
Person e = new Employee();
e.run();
```
这种==尽量引用高层类型，避免引用实际子类型的方式，称之为面向抽象编程==

面向抽象编程的本质就是：

- 上层代码只定义规范（例如：abstract class Person）；

- 不需要子类就可以实现业务逻辑（正常编译）；

- 具体的业务逻辑由不同的子类实现，调用者并不关心。

## 接口

在[[Tree/javaSE#抽象类\|抽象类]]中，抽象方法本质上是定义接口规范：即规定高层类的接口，从而保证所有子类都有相同的接口实现，这样，[[Tree/javaSE#多态\|多态]]就能发挥作用

如果一个抽象类没有字段，所有方法全部都是抽象方法：
```java
abstract class Person {
    public abstract void run();
    public abstract String getName();
}
```
就可以把该抽象类改写为接口：`interface`

在Java中，使用`interface`可以声明一个接口：
```java
interface Person {
    void run();
    String getName();
}
```
所谓`interface`，就是比抽象类还要抽象的纯抽象接口，因为它连字段都不能有。因为接口定义的所有方法默认都是`public` `abstract`的，所以这两个修饰符不需写出

当一个具体的`class`去实现一个`interface`时，需要使用`implements`关键字。举个例子：
```java
class Student implements Person {
    private String name;

    public Student(String name) {
        this.name = name;
    }

    @Override
    public void run() {
        System.out.println(this.name + " run");
    }

    @Override
    public String getName() {
        return this.name;
    }
}
```
在Java中，一个类只能继承自另一个类，不能从多个类继承。但是，一个类可以实现多个`interface`，例如：
```java
class Student implements Person, Hello { // 实现了两个interface
    ...
}
```
### 术语
java的接口特指`interface`的定义，表示一个接口类型和一组方法签名，而编程接口泛指接口规范，如方法签名，数据格式，网络协议等。

抽象类和接口的对比如下：


  |            | abstract  class      | interface                   |
  | ---------- | -------------------- | --------------------------- |
  | 继承       | 只能extends一个class | 可以implements多个interface |
  | 字段       | 可以定义实例字段     | 不能定义实例字段            |
  | 抽象方法   | 可以定义抽象方法     | 可以定义抽象方法            |
  | 非抽象方法 | 可以定义非抽象方法   | 可以定义default方法         |

### 接口继承
一个`interface`可以继承自另一个`interface` `interface`继承自`interface`使用`extends`，它相当于扩展了接口的方法。例如：
```java
interface Hello {
    void hello();
}

interface Person extends Hello {
    void run();
    String getName();
}
```
此时，`Person`接口继承自`Hello`接口，因此，`Person`接口现在实际上有3个抽象方法签名，其中一个来自继承的`Hello`接口

### 继承关系

合理设计`interface`和`abstract class`的继承关系，可以充分复用代码。一般来说，公共逻辑适合放在`abstract class`中，具体逻辑放到各个子类，而接口层次代表抽象程度。可以参考Java的集合类定义的一组接口、抽象类以及具体子类的继承关系：

![](https://gcore.jsdelivr.net/gh/AlexLiu2022/resources/img/an-example-of-inheritance-relationships.png)

在使用的时候，实例化的对象永远只能是某个具体的子类，但总是通过接口去引用它，因为接口比抽象类更抽象：

```java
List list = new ArrayList(); // 用List接口引用具体子类的实例
Collection coll = list; // 向上转型为Collection接口
Iterable it = coll; // 向上转型为Iterable接口
```

### default方法
在接口中，可以定义`default`方法,实现类可以不必覆写`default`方法。`default`方法的目的是，当需要给接口新增一个方法时，会涉及到修改全部子类。如果新增的是`default`方法，那么子类就不必全部修改，只需要在需要覆写的地方去覆写新增方法。

`default`方法和抽象类的普通方法是有所不同的。因为`interface`没有字段，`default`方法无法访问字段，而抽象类的普通方法可以访问实例字段。




## 静态字段和静态方法

### 静态字段 

在一个`class`中定义的字段称之为实例字段。实例字段的特点是，==每个实例都有独立的字段，各个实例的同名字段互不影响==

还有一种字段，是用`static`修饰的字段，称为静态字段：`static field`

实例字段在每个实例中都有自己的一个独立“空间”，但是静态字段只有一个共享“空间”，==所有实例都会共享该字段==

不推荐用`实例变量.静态字段`去访问静态字段.因为在Java程序中，实例对象并没有静态字段。在代码中，实例对象能访问静态字段只是因为编译器可以根据实例类型自动转换为`类名.静态字段`来访问静态对象。

推荐用类名来访问静态字段。可以把静态字段理解为描述`class`本身的字段（非实例字段）

### 静态方法
有静态字段，就有静态方法。用`static`修饰的方法称为静态方法。

调用实例方法必须通过一个实例变量，而调用静态方法则不需要实例变量，通过类名就可以调用。静态方法类似其它编程语言的函数。

因为静态方法属于`class`而不属于实例，因此，静态方法内部，无法访问`this`变量，也无法访问实例字段，它只能访问静态字段。

通过实例变量也可以调用静态方法，但这只是编译器自动帮我们把实例改写成类名而已。

通常情况下，通过实例变量访问静态字段和静态方法，会得到一个编译警告。

静态方法经常用于工具类。例如：

- `Arrays.sort()`

- `Math.random()`

静态方法也经常用于辅助方法。注意到==Java程序的入口main()也是静态方法==

### 接口的静态字段 

因为`interface`是一个纯抽象类，所以它不能定义实例字段。但是，`interface`是可以有静态字段的，并且静态字段必须为`final`类型：
```java
public interface Person {
    public static final int MALE = 1;
    public static final int FEMALE = 2;
}
```
实际上，因为`interface`的字段只能是`public static final`类型，所以可以把这些修饰符都去掉，上述代码可简写为：
```java
public interface Person {
    // 编译器会自动加上public statc final:
    int MALE = 1;
    int FEMALE = 2;
}
```

## 枚举

### 概述
枚举是Java中的一种特殊类型
枚举的作用："是为了做信息的标志和信息的分类"
定义枚举类的格式：
```
修饰符 enum 枚举名称{
	第一行都是罗列枚举类实例的名称
}
```
```java
enum Season{
SPRING,SUMMER,AUTUMN,WINTER;
}
```
### 特征

反编译后观察枚举的特征：
```java
enum Season{
	SPRING, SUMMER, AUTUMN ,WINTER;
}
```
```java
Compiled from "Season.java"
public final class Season extends java.lang.Enum<Season>{
	public static final Season SPRING new Season();
	public static final Season SUMMER new Season();
	public static final Season AUTUMN new Season();
	public static final Season WINTER new Season();
	public static Season[]values();
	public static Season valueof(java.lang.String);
}
```

枚举的特征：
- 枚举类都是继承了枚举类型：java.lang.Enum
- 枚举都是最终类，不可以被继承
- 构造器的构造器都是私有的，枚举对外不能创建对象
- 枚举类的第一行默认都是罗列枚举对象的名称的
- 枚举类相当于是多例模式
## 包(package)

在Java中，我们使用`package`来解决名字冲突。`package`是java定义的一种名字空间，一个类总是属于某个包，类名（比如Person）只是一个简写，真正的完整类名是`包名.类名`

在定义`class`的时候，我们需要在第一行声明这个`class`属于哪个包。

包可以是多层结构，用.隔开。例如：`java.util`

 要特别注意：==包没有父子关系。`java.util`和`java.util.zip`是不同的包，两者没有任何继承关系==
没有定义包名的`class`，它使用的是默认包，非常容易引起名字冲突，因此，不推荐不写包名的做法。

Java文件对应的目录层次要和包的层次一致

### 包作用域

位于同一个包的类，可以访问包作用域的字段和方法。不用`public、protected、private`修饰的字段和方法就是包作用域。例如，`Person`类定义在`hello`包下面：
```java
package hello;

public class Person {
    // 包作用域:
    void hello() {
        System.out.println("Hello!");
    }
}
```
Main类也定义在`hello`包下面：
```java
package hello;

public class Main {
    public static void main(String[] args) {
        Person p = new Person();
        p.hello(); // 可以调用，因为Main和Person在同一个包
    }
}
```

### import

一个`class`总会引用其他的`class`。例如，小明的`ming.Person`类，如果要引用小军的`mr.jun.Arrays`类，有三种写法：

第一种，直接写出完整类名，例如：
```java
// Person.java
package ming;

public class Person {
    public void run() {
        mr.jun.Arrays arrays = new mr.jun.Arrays();
    }
}
```
很显然，每次写完整类名比较痛苦。

因此，第二种写法是用`import`语句，导入小军的`Arrays`，然后写简单类名：
```java
// Person.java
package ming;

// 导入完整类名:
import mr.jun.Arrays;

public class Person {
    public void run() {
        Arrays arrays = new Arrays();
    }
}
```
在写`import`的时候，可以使用*，表示把这个包下面的所有`class`都导入进来（但不包括子包的`class`）：
```java
// Person.java
package ming;

// 导入mr.jun包的所有class:
import mr.jun.*;

public class Person {
    public void run() {
        Arrays arrays = new Arrays();
    }
}
```
我们一般不推荐这种写法，因为在导入了多个包后，很难看出Arrays类属于哪个包。

还有一种`import static`的语法，它可以导入一个类的静态字段和静态方法：
```java
package main;

// 导入System类的所有静态字段和静态方法:
import static java.lang.System.*;

public class Main {
    public static void main(String[] args) {
        // 相当于调用System.out.println(…)
        out.println("Hello, world!");
    }
}
```
`import static`很少使用。

Java编译器最终编译出的.class文件只使用完整类名，因此，在代码中，当编译器遇到一个`class`名称时：

- 如果是完整类名，就直接根据完整类名查找这个`class`；

- 如果是简单类名，按下面的顺序依次查找：

	 - 查找当前`package`是否存在这个`class`；
	
	- 查找`import`的包是否包含这个`class`；
		
	- 查找`java.lang`包是否包含这个`class`。

如果按照上面的规则还无法确定类名，则编译报错。

一个例子：
```java
// Main.java
package test;

import java.text.Format;

public class Main {
    public static void main(String[] args) {
        java.util.List list; // ok，使用完整类名 -> java.util.List
        Format format = null; // ok，使用import的类 -> java.text.Format
        String s = "hi"; // ok，使用java.lang包的String -> java.lang.String
        System.out.println(s); // ok，使用java.lang包的System -> java.lang.System
        MessageFormat mf = null; // 编译错误：无法找到MessageFormat: MessageFormat cannot be resolved to a type
    }
}
```
因此，编写`class`的时候，编译器会自动帮我们做两个`import`动作：

- 默认自动`import`当前`package`的其他`class`

- 默认自动`import java.lang.*`

 注意：自动导入的是`java.lang`包，但类似`java.lang.reflect`这些包仍需要手动导入。
如果有两个`class`名称相同，例如，mr.jun.Arrays和java.util.Arrays，那么只能`import`其中一个，另一个必须写完整类名

### 最佳实践

为了避免名字冲突，需要确定唯一的包名。推荐的做法是使用倒置的域名来确保唯一性。例如：

- org.apache
- org.apache.commons.log

子包就可以根据功能自行命名。

要注意不要和`java.lang`包的类重名，即类不要使用这些名字：

- String
- System
- Runtime
...


要注意也不要和JDK常用类重名：

- java.util.List
- java.text.Format
- java.math.BigInteger
...

## 作用域

`public`、`protected`、`private`这些修饰符在可以用来限定访问作用域

### public
定义为`public`的`class` , `interface`可以被其他任何类访问：
```java
package abc;

public class Hello {
    public void hi() {
    }
}
```
上面的`Hello`是`public`，因此，可以被其他包的类访问：
```java
package xyz;

class Main {
    void foo() {
        // Main可以访问Hello
        Hello h = new Hello();
    }
}
```
定义为`public`的`field`、`method`可以被其他类访问，前提是首先有访问`class`的权限：
```java
package abc;

public class Hello {
    public void hi() {
    }
}
```
上面的`hi()`方法是`public`，可以被其他类调用，前提是首先要能访问Hello类：
```java
package xyz;

class Main {
    void foo() {
        Hello h = new Hello();
        h.hi();
    }
}
```

### private
定义为`private`的`field`、`method`无法被其他类访问：
```java
package abc;

public class Hello {
    // 不能被其他类调用:
    private void hi() {
    }

    public void hello() {
        this.hi();
    }
}
```
实际上，确切地说，`private`访问权限被限定在`class`的内部，而且与方法声明顺序无关。推荐把`private`方法放到后面，因为`public`方法定义了类对外提供的功能，阅读代码的时候，应该先关注public方法：
```java
package abc;

public class Hello {
    public void hello() {
        this.hi();
    }

    private void hi() {
    }
}
```
由于java支持嵌套类，**如果一个类内部还定义了嵌套类，那么，嵌套类拥有访问`private`的权限：**
```java
// private
public class Main {
    public static void main(String[] args) {
        Inner i = new Inner();
        i.hi();
    }

    // private方法:
    private static void hello() {
        System.out.println("private hello!");
    }

    // 静态内部类:
    static class Inner {
        public void hi() {
            Main.hello();
        }
    }
}
```

定义在一个`class`内部的`class`称为嵌套类（nested class），Java支持好几种嵌套类。

### protected
`protected`作用于继承关系。定义为`protected`的字段和方法可以被子类访问，以及子类的子类：
```java
package abc;

public class Hello {
    // protected方法:
    protected void hi() {
    }
}
```
上面的`protected`方法可以被继承的类访问：
```java
package xyz;

class Main extends Hello {
    void foo() {
        // 可以访问protected方法:
        hi();
    }
}
```

### package
最后，包作用域是指一个类允许访问同一个`package`的没有`public`、`private`修饰的`class`，以及没有`public`、`protected`、`private`修饰的字段和方法。
```java
package abc;
// package权限的类:
class Hello {
    // package权限的方法:
    void hi() {
    }
}
```
只要在同一个包，就可以访问`package`权限的`class`、`field`和`method`：
```java
package abc;

class Main {
    void foo() {
        // 可以访问package权限的类:
        Hello h = new Hello();
        // 可以调用package权限的方法:
        h.hi();
    }
}
```
注意，包名必须完全一致，包没有父子关系，com.apache和com.apache.abc是不同的包。

### 局部变量
在方法内部定义的变量称为局部变量，==局部变量作用域从变量声明处开始到对应的块结束。方法参数也是局部变量==
```java
package abc;

public class Hello {
    void hi(String name) { // ①
        String s = name.toLowerCase(); // ②
        int len = s.length(); // ③
        if (len < 10) { // ④
            int p = 10 - len; // ⑤
            for (int i=0; i<10; i++) { // ⑥
                System.out.println(); // ⑦
            } // ⑧
        } // ⑨
    } // ⑩
}
```
观察上面的hi()方法代码：

-方法参数name是局部变量，它的作用域是整个方法，即①～⑩；

- 变量s的作用域是定义处到方法结束，即②～⑩；

- 变量len的作用域是定义处到方法结束，即③～⑩；

- 变量p的作用域是定义处到if块结束，即⑤～⑨；

- 变量i的作用域是for循环，即⑥～⑧。

使用局部变量时，应该尽可能把局部变量的作用域缩小，尽可能延后声明局部变量。

### final
Java还提供了一个`final`修饰符。`final`与访问权限不冲突，它有很多作用。

- 用final修饰class可以阻止被继承：
```java
package abc;

// 无法被继承:
public final class Hello {
    private int n = 0;
    protected void hi(int t) {
        long i = t;
    }
}
```
- 用`final`修饰`method`可以阻止被子类覆写：
```java
package abc;

public class Hello {
    // 无法被覆写:
    protected final void hi() {
    }
}
```
- 用`final`修饰`field`可以阻止被重新赋值：
```java
package abc;

public class Hello {
    private final int n = 0;
    protected void hi() {
        this.n = 1; // error!
    }
}
```
- 用`final`修饰局部变量可以阻止被重新赋值：
```java
package abc;

public class Hello {
    protected void hi(final int t) {
        t = 1; // error!
    }
}
```
### 最佳实践
如果不确定是否需要`public`，就不声明为`public`，即尽可能少地暴露对外的字段和方法。

把方法定义为`package`权限有助于测试，因为测试类和被测试类只要位于同一个`package`，测试代码就可以访问被测试类的`package`权限方法。

一个`.java`文件只能包含一个`public`类，但可以包含多个非public类。==如果有public类，文件名必须和public类的名字相同==

## 内部类

被定义在另一个类的内部的类称为内部类（Nested Class）

### Inner class
如果一个类定义在另一个类的内部，这个类就是Inner Class：
```java
class Outer {
    class Inner {
        // 定义了一个Inner Class
    }
}
```
上述定义的Outer是一个普通类，而Inner是一个`Inner Class`，它与普通类有个最大的不同，就是`Inner Class`的实例不能单独存在，必须依附于一个`Outer Class`的实例。示例代码如下：
```java
// inner class
public class Main {
    public static void main(String[] args) {
        Outer outer = new Outer("Nested"); // 实例化一个Outer
        Outer.Inner inner = outer.new Inner(); // 实例化一个Inner
        inner.hello();
    }
}

class Outer {
    private String name;

    Outer(String name) {
        this.name = name;
    }

    class Inner {
        void hello() {
            System.out.println("Hello, " + Outer.this.name);
        }
    }
}
```
观察上述代码，要实例化一个`Inner`，必须首先创建一个`Outer`的实例，然后，调用Outer实例的new来创建Inner实例：

`Outer.Inner inner = outer.new Inner();`

这是因为`Inner Class`除了有一个`this`指向它自己，还隐含地持有一个`Outer Class`实例，可以用`Outer.this`访问这个实例。所以，实例化一个`Inner Class`不能脱离`Outer`实例。

`Inner Class`和普通`Class`相比，除了能引用`Outer`实例外，还有一个额外的“特权”，就是可以修改`Outer Class`的`private`字段，因为`Inner Class`的作用域在`Outer Class`内部，所以能访问`Outer Class`的`private`字段和方法。

**观察Java编译器编译后的.class文件可以发现，Outer类被编译为Outer.class，而Inner类被编译为Outer$Inner.class**

### Anonymous Class

还有一种定义Inner Class的方法，它不需要在Outer Class中明确地定义这个Class，而是在==方法内部==，通过匿名类（Anonymous Class）来定义。示例代码如下：
```java
// Anonymous Class
public class Main {
    public static void main(String[] args) {
        Outer outer = new Outer("Nested");
        outer.asyncHello();
    }
}

class Outer {
    private String name;

    Outer(String name) {
        this.name = name;
    }

    void asyncHello() {
        Runnable r = new Runnable() {
            @Override
            public void run() {
                System.out.println("Hello, " + Outer.this.name);
            }
        };
        new Thread(r).start();
    }
}
```

观察`asyncHello()`方法，方法内部实例化了一个`Runnable`,`Runnable`本身是接口，接口是不能实例化的，所以这里实际上是定义了一个实现了`Runnable`接口的匿名类，并且通过`new`实例化该匿名类，然后转型为`Runnable`。在==定义匿名类的时候就必须实例化它== 

定义匿名类的写法如下：
```java
Runnable r = new Runnable() {
    // 实现必要的抽象方法...
};
```
匿名类和`Inner Class`一样，可以访问`Outer Class`的`private`字段和方法。之所以要定义匿名类，是因为在这里通常不关心类名，比直接定义Inner Class可以少写很多代码。

观察Java编译器编译后的.class文件可以发现，Outer类被编译为Outer.class，而匿名类被编译为Outer$1.class。如果有多个匿名类，Java编译器会将每个匿名类依次命名为Outer$1、Outer$2、Outer$3……

**除了接口外，匿名类也完全可以继承自普通类** 观察以下代码：

```java
// Anonymous Class
import java.util.HashMap;

public class Main {
    public static void main(String[] args) {
        HashMap<String, String> map1 = new HashMap<>();
        HashMap<String, String> map2 = new HashMap<>() {}; // 匿名类!
        HashMap<String, String> map3 = new HashMap<>() {
            {
                put("A", "1");
                put("B", "2");
            }
        };
        System.out.println(map3.get("A"));
    }
}
```

map1是一个普通的`HashMap`实例，但map2是一个匿名类实例，只是该匿名类继承自`HashMap`。map3也是一个继承自`HashMap`的匿名类实例，并且添加了`static`代码块来初始化数据。观察编译输出可发现`Main$1.class`和`Main$2.class`两个匿名类文件

### Static Nested Class
最后一种内部类和Inner Class类似，但是使用`static`修饰，称为静态内部类（Static Nested Class）：
```java
// Static Nested Class
public class Main {
    public static void main(String[] args) {
        Outer.StaticNested sn = new Outer.StaticNested();
        sn.hello();
    }
}

class Outer {
    private static String NAME = "OUTER";

    private String name;

    Outer(String name) {
        this.name = name;
    }

    static class StaticNested {
        void hello() {
            System.out.println("Hello, " + Outer.NAME);
        }
    }
}
```

用`static`修饰的内部类和Inner Class有很大的不同，它不再依附于Outer的实例，而是一个完全独立的类，因此无法引用`Outer.this`，但它可以访问Outer的`private`静态字段和静态方法。如果把`StaticNested`移到`Outer`之外，就失去了访问`private`的权限。


### 内部类总结

- Java的内部类可分为Inner Class、Anonymous Class和Static Nested Class三种

- Inner Class和Anonymous Class本质上是相同的，都必须依附于Outer Class的实例，==即隐含地持有Outer.this实例==(**是内部持有外部！！！**)，并拥有Outer Class的private访问权限；

- ==Static Nested Class是独立类==，但拥有Outer Class的private访问权限。

## classpath 和 jar

`classpath`是JVM用到的一个环境变量，它用来指示JVM如何搜索`class` 

因为Java是编译型语言，源码文件是.java，而编译后的.class文件才是真正可以被JVM执行的字节码。因此，JVM需要知道，如果要加载一个abc.xyz.Hello的类，应该去哪搜索对应的Hello.class文件。

所以，`classpath`就是**一组目录的集合**，它设置的搜索路径与操作系统相关。例如，在Windows系统上，用`;`分隔，带空格的目录用`""`括起来，可能长这样：

C:\work\project1\bin;C:\shared;"D:\My Documents\project1\bin"
在Linux系统上，用:分隔，可能长这样：

/usr/shared:/usr/local/bin:/home/usr/bin

假设`classpat`h是`.;C:\work\project1\bin;C:\shared`，当JVM在加载abc.xyz.Hello这个类时，会依次查找：

1. <当前目录>\abc\xyz\Hello.class

2. C:\work\project1\bin\abc\xyz\Hello.class

3. C:\shared\abc\xyz\Hello.class

注意到`.`代表当前目录。如果JVM在某个路径下找到了对应的`class`文件，就不再往后继续搜索。如果所有路径下都没有找到，就报错。

classpath的设定方法有两种：

- 在系统环境变量中设置classpath环境变量，不推荐；

- 在启动JVM时设置classpath变量，推荐。

强烈不推荐在系统环境变量中设置classpath，那样会污染整个系统环境。在启动JVM时设置classpath才是推荐的做法。实际上就是给java命令传入-classpath或-cp参数：

`java -classpath .;C:\work\project1\bin;C:\shared abc.xyz.Hello`
或者使用-cp的简写：
`java -cp .;C:\work\project1\bin;C:\shared abc.xyz.Hello`

没有设置系统环境变量，也没有传入-cp参数，那么JVM默认的classpath为`.`，即当前目录：

`java abc.xyz.Hello`

上述命令告诉JVM**只**在当前目录搜索Hello.class。

在IDE中运行Java程序，IDE自动传入的-cp参数是**当前工程的bin目录和引入的jar包**

通常，在自己编写的class中会引用Java核心库的class，例如:`String`、`ArrayList`等 **JVM不依赖classpath加载核心库**

默认的当前目录`.`适用于绝大多数情况

假设我们有一个编译后的Hello.class，它的包名是com.example，当前目录是C:\work，那么，目录结构必须如下：

![](https://gcore.jsdelivr.net/gh/AlexLiu2022/resources/img/an-example-of-java-directory-structure.png)


运行这个Hello.class必须在当前目录下使用如下命令：

`C:\work> java -cp . com.example.Hello`

JVM根据`classpath`设置的`.`在当前目录下查找com.example.Hello，即实际搜索文件必须位于com/example/Hello.class 如果指定的.class文件不存在，或者目录结构和包名对不上，均会报错

### jar包

`jar`可以把`package`组织的目录层级，以及各个目录下的所有文件（包括.class文件和其他文件）都打成一个`jar`文件，这样一来，无论是备份，还是发给客户，就简单多了。

`jar`包实际上就是一个`zip`格式的压缩文件，而`jar`包相当于目录。如果我们要执行一个jar包的`class`，就可以把`jar`包放到`classpath`中：

`java -cp ./hello.jar abc.xyz.Hello`

这样JVM会自动在hello.jar文件里去搜索某个类。

因为`jar`包就是`zip`包，所以**后缀从.zip改为.jar** 一个jar包就创建成功

`jar`包还可以包含一个特殊的**/META-INF/MANIFEST.MF**文件，MANIFEST.MF是纯文本，可以指定Main-Class和其它信息。JVM会自动读取这个MANIFEST.MF文件，如果存在Main-Class，我们就不必在命令行指定启动的类名，而是用更方便的命令：

`java -jar hello.jar`

`jar`包还可以包含其它`jar`包，这个时候，就需要在`MANIFEST.MF`文件里配置`classpath`了。

在大型项目中，不可能手动编写`MANIFEST.MF`文件，再手动创建`zip`包。Java社区提供了大量的开源构建工具，例如**Maven**，可以非常方便地创建`jar`包


## class版本

通常说的Java 8，Java 11，Java 17，是指JDK的版本，也就是JVM的版本，更确切地说，就是java.exe这个程序的版本：

`$ java -version`

而每个版本的JVM，它能执行的class文件版本也不同。例如，Java 11对应的class文件版本是55，而Java 17对应的class文件版本是61

如果用Java 11编译一个Java程序，输出的class文件版本默认就是55，这个class既可以在Java 11上运行，也可以在Java 17上运行，因为Java 17支持的class文件版本是61，表示“最多支持到版本61”

如果用Java 17编译一个Java程序，输出的class文件版本默认就是61，它可以在Java 17、Java 18上运行，但不可能在Java 11上运行，因为Java 11支持的class版本最多到55。如果使用低于Java 17的JVM运行，会得到一个`UnsupportedClassVersionError`，错误信息类似：

`java.lang.UnsupportedClassVersionError: Xxx has been compiled by a more recent version of the Java Runtime...`

只要看到`UnsupportedClassVersionError`就表示当前要加载的`class`文件版本超过了JVM的能力，必须使用更高版本的JVM才能运行。

也可以用Java 17编译一个Java程序，指定输出的class版本要兼容Java 11（即class版本55），这样编译生成的class文件就可以在Java >=11的环境中运行

指定编译输出有两种方式，一种是在javac命令行中用参数--release设置：

`$ javac --release 11 Main.java`

参数`--release 11`表示源码兼容Java 11，编译的`class`输出版本为Java 11兼容，即class版本55

第二种方式是用参数--source指定源码版本，用参数--target指定输出class版本：

`$ javac --source 9 --target 11 Main.java`

上述命令如果使用Java 17的JDK编译，它会把源码视为Java 9兼容版本，并输出class为Java 11兼容版本。注意**--release参数和--source --target参数只能二选一，不能同时设置**

然而，指定版本如果低于当前的JDK版本，会有一些潜在的问题。例如，我们用Java 17编译Hello.java，参数设置--source 9和--target 11：
```java
public class Hello {
    public static void hello(String name) {
        System.out.println("hello".indent(4));
    }
}
```
用低于Java 11的JVM运行Hello会得到一个`LinkageError`，因为无法加载Hello.class文件，而用Java 11运行Hello会得到一个`NoSuchMethodError`，因为`String.indent()`方法是从Java 12才添加进来的，Java 11的String版本根本没有`indent()`方法。

 注：如果使用--release 11则会在编译时检查该方法是否在Java 11中存在。
因此，如果运行时的JVM版本是Java 11，则编译时也最好使用Java 11，而不是用高版本的JDK编译输出低版本的class。

如果使用javac编译时不指定任何版本参数，那么相当于使用--release 当前版本编译，即源码版本和输出版本均为当前版本。

在开发阶段，多个版本的JDK可以同时安装，当前使用的JDK版本可由JAVA_HOME环境变量切换。

#### 源码版本

在编写源代码的时候，我们通常会预设一个源码的版本。在编译的时候，如果用`--source`或`--release`指定源码版本，则使用指定的源码版本检查语法。

例如，使用了lambda表达式的源码版本至少要为8才能编译，使用了var关键字的源码版本至少要为10才能编译，使用switch表达式的源码版本至少要为12才能编译，且12和13版本需要启用`--enable-preview`参数。

运行时使用哪个JDK版本，编译时就尽量使用同一版本编译源码。

## 模块

.class文件是JVM看到的最小可执行文件，而一个大型程序需要编写很多`Class`，并生成一堆.class文件，很不便于管理，所以，`jar`文件就是`class`文件的容器。

在Java 9之前，一个大型Java程序会生成自己的`jar`文件，同时引用依赖的第三方`jar`文件，而JVM自带的Java标准库，实际上也是以jar文件形式存放的，这个文件叫**rt.jar**，一共有60多M。

如果是自己开发的程序，除了一个自己的`app.jar`以外，还需要一堆第三方的jar包，运行一个Java程序，一般来说，命令行写这样：

```shell
java -cp app.jar:a.jar:b.jar:c.jar com.liaoxuefeng.sample.Main
```
如果漏写了某个运行时需要用到的jar，那么在运行期极有可能抛出`ClassNotFoundException`

`jar`只是用于存放`class`的容器，它并不关心`class`之间的依赖

从Java 9开始引入的模块，主要是为了解决“依赖”这个问题 如果a.jar必须依赖另一个b.jar才能运行，那我们应该给a.jar加点说明之类的，让程序在编译和运行的时候能自动定位到b.jar，这种**自带“依赖关系”的class容器就是模块**

为了表明Java模块化的决心，从Java 9开始，原有的Java标准库已经由一个单一巨大的rt.jar分拆成了几十个模块，这些模块以`.jmod`扩展名标识，可以在$JAVA_HOME/jmods目录下找到它们：
- java.base.jmod
- java.compiler.jmod
- java.datatransfer.jmod
- java.desktop.jmod
- ...

这些.jmod文件每一个都是一个模块，模块名就是文件名。例如：模块java.base对应的文件就是java.base.jmod。模块之间的依赖关系已经被写入到模块内的module-info.class文件了。==所有的模块都直接或间接地依赖java.base模块，只有java.base模块不依赖任何模块==，它可以被看作是“根模块”，好比所有的类都是从Object直接或间接继承而来。

把一堆class封装为jar仅仅是一个打包的过程，而把一堆class封装为模块则不但需要打包，还需要写入依赖关系，并且**还可以包含二进制代码（通常是JNI扩展）**。此外，==模块支持多版本，即在同一个模块中可以为不同的JVM提供不同的版本==

### 编写模块

创建模块和原有的创建Java项目是完全一样的，以oop-module工程为例，它的目录结构如下：
```
oop-module
├── bin
├── build.sh
└── src
    ├── com
    │   └── itranswarp
    │       └── sample
    │           ├── Greeting.java
    │           └── Main.java
    └── module-info.java
```
其中，bin目录存放编译后的`class`文件，src目录存放源码，按包名的目录结构存放，仅仅在src目录下多了一个**module-info.java**这个文件，这就是模块的描述文件。在这个模块中，它长这样：
```java 
module hello.world {
	requires java.base; // 可不写，任何模块都会自动引入java.base
	requires java.xml;
}
```
其中，`module`是关键字，后面的hello.world是模块的名称，它的命名规范与包一致。花括号的`requires xxx;`表示这个模块需要引用的其他模块名。除了java.base可以被自动引入外，这里我们引入了一个java.xml的模块

当我们使用模块声明了依赖关系后，才能使用引入的模块。例如，Main.java代码如下：

```java
package com.itranswarp.sample;

// 必须引入java.xml模块后才能使用其中的类:
import javax.xml.XMLConstants;

public class Main {
	public static void main(String[] args) {
		Greeting g = new Greeting();
		System.out.println(g.hello(XMLConstants.XML_NS_PREFIX));
	}
}
```
如果把`requires java.xml;`从module-info.java中去掉，编译将报错。可见，模块的重要作用就是声明依赖关系。

下面用JDK提供的命令行工具来编译并创建模块。

首先把工作目录切换到oop-module，在当前目录下编译所有的.java文件，并存放到bin目录下，命令如下：
```shell
$ javac -d bin src/module-info.java src/com/itranswarp/sample/*.java
```
如果编译成功，现在项目结构如下：
```
oop-module
├── bin
│   ├── com
│   │   └── itranswarp
│   │       └── sample
│   │           ├── Greeting.class
│   │           └── Main.class
│   └── module-info.class
└── src
    ├── com
    │   └── itranswarp
    │       └── sample
    │           ├── Greeting.java
    │           └── Main.java
    └── module-info.java
```

注意到**src目录下的module-info.java被编译到bin目录下的module-info.class**

下一步需要把bin目录下的所有class文件先打包成jar，在打包的时候，注意传入**--main-class**参数，让这个jar包能自己定位main方法所在的类：
```shell
$ jar --create --file hello.jar --main-class com.itranswarp.sample.Main -C bin .
```

现在当前目录下得到了hello.jar这个jar包，它和普通jar包并无区别，可以直接使用命令`java -jar hello.jar`来运行它。但目标是创建模块，所以，继续使用JDK自带的jmod命令把一个jar包转换成模块：
```shell
$ jmod create --class-path hello.jar hello.jmod
```
于是，在当前目录下得到了hello.jmod这个模块文件/

### 运行模块

要运行一个jar，使用java -jar xxx.jar命令。要运行一个模块，只需要指定模块名:
```shell
$ java --module-path hello.jmod --module hello.world
```
结果是一个错误：

`Error occurred during initialization of boot layer
java.lang.module.FindException: JMOD format not supported at execution time: hello.jmod`

原因是.jmod不能被放入--module-path中 换成.jar就没问题了：
```shell
$ java --module-path hello.jar --module hello.world
Hello, xml!
```
可以用创建的hello.jmod模块来打包JRE。

### 打包JRE

为了支持模块化Java 9带头把自己的一个巨大无比的rt.jar拆成了几十个.jmod模块，原因就是，运行Java程序的时候，实际上用到的JDK模块，并没有那么多。不需要的模块，完全可以删除。

过去发布一个Java应用程序，要运行它，必须下载一个完整的JRE，再运行jar包。而完整的JRE有100多M。

现在，JRE自身的标准库已经分拆成了模块，只需要带上程序用到的模块，其他的模块就可以被裁剪掉。裁剪JRE并不是说把系统安装的JRE给删掉部分模块，而是“复制”一份JRE，但只带上用到的模块。为此，JDK提供了`jlink`命令来干这件事。命令如下：

```shell
$ jlink --module-path hello.jmod --add-modules java.base,java.xml,hello.world --output jre/
```

在`--module-path`参数指定了模块hello.jmod，然后，在`--add-modules`参数中指定了用到的3个模块java.base、java.xml和hello.world，用`,`分隔。最后，在`--output`参数指定输出目录。

现在，在当前目录下，可以找到jre目录，这是一个完整的并且带有hello.jmod模块的JRE  运行:

```shell
$ jre/bin/java --module hello.world
Hello, xml!
```

要分发自己的Java应用程序，只需要把这个jre目录打个包给对方发过去，对方直接运行上述命令即可，既不用下载安装JDK，也不用知道如何配置模块，极大地方便了分发和部署。

### 访问权限

Java的class访问权限分为public、protected、private和默认的包访问权限。引入模块后，这些访问权限的规则就要稍微做些调整。

确切地说，**class的这些访问权限只在一个模块内有效，模块和模块之间，例如，a模块要访问b模块的某个class，必要条件是b模块明确地导出了可以访问的包**

举个例子：模块hello.world用到了模块java.xml的一个类javax.xml.XMLConstants，之所以能直接使用这个类，是因为模块java.xml的module-info.java中声明了若干导出：

```java
module java.xml {
    exports java.xml;
    exports javax.xml.catalog;
    exports javax.xml.datatype;
    ...
}
```
只有它声明的导出的包，外部代码才被允许访问。换句话说，如果外部代码想要访问hello.world模块中的com.itranswarp.sample.Greeting类，必须将其导出：
```java
module hello.world {
    exports com.itranswarp.sample;

    requires java.base;
	requires java.xml;
}
```
因此，==模块进一步隔离了代码的访问权限==


# 异常处理
## java异常

程序运行的过程中，总是会出现各种各样的错误。

有一些错误是用户造成的，如希望用户输入一个`int`类型的年龄，但是用户的输入是 abc：
```java
// 假设用户输入了abc：
String s = "abc";
int n = Integer.parseInt(s); // NumberFormatException!
```
程序想要读写某个文件的内容，但是用户已经把它删除了：
```java
// 用户删除了该文件：
String t = readFile("C:\\abc.txt"); // FileNotFoundException!
```
还有一些错误是随机出现，并且永远不可能避免的。比如：

- 网络突然断了，连接不到远程服务器；
- 内存耗尽，程序崩溃了；
- 用户点“打印”，但根本没有打印机；
- ……

一个健壮的程序必须处理各种各样的错误

所谓错误，就是程序调用某个函数的时候，如果失败了，就表示出错。

调用方获知调用失败的信息有两种方法：

1. ：约定返回错误码

例如，处理一个文件，如果返回0，表示成功，返回其他整数，表示约定的错误码：
```java
int code = processFile("C:\\test.txt");
if (code == 0) {
    // ok:
} else {
    // error:
    switch (code) {
    case 1:
        // file not found:
    case 2:
        // no read permission:
    default:
        // unknown error:
    }
}
```
因为使用int类型的错误码，想要处理就非常麻烦。这种方式常见于底层C函数。

2. ：在语言层面上提供一个异常处理机制。

Java内置了一套异常处理机制，总是使用异常来表示错误。

异常是一种class，因此它本身带有类型信息。**异常可以在任何地方抛出，但只需要在上层捕获，这样就和方法调用分离了**：
```java
try {
    String s = processFile(“C:\\test.txt”);
    // ok:
} catch (FileNotFoundException e) {
    // file not found:
} catch (SecurityException e) {
    // no read permission:
} catch (IOException e) {
    // io error:
} catch (Exception e) {
    // other error:
}
```
因为Java的异常是class，它的继承关系如下：
```
                     ┌───────────┐
                     │  Object   │
                     └───────────┘
                           ▲
                           │
                     ┌───────────┐
                     │ Throwable │
                     └───────────┘
                           ▲
                 ┌─────────┴─────────┐
                 │                   │
           ┌───────────┐       ┌───────────┐
           │   Error   │       │ Exception │
           └───────────┘       └───────────┘
                 ▲                   ▲
         ┌───────┘              ┌────┴──────────┐
         │                      │               │
┌─────────────────┐    ┌─────────────────┐┌───────────┐
│OutOfMemoryError │... │RuntimeException ││IOException│...
└─────────────────┘    └─────────────────┘└───────────┘
                                ▲
                    ┌───────────┴─────────────┐
                    │                         │
       ┌─────────────────────┐ ┌────────────────────────┐         
       │NullPointerException │ │IllegalArgumentExceptio │...
       └─────────────────────┘ └────────────────────────┘
```         
从继承关系可知：`Throwable`是异常体系的根，它继承自Object。Throwable有两个体系：Error和Exception，==Error表示严重的错误，程序对此一般无能为力==，例如：

- `OutOfMemoryError`：内存耗尽
- `NoClassDefFoundError`：无法加载某个Class
- `StackOverflowError`：栈溢出

而Exception则是运行时的错误，它可以被捕获并处理。

某些异常是应用程序逻辑处理的一部分，应该捕获并处理。例如：

- `NumberFormatException`：数值类型的格式错误
- `FileNotFoundException`：未找到文件
- `SocketException`：读取网络失败

还有一些异常是程序逻辑编写不对造成的，应该修复程序本身。例如：

- `NullPointerException`：对某个null的对象调用方法或字段
- `IndexOutOfBoundsException`：数组索引越界

Exception又分为两大类：

- `RuntimeException`以及它的子类；
- 非`RuntimeException`（包括`IOException`、`ReflectiveOperationException`等等）

Java规定：

**必须捕获的异常**，包括Exception及其子类，但不包括RuntimeException及其子类，这种类型的异常称为==Checked Exception==

不需要捕获的异常，包括Error及其子类，RuntimeException及其子类。

 注意：==编译器对RuntimeException及其子类不做强制捕获要求，不是指应用程序本身不应该捕获并处理RuntimeException== 是否需要捕获，具体问题具体分析。
 
### 捕获异常
捕获异常使用try...catch语句，把可能发生异常的代码放到try {...}中，然后使用catch捕获对应的Exception及其子类：
```java
// try...catch
import java.io.UnsupportedEncodingException;
import java.util.Arrays;
public class Main {
    public static void main(String[] args) {
        byte[] bs = toGBK("中文");
        System.out.println(Arrays.toString(bs));
    }

    static byte[] toGBK(String s) {
        try {
            // 用指定编码转换String为byte[]:
            return s.getBytes("GBK");
        } catch (UnsupportedEncodingException e) {
            // 如果系统不支持GBK编码，会捕获到UnsupportedEncodingException:
            System.out.println(e); // 打印异常信息
            return s.getBytes(); // 尝试使用用默认编码
        }
    }
}
```
如果不捕获`UnsupportedEncodingException`，会出现编译失败的问题：
```java
// try...catch
import java.io.UnsupportedEncodingException;
import java.util.Arrays;
public class Main {
    public static void main(String[] args) {
        byte[] bs = toGBK("中文");
        System.out.println(Arrays.toString(bs));
    }

    static byte[] toGBK(String s) {
        return s.getBytes("GBK");
    }
}
```
编译器会报错，错误信息类似：unreported exception UnsupportedEncodingException; must be caught or declared to be thrown，并且准确地指出需要捕获的语句是return s.getBytes("GBK"); 意思是像UnsupportedEncodingException这样的Checked Exception，必须被捕获。

这是因为String.getBytes(String)方法定义是：

```java
public byte[] getBytes(String charsetName) throws UnsupportedEncodingException {
    ...
}
```
**在方法定义的时候，使用throws Xxx表示该方法可能抛出的异常类型** 调用方在调用的时候，必须强制捕获这些异常，否则编译器会报错。

在toGBK()方法中，因为调用了String.getBytes(String)方法，就必须捕获UnsupportedEncodingException。可以不捕获它，而是在方法定义处用throws表示toGBK()方法可能会抛出UnsupportedEncodingException，就可以让toGBK()方法通过编译器检查：
```java
// try...catch
import java.io.UnsupportedEncodingException;
import java.util.Arrays;
public class Main {
    public static void main(String[] args) {
        byte[] bs = toGBK("中文");
        System.out.println(Arrays.toString(bs));
    }

    static byte[] toGBK(String s) throws UnsupportedEncodingException {
        return s.getBytes("GBK");
    }
}
```
上述代码仍然会得到编译错误，但这一次，编译器提示的不是调用return s.getBytes("GBK");的问题，而是byte[] bs = toGBK("中文");。因为在main()方法中，调用toGBK()，没有捕获它声明的可能抛出的UnsupportedEncodingException。

修复方法是在main()方法中捕获异常并处理：
```java
// try...catch
import java.io.UnsupportedEncodingException;
import java.util.Arrays;
public class Main {
    public static void main(String[] args) {
        try {
            byte[] bs = toGBK("中文");
            System.out.println(Arrays.toString(bs));
        } catch (UnsupportedEncodingException e) {
            System.out.println(e);
        }
    }

    static byte[] toGBK(String s) throws UnsupportedEncodingException {
        // 用指定编码转换String为byte[]:
        return s.getBytes("GBK");
    }
}
```

只要是方法声明的`Checked Exception`，不在调用层捕获，也必须在更高的调用层捕获。==所有未捕获的异常，最终也必须在main()方法中捕获，不会出现漏写try的情况== 这是由编译器保证的。main()方法也是最后捕获Exception的机会。

如果是测试代码，上面的写法就略显麻烦。如果不想写任何try代码，可以直接把main()方法定义为throws Exception：

```java
// try...catch
import java.io.UnsupportedEncodingException;
import java.util.Arrays;
public class Main {
    public static void main(String[] args) throws Exception {
        byte[] bs = toGBK("中文");
        System.out.println(Arrays.toString(bs));
    }

    static byte[] toGBK(String s) throws UnsupportedEncodingException {
        // 用指定编码转换String为byte[]:
        return s.getBytes("GBK");
    }
}
```
因为main()方法声明了可能抛出Exception，也就声明了可能抛出所有的Exception，因此在内部就无需捕获了。**代价是一旦发生异常，程序会立刻退出**

有人喜欢在toGBK()内部“消化”异常：
```java
static byte[] toGBK(String s) {
    try {
        return s.getBytes("GBK");
    } catch (UnsupportedEncodingException e) {
        // 什么也不干
    }
    return null;
 ```
这种捕获后不处理的方式是非常不好的，即使真的什么也做不了，也要先把异常记录下来：
```java
static byte[] toGBK(String s) {
    try {
        return s.getBytes("GBK");
    } catch (UnsupportedEncodingException e) {
        // 先记下来再说:
        e.printStackTrace();
    }
    return null;
 ```
==所有异常都可以调用`printStackTrace()`方法打印异常栈，这是一个简单有用的快速打印异常的方法==
## 捕获异常

### 多catch语句

可以使用多个catch语句，每个catch分别捕获对应的Exception及其子类。JVM在捕获到异常后，会从上到下匹配catch语句，匹配到某个catch后，执行catch代码块，然后不再继续匹配

简单地说就是：**多个catch语句只有一个能被执行** 例如：
```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (IOException e) {
        System.out.println(e);
    } catch (NumberFormatException e) {
        System.out.println(e);
    }
}
```
存在多个catch的时候，catch的顺序非常重要：==子类必须写在前面== 例如：
```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (IOException e) {
        System.out.println("IO error");
    } catch (UnsupportedEncodingException e) { // 永远捕获不到
        System.out.println("Bad encoding");
    }
}
```
对于上面的代码，UnsupportedEncodingException异常是永远捕获不到的，因为它是IOException的子类。当抛出UnsupportedEncodingException异常时，会被catch (IOException e) { ... }捕获并执行。

因此，正确的写法是把子类放到前面：
```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (UnsupportedEncodingException e) {
        System.out.println("Bad encoding");
    } catch (IOException e) {
        System.out.println("IO error");
    }
}
```

###  finally语句
无论是否有异常发生，如果都希望执行一些语句，例如清理工作，可以把执行语句写若干遍：正常执行的放到try中，每个catch再写一遍。例如：
```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
        System.out.println("END");
    } catch (UnsupportedEncodingException e) {
        System.out.println("Bad encoding");
        System.out.println("END");
    } catch (IOException e) {
        System.out.println("IO error");
        System.out.println("END");
    }
}
```
上述代码无论是否发生异常，都会执行`System.out.println("END");`这条语句

为了消除这些重复的代码，Java的try ... catch机制还提供了finally语句，finally语句块保证有无错误都会执行。上述代码可以改写如下：
```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (UnsupportedEncodingException e) {
        System.out.println("Bad encoding");
    } catch (IOException e) {
        System.out.println("IO error");
    } finally {
        System.out.println("END");
    }
}
```
`finally`有几个特点：

- finally语句不是必须的，可写可不写；
- finally总是最后执行

如果没有发生异常，就正常执行try { ... }语句块，然后执行finally。如果发生了异常，就中断执行try { ... }语句块，然后跳转执行匹配的catch语句块，最后执行finally。

某些情况下，可以没有catch，只使用try ... finally结构。例如：
```java
void process(String file) throws IOException {
    try {
        ...
    } finally {
        System.out.println("END");
    }
}
```
因为**方法声明了可能抛出的异常，所以可以不写catch**

### 捕获多种异常

如果某些异常的处理逻辑相同，但是异常本身不存在继承关系，那么就得编写多条catch子句：
```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (IOException e) {
        System.out.println("Bad input");
    } catch (NumberFormatException e) {
        System.out.println("Bad input");
    } catch (Exception e) {
        System.out.println("Unknown error");
    }
}
```
因为处理IOException和NumberFormatException的代码是相同的，所以我们可以把它两用`|`合并到一起：
```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (IOException | NumberFormatException e) { // IOException或NumberFormatException
        System.out.println("Bad input");
    } catch (Exception e) {
        System.out.println("Unknown error");
    }
}
```
## 抛出异常

### 异常的传播

当某个方法抛出了异常时，如果当前方法没有捕获异常，异常就会被抛到上层调用方法，直到遇到某个try ... catch被捕获为止：
```java
// exception
public class Main {
    public static void main(String[] args) {
        try {
            process1();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    static void process1() {
        process2();
    }

    static void process2() {
        Integer.parseInt(null); // 会抛出NumberFormatException
    }
}
```

通过`printStackTrace()`可以打印出方法的调用栈，类似：

```
java.lang.NumberFormatException: null
    at java.base/java.lang.Integer.parseInt(Integer.java:614)
    at java.base/java.lang.Integer.parseInt(Integer.java:770)
    at Main.process2(Main.java:16)
    at Main.process1(Main.java:12)
    at Main.main(Main.java:5)
```

`printStackTrace()`对于调试错误非常有用，上述信息表示：NumberFormatException是在java.lang.Integer.parseInt方法中被抛出的，从下往上看，调用层次依次是：

1. main()调用process1()；
2. process1()调用process2()；
3. process2()调用Integer.parseInt(String)；
4. Integer.parseInt(String)调用Integer.parseInt(String, int)

查看Integer.java源码可知，抛出异常的方法代码如下：

```java
public static int parseInt(String s, int radix) throws NumberFormatException {
    if (s == null) {
        throw new NumberFormatException("null");
    }
    ...
}
```
并且，每层调用均给出了源代码的行号，可直接定位。

### 抛出异常

当发生错误时，例如，用户输入了非法的字符，就可以抛出异常。

参考Integer.parseInt()方法，抛出异常分两步：

创建某个Exception的实例；
用`throw`语句抛出

一个例子：
```java
void process2(String s) {
    if (s==null) {
        NullPointerException e = new NullPointerException();
        throw e;
    }
}
```
实际上，绝大部分抛出异常的代码都会合并写成一行：
```java
void process2(String s) {
    if (s==null) {
        throw new NullPointerException();
    }
}
```
==如果一个方法捕获了某个异常后，又在catch子句中抛出新的异常，就相当于把抛出的异常类型“转换”了==：
```java
void process1(String s) {
    try {
        process2();
    } catch (NullPointerException e) {
        throw new IllegalArgumentException();
    }
}

void process2(String s) {
    if (s==null) {
        throw new NullPointerException();
    }
}
```

当process2()抛出NullPointerException后，被process1()捕获，然后抛出IllegalArgumentException()。

如果在main()中捕获IllegalArgumentException，打印的异常栈：
```java
// exception
public class Main {
    public static void main(String[] args) {
        try {
            process1();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    static void process1() {
        try {
            process2();
        } catch (NullPointerException e) {
            throw new IllegalArgumentException();
        }
    }

    static void process2() {
        throw new NullPointerException();
    }
}
```
打印出的异常栈类似：
```java
java.lang.IllegalArgumentException
    at Main.process1(Main.java:15)
    at Main.main(Main.java:5)
```
这说明新的异常丢失了原始异常信息，已经看不到原始异常NullPointerException的信息了。

==为了能追踪到完整的异常栈，在构造异常的时候，把原始的Exception实例传进去，新的Exception就可以持有原始Exception信息==对上述代码改进如下：
```java
// exception
public class Main {
    public static void main(String[] args) {
        try {
            process1();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    static void process1() {
        try {
            process2();
        } catch (NullPointerException e) {
            throw new IllegalArgumentException(e);
        }
    }

    static void process2() {
        throw new NullPointerException();
    }
}
```

运行上述代码，打印出的异常栈类似：
```
java.lang.IllegalArgumentException: java.lang.NullPointerException
    at Main.process1(Main.java:15)
    at Main.main(Main.java:5)
Caused by: java.lang.NullPointerException
    at Main.process2(Main.java:20)
    at Main.process1(Main.java:13)
```
注意到Caused by: Xxx，说明捕获的IllegalArgumentException并不是造成问题的根源，根源在于NullPointerException，是在Main.process2()方法抛出的。

在代码中获取原始异常可以使用`Throwable.getCause()`方法。如果返回null，说明已经是“根异常”了。

有了完整的异常栈的信息，能快速定位并修复代码的问题。

 捕获到异常并再次抛出时，一定要留住原始异常，否则很难定位第一案发现场！
如果在`try`或者`catch`语句块中抛出异常，`finally`语句是否会执行？例如：
```java
// exception
public class Main {
    public static void main(String[] args) {
        try {
            Integer.parseInt("abc");
        } catch (Exception e) {
            System.out.println("catched");
            throw new RuntimeException(e);
        } finally {
            System.out.println("finally");
        }
    }
}
```
上述代码执行结果如下：
```
catched
finally
Exception in thread "main" java.lang.RuntimeException: java.lang.NumberFormatException: For input string: "abc"
    at Main.main(Main.java:8)
Caused by: java.lang.NumberFormatException: For input string: "abc"
    at ...
```
第一行打印了catched，说明进入了catch语句块。第二行打印了finally，说明执行了finally语句块。

**在catch中抛出异常，不会影响finally的执行。JVM会先执行finally，然后抛出异常**

### 异常屏蔽
如果在执行finally语句时抛出异常，那么，catch语句的异常还能否继续抛出？例如：

```java
// exception
public class Main {
    public static void main(String[] args) {
        try {
            Integer.parseInt("abc");
        } catch (Exception e) {
            System.out.println("catched");
            throw new RuntimeException(e);
        } finally {
            System.out.println("finally");
            throw new IllegalArgumentException();
        }
    }
}
```

执行上述代码，发现异常信息如下：
```
catched
finally
Exception in thread "main" java.lang.IllegalArgumentException
    at Main.main(Main.java:11)
```
这说明**finally抛出异常后，原来在catch中准备抛出的异常就“消失”了，因为只能抛出一个异常** ==没有被抛出的异常称为“被屏蔽”的异常（Suppressed Exception)==

在极少数的情况下，需要获知所有的异常。保存所有的异常信息的方法是先用`origin`变量保存原始异常，然后调用`Throwable.addSuppressed()`，把原始异常添加进来，最后在`finally`抛出：
```java
// exception
public class Main {
    public static void main(String[] args) throws Exception {
        Exception origin = null;
        try {
            System.out.println(Integer.parseInt("abc"));
        } catch (Exception e) {
            origin = e;
            throw e;
        } finally {
            Exception e = new IllegalArgumentException();
            if (origin != null) {
                e.addSuppressed(origin);
            }
            throw e;
        }
    }
}
```
当catch和finally都抛出了异常时，虽然catch的异常被屏蔽了，但是，finally抛出的异常仍然包含了它：
```
Exception in thread "main" java.lang.IllegalArgumentException
    at Main.main(Main.java:11)
Suppressed: java.lang.NumberFormatException: For input string: "abc"
    at java.base/java.lang.NumberFormatException.forInputString(NumberFormatException.java:65)
    at java.base/java.lang.Integer.parseInt(Integer.java:652)
    at java.base/java.lang.Integer.parseInt(Integer.java:770)
    at Main.main(Main.java:6)
```
通过Throwable.getSuppressed()可以获取所有的Suppressed Exception。

绝大多数情况下，==在finally中不要抛出异常== 因此，通常不需要关心Suppressed Exception

## 自定义异常

Java标准库定义的常用异常包括：
```
Exception
│
├─ RuntimeException
│  │
│  ├─ NullPointerException
│  │
│  ├─ IndexOutOfBoundsException
│  │
│  ├─ SecurityException
│  │
│  └─ IllegalArgumentException
│     │
│     └─ NumberFormatException
│
├─ IOException
│  │
│  ├─ UnsupportedCharsetException
│  │
│  ├─ FileNotFoundException
│  │
│  └─ SocketException
│
├─ ParseException
│
├─ GeneralSecurityException
│
├─ SQLException
│
└─ TimeoutException
```
在代码中需要抛出异常时，尽量使用JDK已定义的异常类型。例如，参数检查不合法，应该抛出IllegalArgumentException：
```java
static void process1(int age) {
    if (age <= 0) {
        throw new IllegalArgumentException();
    }
}
```
在一个大型项目中，可以自定义新的异常类型，但是，保持一个合理的异常继承体系是非常重要的。

一个常见的做法是自定义一个BaseException作为“根异常”，然后，派生出各种业务类型的异常。

BaseException需要从一个适合的Exception派生，通常建议从RuntimeException派生：
```java
public class BaseException extends RuntimeException {
}
```
其他业务类型的异常就可以从BaseException派生：
```java
public class UserNotFoundException extends BaseException {
}

public class LoginFailedException extends BaseException {
}
```
自定义的BaseException应该提供多个构造方法：
```java
public class BaseException extends RuntimeException {
    public BaseException() {
        super();
    }

    public BaseException(String message, Throwable cause) {
        super(message, cause);
    }

    public BaseException(String message) {
        super(message);
    }

    public BaseException(Throwable cause) {
        super(cause);
    }
}
```
上述构造方法实际上都是原样照抄RuntimeException。这样，抛出异常的时候，就可以选择合适的构造方法。通过IDE可以根据父类快速生成子类的构造方法。

## 使用断言

断言（Assertion）是一种调试程序的方式 在Java中，使用`assert`关键字来实现断言。

一个例子：
```java
public static void main(String[] args) {
    double x = Math.abs(-123.45);
    assert x >= 0;
    System.out.println(x);
}
```
语句assert x >= 0;即为断言，断言条件x >= 0预期为true。如果计算结果为false，则断言失败，抛出AssertionError。

使用assert语句时，还可以添加一个可选的断言消息：
```java
assert x >= 0 : "x must >= 0";
```
这样，断言失败的时候，AssertionError会带上消息x must >= 0，更加便于调试。

Java断言的特点是：断言失败时会抛出`AssertionError`，导致程序结束退出。因此，断言不能用于可恢复的程序错误，只应该用于开发和测试阶段。

对于可恢复的程序错误，不应该使用断言。例如：
```java
void sort(int[] arr) {
    assert arr != null;
}
```
应该抛出异常并在上层捕获：
```java
void sort(int[] arr) {
    if (arr == null) {
        throw new IllegalArgumentException("array cannot be null");
    }
}
```
在程序中使用assert时，例如，一个简单的断言：
```java
// assert
public class Main {
    public static void main(String[] args) {
        int x = -1;
        assert x > 0;
        System.out.println(x);
    }
}
```

断言x必须大于0，实际上x为-1，断言肯定失败。执行上述代码，发现程序并未抛出AssertionError，而是正常打印了x的值。

这是因为==JVM默认关闭断言指令，即遇到assert语句就自动忽略了，不执行==

**要执行assert语句，必须给Java虚拟机传递-enableassertions（可简写为-ea）参数启用断言**  所以，上述程序必须在命令行下运行才有效果：
```shell
$ java -ea Main.java
Exception in thread "main" java.lang.AssertionError
	at Main.main(Main.java:5)
```
可以有选择地对特定地类启用断言，命令行参数是：-ea:com.itranswarp.sample.Main，表示只对com.itranswarp.sample.Main这个类启用断言。

或者对特定地包启用断言，命令行参数是：-ea:com.itranswarp.sample...（注意结尾有3个.），表示对com.itranswarp.sample这个包启动断言。

==实际开发中，很少使用断言。更好的方法是编写单元测试.==

# 反射

反射就是Reflection，Java的反射是指程序在运行期可以拿到一个对象的所有信息

正常情况下，如果要调用一个对象的方法，或者访问一个对象的字段，通常会传入对象实例：
```java
// Main.java
import com.itranswarp.learnjava.Person;

public class Main {
    String getFullName(Person p) {
        return p.getFirstName() + " " + p.getLastName();
    }
}
````
但是，如果不能获得Person类，只有一个Object实例，比如这样：
```java
String getFullName(Object obj) {
    return ???
}
```
怎么办？强制转型?
```java
String getFullName(Object obj) {
    Person p = (Person) obj;
    return p.getFirstName() + " " + p.getLastName();
}
```
强制转型时发现一个问题：编译上面的代码，仍然需要引用Person类 不然，去掉import语句，不能编译通过

所以，反射是为了解决在运行期，对某个实例一无所知的情况下，如何调用其方法

## class类

除了`int`等基本类型外，Java的其他类型全部都是`class`（包括interface） 例如：

- String
- Object
- Runnable
- Exception
- ...

可以得出结论：class（包括interface）的本质是数据类型（Type）。无继承关系的数据类型无法赋值：
```java
Number n = new Double(123.456); // OK
String s = new Double(123.456); // compile error!
```
而`class`是由JVM在执行过程中==动态加载==的。JVM在第一次读取到一种class类型时，将其加载进内存

每加载一种class，JVM就为其创建一个Class类型的实例，并关联起来。注意：这里的Class类型是一个名叫Class的class。它长这样：
```java
public final class Class {
    private Class() {}
}
```
以String类为例，当JVM加载String类时，它首先读取String.class文件到内存，然后，为String类创建一个Class实例并关联起来：
```java
Class cls = new Class(String);
```
这个Class实例是JVM内部创建的，查看JDK源码，可以发现Class类的构造方法是private，只有JVM能创建Class实例，Java程序是无法创建Class实例的

所以，JVM持有的每个Class实例都指向一个数据类型（class或interface）：
```
┌───────────────────────────┐
│      Class Instance       │──────> String
├───────────────────────────┤
│name = "java.lang.String"  │
└───────────────────────────┘
┌───────────────────────────┐
│      Class Instance       │──────> Random
├───────────────────────────┤
│name = "java.util.Random"  │
└───────────────────────────┘
┌───────────────────────────┐
│      Class Instance       │──────> Runnable
├───────────────────────────┤
│name = "java.lang.Runnable"│
└───────────────────────────┘
```
一个Class实例包含了该class的所有完整信息：
```
┌───────────────────────────┐
│      Class Instance       │──────> String
├───────────────────────────┤
│name = "java.lang.String"  │
├───────────────────────────┤
│package = "java.lang"      │
├───────────────────────────┤
│super = "java.lang.Object" │
├───────────────────────────┤
│interface = CharSequence...│
├───────────────────────────┤
│field = value[],hash,...   │
├───────────────────────────┤
│method = indexOf()...      │
└───────────────────────────┘
```
由于JVM为每个加载的class创建了对应的Class实例，并在实例中保存了该class的所有信息，包括类名、包名、父类、实现的接口、所有方法、字段等，因此，如果获取了某个Class实例，就可以通过这个Class实例获取到该实例对应的class的所有信息。

==这种通过Class实例获取class信息的方法称为反射（Reflection）==

获取一个class的Class实例有三个方法：

1. 直接通过一个class的静态变量class获取：
```java
Class cls = String.class;
```
2. 如果有一个实例变量，可以通过该实例变量提供的getClass()方法获取：
```java
String s = "Hello";
Class cls = s.getClass();
```
3. 如果知道一个class的完整类名，可以通过静态方法Class.forName()获取：
```java
Class cls = Class.forName("java.lang.String");
```
因为Class实例在JVM中是唯一的，所以，上述方法获取的Class实例是同一个实例。可以用\==比较两个Class实例：
```java
Class cls1 = String.class;

String s = "Hello";
Class cls2 = s.getClass();

boolean sameClass = cls1 == cls2; // true
```
注意一下Class实例比较和instanceof的差别：
```java
Integer n = new Integer(123);

boolean b1 = n instanceof Integer; // true，因为n是Integer类型
boolean b2 = n instanceof Number; // true，因为n是Number类型的子类

boolean b3 = n.getClass() == Integer.class; // true，因为n.getClass()返回Integer.class
boolean b4 = n.getClass() == Number.class; // false，因为Integer.class!=Number.class
```
用instanceof不但匹配指定类型，还匹配指定类型的子类。而用\==判断class实例可以精确地判断数据类型，但==不能作子类型比较==

通常情况下，应该用instanceof判断数据类型，因为面向抽象编程的时候，不关心具体的子类型。**只有在需要精确判断一个类型是不是某个class的时候，才使用\==判断class实例**

反射的目的是为了获得某个实例的信息。当拿到某个Object实例时，可以通过反射获取该Object的class信息：

```java
void printObjectInfo(Object obj) {
    Class cls = obj.getClass();
}
```
要从Class实例获取获取的基本信息，参考下面的代码：
```java
// reflection
public class Main {
    public static void main(String[] args) {
        printClassInfo("".getClass());
        printClassInfo(Runnable.class);
        printClassInfo(java.time.Month.class);
        printClassInfo(String[].class);
        printClassInfo(int.class);
    }

    static void printClassInfo(Class cls) {
        System.out.println("Class name: " + cls.getName());
        System.out.println("Simple name: " + cls.getSimpleName());
        if (cls.getPackage() != null) {
            System.out.println("Package name: " + cls.getPackage().getName());
        }
        System.out.println("is interface: " + cls.isInterface());
        System.out.println("is enum: " + cls.isEnum());
        System.out.println("is array: " + cls.isArray());
        System.out.println("is primitive: " + cls.isPrimitive());
    }
}
```

注意到数组（例如String\[]）也是一种类，而且不同于String.class，它的类名是\[Ljava.lang.String;。此外，**JVM为每一种基本类型如int也创建了Class实例，通过int.class访问*

如果获取到了一个Class实例，就可以通过该Class实例来创建对应类型的实例：
```java
// 获取String的Class实例:
Class cls = String.class;
// 创建一个String实例:
String s = (String) cls.newInstance();
```
上述代码相当于new String()。通过Class.newInstance()可以创建类实例，它的局限是：**只能调用public的无参数构造方法。带参数的构造方法，或者非public的构造方法都无法通过Class.newInstance()被调用**

### 动态加载
JVM在执行Java程序的时候，并不是一次性把所有用到的class全部加载到内存，而是第一次需要用到class时才加载。例如：

```java
// Main.java
public class Main {
    public static void main(String[] args) {
        if (args.length > 0) {
            create(args[0]);
        }
    }

    static void create(String name) {
        Person p = new Person(name);
    }
}
```
当执行Main.java时，由于用到了Main，因此，JVM首先会把Main.class加载到内存。然而，并不会加载Person.class，除非程序执行到create()方法，JVM发现需要加载Person类时，才会首次加载Person.class。如果没有执行create()方法，那么Person.class根本就不会被加载。

这就是JVM动态加载class的特性。

动态加载class的特性对于Java程序非常重要。利用JVM动态加载class的特性，才能在运行期根据条件加载不同的实现类。例如，Commons Logging总是优先使用Log4j，只有当Log4j不存在时，才使用JDK的logging。利用JVM动态加载特性，大致的实现代码如下：

```java
// Commons Logging优先使用Log4j:
LogFactory factory = null;
if (isClassPresent("org.apache.logging.log4j.Logger")) {
    factory = createLog4j();
} else {
    factory = createJdkLog();
}

boolean isClassPresent(String name) {
    try {
        Class.forName(name);
        return true;
    } catch (Exception e) {
        return false;
    }
}
```
这就是为什么只需要把Log4j的jar包放到classpath中，Commons Logging就会自动使用Log4j

# 注解

注解是放在Java源码的类、方法、字段、参数前的一种特殊“注释”：
```java
// this is a component:
@Resource("hello")
public class Hello {
    @Inject
    int n;

    @PostConstruct
    public void hello(@Param String name) {
        System.out.println(name);
    }

    @Override
    public String toString() {
        return "Hello";
    }
}
```
注释会被编译器直接忽略，注解则可以被编译器打包进入class文件，因此，==注解是一种用作标注的“元数据”==

## 注解的作用
从JVM的角度看，注解本身对代码逻辑没有任何影响，如何使用注解完全由工具决定。

Java的注解可以分为三类：

1. 由编译器使用的注解，例如：

- @Override：让编译器检查该方法是否正确地实现了覆写；
- @SuppressWarnings：告诉编译器忽略此处代码产生的警告。

这类注解不会被编译进入.class文件，它们在编译后就被编译器扔掉了。

2. 由工具处理.class文件使用的注解，比如有些工具会在加载class的时候，对class做动态修改，实现一些特殊的功能。这类注解会被编译进入.class文件，但加载结束后并不会存在于内存中。
 
这类注解只被一些底层库使用，一般不必自己处理。

3. 在程序运行期能够读取的注解，它们在加载后一直存在于JVM中，这也是最常用的注解。例如，一个配置了`@PostConstruct`的方法会在调用构造方法后自动被调用（这是Java代码读取该注解实现的功能，JVM并不会识别该注解）。

定义一个注解时，可以定义配置参数。配置参数可包括：

- 所有基本类型；
- String；
- 枚举类型；
- 基本类型、String、Class以及枚举的数组。

因为**配置参数必须是常量**，所以，上述限制保证了注解在定义时就已经确定了每个参数的值。

注解的配置参数可以有默认值，缺少某个配置参数时将使用默认值。

此外，大部分注解会有一个名为`value`的配置参数，对此参数赋值，可以只写常量，相当于省略了`value`参数。

如果只写注解，相当于全部使用默认值。

举例，对以下代码：
```java
public class Hello {
    @Check(min=0, max=100, value=55)
    public int n;

    @Check(value=99)
    public int p;

    @Check(99) // @Check(value=99)
    public int x;

    @Check
    public int y;
}
```
`@Check`就是一个注解。第一个@Check(min=0, max=100, value=55)明确定义了三个参数，第二个@Check(value=99)只定义了一个value参数，它实际上和@Check(99)是完全一样的。最后一个@Check表示所有参数都使用默认值。

## 定义注解

java语言使用`@interface`语法来定义注解（Annotation），它的格式如下：
```java
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```
注解的参数类似无参数方法，可以用`default`设定一个默认值（强烈推荐）。最常用的参数应当命名为`value`

### 元注解
**有一些注解可以修饰其他注解，这些注解就称为元注解（meta annotation）** Java标准库已经定义了一些元注解，我们只需要使用元注解，通常不需要自己去编写元注解。

#### @Target
最常用的元注解是@Target。使用@Target可以定义Annotation能够被应用于源码的哪些位置：

- 类或接口：ElementType.TYPE；
- 字段：ElementType.FIELD；
- 方法：ElementType.METHOD；
- 构造方法：ElementType.CONSTRUCTOR；
- 方法参数：ElementType.PARAMETER。
例如，定义注解@Report可用在方法上，我们必须添加一个
@Target(ElementType.METHOD)：
```java
@Target(ElementType.METHOD)
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```
定义注解@Report可用在方法或字段上，可以把@Target注解参数变为数组{ ElementType.METHOD, ElementType.FIELD }：
```java
@Target({
    ElementType.METHOD,
    ElementType.FIELD
})
public @interface Report {
    ...
}
```
实际上@Target定义的value是ElementType\[]数组，只有一个元素时，可以省略数组的写法。


#### @Retention

另一个重要的元注解@Retention定义了Annotation的生命周期：

- 仅编译期：RetentionPolicy.SOURCE；
- 仅class文件：RetentionPolicy.CLASS；
- 运行期：RetentionPolicy.RUNTIME。

**如果@Retention不存在，则该Annotation默认为CLASS** ==因为通常我们自定义的Annotation都是RUNTIME，所以，务必要加上@Retention(RetentionPolicy.RUNTIME)这个元注解==：
```java
@Retention(RetentionPolicy.RUNTIME)
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

#### @Repeatable
使用@Repeatable这个元注解可以定义Annotation是否可重复。这个注解应用不是特别广泛。
```java
@Repeatable(Reports.class)
@Target(ElementType.TYPE)
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}

@Target(ElementType.TYPE)
public @interface Reports {
    Report[] value();
}
```
经过@Repeatable修饰后，在某个类型声明处，就可以添加多个@Report注解：
```java
@Report(type=1, level="debug")
@Report(type=2, level="warning")
public class Hello {
}
```
#### @Inherited
使用@Inherited定义子类是否可继承父类定义的Annotation。@Inherited仅针对@Target(ElementType.TYPE)类型的annotation有效，**并且仅针对class的继承，对interface的继承无效：**
```java
@Inherited
@Target(ElementType.TYPE)
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```
在使用的时候，如果一个类用到了@Report：
```java
@Report(type=1)
public class Person {
}
```
则它的子类默认也定义了该注解：
```java
public class Student extends Person {
}
```

### 定义注解步骤

1. 用@interface定义注解：
```java
public @interface Report {
}
```
2. 添加参数、默认值：
```java
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

把最常用的参数定义为value()，推荐所有参数都尽量设置默认值。

3. 用元注解配置注解：
```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```
其中，必须设置@Target和@Retention，@Retention一般设置为RUNTIME，因为自定义的注解通常要求在运行期读取。一般情况下，不必写@Inherited和@Repeatable。

## 处理注解

Java的注解本身对代码逻辑没有任何影响。根据@Retention的配置：

- SOURCE类型的注解在编译期就被丢掉了；
- CLASS类型的注解仅保存在class文件中，它们不会被加载进JVM；
- RUNTIME类型的注解会被加载进JVM，并且在运行期可以被程序读取。

如何使用注解完全由工具决定。SOURCE类型的注解主要由编译器使用，因此一般只使用，不编写。CLASS类型的注解主要由底层工具库使用，涉及到class的加载，一般很少用到。只有RUNTIME类型的注解不但要使用，还经常需要编写。

因此只讨论如何读取RUNTIME类型的注解。

因为注解定义后也是一种`class`，所有的注解都继承自java.lang.annotation.Annotation，因此，==读取注解，需要使用反射API==

Java提供的使用反射API读取Annotation的方法包括：

判断某个注解是否存在于Class、Field、Method或Constructor：

- Class.isAnnotationPresent(Class)
- Field.isAnnotationPresent(Class)
- Method.isAnnotationPresent(Class)
- Constructor.isAnnotationPresent(Class)

例如：
```java
// 判断@Report是否存在于Person类:
Person.class.isAnnotationPresent(Report.class);
```
使用反射API读取Annotation：
```java
Class.getAnnotation(Class)
Field.getAnnotation(Class)
Method.getAnnotation(Class)
Constructor.getAnnotation(Class)
```
例如：
```java
// 获取Person定义的@Report注解:
Report report = Person.class.getAnnotation(Report.class);
int type = report.type();
String level = report.level();
```
使用反射API读取Annotation有两种方法。方法一是先判断Annotation是否存在，如果存在，就直接读取：
```java
Class cls = Person.class;
if (cls.isAnnotationPresent(Report.class)) {
    Report report = cls.getAnnotation(Report.class);
    ...
}
```
第二种方法是直接读取Annotation，如果Annotation不存在，将返回null：
```java
Class cls = Person.class;
Report report = cls.getAnnotation(Report.class);
if (report != null) {
   ...
}
```
读取方法、字段和构造方法的Annotation和Class类似。但要读取方法参数的Annotation就比较麻烦一点，因为方法参数本身可以看成一个数组，而每个参数又可以定义多个注解，所以，一次获取方法参数的所有注解就必须用一个二维数组来表示。例如，对于以下方法定义的注解：
```java
public void hello(@NotNull @Range(max=5) String name, @NotNull String prefix) {
}
```
要读取方法参数的注解，我们先用反射获取Method实例，然后读取方法参数的所有注解：
```java
// 获取Method实例:
Method m = ...
// 获取所有参数的Annotation:
Annotation[][] annos = m.getParameterAnnotations();
// 第一个参数（索引为0）的所有Annotation:
Annotation[] annosOfName = annos[0];
for (Annotation anno : annosOfName) {
    if (anno instanceof Range) { // @Range注解
        Range r = (Range) anno;
    }
    if (anno instanceof NotNull) { // @NotNull注解
        NotNull n = (NotNull) anno;
    }
}
```
### 使用注解
注解如何使用，完全由程序自己决定。例如，`JUnit`是一个测试框架，**它会自动运行所有标记为@Test的方法**

观察一个@Range注解，希望用它来定义一个String字段的规则：字段长度满足@Range的参数定义：
```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
public @interface Range {
    int min() default 0;
    int max() default 255;
}
```
在某个JavaBean中，可以使用该注解：
```java
public class Person {
    @Range(min=1, max=20)
    public String name;

    @Range(max=10)
    public String city;
}
```
但是，定义了注解，本身对程序逻辑没有任何影响。须自己编写代码来使用注解。这里编写了一个Person实例的检查方法，它可以检查Person实例的String字段长度是否满足@Range的定义：
```java
void check(Person person) throws IllegalArgumentException, ReflectiveOperationException {
    // 遍历所有Field:
    for (Field field : person.getClass().getFields()) {
        // 获取Field定义的@Range:
        Range range = field.getAnnotation(Range.class);
        // 如果@Range存在:
        if (range != null) {
            // 获取Field的值:
            Object value = field.get(person);
            // 如果值是String:
            if (value instanceof String) {
                String s = (String) value;
                // 判断值是否满足@Range的min/max:
                if (s.length() < range.min() || s.length() > range.max()) {
                    throw new IllegalArgumentException("Invalid field: " + field.getName());
                }
            }
        }
    }
}
```
这样一来，通过@Range注解，配合check()方法，就可以完成Person实例的检查。注意检查逻辑完全是我们自己编写的，JVM不会自动给注解添加任何额外的逻辑。

# 泛型

## 引出泛型定义

观察Java标准库提供的`ArrayList`，它可以看作“可变长度”的数组，因为用起来比数组更方便。

实际上`ArrayList`内部就是一个Object[]数组，配合存储一个当前分配的长度，就可以充当“可变数组”：
```java
public class ArrayList {
    private Object[] array;
    private int size;
    public void add(Object e) {...}
    public void remove(int index) {...}
    public Object get(int index) {...}
}
```
如果用上述`ArrayList`存储`String`类型，会有这么几个缺点：

- 需要强制转型；

- 不方便，易出错。

例如，代码必须这么写：
```java
ArrayList list = new ArrayList();
list.add("Hello");
// 获取到Object，必须强制转型为String:
String first = (String) list.get(0);
```
很容易出现`ClassCastException`，因为容易“误转型”：

```java
list.add(new Integer(123));
// ERROR: ClassCastException:
String second = (String) list.get(1);
```
要解决上述问题，我们可以为String单独编写一种ArrayList：
```java
public class StringArrayList {
    private String[] array;
    private int size;
    public void add(String e) {...}
    public void remove(int index) {...}
    public String get(int index) {...}
}
```
这样一来，存入的必须是String，取出的也一定是String，不需要强制转型，因为编译器会强制检查放入的类型：
```java
StringArrayList list = new StringArrayList();
list.add("Hello");
String first = list.get(0);
// 编译错误: 不允许放入非String类型:
list.add(new Integer(123));
```
问题暂时解决。

然而，新的问题是，如果要存储Integer，还需要为Integer单独编写一种ArrayList：
```java
public class IntegerArrayList {
    private Integer[] array;
    private int size;
    public void add(Integer e) {...}
    public void remove(int index) {...}
    public Integer get(int index) {...}
}
```

实际上，还需要为其他所有class单独编写一种ArrayList：

- LongArrayList
- DoubleArrayList
- PersonArrayList
- ...

这是不可能的，JDK的class就有上千个，而且它还不知道其他人编写的class

为了解决新的问题，把ArrayList变成一种模板：ArrayList\<T>，代码如下：

```java
public class ArrayList<T> {
    private T[] array;
    private int size;
    public void add(T e) {...}
    public void remove(int index) {...}
    public T get(int index) {...}
}
```
T可以是任何class。这样一来，我们就实现了：编写一次模版，可以创建任意类型的ArrayList：

```java
// 创建可以存储String的ArrayList:
ArrayList<String> strList = new ArrayList<String>();
// 创建可以存储Float的ArrayList:
ArrayList<Float> floatList = new ArrayList<Float>();
// 创建可以存储Person的ArrayList:
ArrayList<Person> personList = new ArrayList<Person>();
``` 



因此，**泛型就是定义一种模板，例如ArrayList\<T>，然后在代码中为用到的的类创建对应的ArrayList<类型>:**


`ArrayList<String> strList = new ArrayList<String>();`

由编译器针对类型作检查：

```java
strList.add("hello"); // OK
String s = strList.get(0); // OK
strList.add(new Integer(123)); // compile error!
Integer n = strList.get(0); // compile error!
```

这样一来，既实现了编写一次，万能匹配，又通过编译器保证了类型安全：这就是泛型。

## 向上转型
在Java标准库中的ArrayList\<T>实现了List\<T>接口，它可以向上转型为List\<T>：
```java
public class ArrayList<T> implements List<T> {
    ...
}

List<String> list = new ArrayList<String>();
```
即类型ArrayList\<T>可以向上转型为List\<T>。

要特别注意：==不能把ArrayList\<Integer>向上转型为ArrayList\<Number>或List\<Number>==

假设ArrayList\<Integer>可以向上转型为ArrayList\<Number>，观察一下代码：
```java
// 创建ArrayList<Integer>类型：
ArrayList<Integer> integerList = new ArrayList<Integer>();
// 添加一个Integer：
integerList.add(new Integer(123));
// “向上转型”为ArrayList<Number>：
ArrayList<Number> numberList = integerList;
// 添加一个Float，因为Float也是Number：
numberList.add(new Float(12.34));
// 从ArrayList<Integer>获取索引为1的元素（即添加的Float）：
Integer n = integerList.get(1); // ClassCastException!
```
我们把一个`ArrayList\<Integer>`转型为`ArrayList\<Number>`类型后，这个`ArrayList\<Number>`就可以接受`Float`类型，因为`Float是Number`的子类。但是，`ArrayList\<Number>`实际上和`ArrayList\<Integer>`是同一个对象，也就是`ArrayList\<Integer>`类型，它不可能接受`Float`类型， 所以在获取`Integer`的时候将产生`ClassCastException`

实际上，编译器为了避免这种错误，根本就不允许把`ArrayList\<Integer>`转型为`ArrayList\<Number>`

 `ArrayList\<Integer>`和`ArrayList\<Number>`两者完全没有继承关系。
 

**泛型就是编写模板代码来适应任意类型；**

泛型的好处是使用时不必对类型进行强制转换，它通过编译器对类型进行检查；

注意泛型的继承关系：可以把ArrayList\<Integer>向上转型为List\<Integer>（T不能变！），但不能把ArrayList\<Integer>向上转型为ArrayList\<Number>（T不能变成父类）



# 集合

## 集合简介
集合就是“由若干个确定的元素所构成的整体”

在Java中，如果一个Java对象可以在内部持有若干其他Java对象，并对外提供访问接口,这种Java对象被称为集合。很显然，Java的数组可以看作是一种集合：
```java
String[] ss = new String[10]; // 可以持有10个String对象
ss[0] = "Hello"; // 可以放入String对象
String first = ss[0]; // 可以获取String对象
```
数组这种数据类型可以充当集合，还需要其他集合类的原因是数组有如下限制：

- 数组初始化后大小不可变；

- 数组只能按索引顺序存取。

需要各种不同类型的集合类来处理不同的数据，例如：

- 可变大小的顺序链表；
- 保证无重复元素的集合；
- ...

## Collection

Java标准库自带的java.util包提供了集合类：`Collection`，它是除`Map`外所有其他集合类的**根接口**。Java的java.util包主要提供了以下三种类型的集合：

- List：一种有序列表的集合，例如，按索引排列的Student的List；
- Set：一种保证没有重复元素的集合，例如，所有无重复名称的Student的Set；
- Map：一种通过键值（key-value）查找的映射表集合，例如，根据Student的name查找对应Student的Map


Java集合的设计有几个特点：一是实现了接口和实现类相分离，例如，有序表的接口是List，具体的实现类有ArrayList，LinkedList等，二是支持泛型，我们可以限制在一个集合中只能放入同一种数据类型的元素，例如：

`List<String> list = new ArrayList<>(); // 只能放入String类型`

最后，Java访问集合总是通过统一的方式——迭代器（`Iterator`）来实现，它最明显的好处在于无需知道集合内部元素是按什么方式存储的。

由于Java的集合设计非常久远，中间经历过大规模改进，==有一小部分集合类是遗留类，不应该继续使用：==

- Hashtable：一种线程安全的Map实现；
- Vector：一种线程安全的List实现；
- Stack：基于Vector实现的LIFO的栈。

还有一小部分接口是遗留接口，也不应该继续使用：

Enumeration\<E>：已被Iterator\<E>取代。


## List

在集合类中，**List是最基础的一种集合：它是一种有序列表**

List的行为和数组几乎完全相同：List内部按照放入元素的先后顺序存放，每个元素都可以通过索引确定自己的位置，List的索引和数组一样，从0开始。

数组和List类似，也是有序结构，如果我们使用数组，在添加和删除元素的时候，会非常不方便。例如，从一个已有的数组{'A', 'B', 'C', 'D', 'E'}中删除索引为2的元素：
```
┌───┬───┬───┬───┬───┬───┐
│ A │ B │ C │ D │ E │   │
└───┴───┴───┴───┴───┴───┘
              │   │
          ┌───┘   │
          │   ┌───┘
          │   │
          ▼   ▼
┌───┬───┬───┬───┬───┬───┐
│ A │ B │ D │ E │   │   │
└───┴───┴───┴───┴───┴───┘
```
这个“删除”操作实际上是把'C'后面的元素依次往前挪一个位置，而“添加”操作实际上是把指定位置以后的元素都依次向后挪一个位置，腾出来的位置给新加的元素。这两种操作，用数组实现非常麻烦。

因此，在实际应用中，==需要增删元素的有序列表，使用最多的是ArrayList==实际上，ArrayList在内部使用了数组来存储所有元素。例如，一个ArrayList拥有5个元素，实际数组大小为6（即有一个空位）：

```
size = 5
┌───┬───┬───┬───┬───┬───┐
│ A │ B │ C │ D │ E │   │
└───┴───┴───┴───┴───┴───┘
```
当添加一个元素并指定索引到ArrayList时，ArrayList自动移动需要移动的元素：
```
size=5
┌───┬───┬───┬───┬───┬───┐
│ A │ B │   │ C │ D │ E │
└───┴───┴───┴───┴───┴───┘
```
然后，往内部指定索引的数组位置添加一个元素，然后把size加1：
```
size=6
┌───┬───┬───┬───┬───┬───┐
│ A │ B │ F │ C │ D │ E │
└───┴───┴───┴───┴───┴───┘
```
继续添加元素，但是数组已满，没有空闲位置的时候，ArrayList先创建一个更大的新数组，然后把旧数组的所有元素复制到新数组，紧接着用新数组取代旧数组：
```
size=6
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│ A │ B │ F │ C │ D │ E │   │   │   │   │   │   │
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
```
现在，新数组就有了空位，可以继续添加一个元素到数组末尾，同时size加1：
```
size=7
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│ A │ B │ F │ C │ D │ E │ G │   │   │   │   │   │
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
```
可见，ArrayList把添加和删除的操作封装起来，让操作List类似于操作数组，却不用关心内部元素如何移动。

考察List\<E>接口，可以看到几个主要的接口方法：

- 在末尾添加一个元素：boolean add(E e)
- 在指定索引添加一个元素：boolean add(int index, E e)
- 删除指定索引的元素：E remove(int index)
- 删除某个元素：boolean remove(Object e)
- 获取指定索引的元素：E get(int index)
- 获取链表大小（包含元素的个数）：int size()


但是，实现List接口并非只能通过数组（即ArrayList的实现方式）来实现，另一种LinkedList通过“链表”也实现了List接口。在LinkedList中，它的内部每个元素都指向下一个元素
```
        ┌───┬───┐   ┌───┬───┐   ┌───┬───┐   ┌───┬───┐
HEAD ──>│ A │ ●─┼──>│ B │ ●─┼──>│ C │ ●─┼──>│ D │   │
        └───┴───┘   └───┴───┘   └───┴───┘   └───┴───┘
```
比较一下ArrayList和LinkedList：

|     | ArrayList | LinkedList |
| --- | --------- | ---------- |
获取指定元素	|速度很快|	需要从头开始查找元素
添加元素到末尾	|速度很快|	速度很快
在指定位置添加/删除	|需要移动元素	|不需要移动元素
内存占用	|少	|较大

通常情况下，优先使用ArrayList

### List特点

使用List时，要关注List接口的规范。List接口允许添加重复的元素，即List内部的元素可以重复：
```java
import java.util.ArrayList;
import java.util.List;
public class Main {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("apple"); // size=1
        list.add("pear"); // size=2
        list.add("apple"); // 允许重复添加元素，size=3
        System.out.println(list.size());
    }
}
```

List还允许添加null：
```java
import java.util.ArrayList;
import java.util.List;
public class Main {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("apple"); // size=1
        list.add(null); // size=2
        list.add("pear"); // size=3
        String second = list.get(1); // null
        System.out.println(second);
    }
}
```
### 创建List

除了使用ArrayList和LinkedList，我们还可以通过List接口提供的of()方法，根据给定元素快速创建List：

`List\<Integer> list = List.of(1, 2, 5);`
但是List.of()方法不接受null值，如果传入null，会抛出`NullPointerException`异常。

### 遍历List

要遍历一个List，可以用for循环根据索引配合get(int)方法遍历：
```java
import java.util.List;
public class Main {
    public static void main(String[] args) {
        List\<String> list = List.of("apple", "pear", "banana");
        for (int i=0; i<list.size(); i++) {
            String s = list.get(i);
            System.out.println(s);
        }
    }
}
```


但这种方式并不推荐，一是代码复杂，二是因为get(int)方法只有ArrayList的实现是高效的，换成LinkedList后，索引越大，访问速度越慢。

所以要始终坚持使用迭代器`Iterator`来访问List。Iterator本身也是一个对象，但它是由List的实例调用iterator()方法的时候创建的。Iterator对象知道如何遍历一个List，并且不同的List类型，返回的Iterator对象实现也是不同的，但总是具有最高的访问效率。

Iterator对象有两个方法：`boolean hasNext()`判断是否有下一个元素，`E next()`返回下一个元素。因此，使用Iterator遍历List代码如下：
```java
import java.util.Iterator;
import java.util.List;
public class Main {
    public static void main(String[] args) {
        List<String> list = List.of("apple", "pear", "banana");
        for (Iterator<String> it = list.iterator(); it.hasNext(); ) {
            String s = it.next();
            System.out.println(s);
        }
    }
}
```
通过Iterator遍历List是最高效的方式。并且，由于Iterator遍历是如此常用，所以，Java的for each循环本身就可以帮我们使用Iterator遍历。上面代码再改写如下：
```java
import java.util.List;
public class Main {
    public static void main(String[] args) {
        List<String> list = List.of("apple", "pear", "banana");
        for (String s : list) {
            System.out.println(s);
        }
    }
}
```
上述代码是编写遍历List的常见代码。

实际上，只要实现了`Iterable`接口的集合类都可以直接用for each循环来遍历，Java编译器本身并不知道如何遍历集合对象，但它会自动把for each循环变成Iterator的调用，原因在于Iterable接口定义了一个Iterator\<E> iterator()方法，强迫集合类必须返回一个Iterator实例。

### List和Array转换

把List变为Array有三种方法，第一种是调用toArray()方法直接返回一个Object[]数组：
```java
import java.util.List;
public class Main {
    public static void main(String[] args) {
        List<String> list = List.of("apple", "pear", "banana");
        Object[] array = list.toArray();
        for (Object s : array) {
            System.out.println(s);
        }
    }
}
```
==这种方法会丢失类型信息，所以实际应用很少==

第二种方式是给toArray(T[])传入一个类型相同的Array，List内部自动把元素复制到传入的Array中：
```java
import java.util.List;
public class Main {
    public static void main(String[] args) {
        List\<Integer> list = List.of(12, 34, 56);
        Integer[] array = list.toArray(new Integer[3]);
        for (Integer n : array) {
            System.out.println(n);
        }
    }
}
```
注意到这个toArray(T[])方法的泛型参数\<T>并不是List接口定义的泛型参数\<E>，实际上可以传入其他类型的数组，传入Number类型的数组，返回的是Number类型：
```java
import java.util.List;
public class Main {
    public static void main(String[] args) {
        List<Integer> list = List.of(12, 34, 56);
        Number[] array = list.toArray(new Number[3]);
        for (Number n : array) {
            System.out.println(n);
        }
    }
}
```
但是，如果传入类型不匹配的数组，例如，String[]类型的数组，由于List的元素是Integer，所以无法放入String数组，这个方法会抛出`ArrayStoreException`

如果传入的数组大小和List实际的元素个数不一致,根据List接口的文档：

**如果传入的数组不够大，那么List内部会创建一个新的刚好够大的数组，填充后返回；如果传入的数组比List元素还要多，那么填充完元素后，剩下的数组元素一律填充null**

实际上，最常用的是传入一个“恰好”大小的数组：
```java
Integer[] array = list.toArray(new Integer[list.size()]);
```
最后一种更简洁的写法是通过List接口定义的T[] toArray(IntFunction<T[]> generator)方法：
```java
Integer[] array = list.toArray(Integer[]::new);
```
这是函数式写法.

反过来，把Array变为List非常简单。通过List.of(T...)方法最简单：
```java
Integer[] array = { 1, 2, 3 };
List<Integer> list = List.of(array);
```
对于JDK 11之前的版本，可以使用Arrays.asList(T...)方法把数组转换成List

要注意的是，返回的List不一定就是ArrayList或者LinkedList，因为List只是一个接口，如果我们调用List.of()，**它返回的是一个只读List：**
```java
import java.util.List;
public class Main {
    public static void main(String[] args) {
        List<Integer> list = List.of(12, 34, 56);
        list.add(999); // UnsupportedOperationException
    }
}
```

对只读List调用add()、remove()方法会抛出`UnsupportedOperationException`

## equels

List是一种有序链表：List内部按照放入元素的先后顺序存放，并且每个元素都可以通过索引确定自己的位置。

List还提供了`boolean contains(Object o)`方法来判断List是否包含某个指定元素。此外，`int indexOf(Object o)`方法可以返回某个元素的索引，如果元素不存在，就返回-1。

一个例子：
```java
import java.util.List;
public class Main {
    public static void main(String[] args) {
        List<String> list = List.of("A", "B", "C");
        System.out.println(list.contains("C")); // true
        System.out.println(list.contains("X")); // false
        System.out.println(list.indexOf("C")); // 2
        System.out.println(list.indexOf("X")); // -1
    }
}
```
注意一个问题，往List中添加的"C"和调用contains("C")传入的"C"是不是同一个实例？

测试一下：
```java
import java.util.List;
public class Main {
    public static void main(String[] args) {
        List<String> list = List.of("A", "B", "C");
        System.out.println(list.contains(new String("C"))); // true or false?
        System.out.println(list.indexOf(new String("C"))); // 2 or -1?
    }
}
```
 传入的是new String("C")，是不同的实例。结果仍然符合预期，因为List内部并不是通过== 判断两个元素是否相等，而是使用`equals()`方法判断两个元素是否相等，例如contains()方法可以实现如下：
```java
public class ArrayList {
    Object[] elementData;
    public boolean contains(Object o) {
        for (int i = 0; i < elementData.length; i++) {
            if (o.equals(elementData[i])) {
                return true;
            }
        }
        return false;
    }
}
```
因此，==要正确使用List的contains()、indexOf()这些方法，放入的实例必须正确覆写equals()方法==，否则，放进去的实例，查找不到。**之所以能正常放入String、Integer这些对象，是因为Java标准库定义的这些类已经正确实现了equals()方法**

以Person对象为例测试
```java
import java.util.List;
public class Main {
    public static void main(String[] args) {
        List<Person> list = List.of(
            new Person("Xiao Ming"),
            new Person("Xiao Hong"),
            new Person("Bob")
        );
        System.out.println(list.contains(new Person("Bob"))); // false
    }
}

class Person {
    String name;
    public Person(String name) {
        this.name = name;
    }
}
```

虽然放入了new Person("Bob")，但是用另一个new Person("Bob")查询不到，原因是Person类没有覆写equals()方法。

### 编写equals

equals()方法要求我们必须满足以下条件：

- 自反性（Reflexive）：对于非null的x来说，x.equals(x)必须返回true；
- 对称性（Symmetric）：对于非null的x和y来说，如果x.equals(y)为true，则y.equals(x)也必须为true；
- 传递性（Transitive）：对于非null的x、y和z来说，如果x.equals(y)为true，y.equals(z)也为true，那么x.equals(z)也必须为true；
- 一致性（Consistent）：对于非null的x和y来说，只要x和y状态不变，则x.equals(y)总是一致地返回true或者false；
- 对null的比较：即x.equals(null)永远返回false。


以Person类为例实现：
```java
public class Person {
    public String name;
    public int age;
}
```
首先要定义“相等”的逻辑含义。对于Person类，如果name相等，并且age相等，就认为两个Person实例相等。

因此，编写equals()方法如下：
```java
public boolean equals(Object o) {
    if (o instanceof Person) {
        Person p = (Person) o;
        return this.name.equals(p.name) && this.age == p.age;
    }
    return false;
}
```
对于引用字段比较，使用equals()，对于基本类型字段的比较，使用== 

如果this.name为null，那么equals()方法会报错，因此，需要继续改写如下：

```java
public boolean equals(Object o) {
    if (o instanceof Person) {
        Person p = (Person) o;
        boolean nameEquals = false;
        if (this.name == null && p.name == null) {
            nameEquals = true;
        }
        if (this.name != null) {
            nameEquals = this.name.equals(p.name);
        }
        return nameEquals && this.age == p.age;
    }
    return false;
}
```
如果Person有好几个引用类型的字段，上面的写法太复杂。简化引用类型的比较，使用Objects.equals()静态方法：
```java
public boolean equals(Object o) {
    if (o instanceof Person) {
        Person p = (Person) o;
        return Objects.equals(this.name, p.name) && this.age == p.age;
    }
    return false;
}
```
总结equals()的正确编写方法：

- 先确定实例“相等”的逻辑，即哪些字段相等，就认为实例相等；
- 用instanceof判断传入的待比较的Object是不是当前类型，如果是，继续比较，否则，返回false；
- 对引用类型用Objects.equals()比较，对基本类型直接用== 比较。
- 使用Objects.equals()比较两个引用类型是否相等的目的是省去了判断null的麻烦。两个引用类型都是null时它们也是相等的。

如果不调用List的contains()、indexOf()这些方法，那么放入的元素就不需要实现equals()方法。

## Map

List是一种顺序列表，如果有一个存储学生Student实例的List，要在List中根据name查找某个指定的Student的分数，最简单的方法是遍历List并判断name是否相等，然后返回指定元素：
```java
List<Student> list = ...
Student target = null;
for (Student s : list) {
    if ("Xiao Ming".equals(s.name)) {
        target = s;
        break;
    }
}
System.out.println(target.score);
```

这种需求非常常见，即通过一个键去查询对应的值。使用List来实现存在效率非常低的问题，因为平均需要扫描一半的元素才能确定，而Map这种键值（key-value）映射表的数据结构，作用就是能高效通过key快速查找value（元素）

用Map来实现根据name查询某个Student的代码如下：
```java
import java.util.HashMap;
import java.util.Map;
public class Main {
    public static void main(String[] args) {
        Student s = new Student("Xiao Ming", 99);
        Map<String, Student> map = new HashMap<>();
        map.put("Xiao Ming", s); // 将"Xiao Ming"和Student实例映射并关联
        Student target = map.get("Xiao Ming"); // 通过key查找并返回映射的Student实例
        System.out.println(target == s); // true，同一个实例
        System.out.println(target.score); // 99
        Student another = map.get("Bob"); // 通过另一个key查找
        System.out.println(another); // 未找到返回null
    }
}
class Student {
    public String name;
    public int score;
    public Student(String name, int score) {
        this.name = name;
        this.score = score;
    }
}
```

通过上述代码可知：Map<K, V>是一种键-值映射表，调用put(K key, V value)方法时，就把key和value做了映射并放入Map。当我们调用V get(K key)时，就可以通过key获取到对应的value。如果key不存在，则返回null。**和List类似，Map也是一个接口，最常用的实现类是HashMap**

如果只是想查询某个key是否存在，可以调用`boolean containsKey(K key)`方法

如果在存储Map映射关系的时候，对同一个key调用两次put()方法，分别放入不同的value，例如：
```java
import java.util.HashMap;
import java.util.Map;
public class Main {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        map.put("apple", 123);
        map.put("pear", 456);
        System.out.println(map.get("apple")); // 123
        map.put("apple", 789); // 再次放入apple作为key，但value变为789
        System.out.println(map.get("apple")); // 789
    }
}
```
重复放入key-value并不会有任何问题，但是一个key只能关联一个value。在上面的代码中，一开始把key对象"apple"映射到Integer对象123，然后再次调用put()方法把"apple"映射到789，这时，原来关联的value对象123就被“冲掉”了。实际上，put()方法的签名是V put(K key, V value)，==如果放入的key已经存在，put()方法会返回被删除的旧的value，否则，返回null==

 牢记：Map中不存在重复的key，因为放入相同的key，只会把原有的key-value对应的value给替换掉
 
此外，在一个Map中，虽然key不能重复，但value是可以重复的：
```java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 123);
map.put("pear", 123); // ok
```
### 遍历Map

对Map来说，要遍历key可以使用for each循环遍历Map实例的`keySet()`方法返回的Set集合，它包含不重复的key的集合：

```java
import java.util.HashMap;
import java.util.Map;
public class Main {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        map.put("apple", 123);
        map.put("pear", 456);
        map.put("banana", 789);
        for (String key : map.keySet()) {
            Integer value = map.get(key);
            System.out.println(key + " = " + value);
        }
    }
}
```
 
同时遍历key和value可以使用for each循环遍历Map对象的`entrySet()`集合，它包含每一个key-value映射：
```java
import java.util.HashMap;
import java.util.Map;
public class Main {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        map.put("apple", 123);
        map.put("pear", 456);
        map.put("banana", 789);
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            String key = entry.getKey();
            Integer value = entry.getValue();
            System.out.println(key + " = " + value);
        }
    }
}
```

`Map`和`List`不同的是，`Map`存储的是`key-value`的映射关系，并且，**它不保证顺序** 在遍历的时候，遍历的顺序既不一定是`put()`时放入的`key`的顺序，也不一定是`key`的排序顺序。使用`Map`时，任何依赖顺序的逻辑都是不可靠的 
 以HashMap为例，假设我们放入"A"，"B"，"C"这3个`key`，遍历的时候，每个`key`会保证被遍历一次且仅遍历一次，但顺序完全没有保证，甚至对于不同的JDK版本，相同的代码遍历的输出顺序都是不同的


# IO

## Introduction

IO指Input/Output，即输入和输出。以内存为中心：

- Input指从外部读入数据到内存，例如，把文件从磁盘读取到内存，从网络读取数据到内存等等。

- Output指把数据从内存输出到外部，例如，把数据从内存写入到文件，把数据从内存输出到网络等等。

把数据读到内存才能处理的原因是：代码是在内存中运行的，数据也必须读到内存，最终的表示方式无非是byte数组，字符串等，都必须存放在内存里。

从Java代码来看，输入实际上就是从外部，例如，硬盘上的某个文件，把内容读到内存，并且以Java提供的某种数据类型表示，例如，byte[]，String，这样，后续代码才能处理这些数据。

因为内存有“易失性”的特点，所以必须把处理后的数据以某种方式输出，例如，写入到文件。Output实际上就是把Java表示的数据格式，例如，byte[]，String等输出到某个地方。

IO流是一种**顺序**读写数据的模式，它的特点是单向流动。数据类似自来水一样在水管中流动，所以我们把它称为IO流。


### InputStream / OutputStream

IO流以`byte`（字节）为最小单位，因此也称为字节流。例如，我们要从磁盘读入一个文件，包含6个字节，就相当于读入了6个字节的数据：
```
╔════════════╗
║   Memory   ║
╚════════════╝
       ▲
       │0x48
       │0x65
       │0x6c
       │0x6c
       │0x6f
       │0x21
 ╔═══════════╗
 ║ Hard Disk ║
 ╚═══════════╝
 ```
这6个字节是按顺序读入的，所以是输入字节流。

反过来，我们把6个字节从内存写入磁盘文件，就是输出字节流：
```
╔════════════╗
║   Memory   ║
╚════════════╝
       │0x21
       │0x6f
       │0x6c
       │0x6c
       │0x65
       │0x48
       ▼
 ╔═══════════╗
 ║ Hard Disk ║
 ╚═══════════╝
 ```
在Java中，InputStream代表输入字节流，OuputStream代表输出字节流，这是最基本的两种IO流。

### Reader / Writer

如果需要读写的是字符，并且字符不全是单字节表示的ASCII字符，那么，按照char来读写显然更方便，这种流称为字符流。

Java提供了Reader和Writer表示字符流，字符流传输的最小数据单位是char。

例如，把`char[]`数组`Hi你好`这4个字符用Writer字符流写入文件，并且使用UTF-8编码，得到的最终文件内容是8个字节，英文字符H和i各占一个字节，中文字符你好各占3个字节：
```
0x48
0x69
0xe4bda0
0xe5a5bd
```
反过来，用Reader读取以UTF-8编码的这8个字节，会从Reader中得到Hi你好这4个字符。

因此，==Reader和Writer本质上是一个能自动编解码的InputStream和OutputStream==

使用Reader，数据源虽然是字节，但读入的数据都是char类型的字符，原因是Reader内部把读入的byte做了解码，转换成了char。使用InputStream，读入的数据和原始二进制数据一模一样，是byte[]数组，但是可以自己把二进制byte[]数组按照某种编码转换为字符串。究竟使用Reader还是InputStream，要取决于具体的使用场景。如果数据源不是文本，就只能使用InputStream，如果数据源是文本，使用Reader更方便一些。Writer和OutputStream是类似的。

### 同步和异步

同步IO是指，读写IO时代码必须等待数据返回后才继续执行后续代码，它的优点是代码编写简单，缺点是CPU执行效率低。

而异步IO是指，读写IO时仅发出请求，然后立刻执行后续代码，它的优点是CPU执行效率高，缺点是代码编写复杂。

Java标准库的包`java.io`提供了同步IO，而`java.nio`则是异步IO。上面讨论的InputStream、OutputStream、Reader和Writer都**是同步IO的抽象类**，对应的具体实现类，以文件为例，有FileInputStream、FileOutputStream、FileReader和FileWriter。

## File对象

在计算机系统中，文件是非常重要的存储方式。Java的标准库java.io提供了File对象来操作文件和目录。

要构造一个File对象，需要传入文件路径：
```java
import java.io.*;
public class Main {
    public static void main(String[] args) {
        File f = new File("C:\\Windows\\notepad.exe");
        System.out.println(f);
    }
}
```

构造File对象时，既可以传入绝对路径，也可以传入相对路径。绝对路径是以根目录开头的完整路径，例如：
```java
File f = new File("C:\\Windows\\notepad.exe");
```
注意Windows平台使用\作为路径分隔符，在Java字符串中需要用\\\表示一个\ Linux平台使用/作为路径分隔符：
```java
File f = new File("/usr/bin/javac");
```
传入相对路径时，相对路径前面加上当前目录就是绝对路径：
```java
// 假设当前目录是C:\Docs
File f1 = new File("sub\\javac"); // 绝对路径是C:\Docs\sub\javac
File f3 = new File(".\\sub\\javac"); // 绝对路径是C:\Docs\sub\javac
File f3 = new File("..\\sub\\javac"); // 绝对路径是C:\sub\javac
```
可以用.表示当前目录，..表示上级目录

File对象有3种形式表示的路径，一种是getPath()，返回构造方法传入的路径，一种是getAbsolutePath()，返回绝对路径，一种是getCanonicalPath，它和绝对路径类似，但是返回的是规范路径。

什么是规范路径？看以下代码：
```java
import java.io.*;
public class Main {
    public static void main(String[] args) throws IOException {
        File f = new File("..");
        System.out.println(f.getPath());
        System.out.println(f.getAbsolutePath());
        System.out.println(f.getCanonicalPath());
    }
}
```

绝对路径可以表示成`C:\Windows\System32\..\notepad.exe`，而规范路径就是把.和..转换成标准的绝对路径后的路径：`C:\Windows\notepad.exe`

因为Windows和Linux的路径分隔符不同，File对象有一个[[Tree/javaSE#静态字段\|静态字段]]用于表示当前平台的系统分隔符：
```java
System.out.println(File.separator); // 根据当前平台打印"\"或"/"
```

### 文件和目录

**File对象既可以表示文件，也可以表示目录** 特别要注意的是，==构造一个File对象，即使传入的文件或目录不存在，代码也不会出错==，因为构造一个File对象，并不会导致任何磁盘操作。**只有调用File对象的某些方法的时候，才真正进行磁盘操作**

例如，调用isFile()，判断该File对象是否是一个已存在的文件，调用isDirectory()，判断该File对象是否是一个已存在的目录：

```java
import java.io.*;
public class Main {
    public static void main(String[] args) throws IOException {
        File f1 = new File("C:\\Windows");
        File f2 = new File("C:\\Windows\\notepad.exe");
        File f3 = new File("C:\\Windows\\nothing");
        System.out.println(f1.isFile());
        System.out.println(f1.isDirectory());
        System.out.println(f2.isFile());
        System.out.println(f2.isDirectory());
        System.out.println(f3.isFile());
        System.out.println(f3.isDirectory());
    }
}
```

用File对象获取到一个文件时，还可以进一步判断文件的权限和大小：

- boolean canRead()：是否可读；
- boolean canWrite()：是否可写； 
- boolean canExecute()：是否可执行；
- long length()：文件字节大小。

对目录而言，是否可执行表示能否列出它包含的文件和子目录。

### 创建和删除文件

当File对象表示一个文件时，可以通过createNewFile()创建一个新文件，用delete()删除该文件：
```java
File file = new File("/path/to/file");
if (file.createNewFile()) {
    // 文件创建成功:
    // TODO:
    if (file.delete()) {
        // 删除文件成功:
    }
}
```
有些时候，程序需要读写一些临时文件，File对象提供了createTempFile()来创建一个临时文件，以及deleteOnExit()在JVM退出时自动删除该文件。
```java
import java.io.*;
public class Main {
    public static void main(String[] args) throws IOException {
        File f = File.createTempFile("tmp-", ".txt"); // 提供临时文件的前缀和后缀
        f.deleteOnExit(); // JVM退出时自动删除
        System.out.println(f.isFile());
        System.out.println(f.getAbsolutePath());
    }
}
```
 
### 遍历文件和目录

当File对象表示一个目录时，可以使用list()和listFiles()列出目录下的文件和子目录名。listFiles()提供了一系列重载方法，可以过滤不想要的文件和目录：
```java
import java.io.*;
public class Main {
    public static void main(String[] args) throws IOException {
        File f = new File("C:\\Windows");
        File[] fs1 = f.listFiles(); // 列出所有文件和子目录
        printFiles(fs1);
        File[] fs2 = f.listFiles(new FilenameFilter() { // 仅列出.exe文件
            public boolean accept(File dir, String name) {
                return name.endsWith(".exe"); // 返回true表示接受该文件
            }
        });
        printFiles(fs2);
    }

    static void printFiles(File[] files) {
        System.out.println("==========");
        if (files != null) {
            for (File f : files) {
                System.out.println(f);
            }
        }
        System.out.println("==========");
    }
}
```

和文件操作类似，File对象如果表示一个目录，可以通过以下方法创建和删除目录：

- boolean mkdir()：创建当前File对象表示的目录；
- boolean mkdirs()：创建当前File对象表示的目录，并在必要时将不存在的父目录也创建出来；
- boolean delete ()：删除当前File对象表示的目录，当前目录必须为空才能删除成功。

### Path

Java标准库还提供了一个Path对象，它位于java.nio.file包。Path对象和File对象类似，但操作更加简单：

```java
import java.io.*;
import java.nio.file.*;
public class Main {
    public static void main(String[] args) throws IOException {
        Path p1 = Paths.get(".", "project", "study"); // 构造一个Path对象
        System.out.println(p1);
        Path p2 = p1.toAbsolutePath(); // 转换为绝对路径
        System.out.println(p2);
        Path p3 = p2.normalize(); // 转换为规范路径
        System.out.println(p3);
        File f = p3.toFile(); // 转换为File对象
        System.out.println(f);
        for (Path p : Paths.get("..").toAbsolutePath()) { // 可以直接遍历Path
            System.out.println("  " + p);
        }
    }
}
```
如果需要对目录进行复杂的拼接、遍历等操作，使用Path对象更方便


# 多线程

## 多线程基础

现代操作系统（Windows，macOS，Linux）都可以执行多任务。多任务就是同时运行多个任务，例如：

CPU执行代码都是一条一条顺序执行的，但是，即使是单核cpu，也可以同时运行多个任务。因为操作系统执行多任务实际上就是让CPU对多个任务轮流交替执行。

例如，假设我们有语文、数学、英语3门作业要做，每个作业需要30分钟。我们把这3门作业看成是3个任务，可以做1分钟语文作业，再做1分钟数学作业，再做1分钟英语作业：

这样轮流做下去，在某些人眼里看来，做作业的速度就非常快，看上去就像同时在做3门作业一样

类似的，操作系统轮流让多个任务交替执行，例如，让浏览器执行0.001秒，让QQ执行0.001秒，再让音乐播放器执行0.001秒，在人看来，CPU就是在同时执行多个任务。

即使是多核CPU，因为通常任务的数量远远多于CPU的核数，所以任务也是交替执行的。

### 进程

在计算机中，我们把一个任务称为一个进程，浏览器就是一个进程，视频播放器是另一个进程，类似的，音乐播放器和Word都是进程。

某些进程内部还需要同时执行多个子任务。例如，我们在使用Word时，Word可以让我们一边打字，一边进行拼写检查，同时还可以在后台进行打印，我们把子任务称为线程。

进程和线程的关系就是：一个进程可以包含一个或多个线程，但至少会有一个线程。

```
                        ┌──────────┐
                        │Process   │
                        │┌────────┐│
            ┌──────────┐││ Thread ││┌──────────┐
            │Process   ││└────────┘││Process   │
            │┌────────┐││┌────────┐││┌────────┐│
┌──────────┐││ Thread ││││ Thread ││││ Thread ││
│Process   ││└────────┘││└────────┘││└────────┘│
│┌────────┐││┌────────┐││┌────────┐││┌────────┐│
││ Thread ││││ Thread ││││ Thread ││││ Thread ││
│└────────┘││└────────┘││└────────┘││└────────┘│
└──────────┘└──────────┘└──────────┘└──────────┘
┌──────────────────────────────────────────────┐
│               Operating System               │
└──────────────────────────────────────────────┘
```

操作系统调度的最小任务单位其实不是进程，而是线程。常用的Windows、Linux等操作系统都采用抢占式多任务，如何调度线程完全由操作系统决定，程序自己不能决定什么时候执行，以及执行多长时间。

因为同一个应用程序，既可以有多个进程，也可以有多个线程，因此，实现多任务的方法，有以下几种：

多进程模式（每个进程只有一个线程）：
```
┌──────────┐ ┌──────────┐ ┌──────────┐
│Process   │ │Process   │ │Process   │
│┌────────┐│ │┌────────┐│ │┌────────┐│
││ Thread ││ ││ Thread ││ ││ Thread ││
│└────────┘│ │└────────┘│ │└────────┘│
└──────────┘ └──────────┘ └──────────┘
```
多线程模式（一个进程有多个线程）：
```
┌────────────────────┐
│Process             │
│┌────────┐┌────────┐│
││ Thread ││ Thread ││
│└────────┘└────────┘│
│┌────────┐┌────────┐│
││ Thread ││ Thread ││
│└────────┘└────────┘│
└────────────────────┘
```
多进程＋多线程模式（复杂度最高）：
```
┌──────────┐┌──────────┐┌──────────┐
│Process   ││Process   ││Process   │
│┌────────┐││┌────────┐││┌────────┐│
││ Thread ││││ Thread ││││ Thread ││
│└────────┘││└────────┘││└────────┘│
│┌────────┐││┌────────┐││┌────────┐│
││ Thread ││││ Thread ││││ Thread ││
│└────────┘││└────────┘││└────────┘│
└──────────┘└──────────┘└──────────┘
```

### 进程 vs 线程

进程和线程是包含关系，但是多任务既可以由多进程实现，也可以由单进程内的多线程实现，还可以混合多进程＋多线程。

具体采用哪种方式，要考虑到进程和线程的特点。

和多线程相比，多进程的缺点在于：

- 创建进程比创建线程开销大，尤其是在Windows系统上；
- 进程间通信比线程间通信要慢，因为线程间通信就是读写同一个变量，速度很快。


而多进程的优点在于：

多进程稳定性比多线程高，因为在多进程的情况下，一个进程崩溃不会影响其他进程，而在多线程的情况下，任何一个线程崩溃会直接导致整个进程崩溃。

### 多线程

Java语言内置了多线程支持：一个Java程序实际上是一个JVM进程，JVM进程用一个主线程来执行main()方法，在main()方法内部，我们又可以启动多个线程。此外，JVM还有负责垃圾回收的其他工作线程等。

因此，对于大多数Java程序来说，多任务，实际上是如何使用多线程实现多任务。

和单线程相比，多线程编程的特点在于：多线程经常需要读写共享数据，并且需要同步。例如，播放电影时，就必须由一个线程播放视频，另一个线程播放音频，两个线程需要协调运行，否则画面和声音就不同步。因此，多线程编程的复杂度高，调试更困难。

Java多线程编程的特点又在于：

- 多线程模型是Java程序最基本的并发模型；
- 后续读写网络、数据库、Web开发等都依赖Java多线程模型。


# JDBC

Details in [[Tree/JDBC\|JDBC]] page. 


