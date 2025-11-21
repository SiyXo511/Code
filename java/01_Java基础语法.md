# Java基础语法

本教程将学习Java的基础语法，包括第一个Java程序、变量、数据类型、输入输出等。

## 1. 第一个Java程序

```java
// HelloWorld.java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}

// 编译: javac HelloWorld.java
// 运行: java HelloWorld
```

### 程序解析

- `public class HelloWorld`: 公共类，类名必须与文件名相同
- `public static void main(String[] args)`: 程序入口点
- `System.out.println()`: 输出语句
- Java区分大小写
- 每个语句以分号结尾

## 2. 变量和数据类型

### 基本数据类型

```java
public class DataTypes {
    public static void main(String[] args) {
        // 整数类型
        byte b = 127;           // 8位，-128到127
        short s = 32767;        // 16位
        int i = 2147483647;     // 32位
        long l = 9223372036854775807L;  // 64位，注意L后缀
        
        // 浮点类型
        float f = 3.14f;        // 32位，注意f后缀
        double d = 3.141592653589793;  // 64位
        
        // 字符类型
        char c = 'A';           // 16位Unicode字符
        char chinese = '中';    // 支持中文
        
        // 布尔类型
        boolean flag = true;    // true或false
        
        // 显示大小
        System.out.println("byte: " + Byte.BYTES + " 字节");
        System.out.println("int: " + Integer.BYTES + " 字节");
        System.out.println("double: " + Double.BYTES + " 字节");
    }
}
```

### 引用数据类型

```java
public class ReferenceTypes {
    public static void main(String[] args) {
        // String类
        String str1 = "Hello";
        String str2 = new String("World");
        
        // 数组
        int[] arr1 = new int[5];
        int[] arr2 = {1, 2, 3, 4, 5};
        
        // 对象
        Object obj = new Object();
        
        System.out.println(str1 + " " + str2);
    }
}
```

## 3. 常量

### final关键字

```java
public class Constants {
    public static void main(String[] args) {
        // final常量
        final int MAX_SIZE = 100;
        final double PI = 3.14159;
        
        // MAX_SIZE = 200;  // 错误：常量不能修改
        
        System.out.println("最大值: " + MAX_SIZE);
        System.out.println("PI: " + PI);
        
        // 类常量（static final）
        System.out.println("类常量: " + MyConstants.VERSION);
    }
}

class MyConstants {
    public static final String VERSION = "1.0.0";
}
```

## 4. 输入输出

### 输出

```java
import java.util.Scanner;

public class InputOutput {
    public static void main(String[] args) {
        // 输出
        System.out.print("Hello ");      // 不换行
        System.out.println("World!");     // 换行
        System.out.printf("PI = %.2f%n", 3.14159);  // 格式化输出
        
        // 输入
        Scanner scanner = new Scanner(System.in);
        
        System.out.print("请输入你的姓名: ");
        String name = scanner.nextLine();  // 读取整行
        
        System.out.print("请输入你的年龄: ");
        int age = scanner.nextInt();       // 读取整数
        
        System.out.print("请输入你的分数: ");
        double score = scanner.nextDouble();  // 读取浮点数
        
        // 输出结果
        System.out.println("你好, " + name + "!");
        System.out.println("你今年 " + age + " 岁");
        System.out.printf("你的分数是: %.2f%n", score);
        
        scanner.close();
    }
}
```

### 格式化输出

```java
public class FormatOutput {
    public static void main(String[] args) {
        String name = "张三";
        int age = 25;
        double score = 95.5;
        
        // printf格式化
        System.out.printf("姓名: %s, 年龄: %d, 分数: %.2f%n", 
                         name, age, score);
        
        // String.format
        String info = String.format("姓名: %s, 年龄: %d, 分数: %.2f", 
                                   name, age, score);
        System.out.println(info);
        
        // 格式化说明符
        System.out.printf("%d%n", 100);      // 十进制
        System.out.printf("%x%n", 100);      // 十六进制
        System.out.printf("%o%n", 100);      // 八进制
        System.out.printf("%10s%n", name);   // 右对齐，宽度10
        System.out.printf("%-10s%n", name);  // 左对齐，宽度10
    }
}
```

