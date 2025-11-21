# Java常用类库

本教程将学习Java中的常用类库，包括Object类、String类、包装类、Math类等。

## 1. Object类

```java
public class ObjectDemo {
    public static void main(String[] args) {
        Object obj1 = new Object();
        Object obj2 = new Object();
        
        // toString()方法
        System.out.println(obj1.toString());  // 默认返回：类名@哈希码
        System.out.println(obj1);             // println会自动调用toString()
        
        // equals()方法
        System.out.println(obj1.equals(obj2));  // false（默认比较引用）
        
        // hashCode()方法
        System.out.println("obj1哈希码: " + obj1.hashCode());
        System.out.println("obj2哈希码: " + obj2.hashCode());
        
        // getClass()方法
        System.out.println("obj1的类: " + obj1.getClass().getName());
    }
}

// 重写Object类的方法
class Person {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    // 重写toString()
    @Override
    public String toString() {
        return "Person{name='" + name + "', age=" + age + "}";
    }
    
    // 重写equals()
    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Person person = (Person) obj;
        return age == person.age && name.equals(person.name);
    }
    
    // 重写hashCode()
    @Override
    public int hashCode() {
        return name.hashCode() * 31 + age;
    }
    
    public static void main(String[] args) {
        Person p1 = new Person("张三", 25);
        Person p2 = new Person("张三", 25);
        
        System.out.println(p1);              // 调用toString()
        System.out.println(p1.equals(p2));   // true
        System.out.println(p1.hashCode() == p2.hashCode());  // true
    }
}
```

## 2. String类

### String不可变性

```java
public class StringDemo {
    public static void main(String[] args) {
        String str1 = "Hello";
        String str2 = "Hello";
        
        // 字符串常量池：相同的字符串字面量共享同一个对象
        System.out.println(str1 == str2);      // true
        System.out.println(str1.equals(str2)); // true
        
        String str3 = new String("Hello");
        System.out.println(str1 == str3);      // false（不同对象）
        System.out.println(str1.equals(str3)); // true（内容相同）
        
        // String是不可变的
        str1 = str1 + " World";  // 创建新对象，str1指向新对象
        System.out.println(str1);  // "Hello World"
    }
}
```

### String常用方法

```java
public class StringMethods {
    public static void main(String[] args) {
        String str = "Hello World";
        
        // 长度
        System.out.println("长度: " + str.length());  // 11
        
        // 字符访问
        System.out.println("第一个字符: " + str.charAt(0));  // 'H'
        
        // 子字符串
        System.out.println("子串(0,5): " + str.substring(0, 5));  // "Hello"
        System.out.println("子串(6): " + str.substring(6));  // "World"
        
        // 查找
        System.out.println("索引of'World': " + str.indexOf("World"));  // 6
        System.out.println("包含'World': " + str.contains("World"));   // true
        
        // 替换
        String replaced = str.replace("World", "Java");
        System.out.println("替换后: " + replaced);
        
        // 分割
        String[] parts = "apple,banana,orange".split(",");
        for (String part : parts) {
            System.out.println(part);
        }
        
        // 连接
        String joined = String.join("-", "a", "b", "c");
        System.out.println("连接: " + joined);  // "a-b-c"
        
        // 大小写转换
        System.out.println("大写: " + str.toUpperCase());  // "HELLO WORLD"
        System.out.println("小写: " + str.toLowerCase());  // "hello world"
        
        // 去除空白
        String trimmed = "  Hello  ".trim();
        System.out.println("去除空白: '" + trimmed + "'");
        
        // 比较
        System.out.println("比较: " + str.compareTo("Hello"));  // 正数
        System.out.println("忽略大小写比较: " + 
                         str.equalsIgnoreCase("HELLO WORLD"));  // true
        
        // 格式化
        String formatted = String.format("姓名: %s, 年龄: %d", "张三", 25);
        System.out.println(formatted);
    }
}
```

### StringBuilder和StringBuffer

