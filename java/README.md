# Java学习教程

本目录包含系统化的Java学习教程，从基础到进阶，帮助你掌握Java编程。

## 教程列表

### 基础篇

1. **01_Java基础语法.md**
   - 第一个Java程序
   - 变量和数据类型（基本类型、引用类型）
   - 常量（final关键字）
   - 输入输出（Scanner、printf）
   - 运算符（算术、关系、逻辑、位运算）
   - 类型转换
   - 字符串（String类）
   - 注释和文档注释（Javadoc）
   - 包和导入

2. **02_Java控制流.md**
   - if条件语句
   - switch语句（传统和switch表达式）
   - for循环（传统和增强for循环）
   - while和do-while循环
   - break和continue
   - 嵌套循环
   - 综合示例

### 进阶篇

3. **03_Java数组和集合.md**
   - 一维数组和二维数组
   - ArrayList、LinkedList
   - HashSet、TreeSet
   - HashMap、TreeMap
   - 集合工具类（Collections）
   - 迭代器（Iterator）
   - 综合示例

4. **04_Java面向对象编程.md**
   - 类和对象
   - 构造函数
   - 成员变量和成员方法
   - 访问控制（public、private、protected）
   - static关键字
   - this关键字
   - 方法重载
   - 封装

5. **05_Java继承和多态.md**
   - 继承基础
   - super关键字
   - 方法重写
   - 多态
   - 抽象类和抽象方法
   - final关键字
   - 接口（interface）
   - 函数式接口

6. **06_Java异常处理.md**
   - 异常处理基础
   - try-catch-finally
   - 抛出异常（throw、throws）
   - 自定义异常
   - 异常层次结构
   - try-with-resources（Java 7+）
   - 异常链

7. **07_Java常用类库.md**
   - Object类（toString、equals、hashCode）
   - String类（不可变性、常用方法）
   - StringBuilder和StringBuffer
   - 包装类（Integer、Double等）
   - Math类
   - Random类
   - Date和Calendar（已废弃）
   - LocalDateTime（Java 8+）
   - 格式化类（DecimalFormat、SimpleDateFormat）

8. **08_Java文件操作.md**
   - File类（文件和目录操作）
   - 字节流（FileInputStream、FileOutputStream）
   - 字符流（FileReader、FileWriter）
   - 缓冲流（BufferedReader、BufferedWriter）
   - PrintWriter和Scanner
   - 对象序列化（Serializable）
   - NIO Files类（Java 7+）
   - 综合示例

### 高级篇（待创建）

9. **09_Java泛型.md**（待创建）
   - 泛型基础
   - 泛型类
   - 泛型方法
   - 通配符
   - 类型擦除

10. **10_Java集合框架详解.md**（待创建）
    - Collection接口
    - List接口及实现
    - Set接口及实现
    - Map接口及实现
    - 迭代器
    - 集合工具类

11. **11_Java多线程编程.md**（待创建）
    - 线程创建（Thread、Runnable）
    - 线程生命周期
    - 线程同步（synchronized）
    - 锁（Lock、ReentrantLock）
    - 线程通信
    - 线程池
    - 并发工具类

12. **12_Java网络编程.md**（待创建）
    - Socket编程
    - TCP/UDP通信
    - URL和URI
    - HTTP客户端
    - 网络编程示例

13. **13_Java数据库编程（JDBC）.md**（待创建）
    - JDBC基础
    - 连接数据库
    - 执行SQL语句
    - 结果集处理
    - 事务处理
    - 连接池

14. **14_Java IO和NIO.md**（待创建）
    - 传统IO（字节流、字符流）
    - NIO（Channel、Buffer、Selector）
    - 文件操作
    - 网络IO

15. **15_Java Lambda表达式和Stream API.md**（待创建）
    - Lambda表达式（Java 8+）
    - 函数式接口
    - 方法引用
    - Stream API
    - Optional类

16. **16_Java反射和注解.md**
    - 反射基础（Class类）
    - 获取类的成员（字段、方法、构造函数）
    - 创建对象和调用方法
    - 访问和修改字段
    - 注解基础（内置注解）
    - 自定义注解
    - 注解处理（运行时）
    - 综合示例

