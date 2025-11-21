# Java继承和多态

本教程将学习Java中的继承和多态，包括继承基础、方法重写、super关键字、抽象类、接口等。

## 1. 继承基础

### 单继承

```java
// 基类
class Animal {
    protected String name;
    protected int age;
    
    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public void eat() {
        System.out.println(name + " 正在吃东西");
    }
    
    public void sleep() {
        System.out.println(name + " 正在睡觉");
    }
    
    public void display() {
        System.out.println("名称: " + name + ", 年龄: " + age);
    }
}

// 派生类
class Dog extends Animal {
    private String breed;
    
    // 派生类构造函数
    public Dog(String name, int age, String breed) {
        super(name, age);  // 调用基类构造函数
        this.breed = breed;
    }
    
    // 新增功能
    public void bark() {
        System.out.println(name + " 在叫: 汪汪！");
    }
    
    // 重写基类方法
    @Override
    public void display() {
        super.display();  // 调用基类方法
        System.out.println("品种: " + breed);
    }
}

public class InheritanceDemo {
    public static void main(String[] args) {
        Dog dog = new Dog("旺财", 3, "金毛");
        dog.eat();      // 继承自基类
        dog.sleep();    // 继承自基类
        dog.bark();     // 派生类新方法
        dog.display();  // 重写的display方法
    }
}
```

### super关键字

```java
class Parent {
    protected int value;
    
    public Parent(int value) {
        this.value = value;
    }
    
    public void show() {
        System.out.println("Parent value: " + value);
    }
}

class Child extends Parent {
    private int value;
    
    public Child(int parentValue, int childValue) {
        super(parentValue);  // 调用父类构造函数
        this.value = childValue;
    }
    
    @Override
    public void show() {
        super.show();  // 调用父类方法
        System.out.println("Child value: " + value);
        System.out.println("Parent value: " + super.value);  // 访问父类成员
    }
}

public class SuperDemo {
    public static void main(String[] args) {
        Child child = new Child(10, 20);
        child.show();
    }
}
```

## 2. 方法重写

```java
class Shape {
    protected String name;
    
    public Shape(String name) {
        this.name = name;
    }
    
    // 可以被子类重写的方法
    public void draw() {
        System.out.println("绘制 " + name);
    }
    
    public double area() {
        return 0;
    }
}

class Circle extends Shape {
    private double radius;
    
    public Circle(String name, double radius) {
        super(name);
        this.radius = radius;
    }
    
    // 重写draw方法
    @Override
    public void draw() {
        System.out.println("绘制圆形 " + name + "，半径: " + radius);
    }
    
    // 重写area方法
    @Override
    public double area() {
        return Math.PI * radius * radius;
    }
}

class Rectangle extends Shape {
    private double width;
    private double height;
    
    public Rectangle(String name, double width, double height) {
        super(name);
        this.width = width;
        this.height = height;
    }
    
    @Override
    public void draw() {
        System.out.println("绘制矩形 " + name 
                         + "，宽度: " + width + "，高度: " + height);
    }
    
    @Override
    public double area() {
        return width * height;
    }
}

public class OverrideDemo {
    public static void main(String[] args) {
        Circle circle = new Circle("圆形1", 5.0);
        Rectangle rect = new Rectangle("矩形1", 10.0, 8.0);
        
        circle.draw();
        System.out.println("面积: " + circle.area());
        
        rect.draw();
        System.out.println("面积: " + rect.area());
    }
}
```

## 3. 多态

```java
public class PolymorphismDemo {
    public static void main(String[] args) {
        // 多态：父类引用指向子类对象
        Shape shape1 = new Circle("圆形1", 5.0);
        Shape shape2 = new Rectangle("矩形1", 10.0, 8.0);
        
        // 调用子类重写的方法
        shape1.draw();  // 绘制圆形
        shape2.draw();  // 绘制矩形
        
        // 多态数组
        Shape[] shapes = {
            new Circle("圆形2", 3.0),
            new Rectangle("矩形2", 5.0, 4.0),
            new Circle("圆形3", 2.0)
        };
        
        // 多态调用
        for (Shape shape : shapes) {
            shape.draw();
            System.out.println("面积: " + shape.area());
            System.out.println();
        }
    }
}
```

