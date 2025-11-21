# Java异常处理

本教程将学习Java中的异常处理，包括try-catch块、抛出异常、自定义异常等。

## 1. 异常处理基础

### try-catch块

```java
public class ExceptionDemo {
    public static void main(String[] args) {
        try {
            int result = divide(10, 0);
            System.out.println("结果: " + result);
        } catch (ArithmeticException e) {
            System.out.println("捕获异常: " + e.getMessage());
        }
        
        try {
            int result = divide(10, 2);
            System.out.println("结果: " + result);
        } catch (ArithmeticException e) {
            System.out.println("捕获异常: " + e.getMessage());
        }
    }
    
    static int divide(int a, int b) {
        if (b == 0) {
            throw new ArithmeticException("除数不能为零！");
        }
        return a / b;
    }
}
```

### 多个catch块

```java
import java.util.InputMismatchException;
import java.util.Scanner;

public class MultipleCatch {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        try {
            System.out.print("请输入一个数字: ");
            int num = scanner.nextInt();
            
            System.out.print("请输入另一个数字: ");
            int num2 = scanner.nextInt();
            
            int result = num / num2;
            System.out.println("结果: " + result);
            
        } catch (InputMismatchException e) {
            System.out.println("输入格式错误: " + e.getMessage());
        } catch (ArithmeticException e) {
            System.out.println("算术错误: " + e.getMessage());
        } catch (Exception e) {
            System.out.println("未知错误: " + e.getMessage());
        } finally {
            scanner.close();
            System.out.println("资源已释放");
        }
    }
}
```

## 2. 异常层次结构

```java
public class ExceptionHierarchy {
    public static void main(String[] args) {
        // Throwable是所有异常和错误的基类
        // ├── Error：系统错误（不应捕获）
        // └── Exception：可处理的异常
        //     ├── RuntimeException：运行时异常
        //     └── 检查异常（Checked Exception）
        
        // RuntimeException（运行时异常，可以不处理）
        try {
            int[] arr = new int[5];
            int value = arr[10];  // ArrayIndexOutOfBoundsException
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("数组越界: " + e.getMessage());
        }
        
        // 检查异常（必须处理）
        try {
            Class.forName("NonExistentClass");  // ClassNotFoundException
        } catch (ClassNotFoundException e) {
            System.out.println("类未找到: " + e.getMessage());
        }
    }
}
```

## 3. 抛出异常

### throw关键字

```java
public class ThrowDemo {
    public static void main(String[] args) {
        try {
            validateAge(15);
        } catch (IllegalArgumentException e) {
            System.out.println("错误: " + e.getMessage());
        }
        
        try {
            validateAge(25);
            System.out.println("年龄验证通过");
        } catch (IllegalArgumentException e) {
            System.out.println("错误: " + e.getMessage());
        }
    }
    
    static void validateAge(int age) {
        if (age < 18) {
            throw new IllegalArgumentException("年龄必须大于等于18岁");
        }
    }
}
```

### throws关键字

```java
import java.io.IOException;

public class ThrowsDemo {
    // 声明可能抛出的异常
    public static void readFile(String filename) throws IOException {
        if (filename == null || filename.isEmpty()) {
            throw new IOException("文件名不能为空");
        }
        // 文件读取操作...
    }
    
    public static void main(String[] args) {
        try {
            readFile("test.txt");
        } catch (IOException e) {
            System.out.println("文件错误: " + e.getMessage());
        }
        
        try {
            readFile(null);  // 抛出异常
        } catch (IOException e) {
            System.out.println("文件错误: " + e.getMessage());
        }
    }
}
```

## 4. 自定义异常

```java
// 自定义异常类
class InsufficientBalanceException extends Exception {
    private double balance;
    private double amount;
    
    public InsufficientBalanceException(double balance, double amount) {
        super("余额不足！余额: " + balance + ", 需要: " + amount);
        this.balance = balance;
        this.amount = amount;
    }
    
    public double getBalance() {
        return balance;
    }
    
    public double getAmount() {
        return amount;
    }
}

class BankAccount {
    private double balance;
    
    public BankAccount(double balance) {
        this.balance = balance;
    }
    
    public void withdraw(double amount) throws InsufficientBalanceException {
        if (amount > balance) {
            throw new InsufficientBalanceException(balance, amount);
        }
        balance -= amount;
        System.out.println("取款成功，剩余余额: " + balance);
    }
    
    public double getBalance() {
        return balance;
    }
}

public class CustomException {
    public static void main(String[] args) {
        BankAccount account = new BankAccount(1000);
        
        try {
            account.withdraw(500);
            account.withdraw(800);  // 抛出异常
        } catch (InsufficientBalanceException e) {
            System.out.println("捕获自定义异常: " + e.getMessage());
            System.out.println("当前余额: " + e.getBalance());
            System.out.println("需要金额: " + e.getAmount());
        }
    }
}
```