```java
public class StringBuilderDemo {
    public static void main(String[] args) {
        // StringBuilder：可变字符串，非线程安全（更快）
        StringBuilder sb = new StringBuilder();
        
        sb.append("Hello");
        sb.append(" ");
        sb.append("World");
        
        System.out.println(sb.toString());  // "Hello World"
        
        // 插入
        sb.insert(5, ",");
        System.out.println(sb.toString());  // "Hello, World"
        
        // 删除
        sb.delete(5, 6);
        System.out.println(sb.toString());  // "Hello World"
        
        // 反转
        sb.reverse();
        System.out.println(sb.toString());  // "dlroW olleH"
        
        // 重置
        sb.setLength(0);
        
        // StringBuffer：线程安全（但更慢）
        StringBuffer sbf = new StringBuffer();
        sbf.append("Thread safe");
        System.out.println(sbf.toString());
    }
}
```

## 3. 包装类

### 包装类基础

```java
public class WrapperClasses {
    public static void main(String[] args) {
        // 基本类型和包装类的对应关系
        // byte -> Byte
        // short -> Short
        // int -> Integer
        // long -> Long
        // float -> Float
        // double -> Double
        // char -> Character
        // boolean -> Boolean
        
        // 自动装箱（Autoboxing）
        Integer i = 10;  // 自动转换为Integer对象
        
        // 自动拆箱（Unboxing）
        int value = i;   // 自动转换为int值
        
        // 手动装箱和拆箱
        Integer i1 = Integer.valueOf(10);
        int v1 = i1.intValue();
        
        // 字符串转换
        String str = "123";
        int num = Integer.parseInt(str);
        Integer numObj = Integer.valueOf(str);
        
        String numStr = Integer.toString(123);
        String numStr2 = String.valueOf(123);
        
        // 常用方法
        System.out.println("最大值: " + Integer.MAX_VALUE);
        System.out.println("最小值: " + Integer.MIN_VALUE);
        System.out.println("位数: " + Integer.SIZE);
        
        // 进制转换
        System.out.println("二进制: " + Integer.toBinaryString(10));
        System.out.println("十六进制: " + Integer.toHexString(255));
        System.out.println("八进制: " + Integer.toOctalString(64));
        
        // Character类
        char c = 'A';
        System.out.println("是否字母: " + Character.isLetter(c));
        System.out.println("是否数字: " + Character.isDigit(c));
        System.out.println("是否空白: " + Character.isWhitespace(c));
        System.out.println("是否大写: " + Character.isUpperCase(c));
        System.out.println("转小写: " + Character.toLowerCase(c));
        System.out.println("转大写: " + Character.toUpperCase(c));
    }
}
```

## 4. Math类

```java
public class MathDemo {
    public static void main(String[] args) {
        // 数学常数
        System.out.println("PI: " + Math.PI);
        System.out.println("E: " + Math.E);
        
        // 绝对值
        System.out.println("绝对值: " + Math.abs(-10));  // 10
        
        // 最大值和最小值
        System.out.println("最大值: " + Math.max(10, 20));  // 20
        System.out.println("最小值: " + Math.min(10, 20));  // 10
        
        // 幂运算
        System.out.println("2的3次方: " + Math.pow(2, 3));  // 8.0
        
        // 平方根
        System.out.println("16的平方根: " + Math.sqrt(16));  // 4.0
        
        // 立方根
        System.out.println("27的立方根: " + Math.cbrt(27));  // 3.0
        
        // 对数
        System.out.println("自然对数: " + Math.log(Math.E));  // 1.0
        System.out.println("10为底对数: " + Math.log10(100));  // 2.0
        
        // 三角函数
        System.out.println("sin(π/2): " + Math.sin(Math.PI / 2));  // 1.0
        System.out.println("cos(0): " + Math.cos(0));  // 1.0
        System.out.println("tan(π/4): " + Math.tan(Math.PI / 4));  // 1.0
        
        // 取整
        System.out.println("向上取整: " + Math.ceil(3.7));   // 4.0
        System.out.println("向下取整: " + Math.floor(3.7));  // 3.0
        System.out.println("四舍五入: " + Math.round(3.7));  // 4
        
        // 随机数
        System.out.println("随机数[0,1): " + Math.random());
        int randomInt = (int) (Math.random() * 100);  // [0, 100)
        System.out.println("随机整数[0,100): " + randomInt);
    }
}
```

## 5. Random类