## 4. 抽象类和抽象方法

```java
// 抽象类：包含抽象方法
abstract class Shape {
    protected String name;
    
    public Shape(String name) {
        this.name = name;
    }
    
    // 抽象方法：子类必须实现
    public abstract double area();
    public abstract double perimeter();
    
    // 可以有非抽象方法
    public void display() {
        System.out.println("形状: " + name);
    }
}

class Circle extends Shape {
    private double radius;
    
    public Circle(String name, double radius) {
        super(name);
        this.radius = radius;
    }
    
    // 必须实现抽象方法
    @Override
    public double area() {
        return Math.PI * radius * radius;
    }
    
    @Override
    public double perimeter() {
        return 2 * Math.PI * radius;
    }
    
    @Override
    public void display() {
        super.display();
        System.out.println("半径: " + radius);
        System.out.println("面积: " + area());
        System.out.println("周长: " + perimeter());
    }
}

class Rectangle extends Shape {
    private double width;
    private double height;
    
    public Rectangle(String name, double width, double height) {
        super(name);
        this.width = width;
        this.height = height;
    }
    
    @Override
    public double area() {
        return width * height;
    }
    
    @Override
    public double perimeter() {
        return 2 * (width + height);
    }
    
    @Override
    public void display() {
        super.display();
        System.out.println("宽度: " + width + "，高度: " + height);
        System.out.println("面积: " + area());
        System.out.println("周长: " + perimeter());
    }
}

public class AbstractClassDemo {
    public static void main(String[] args) {
        // Shape shape = new Shape("抽象形状");  // 错误：不能实例化抽象类
        
        Circle circle = new Circle("圆形", 5.0);
        Rectangle rect = new Rectangle("矩形", 10.0, 8.0);
        
        circle.display();
        System.out.println();
        rect.display();
        
        // 使用父类引用实现多态
        Shape[] shapes = {circle, rect};
        for (Shape shape : shapes) {
            shape.display();
            System.out.println();
        }
    }
}
```

## 5. final关键字

```java
// final类：不能被继承
final class FinalClass {
    public void show() {
        System.out.println("这是final类");
    }
}

// class Child extends FinalClass {}  // 错误：不能继承final类

// final方法：不能被重写
class Parent {
    public final void show() {
        System.out.println("这是final方法");
    }
}

class Child extends Parent {
    // public void show() {}  // 错误：不能重写final方法
}

// final变量：常量
class Constants {
    public static final double PI = 3.14159;
    public final int value = 100;
    
    public void test() {
        // value = 200;  // 错误：final变量不能修改
    }
}

public class FinalDemo {
    public static void main(String[] args) {
        System.out.println("PI: " + Constants.PI);
        Constants constants = new Constants();
        System.out.println("value: " + constants.value);
    }
}
```

## 6. 接口（interface）

### 接口基础

```java
// 接口定义
interface Drawable {
    // 接口中的方法默认是public abstract
    void draw();
    
    // Java 8+: 默认方法
    default void display() {
        System.out.println("显示图形");
    }
    
    // Java 8+: 静态方法
    static void showInfo() {
        System.out.println("这是一个可绘制的接口");
    }
}

interface Colorable {
    void setColor(String color);
    String getColor();
}

// 类实现接口
class Circle implements Drawable, Colorable {
    private String color;
    
    @Override
    public void draw() {
        System.out.println("绘制圆形");
    }
    
    @Override
    public void setColor(String color) {
        this.color = color;
    }
    
    @Override
    public String getColor() {
        return color;
    }
}

public class InterfaceDemo {
    public static void main(String[] args) {
        Circle circle = new Circle();
        circle.draw();
        circle.setColor("红色");
        System.out.println("颜色: " + circle.getColor());
        circle.display();  // 调用默认方法
        Drawable.showInfo();  // 调用静态方法
    }
}
```

### 接口继承

```java
interface Shape {
    double area();
}

interface Drawable {
    void draw();
}

// 接口可以继承多个接口
interface DrawableShape extends Shape, Drawable {
    void display();
}

class Circle implements DrawableShape {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    @Override
    public double area() {
        return Math.PI * radius * radius;
    }
    
    @Override
    public void draw() {
        System.out.println("绘制圆形");
    }
    
    @Override
    public void display() {
        System.out.println("圆形，面积: " + area());
    }
}

public class InterfaceInheritance {
    public static void main(String[] args) {
        DrawableShape shape = new Circle(5.0);
        shape.draw();
        shape.display();
    }
}
```

