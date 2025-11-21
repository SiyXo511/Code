# Java控制流

本教程将学习Java中的控制流语句，包括条件语句、循环语句等。

## 1. 条件语句

### if语句

```java
import java.util.Scanner;

public class IfStatement {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.print("请输入分数: ");
        int score = scanner.nextInt();
        
        if (score >= 90) {
            System.out.println("优秀");
        } else if (score >= 80) {
            System.out.println("良好");
        } else if (score >= 60) {
            System.out.println("及格");
        } else {
            System.out.println("不及格");
        }
        
        // 三元运算符
        int a = 10, b = 20;
        int max = (a > b) ? a : b;
        System.out.println("最大值: " + max);
        
        scanner.close();
    }
}
```

### switch语句

```java
import java.util.Scanner;

public class SwitchStatement {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.print("请选择操作 (1-4): ");
        int choice = scanner.nextInt();
        
        switch (choice) {
            case 1:
                System.out.println("你选择了选项1");
                break;
            case 2:
                System.out.println("你选择了选项2");
                break;
            case 3:
                System.out.println("你选择了选项3");
                break;
            case 4:
                System.out.println("你选择了选项4");
                break;
            default:
                System.out.println("无效的选择");
                break;
        }
        
        // Java 14+: switch表达式
        String result = switch (choice) {
            case 1, 2, 3 -> "选项1-3";
            case 4 -> "选项4";
            default -> "其他选项";
        };
        System.out.println(result);
        
        scanner.close();
    }
}
```

### switch表达式（Java 14+）

```java
public class SwitchExpression {
    public static void main(String[] args) {
        int day = 3;
        
        // 传统switch语句
        String dayName1;
        switch (day) {
            case 1: dayName1 = "星期一"; break;
            case 2: dayName1 = "星期二"; break;
            case 3: dayName1 = "星期三"; break;
            default: dayName1 = "其他";
        }
        
        // switch表达式（Java 14+）
        String dayName2 = switch (day) {
            case 1 -> "星期一";
            case 2 -> "星期二";
            case 3 -> "星期三";
            default -> "其他";
        };
        
        // 使用yield（Java 14+）
        String dayName3 = switch (day) {
            case 1: yield "星期一";
            case 2: yield "星期二";
            case 3: yield "星期三";
            default: yield "其他";
        };
        
        System.out.println(dayName2);
    }
}
```

## 2. 循环语句

### for循环

```java
public class ForLoop {
    public static void main(String[] args) {
        // 传统for循环
        for (int i = 0; i < 10; i++) {
            System.out.print(i + " ");
        }
        System.out.println();
        
        // 增强for循环（for-each）
        int[] arr = {1, 2, 3, 4, 5};
        for (int value : arr) {
            System.out.print(value + " ");
        }
        System.out.println();
        
        // 字符串遍历
        String str = "Hello";
        for (char c : str.toCharArray()) {
            System.out.print(c + " ");
        }
        System.out.println();
        
        // 嵌套循环
        for (int i = 1; i <= 9; i++) {
            for (int j = 1; j <= i; j++) {
                System.out.print(j + "x" + i + "=" + (i * j) + "\t");
            }
            System.out.println();
        }
    }
}
```

### while循环

```java
import java.util.Scanner;

public class WhileLoop {
    public static void main(String[] args) {
        int count = 0;
        
        while (count < 5) {
            System.out.print(count + " ");
            count++;
        }
        System.out.println();
        
        // 读取直到满足条件
        Scanner scanner = new Scanner(System.in);
        int num;
        System.out.print("请输入一个正数: ");
        while (!scanner.hasNextInt() || (num = scanner.nextInt()) <= 0) {
            System.out.print("请输入一个正数: ");
            if (scanner.hasNextInt()) {
                scanner.nextLine();  // 清除输入
            }
        }
        System.out.println("你输入的是: " + num);
        
        scanner.close();
    }
}
```

### do-while循环