```java
import java.util.Random;

public class RandomDemo {
    public static void main(String[] args) {
        Random random = new Random();
        
        // 随机整数
        int randomInt = random.nextInt();
        System.out.println("随机整数: " + randomInt);
        
        // 指定范围的随机整数
        int randomInt0to99 = random.nextInt(100);  // [0, 100)
        System.out.println("随机整数[0,100): " + randomInt0to99);
        
        // 随机浮点数
        double randomDouble = random.nextDouble();  // [0.0, 1.0)
        System.out.println("随机浮点数: " + randomDouble);
        
        // 随机布尔值
        boolean randomBoolean = random.nextBoolean();
        System.out.println("随机布尔值: " + randomBoolean);
        
        // 随机字节数组
        byte[] randomBytes = new byte[5];
        random.nextBytes(randomBytes);
        System.out.print("随机字节数组: ");
        for (byte b : randomBytes) {
            System.out.print(b + " ");
        }
        System.out.println();
        
        // 使用种子（可重复的随机序列）
        Random seededRandom = new Random(12345);
        System.out.println("种子随机数1: " + seededRandom.nextInt(100));
        System.out.println("种子随机数2: " + seededRandom.nextInt(100));
        
        // 重置种子
        seededRandom.setSeed(12345);
        System.out.println("重置后随机数1: " + seededRandom.nextInt(100));  // 与第一个相同
    }
}
```

## 6. Date和Calendar（已废弃）

### Date类

```java
import java.util.Date;

public class DateDemo {
    public static void main(String[] args) {
        // 创建当前时间的Date对象
        Date now = new Date();
        System.out.println("当前时间: " + now);
        
        // 创建指定时间的Date对象（已废弃）
        // Date date = new Date(year, month, day);
        
        // 获取时间戳
        long timestamp = now.getTime();
        System.out.println("时间戳: " + timestamp);
        
        // 从时间戳创建Date
        Date dateFromTimestamp = new Date(timestamp);
        System.out.println("从时间戳创建: " + dateFromTimestamp);
        
        // 比较时间
        Date date1 = new Date();
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        Date date2 = new Date();
        
        System.out.println("date1在date2之前: " + date1.before(date2));
        System.out.println("date1在date2之后: " + date1.after(date2));
        System.out.println("date1等于date2: " + date1.equals(date2));
    }
}
```

### Calendar类

```java
import java.util.Calendar;

public class CalendarDemo {
    public static void main(String[] args) {
        Calendar calendar = Calendar.getInstance();
        
        // 获取日期和时间
        int year = calendar.get(Calendar.YEAR);
        int month = calendar.get(Calendar.MONTH) + 1;  // 月份从0开始
        int day = calendar.get(Calendar.DAY_OF_MONTH);
        int hour = calendar.get(Calendar.HOUR_OF_DAY);
        int minute = calendar.get(Calendar.MINUTE);
        int second = calendar.get(Calendar.SECOND);
        
        System.out.printf("当前时间: %d-%02d-%02d %02d:%02d:%02d%n",
                         year, month, day, hour, minute, second);
        
        // 设置日期和时间
        calendar.set(2024, Calendar.JANUARY, 1, 0, 0, 0);
        
        // 添加时间
        calendar.add(Calendar.DAY_OF_MONTH, 10);  // 加10天
        calendar.add(Calendar.MONTH, 2);          // 加2个月
        
        // 获取星期
        int dayOfWeek = calendar.get(Calendar.DAY_OF_WEEK);
        String[] days = {"", "日", "一", "二", "三", "四", "五", "六"};
        System.out.println("星期" + days[dayOfWeek]);
    }
}
```

### LocalDateTime（Java 8+推荐）

```java
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class LocalDateTimeDemo {
    public static void main(String[] args) {
        // 获取当前时间
        LocalDateTime now = LocalDateTime.now();
        System.out.println("当前时间: " + now);
        
        // 创建指定时间
        LocalDateTime dateTime = LocalDateTime.of(2024, 1, 1, 12, 0, 0);
        System.out.println("指定时间: " + dateTime);
        
        // 获取各部分
        System.out.println("年: " + now.getYear());
        System.out.println("月: " + now.getMonthValue());
        System.out.println("日: " + now.getDayOfMonth());
        System.out.println("时: " + now.getHour());
        System.out.println("分: " + now.getMinute());
        System.out.println("秒: " + now.getSecond());
        
        // 格式化
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        String formatted = now.format(formatter);
        System.out.println("格式化: " + formatted);
        
        // 解析
        LocalDateTime parsed = LocalDateTime.parse("2024-01-01 12:00:00", formatter);
        System.out.println("解析: " + parsed);
        
        // 添加时间
        LocalDateTime future = now.plusDays(10).plusMonths(2);
        System.out.println("10天2个月后: " + future);
    }
}
```