### 函数式接口（Java 8+）

```java
// 函数式接口：只有一个抽象方法
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
    
    // 可以有默认方法和静态方法
    default void printResult(int result) {
        System.out.println("结果: " + result);
    }
}

public class FunctionalInterfaceDemo {
    public static void main(String[] args) {
        // 使用Lambda表达式实现接口
        Calculator add = (a, b) -> a + b;
        Calculator multiply = (a, b) -> a * b;
        
        System.out.println(add.calculate(10, 20));      // 30
        System.out.println(multiply.calculate(5, 6));    // 30
        
        add.printResult(add.calculate(10, 20));
    }
}
```

## 7. 接口vs抽象类

```java
// 抽象类：可以有成员变量、构造函数、具体方法
abstract class Animal {
    protected String name;
    
    public Animal(String name) {
        this.name = name;
    }
    
    public abstract void makeSound();
    
    public void eat() {
        System.out.println(name + " 正在吃东西");
    }
}

// 接口：只能有常量、抽象方法（Java 8+可以有默认方法和静态方法）
interface Flyable {
    void fly();
}

interface Swimmable {
    void swim();
}

// 类可以继承一个类，但可以实现多个接口
class Duck extends Animal implements Flyable, Swimmable {
    public Duck(String name) {
        super(name);
    }
    
    @Override
    public void makeSound() {
        System.out.println(name + " 嘎嘎叫");
    }
    
    @Override
    public void fly() {
        System.out.println(name + " 在飞");
    }
    
    @Override
    public void swim() {
        System.out.println(name + " 在游");
    }
}

public class InterfaceVsAbstract {
    public static void main(String[] args) {
        Duck duck = new Duck("鸭子");
        duck.makeSound();
        duck.eat();
        duck.fly();
        duck.swim();
    }
}
```

## 8. 综合示例

```java
// 动物类层次结构
abstract class Animal {
    protected String name;
    protected int age;
    
    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public abstract void makeSound();
    
    public void display() {
        System.out.println("名称: " + name + ", 年龄: " + age);
    }
}

interface Flyable {
    void fly();
}

interface Swimmable {
    void swim();
}

class Dog extends Animal {
    public Dog(String name, int age) {
        super(name, age);
    }
    
    @Override
    public void makeSound() {
        System.out.println(name + " 汪汪叫");
    }
}

class Bird extends Animal implements Flyable {
    public Bird(String name, int age) {
        super(name, age);
    }
    
    @Override
    public void makeSound() {
        System.out.println(name + " 啾啾叫");
    }
    
    @Override
    public void fly() {
        System.out.println(name + " 在飞");
    }
}

class Duck extends Animal implements Flyable, Swimmable {
    public Duck(String name, int age) {
        super(name, age);
    }
    
    @Override
    public void makeSound() {
        System.out.println(name + " 嘎嘎叫");
    }
    
    @Override
    public void fly() {
        System.out.println(name + " 在飞");
    }
    
    @Override
    public void swim() {
        System.out.println(name + " 在游");
    }
}

public class AnimalDemo {
    public static void main(String[] args) {
        Animal[] animals = {
            new Dog("旺财", 3),
            new Bird("小鸟", 1),
            new Duck("鸭子", 2)
        };
        
        for (Animal animal : animals) {
            animal.display();
            animal.makeSound();
            
            if (animal instanceof Flyable) {
                ((Flyable) animal).fly();
            }
            
            if (animal instanceof Swimmable) {
                ((Swimmable) animal).swim();
            }
            
            System.out.println();
        }
    }
}
```

## 练习

1. 创建一个Vehicle基类，派生出Car和Bicycle类
2. 实现一个Employee基类，派生出Manager和Developer类
3. 使用多态实现一个图形绘制系统（Circle、Rectangle、Triangle）
4. 实现一个动物类的继承体系，使用接口实现不同能力
5. 创建一个Shape抽象类，实现多个具体的形状类

