# Java面向对象编程

本教程将学习Java中的面向对象编程，包括类和对象、构造函数、成员变量、成员方法等。

## 1. 类和对象

### 基本类定义

```java
// Person.java
public class Person {
    // 成员变量（属性）
    private String name;
    private int age;
    
    // 成员方法
    public void setName(String name) {
        this.name = name;
    }
    
    public String getName() {
        return name;
    }
    
    public void setAge(int age) {
        if (age > 0) {
            this.age = age;
        }
    }
    
    public int getAge() {
        return age;
    }
    
    public void introduce() {
        System.out.println("我是 " + name + "，今年 " + age + " 岁");
    }
}

// Main.java
public class Main {
    public static void main(String[] args) {
        // 创建对象
        Person person1 = new Person();
        person1.setName("张三");
        person1.setAge(25);
        person1.introduce();
        
        // 创建另一个对象
        Person person2 = new Person();
        person2.setName("李四");
        person2.setAge(30);
        person2.introduce();
    }
}
```

## 2. 构造函数

### 默认构造函数

```java
public class Student {
    private String name;
    private int age;
    
    // 默认构造函数（无参）
    public Student() {
        name = "未知";
        age = 0;
        System.out.println("默认构造函数被调用");
    }
    
    // 带参数的构造函数
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
        System.out.println("带参数的构造函数被调用: " + name);
    }
    
    // 构造函数重载
    public Student(String name) {
        this(name, 0);  // 调用另一个构造函数
    }
    
    // 拷贝构造函数（Java中没有默认的拷贝构造函数）
    public Student(Student other) {
        this.name = other.name;
        this.age = other.age;
        System.out.println("拷贝构造函数被调用");
    }
    
    public void display() {
        System.out.println("姓名: " + name + ", 年龄: " + age);
    }
    
    public static void main(String[] args) {
        Student s1 = new Student();              // 默认构造函数
        Student s2 = new Student("李四", 20);    // 带参数构造函数
        Student s3 = new Student("王五");        // 重载构造函数
        Student s4 = new Student(s2);            // 拷贝构造函数
        
        s2.display();
        s4.display();
    }
}
```

## 3. 访问控制

### public、private、protected

```java
package com.example;

public class BankAccount {
    // private：只能在类内部访问
    private double balance;
    private String accountNumber;
    
    // public：可以在任何地方访问
    public String ownerName;
    
    // protected：可以在同包或子类中访问
    protected String accountType;
    
    // 公共接口
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }
    
    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
        }
    }
    
    public double getBalance() {
        return balance;
    }
    
    // private方法：只能在类内部使用
    private boolean validateAccount() {
        return accountNumber != null && !accountNumber.isEmpty();
    }
}

// 测试类
class TestAccount {
    public static void main(String[] args) {
        BankAccount account = new BankAccount();
        account.ownerName = "张三";
        // account.balance = 1000;  // 错误：不能直接访问私有成员
        // account.accountNumber = "12345";  // 错误：不能直接访问私有成员
        
        account.deposit(1000);
        account.withdraw(200);
        System.out.println("余额: " + account.getBalance());
    }
}
```

## 4. this关键字

```java
public class Rectangle {
    private int width;
    private int height;
    
    public Rectangle(int width, int height) {
        // 使用this区分成员变量和参数
        this.width = width;
        this.height = height;
    }
    
    // this返回当前对象，支持链式调用
    public Rectangle setWidth(int width) {
        this.width = width;
        return this;
    }
    
    public Rectangle setHeight(int height) {
        this.height = height;
        return this;
    }
    
    public int area() {
        return width * height;
    }
    
    public void display() {
        System.out.println("宽度: " + width + ", 高度: " + height 
                         + ", 面积: " + area());
    }
    
    public static void main(String[] args) {
        Rectangle rect = new Rectangle(10, 20);
        rect.display();
        
        // 链式调用
        rect.setWidth(15).setHeight(25);
        rect.display();
    }
}
```

## 5. static关键字

### 静态成员变量

```java
public class Counter {
    private static int count = 0;  // 静态成员变量（类级别的变量）
    private int id;
    
    public Counter() {
        id = ++count;  // 每创建一个对象，count自增
    }
    
    public static int getCount() {  // 静态方法
        return count;
    }
    
    public int getId() {
        return id;
    }
    
    public static void main(String[] args) {
        System.out.println("初始计数: " + Counter.getCount());
        
        Counter c1 = new Counter();
        Counter c2 = new Counter();
        Counter c3 = new Counter();
        
        System.out.println("对象数量: " + Counter.getCount());
        System.out.println("c1的ID: " + c1.getId());
        System.out.println("c2的ID: " + c2.getId());
        System.out.println("c3的ID: " + c3.getId());
    }
}
```

### 静态方法

```java
public class Math {
    // 静态方法：不依赖对象，可以直接通过类名调用
    public static double add(double a, double b) {
        return a + b;
    }
    
    public static double multiply(double a, double b) {
        return a * b;
    }
    
    // 静态常量
    public static final double PI = 3.141592653589793;
    
    // 静态代码块
    static {
        System.out.println("Math类被加载");
    }
    
    public static void main(String[] args) {
        // 不需要创建对象就可以调用
        System.out.println(Math.add(10, 20));       // 30.0
        System.out.println(Math.multiply(3, 4));    // 12.0
        System.out.println("PI: " + Math.PI);
    }
}
```