## 5. 运算符

### 算术运算符

```java
public class Operators {
    public static void main(String[] args) {
        int a = 10, b = 3;
        
        System.out.println("a + b = " + (a + b));  // 13
        System.out.println("a - b = " + (a - b));  // 7
        System.out.println("a * b = " + (a * b));  // 30
        System.out.println("a / b = " + (a / b));  // 3 (整数除法)
        System.out.println("a % b = " + (a % b));  // 1 (取模)
        
        // 浮点除法
        double x = 10.0, y = 3.0;
        System.out.println("x / y = " + (x / y));  // 3.333...
        
        // 自增自减
        int i = 10;
        System.out.println(i++);  // 10 (先使用后增加)
        System.out.println(i);    // 11
        System.out.println(++i);  // 12 (先增加后使用)
        System.out.println(i);    // 12
    }
}
```

### 关系运算符

```java
public class RelationalOperators {
    public static void main(String[] args) {
        int a = 10, b = 20;
        
        System.out.println(a == b);  // false
        System.out.println(a != b);  // true
        System.out.println(a < b);   // true
        System.out.println(a > b);   // false
        System.out.println(a <= b);  // true
        System.out.println(a >= b);  // false
    }
}
```

### 逻辑运算符

```java
public class LogicalOperators {
    public static void main(String[] args) {
        boolean p = true, q = false;
        
        System.out.println(p && q);  // false (逻辑与)
        System.out.println(p || q);  // true (逻辑或)
        System.out.println(!p);      // false (逻辑非)
        
        // 短路求值
        boolean result = (p && (q || true));  // true
        System.out.println(result);
    }
}
```

### 赋值运算符

```java
public class AssignmentOperators {
    public static void main(String[] args) {
        int x = 10;
        
        x += 5;   // x = x + 5
        System.out.println(x);  // 15
        
        x -= 3;   // x = x - 3
        System.out.println(x);  // 12
        
        x *= 2;   // x = x * 2
        System.out.println(x);  // 24
        
        x /= 4;   // x = x / 4
        System.out.println(x);  // 6
        
        x %= 4;   // x = x % 4
        System.out.println(x);  // 2
    }
}
```

### 位运算符

```java
public class BitwiseOperators {
    public static void main(String[] args) {
        int a = 5;  // 0101
        int b = 3;  // 0011
        
        System.out.println(a & b);  // 1 (按位与: 0001)
        System.out.println(a | b);  // 7 (按位或: 0111)
        System.out.println(a ^ b);  // 6 (按位异或: 0110)
        System.out.println(~a);     // -6 (按位取反)
        System.out.println(a << 1); // 10 (左移: 1010)
        System.out.println(a >> 1); // 2 (右移: 0010)
        System.out.println(a >>> 1); // 2 (无符号右移)
    }
}
```

## 6. 类型转换

### 自动类型转换

```java
public class TypeConversion {
    public static void main(String[] args) {
        // 自动类型转换（小类型到大类型）
        byte b = 10;
        int i = b;  // byte自动转换为int
        
        int x = 100;
        long y = x;  // int自动转换为long
        
        float f = 3.14f;
        double d = f;  // float自动转换为double
        
        // char到int
        char c = 'A';
        int code = c;  // 65 (ASCII码)
        System.out.println(code);
    }
}
```

### 强制类型转换