17. **17_Java设计模式.md**
    - 单例模式（多种实现方式）
    - 工厂模式（简单工厂、工厂方法）
    - 观察者模式（Java内置Observer、自定义）
    - 策略模式
    - 适配器模式
    - 装饰器模式

18. **18_Java实战项目.md**
    - 学生管理系统（完整实现）
    - 图书管理系统
    - 计算器程序
    - 项目结构组织
    - Maven构建系统
    - 综合项目示例

## 学习方法

1. **按顺序学习**：建议按照编号顺序学习，每个教程都是在前面的基础上构建的
2. **动手实践**：每个教程都包含大量代码示例，建议自己编写和运行这些代码
3. **完成练习**：每个教程末尾都有练习题，完成它们可以加深理解
4. **阅读源码**：理解示例代码的逻辑，尝试修改和扩展

## 编译和运行

### 使用javac和java

```bash
# 编译
javac HelloWorld.java

# 运行
java HelloWorld
```

### 使用IDE

- **Eclipse** - 功能强大的Java IDE
- **IntelliJ IDEA** - JetBrains的Java IDE
- **NetBeans** - Oracle的Java IDE
- **VS Code** - 轻量级编辑器（配合Java扩展）

## 常用编译选项

- `-encoding UTF-8`: 指定编码
- `-d`: 指定输出目录
- `-cp` 或 `-classpath`: 指定类路径
- `-source`: 指定源代码版本
- `-target`: 指定目标字节码版本

## 开发工具推荐

### JDK版本
- **Java 8 (LTS)** - 广泛使用
- **Java 11 (LTS)** - 长期支持版本
- **Java 17 (LTS)** - 最新长期支持版本
- **Java 21 (LTS)** - 最新版本

### 构建工具
- **Maven** - 项目管理和构建工具
- **Gradle** - 现代化构建工具
- **Ant** - 传统构建工具

### 版本控制
- **Git** - 分布式版本控制系统
- **SVN** - 集中式版本控制系统

## 推荐资源

- [Oracle Java文档](https://docs.oracle.com/javase/)
- [Java API文档](https://docs.oracle.com/javase/8/docs/api/)
- [Java教程（Oracle）](https://docs.oracle.com/javase/tutorial/)
- [Effective Java](https://www.oracle.com/technical-resources/articles/java/effectivejava.html)

## 学习路径

```
基础语法 → 控制流 → 数组和集合 → 面向对象编程
    ↓
继承和多态 → 异常处理 → 常用类库 → 文件操作
    ↓
泛型 → 集合框架 → 多线程 → 网络编程
    ↓
JDBC → IO/NIO → Lambda → 反射和注解
    ↓
设计模式 → 实战项目
```

## 注意事项

1. **Java是区分大小写的**
2. **类名首字母大写，方法名首字母小写**
3. **每个类应该在一个单独的文件中**
4. **文件名必须与公共类名相同**
5. **Java中的参数传递都是值传递**
6. **使用包来组织代码**
7. **遵循Java编码规范**

## 常见问题

### Q: Java和JavaScript有什么区别？
A: 
- Java是编译型语言，JavaScript是解释型语言
- Java主要用于后端开发，JavaScript主要用于前端开发
- Java是强类型语言，JavaScript是弱类型语言
- 两者语法完全不同

### Q: Java如何实现跨平台？
A: 
- Java源代码编译成字节码（.class文件）
- 字节码在JVM（Java虚拟机）上运行
- 不同平台有对应的JVM实现
- "一次编写，到处运行"（Write Once, Run Anywhere）

### Q: 什么是JRE、JDK和JVM？
A: 
- **JVM** (Java Virtual Machine): Java虚拟机，运行字节码
- **JRE** (Java Runtime Environment): Java运行环境，包含JVM和运行时库
- **JDK** (Java Development Kit): Java开发工具包，包含JRE和开发工具

祝你学习愉快！

