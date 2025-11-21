# Java Lambda表达式和Stream API

本教程将学习Java 8+引入的Lambda表达式和Stream API，这是现代Java编程的重要特性。

## 1. Lambda表达式基础

### 函数式接口

```java
// 函数式接口：只有一个抽象方法的接口
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
}

public class LambdaBasics {
    public static void main(String[] args) {
        // 传统方式：使用匿名内部类
        Calculator add1 = new Calculator() {
            @Override
            public int calculate(int a, int b) {
                return a + b;
            }
        };
        
        // Lambda表达式
        Calculator add2 = (a, b) -> a + b;
        Calculator multiply = (a, b) -> a * b;
        Calculator subtract = (a, b) -> a - b;
        
        System.out.println(add2.calculate(10, 20));      // 30
        System.out.println(multiply.calculate(5, 6));    // 30
        System.out.println(subtract.calculate(10, 5));   // 5
    }
}
```

### Lambda表达式语法

```java
import java.util.function.*;

public class LambdaSyntax {
    public static void main(String[] args) {
        // 1. 无参数
        Runnable runnable = () -> System.out.println("Hello");
        runnable.run();
        
        // 2. 单个参数
        Consumer<String> consumer = (s) -> System.out.println(s);
        // 简化：单个参数可以省略括号
        Consumer<String> consumer2 = s -> System.out.println(s);
        consumer.accept("Hello");
        
        // 3. 多个参数
        BiFunction<Integer, Integer, Integer> add = (a, b) -> a + b;
        System.out.println(add.apply(10, 20));  // 30
        
        // 4. 有返回值的Lambda
        Function<String, Integer> length = s -> s.length();
        System.out.println(length.apply("Hello"));  // 5
        
        // 5. 多行代码（使用大括号和return）
        Function<Integer, Integer> square = x -> {
            int result = x * x;
            return result;
        };
        System.out.println(square.apply(5));  // 25
    }
}
```

## 2. 内置函数式接口

### Predicate

```java
import java.util.function.Predicate;
import java.util.ArrayList;
import java.util.List;

public class PredicateDemo {
    public static void main(String[] args) {
        List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        
        // Predicate：判断条件
        Predicate<Integer> isEven = n -> n % 2 == 0;
        Predicate<Integer> isGreaterThan5 = n -> n > 5;
        
        // test方法
        System.out.println(isEven.test(4));  // true
        System.out.println(isGreaterThan5.test(3));  // false
        
        // 组合Predicate
        Predicate<Integer> isEvenAndGreaterThan5 = isEven.and(isGreaterThan5);
        System.out.println(isEvenAndGreaterThan5.test(8));  // true
        
        // negate：取反
        Predicate<Integer> isOdd = isEven.negate();
        System.out.println(isOdd.test(3));  // true
        
        // 使用Predicate过滤
        numbers.stream()
               .filter(isEven)
               .forEach(System.out::println);
    }
}
```

### Function

```java
import java.util.function.Function;

public class FunctionDemo {
    public static void main(String[] args) {
        // Function：输入一个类型，返回另一个类型
        Function<String, Integer> length = s -> s.length();
        Function<Integer, Integer> square = x -> x * x;
        
        // apply方法
        System.out.println(length.apply("Hello"));  // 5
        System.out.println(square.apply(5));        // 25
        
        // compose：组合函数（先执行compose中的函数）
        Function<String, Integer> lengthThenSquare = square.compose(length);
        System.out.println(lengthThenSquare.apply("Hello"));  // 25
        
        // andThen：链式调用（先执行当前函数，再执行andThen中的函数）
        Function<String, String> toUpper = s -> s.toUpperCase();
        Function<String, Integer> upperThenLength = toUpper.andThen(length);
        System.out.println(upperThenLength.apply("Hello"));  // 5
    }
}
```

### Consumer和Supplier

```java
import java.util.function.Consumer;
import java.util.function.Supplier;

public class ConsumerSupplierDemo {
    public static void main(String[] args) {
        // Consumer：接受一个参数，不返回值
        Consumer<String> printer = s -> System.out.println(s);
        printer.accept("Hello");
        
        // 方法引用
        Consumer<String> printer2 = System.out::println;
        printer2.accept("World");
        
        // Supplier：不接受参数，返回一个值
        Supplier<String> supplier = () -> "Hello from Supplier";
        System.out.println(supplier.get());
        
        // 多个Consumer组合
        Consumer<String> consumer1 = s -> System.out.print("Prefix: ");
        Consumer<String> consumer2 = s -> System.out.println(s);
        Consumer<String> combined = consumer1.andThen(consumer2);
        combined.accept("Hello");
    }
}
```

## 3. 方法引用

