---
{"dg-publish":true,"permalink":"/tree/design-patterns/","tags":["CS"],"created":"2022-08-15T19:50:07.984+08:00","updated":"2023-08-27T03:40:06.503+08:00"}
---


# Introduction

设计模式（Design pattern）代表了最佳的实践, 是软件开发人员在软件开发过程中面临的一般问题的解决方案。

设计模式是一套被反复使用的、多数人知晓的、经过分类编目的、代码设计经验的总结。使用设计模式是为了重用代码、让代码更容易被他人理解、保证代码可靠性。

设计模式使代码编制真正工程化，是软件工程的基石. 每种模式在现实中都有相应的原理来与之对应，都描述了一个不断重复发生的问题，以及该问题的核心解决方案.

# Category

根据设计模式的参考书 Design Patterns - Elements of Reusable Object-Oriented Software中所提到的，总共有 23 种设计模式。
这些模式可以分为三大类：
- 创建型模式（[[Tree/creational-patterns\|creational-patterns]]）
- 结构型模式（[[Tree/structural-patterns\|structural-patterns]]）
- 行为型模式（[[behavioral-patterns\|behavioral-patterns]]）

此外的另一类设计模式：[[Tree/javaEE-design-patterns\|javaEE-design-patterns]]

---

设计模式间的联系

---

![](https://gcore.jsdelivr.net/gh/AlexLiu2022/resources/img/relationships-between-design-patterns.png)


# Principles

设计模式的六大原则

1. 开闭原则（Open Close Principle）

开闭原则的意思是：对扩展开放，对修改关闭。在程序需要进行拓展的时候，不能去修改原有的代码，实现一个热插拔的效果。简言之，是为了使程序的扩展性好，易于维护和升级。想要达到这样的效果，需要使用接口和抽象类.

2. 里氏代换原则（Liskov Substitution Principle）

里氏代换原则是面向对象设计的基本原则之一。 里氏代换原则中说，任何基类可以出现的地方，子类一定可以出现。LSP 是继承复用的基石，只有当派生类可以替换掉基类，且软件单位的功能不受到影响时，基类才能真正被复用，而派生类也能够在基类的基础上增加新的行为。里氏代换原则是对开闭原则的补充。实现开闭原则的关键步骤就是抽象化，而基类与子类的继承关系就是抽象化的具体实现，所以里氏代换原则是对实现抽象化的具体步骤的规范。

3. 依赖倒转原则（Dependence Inversion Principle）

这个原则是开闭原则的基础，具体内容：针对接口编程，依赖于抽象而不依赖于具体。

4. 接口隔离原则（Interface Segregation Principle）

这个原则的意思是：使用多个隔离的接口，比使用单个接口要好。它还有另外一个意思是：降低类之间的耦合度。由此可见，其实设计模式就是从大型软件架构出发、便于升级和维护的软件设计思想，它强调降低依赖，降低耦合。

5. 迪米特法则，又称最少知道原则（Demeter Principle）

最少知道原则是指：一个实体应当尽量少地与其他实体之间发生相互作用，使得系统功能模块相对独立。

6. 合成复用原则（Composite Reuse Principle）

合成复用原则是指：尽量使用合成/聚合的方式，而不是使用继承。