## 5. 标准异常类

```java
import java.io.FileNotFoundException;
import java.io.IOException;
import java.util.InputMismatchException;

public class StandardExceptions {
    public static void main(String[] args) {
        // 1. NullPointerException
        try {
            String str = null;
            int length = str.length();  // NullPointerException
        } catch (NullPointerException e) {
            System.out.println("空指针异常: " + e.getMessage());
        }
        
        // 2. ArrayIndexOutOfBoundsException
        try {
            int[] arr = {1, 2, 3};
            int value = arr[10];
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("数组越界: " + e.getMessage());
        }
        
        // 3. IllegalArgumentException
        try {
            setAge(-10);
        } catch (IllegalArgumentException e) {
            System.out.println("非法参数: " + e.getMessage());
        }
        
        // 4. NumberFormatException
        try {
            int num = Integer.parseInt("abc");
        } catch (NumberFormatException e) {
            System.out.println("数字格式错误: " + e.getMessage());
        }
        
        // 5. ClassCastException
        try {
            Object obj = "Hello";
            Integer num = (Integer) obj;
        } catch (ClassCastException e) {
            System.out.println("类型转换错误: " + e.getMessage());
        }
    }
    
    static void setAge(int age) {
        if (age < 0) {
            throw new IllegalArgumentException("年龄不能为负数");
        }
    }
}
```

## 6. finally块

```java
import java.io.FileReader;
import java.io.IOException;

public class FinallyDemo {
    public static void main(String[] args) {
        FileReader reader = null;
        try {
            reader = new FileReader("test.txt");
            // 读取文件...
            System.out.println("文件读取成功");
        } catch (IOException e) {
            System.out.println("文件读取错误: " + e.getMessage());
        } finally {
            // finally块总是执行（用于清理资源）
            if (reader != null) {
                try {
                    reader.close();
                    System.out.println("文件已关闭");
                } catch (IOException e) {
                    System.out.println("关闭文件错误: " + e.getMessage());
                }
            }
        }
    }
}
```

### try-with-resources（Java 7+）

```java
import java.io.FileReader;
import java.io.IOException;

public class TryWithResources {
    public static void main(String[] args) {
        // try-with-resources：自动关闭资源
        try (FileReader reader = new FileReader("test.txt")) {
            // 读取文件...
            System.out.println("文件读取成功");
        } catch (IOException e) {
            System.out.println("文件读取错误: " + e.getMessage());
        }
        // 资源自动关闭，不需要finally块
    }
}
```

## 7. 异常链

```java
public class ExceptionChain {
    public static void main(String[] args) {
        try {
            method1();
        } catch (Exception e) {
            System.out.println("捕获异常: " + e.getMessage());
            System.out.println("原因: " + e.getCause().getMessage());
            e.printStackTrace();
        }
    }
    
    static void method1() throws Exception {
        try {
            method2();
        } catch (Exception e) {
            throw new Exception("方法1出错", e);  // 保留原始异常
        }
    }
    
    static void method2() throws Exception {
        throw new Exception("方法2出错");
    }
}
```

## 8. 综合示例

### 完整的异常处理示例

```java
import java.util.Scanner;

class Calculator {
    public static double divide(double a, double b) 
            throws IllegalArgumentException {
        if (b == 0) {
            throw new IllegalArgumentException("除数不能为零");
        }
        return a / b;
    }
    
    public static double sqrt(double x) 
            throws IllegalArgumentException {
        if (x < 0) {
            throw new IllegalArgumentException("不能对负数开平方");
        }
        return Math.sqrt(x);
    }
}

public class CalculatorDemo {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        try {
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
                    result = Calculator.divide(num1, num2);
                    break;
                default:
                    throw new IllegalArgumentException("无效的运算符: " + operator);
            }
            
            System.out.printf("%.2f %c %.2f = %.2f%n", 
                             num1, operator, num2, result);
            
        } catch (IllegalArgumentException e) {
            System.out.println("错误: " + e.getMessage());
        } catch (Exception e) {
            System.out.println("未知错误: " + e.getMessage());
        } finally {
            scanner.close();
            System.out.println("计算器已关闭");
        }
    }
}
```

## 练习

1. 创建一个银行账户类，处理余额不足等异常
2. 实现一个文件读取类，处理文件不存在、读取失败等异常
3. 创建一个计算器类，处理除零、无效操作等异常
4. 实现一个安全的数组类，处理越界访问等异常
5. 使用异常处理改进你的项目中的错误处理