```java
import java.util.List;
import java.util.function.*;

public class MethodReference {
    public static void main(String[] args) {
        List<String> list = List.of("Apple", "Banana", "Orange");
        
        // 1. 静态方法引用
        Function<String, Integer> parseInt = Integer::parseInt;
        System.out.println(parseInt.apply("123"));
        
        // 2. 实例方法引用（对象::方法）
        String str = "Hello";
        Supplier<Integer> length = str::length;
        System.out.println(length.get());
        
        // 3. 实例方法引用（类::实例方法）
        Function<String, String> toUpper = String::toUpperCase;
        System.out.println(toUpper.apply("hello"));
        
        // 4. 构造函数引用
        Supplier<List<String>> listSupplier = ArrayList::new;
        List<String> newList = listSupplier.get();
        
        // 5. 使用场景
        list.forEach(System.out::println);  // 方法引用
        list.forEach(s -> System.out.println(s));  // Lambda表达式（等价）
    }
}
```

## 4. Stream API基础

### Stream创建和基本操作

```java
import java.util.*;
import java.util.stream.*;

public class StreamBasics {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        
        // 创建Stream
        Stream<Integer> stream1 = numbers.stream();
        Stream<Integer> stream2 = Stream.of(1, 2, 3, 4, 5);
        Stream<Integer> stream3 = Stream.generate(() -> 1).limit(5);
        Stream<Integer> stream4 = Stream.iterate(0, n -> n + 2).limit(5);
        
        // 中间操作：过滤
        numbers.stream()
               .filter(n -> n % 2 == 0)  // 过滤偶数
               .forEach(System.out::print);  // 2 4 6 8 10
        System.out.println();
        
        // 中间操作：映射
        numbers.stream()
               .map(n -> n * 2)  // 每个元素乘以2
               .forEach(n -> System.out.print(n + " "));  // 2 4 6 8 10 ...
        System.out.println();
        
        // 终端操作：收集
        List<Integer> doubled = numbers.stream()
                                       .map(n -> n * 2)
                                       .collect(Collectors.toList());
        System.out.println("翻倍后: " + doubled);
    }
}
```

## 5. Stream常用操作

### 过滤和映射

```java
import java.util.*;
import java.util.stream.*;

public class StreamOperations {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David", "Eve");
        
        // filter：过滤
        List<String> longNames = names.stream()
                                      .filter(name -> name.length() > 4)
                                      .collect(Collectors.toList());
        System.out.println("长名字: " + longNames);
        
        // map：转换
        List<Integer> lengths = names.stream()
                                     .map(String::length)
                                     .collect(Collectors.toList());
        System.out.println("长度: " + lengths);
        
        // flatMap：扁平化
        List<List<String>> nested = Arrays.asList(
            Arrays.asList("a", "b"),
            Arrays.asList("c", "d")
        );
        List<String> flat = nested.stream()
                                  .flatMap(List::stream)
                                  .collect(Collectors.toList());
        System.out.println("扁平化: " + flat);
        
        // distinct：去重
        List<Integer> numbers = Arrays.asList(1, 2, 2, 3, 3, 3, 4, 4, 4, 4);
        List<Integer> unique = numbers.stream()
                                      .distinct()
                                      .collect(Collectors.toList());
        System.out.println("去重: " + unique);
        
        // sorted：排序
        List<String> sorted = names.stream()
                                   .sorted()
                                   .collect(Collectors.toList());
        System.out.println("排序: " + sorted);
        
        // limit和skip
        List<Integer> limited = numbers.stream()
                                       .limit(5)
                                       .collect(Collectors.toList());
        System.out.println("前5个: " + limited);
    }
}
```

### 终端操作

```java
import java.util.*;
import java.util.stream.*;

public class TerminalOperations {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        
        // forEach：遍历
        numbers.stream().forEach(System.out::print);
        System.out.println();
        
        // collect：收集
        List<Integer> evenNumbers = numbers.stream()
                                           .filter(n -> n % 2 == 0)
                                           .collect(Collectors.toList());
        
        // toArray：转换为数组
        Integer[] array = numbers.stream().toArray(Integer[]::new);
        
        // reduce：归约
        int sum = numbers.stream()
                         .reduce(0, Integer::sum);
        System.out.println("和: " + sum);
        
        int product = numbers.stream()
                             .reduce(1, (a, b) -> a * b);
        System.out.println("积: " + product);
        
        // max和min
        Optional<Integer> max = numbers.stream().max(Integer::compare);
        Optional<Integer> min = numbers.stream().min(Integer::compare);
        max.ifPresent(m -> System.out.println("最大值: " + m));
        min.ifPresent(m -> System.out.println("最小值: " + m));
        
        // count：计数
        long count = numbers.stream().count();
        System.out.println("数量: " + count);
        
        // anyMatch、allMatch、noneMatch
        boolean anyEven = numbers.stream().anyMatch(n -> n % 2 == 0);
        boolean allEven = numbers.stream().allMatch(n -> n % 2 == 0);
        boolean noneNegative = numbers.stream().noneMatch(n -> n < 0);
        
        System.out.println("有偶数: " + anyEven);
        System.out.println("都是偶数: " + allEven);
        System.out.println("没有负数: " + noneNegative);
        
        // findFirst、findAny
        Optional<Integer> first = numbers.stream().findFirst();
        Optional<Integer> any = numbers.stream().findAny();
        
        first.ifPresent(f -> System.out.println("第一个: " + f));
    }
}
```