```java
public class ExplicitConversion {
    public static void main(String[] args) {
        // 强制类型转换（大类型到小类型）
        int x = 100;
        byte b = (byte)x;  // 强制转换
        
        double d = 3.14;
        int i = (int)d;  // 截断，i = 3
        
        long l = 1000000L;
        int n = (int)l;
        
        // 字符串转数字
        String str = "123";
        int num = Integer.parseInt(str);
        double num2 = Double.parseDouble("3.14");
        
        // 数字转字符串
        int number = 100;
        String s1 = String.valueOf(number);
        String s2 = Integer.toString(number);
        String s3 = number + "";  // 字符串拼接
        
        System.out.println(s1 + " " + s2 + " " + s3);
    }
}
```

## 7. 字符串

### String类

```java
public class StringDemo {
    public static void main(String[] args) {
        // 字符串创建
        String str1 = "Hello";
        String str2 = new String("World");
        String str3 = new String(str1);  // 复制
        
        // 字符串连接
        String result = str1 + " " + str2;
        System.out.println(result);  // "Hello World"
        
        // 字符串长度
        System.out.println("长度: " + result.length());
        
        // 访问字符
        System.out.println(result.charAt(0));  // 'H'
        
        // 子字符串
        String sub = result.substring(0, 5);
        System.out.println(sub);  // "Hello"
        
        // 查找
        int index = result.indexOf("World");
        System.out.println("位置: " + index);
        
        // 替换
        String replaced = result.replace("World", "Java");
        System.out.println(replaced);
        
        // 分割
        String[] parts = "apple,banana,orange".split(",");
        for (String part : parts) {
            System.out.println(part);
        }
        
        // 比较
        String s1 = "Hello";
        String s2 = "Hello";
        String s3 = new String("Hello");
        
        System.out.println(s1.equals(s2));      // true
        System.out.println(s1 == s2);           // true (字符串常量池)
        System.out.println(s1.equals(s3));      // true
        System.out.println(s1 == s3);           // false (不同对象)
        
        // 忽略大小写比较
        System.out.println("Hello".equalsIgnoreCase("hello"));  // true
    }
}
```

## 8. 注释

```java
// 这是单行注释

/* 
 * 这是多行注释
 * 可以写多行
 */

/**
 * 这是文档注释（Javadoc）
 * 用于生成API文档
 * 
 * @author 作者名
 * @version 1.0
 * @since 2024
 */
public class Comments {
    /**
     * 主方法
     * @param args 命令行参数
     */
    public static void main(String[] args) {
        // 代码注释
        System.out.println("Hello, Java!");
    }
}
```

## 9. 包和导入

```java
// 声明包（必须在文件第一行，除了注释）
package com.example.learnjava;

// 导入类
import java.util.Scanner;
import java.util.Date;
import java.util.*;  // 导入整个包

public class PackageDemo {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Date date = new Date();
        System.out.println("当前时间: " + date);
        scanner.close();
    }
}
```

## 10. 综合示例

```java
import java.util.Scanner;

public class Calculator {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.print("请输入第一个数字: ");
        double num1 = scanner.nextDouble();
        
        System.out.print("请输入运算符 (+, -, *, /): ");
        char operator = scanner.next().charAt(0);
        
        System.out.print("请输入第二个数字: ");
        double num2 = scanner.nextDouble();
        
        double result = 0;
        
        switch (operator) {
            case '+':
                result = num1 + num2;
                break;
            case '-':
                result = num1 - num2;
                break;
            case '*':
                result = num1 * num2;
                break;
            case '/':
                if (num2 != 0) {
                    result = num1 / num2;
                } else {
                    System.out.println("除数不能为零！");
                    return;
                }
                break;
            default:
                System.out.println("无效的运算符！");
                return;
        }
        
        System.out.printf("%.2f %c %.2f = %.2f%n", 
                         num1, operator, num2, result);
        
        scanner.close();
    }
}
```

## 练习

1. 编写一个程序，从用户输入读取姓名、年龄和身高，并输出格式化信息
2. 编写一个程序，计算圆的面积和周长
3. 编写一个程序，交换两个变量的值
4. 编写一个程序，判断一个数是否为偶数
5. 编写一个程序，实现简单的字符串操作（反转、大小写转换等）