## 7. 格式化类

### DecimalFormat

```java
import java.text.DecimalFormat;

public class DecimalFormatDemo {
    public static void main(String[] args) {
        double value = 1234.56789;
        
        // 格式化模式
        DecimalFormat df1 = new DecimalFormat("#.##");
        System.out.println("格式1: " + df1.format(value));  // 1234.57
        
        DecimalFormat df2 = new DecimalFormat("#.00");
        System.out.println("格式2: " + df2.format(value));  // 1234.57
        
        DecimalFormat df3 = new DecimalFormat("0.00");
        System.out.println("格式3: " + df3.format(value));  // 1234.57
        
        DecimalFormat df4 = new DecimalFormat("###,###.##");
        System.out.println("格式4: " + df4.format(value));  // 1,234.57
        
        DecimalFormat df5 = new DecimalFormat("0000.00");
        System.out.println("格式5: " + df5.format(value));  // 1234.57
        
        // 货币格式
        DecimalFormat currency = new DecimalFormat("¥###,###.##");
        System.out.println("货币格式: " + currency.format(value));  // ¥1,234.57
        
        // 百分比格式
        DecimalFormat percent = new DecimalFormat("#.##%");
        System.out.println("百分比格式: " + percent.format(0.123));  // 12.3%
    }
}
```

### SimpleDateFormat

```java
import java.text.SimpleDateFormat;
import java.util.Date;

public class SimpleDateFormatDemo {
    public static void main(String[] args) {
        Date now = new Date();
        
        // 格式化日期
        SimpleDateFormat sdf1 = new SimpleDateFormat("yyyy-MM-dd");
        System.out.println("格式1: " + sdf1.format(now));
        
        SimpleDateFormat sdf2 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        System.out.println("格式2: " + sdf2.format(now));
        
        SimpleDateFormat sdf3 = new SimpleDateFormat("yyyy年MM月dd日 HH时mm分ss秒");
        System.out.println("格式3: " + sdf3.format(now));
        
        SimpleDateFormat sdf4 = new SimpleDateFormat("EEEE, MMMM dd, yyyy");
        System.out.println("格式4: " + sdf4.format(now));
        
        // 解析日期
        try {
            String dateStr = "2024-01-01 12:00:00";
            Date parsedDate = sdf2.parse(dateStr);
            System.out.println("解析: " + parsedDate);
        } catch (Exception e) {
            System.out.println("解析错误: " + e.getMessage());
        }
    }
}
```

## 8. 综合示例

```java
import java.util.Random;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.text.DecimalFormat;

public class ClassLibraryDemo {
    public static void main(String[] args) {
        // 随机数生成
        Random random = new Random();
        double[] scores = new double[5];
        for (int i = 0; i < scores.length; i++) {
            scores[i] = 60 + random.nextDouble() * 40;  // 60-100分
        }
        
        // 计算平均分
        double sum = 0;
        for (double score : scores) {
            sum += score;
        }
        double average = sum / scores.length;
        
        // 格式化输出
        DecimalFormat df = new DecimalFormat("#.##");
        System.out.println("分数: ");
        for (int i = 0; i < scores.length; i++) {
            System.out.println("学生" + (i + 1) + ": " + df.format(scores[i]));
        }
        System.out.println("平均分: " + df.format(average));
        
        // 时间戳
        LocalDateTime now = LocalDateTime.now();
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        System.out.println("统计时间: " + now.format(formatter));
    }
}
```

## 练习

1. 实现一个工具类，提供字符串操作的各种方法
2. 创建一个数学工具类，封装常用的数学运算
3. 实现一个日期时间工具类，提供格式化、解析等功能
4. 使用StringBuilder实现一个字符串拼接工具
5. 创建一个随机数生成器类，提供各种随机数生成方法