### 收集器（Collectors）

```java
import java.util.*;
import java.util.stream.*;

public class CollectorsDemo {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David", "Eve");
        
        // toList
        List<String> list = names.stream().collect(Collectors.toList());
        
        // toSet
        Set<String> set = names.stream().collect(Collectors.toSet());
        
        // toMap
        Map<String, Integer> map = names.stream()
                                        .collect(Collectors.toMap(
                                            name -> name,
                                            String::length
                                        ));
        System.out.println("Map: " + map);
        
        // joining：连接字符串
        String joined = names.stream()
                             .collect(Collectors.joining(", "));
        System.out.println("连接: " + joined);
        
        // groupingBy：分组
        Map<Integer, List<String>> grouped = names.stream()
                                                   .collect(Collectors.groupingBy(String::length));
        System.out.println("按长度分组: " + grouped);
        
        // partitioningBy：分区
        Map<Boolean, List<String>> partitioned = names.stream()
                                                       .collect(Collectors.partitioningBy(
                                                           s -> s.length() > 4
                                                       ));
        System.out.println("分区（长度>4）: " + partitioned);
        
        // counting、summing、averaging
        long count = names.stream().collect(Collectors.counting());
        int totalLength = names.stream()
                               .collect(Collectors.summingInt(String::length));
        double avgLength = names.stream()
                                .collect(Collectors.averagingDouble(String::length));
        
        System.out.println("数量: " + count);
        System.out.println("总长度: " + totalLength);
        System.out.println("平均长度: " + avgLength);
    }
}
```

## 6. Optional类

```java
import java.util.Optional;

public class OptionalDemo {
    public static void main(String[] args) {
        // 创建Optional
        Optional<String> empty = Optional.empty();
        Optional<String> of = Optional.of("Hello");  // 不能为null
        Optional<String> ofNullable = Optional.ofNullable(null);  // 可以为null
        
        // 检查值是否存在
        if (of.isPresent()) {
            System.out.println("值存在: " + of.get());
        }
        
        // ifPresent：如果存在则执行
        of.ifPresent(s -> System.out.println("值: " + s));
        
        // orElse：如果不存在则返回默认值
        String value = ofNullable.orElse("默认值");
        System.out.println("值: " + value);
        
        // orElseGet：使用Supplier提供默认值
        String value2 = ofNullable.orElseGet(() -> "生成的默认值");
        
        // orElseThrow：如果不存在则抛出异常
        try {
            String value3 = ofNullable.orElseThrow(() -> new RuntimeException("值不存在"));
        } catch (RuntimeException e) {
            System.out.println("异常: " + e.getMessage());
        }
        
        // map和flatMap
        Optional<Integer> length = of.map(String::length);
        length.ifPresent(l -> System.out.println("长度: " + l));
        
        // filter
        Optional<String> filtered = of.filter(s -> s.length() > 5);
        System.out.println("过滤后: " + filtered.isPresent());
    }
}
```

## 7. 综合示例

```java
import java.util.*;
import java.util.stream.*;

class Person {
    private String name;
    private int age;
    private String city;
    
    public Person(String name, int age, String city) {
        this.name = name;
        this.age = age;
        this.city = city;
    }
    
    public String getName() { return name; }
    public int getAge() { return age; }
    public String getCity() { return city; }
    
    @Override
    public String toString() {
        return name + "(" + age + ", " + city + ")";
    }
}

public class StreamCompleteDemo {
    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
            new Person("Alice", 25, "Beijing"),
            new Person("Bob", 30, "Shanghai"),
            new Person("Charlie", 28, "Beijing"),
            new Person("David", 35, "Shanghai"),
            new Person("Eve", 22, "Beijing")
        );
        
        // 找出北京的人，按年龄排序，提取姓名
        List<String> beijingNames = people.stream()
                                          .filter(p -> p.getCity().equals("Beijing"))
                                          .sorted(Comparator.comparing(Person::getAge))
                                          .map(Person::getName)
                                          .collect(Collectors.toList());
        System.out.println("北京的人: " + beijingNames);
        
        // 按城市分组
        Map<String, List<Person>> byCity = people.stream()
                                                 .collect(Collectors.groupingBy(Person::getCity));
        System.out.println("按城市分组: " + byCity);
        
        // 计算每个城市的平均年龄
        Map<String, Double> avgAgeByCity = people.stream()
                                                  .collect(Collectors.groupingBy(
                                                      Person::getCity,
                                                      Collectors.averagingInt(Person::getAge)
                                                  ));
        System.out.println("每个城市的平均年龄: " + avgAgeByCity);
        
        // 找出年龄最大的人
        Optional<Person> oldest = people.stream()
                                        .max(Comparator.comparing(Person::getAge));
        oldest.ifPresent(p -> System.out.println("年龄最大: " + p));
    }
}
```

## 练习

1. 使用Lambda表达式实现各种函数式接口
2. 使用Stream API处理集合数据
3. 实现一个函数式编程风格的数据处理程序
4. 使用Optional处理可能为空的值
5. 使用Stream API实现复杂的数据查询和统计

