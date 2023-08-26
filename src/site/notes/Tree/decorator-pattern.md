---
{"dg-publish":true,"permalink":"/tree/decorator-pattern/","tags":["CS/design-patterns/structural-patterns"],"created":"2022-08-15T20:16:56.044+08:00","updated":"2023-08-27T04:44:54.562+08:00"}
---


装饰设计模式

创建一个新类，包装原始类，从而在新类中提升原来类的功能。

装饰设计模式的作用：
- 在不改变原类的基础上，动态地扩展一个类的功能。

eg.

- InputStream(抽象父类)
- FileInputStream(实现子类，读写性能较差)
- BufferedInputStream(实现子类，装饰类，读写性能高)

1. 定义父类。
2. 定义原始类，继承父类，定义功能。
3. 定义装饰类，继承父类，包装原始类，增强功能