### 静态代码块

```java
public class StaticBlock {
    private static int value;
    
    // 静态代码块：在类加载时执行（只执行一次）
    static {
        value = 100;
        System.out.println("静态代码块执行");
    }
    
    // 实例代码块：在每次创建对象时执行
    {
        System.out.println("实例代码块执行");
    }
    
    public StaticBlock() {
        System.out.println("构造函数执行");
    }
    
    public static void main(String[] args) {
        System.out.println("main方法开始");
        StaticBlock obj1 = new StaticBlock();
        StaticBlock obj2 = new StaticBlock();
    }
}
```

## 6. 方法重载

```java
public class Calculator {
    // 方法重载：同名方法，不同参数
    public int add(int a, int b) {
        return a + b;
    }
    
    public double add(double a, double b) {
        return a + b;
    }
    
    public int add(int a, int b, int c) {
        return a + b + c;
    }
    
    public String add(String a, String b) {
        return a + b;  // 字符串连接
    }
    
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        
        System.out.println(calc.add(10, 20));              // 调用int版本
        System.out.println(calc.add(3.14, 2.71));          // 调用double版本
        System.out.println(calc.add(1, 2, 3));             // 调用三个参数版本
        System.out.println(calc.add("Hello", " World"));   // 调用String版本
    }
}
```

## 7. 封装

```java
public class Student {
    // 私有成员变量
    private String name;
    private int age;
    private double score;
    
    // Getter方法
    public String getName() {
        return name;
    }
    
    public int getAge() {
        return age;
    }
    
    public double getScore() {
        return score;
    }
    
    // Setter方法（可以添加验证逻辑）
    public void setName(String name) {
        if (name != null && !name.isEmpty()) {
            this.name = name;
        }
    }
    
    public void setAge(int age) {
        if (age > 0 && age < 150) {
            this.age = age;
        }
    }
    
    public void setScore(double score) {
        if (score >= 0 && score <= 100) {
            this.score = score;
        }
    }
    
    // 其他方法
    public void display() {
        System.out.println("姓名: " + name + ", 年龄: " + age 
                         + ", 分数: " + score);
    }
    
    public String getGrade() {
        if (score >= 90) return "优秀";
        else if (score >= 80) return "良好";
        else if (score >= 60) return "及格";
        else return "不及格";
    }
    
    public static void main(String[] args) {
        Student student = new Student();
        student.setName("张三");
        student.setAge(20);
        student.setScore(95.5);
        
        student.display();
        System.out.println("等级: " + student.getGrade());
        
        // 不能直接访问私有成员
        // student.name = "李四";  // 错误
        // System.out.println(student.age);  // 错误
    }
}
```

## 8. 综合示例

### 完整的类设计

```java
public class Book {
    // 私有成员变量
    private String title;
    private String author;
    private double price;
    private static int totalBooks = 0;  // 静态变量
    
    // 构造函数
    public Book(String title, String author, double price) {
        this.title = title;
        this.author = author;
        this.price = price;
        totalBooks++;
    }
    
    // Getter方法
    public String getTitle() {
        return title;
    }
    
    public String getAuthor() {
        return author;
    }
    
    public double getPrice() {
        return price;
    }
    
    // Setter方法
    public void setPrice(double price) {
        if (price > 0) {
            this.price = price;
        }
    }
    
    // 显示信息
    public void display() {
        System.out.println("书名: " + title + ", 作者: " + author 
                         + ", 价格: " + price);
    }
    
    // 静态方法
    public static int getTotalBooks() {
        return totalBooks;
    }
    
    // 重写toString方法
    @Override
    public String toString() {
        return "Book{title='" + title + "', author='" + author 
               + "', price=" + price + "}";
    }
    
    // 重写equals方法
    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Book book = (Book) obj;
        return Double.compare(book.price, price) == 0
                && title.equals(book.title)
                && author.equals(book.author);
    }
    
    public static void main(String[] args) {
        System.out.println("初始书籍数: " + Book.getTotalBooks());
        
        Book book1 = new Book("C++入门", "张三", 59.9);
        Book book2 = new Book("算法导论", "李四", 89.9);
        
        book1.display();
        book2.display();
        
        System.out.println("当前书籍数: " + Book.getTotalBooks());
        
        // 使用toString
        System.out.println(book1);
        
        // 使用equals
        Book book3 = new Book("C++入门", "张三", 59.9);
        System.out.println("book1 == book3: " + book1.equals(book3));
    }
}
```

## 练习

1. 设计一个Person类，包含姓名、年龄等属性，并实现相关的成员方法
2. 创建一个Circle类，包含半径属性，实现计算面积和周长的功能
3. 实现一个Date类，包含年月日，实现日期比较等功能
4. 创建一个BankAccount类，实现存款、取款、查询余额等功能
5. 设计一个Library类，管理Book对象集合