```java
import java.util.Scanner;

public class DoWhileLoop {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int choice;
        
        do {
            System.out.println("请选择操作 (1-3): ");
            choice = scanner.nextInt();
            
            if (choice >= 1 && choice <= 3) {
                System.out.println("你选择了选项 " + choice);
            } else {
                System.out.println("无效选择，请重试");
            }
        } while (choice < 1 || choice > 3);
        
        scanner.close();
    }
}
```

## 3. 跳转语句

### break语句

```java
public class BreakStatement {
    public static void main(String[] args) {
        // 在循环中使用break
        for (int i = 0; i < 10; i++) {
            if (i == 5) {
                break;  // 跳出循环
            }
            System.out.print(i + " ");
        }
        System.out.println();
        
        // 带标签的break
        outer: for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (i == 1 && j == 1) {
                    break outer;  // 跳出外层循环
                }
                System.out.println("i=" + i + ", j=" + j);
            }
        }
    }
}
```

### continue语句

```java
public class ContinueStatement {
    public static void main(String[] args) {
        // 跳过当前迭代
        for (int i = 0; i < 10; i++) {
            if (i % 2 == 0) {
                continue;  // 跳过偶数
            }
            System.out.print(i + " ");  // 只输出奇数
        }
        System.out.println();
        
        // 带标签的continue
        outer: for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (i == 1 && j == 1) {
                    continue outer;  // 继续外层循环的下一次迭代
                }
                System.out.println("i=" + i + ", j=" + j);
            }
        }
    }
}
```

## 4. 综合示例

### 猜数字游戏

```java
import java.util.Scanner;
import java.util.Random;

public class GuessNumber {
    public static void main(String[] args) {
        Random random = new Random();
        Scanner scanner = new Scanner(System.in);
        
        int secret = random.nextInt(100) + 1;  // 1-100的随机数
        int guess;
        int attempts = 0;
        boolean guessed = false;
        
        System.out.println("欢迎来到猜数字游戏！");
        System.out.println("我想了一个1-100之间的数字，猜猜看？");
        
        while (!guessed && attempts < 10) {
            System.out.print("请输入你的猜测: ");
            guess = scanner.nextInt();
            attempts++;
            
            if (guess < secret) {
                System.out.println("太小了！");
            } else if (guess > secret) {
                System.out.println("太大了！");
            } else {
                System.out.println("恭喜你，猜对了！");
                System.out.println("你用了 " + attempts + " 次尝试");
                guessed = true;
            }
        }
        
        if (!guessed) {
            System.out.println("很遗憾，你没有猜中。答案是: " + secret);
        }
        
        scanner.close();
    }
}
```

### 打印图案

```java
public class PatternPrinting {
    public static void main(String[] args) {
        int n = 5;
        
        // 打印三角形
        for (int i = 1; i <= n; i++) {
            // 打印空格
            for (int j = 1; j <= n - i; j++) {
                System.out.print(" ");
            }
            // 打印星号
            for (int j = 1; j <= 2 * i - 1; j++) {
                System.out.print("*");
            }
            System.out.println();
        }
        
        // 打印菱形
        System.out.println("\n菱形:");
        // 上半部分
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n - i; j++) {
                System.out.print(" ");
            }
            for (int j = 1; j <= 2 * i - 1; j++) {
                System.out.print("*");
            }
            System.out.println();
        }
        // 下半部分
        for (int i = n - 1; i >= 1; i--) {
            for (int j = 1; j <= n - i; j++) {
                System.out.print(" ");
            }
            for (int j = 1; j <= 2 * i - 1; j++) {
                System.out.print("*");
            }
            System.out.println();
        }
    }
}
```

## 练习

1. 编写一个程序，判断一个数是否为素数
2. 编写一个程序，计算斐波那契数列的前n项
3. 编写一个程序，打印九九乘法表
4. 编写一个程序，实现简单的菜单系统（使用do-while循环）
5. 编写一个程序，找出1-100之间的所有质数

