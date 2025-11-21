# Java泛型

本教程将学习Java中的泛型，包括泛型类、泛型方法、通配符等。

## 1. 泛型基础

### 为什么需要泛型

```java
import java.util.ArrayList;
import java.util.List;

public class WhyGenerics {
    public static void main(String[] args) {
        // 不使用泛型（Java 5之前）
        List list = new ArrayList();
        list.add("Hello");
        list.add(100);  // 可以添加任何类型
        
        // 需要强制类型转换
        String str = (String) list.get(0);
        // int num = (Integer) list.get(1);  // 可能抛出ClassCastException
        
        // 使用泛型（类型安全）
        List<String> stringList = new ArrayList<>();
        stringList.add("Hello");
        // stringList.add(100);  // 编译错误：类型不匹配
        
        // 不需要强制类型转换
        String s = stringList.get(0);
    }
}
```

### 泛型类

```java
// 泛型类
class Box<T> {
    private T content;
    
    public void setContent(T content) {
        this.content = content;
    }
    
    public T getContent() {
        return content;
    }
}

public class GenericClass {
    public static void main(String[] args) {
        // 使用泛型类
        Box<String> stringBox = new Box<>();
        stringBox.setContent("Hello");
        String str = stringBox.getContent();  // 不需要类型转换
        
        Box<Integer> intBox = new Box<>();
        intBox.setContent(100);
        Integer num = intBox.getContent();  // 不需要类型转换
        
        // 多个类型参数
        Pair<String, Integer> pair = new Pair<>("Alice", 95);
        System.out.println(pair.getFirst() + ": " + pair.getSecond());
    }
}

// 多个类型参数
class Pair<T, U> {
    private T first;
    private U second;
    
    public Pair(T first, U second) {
        this.first = first;
        this.second = second;
    }
    
    public T getFirst() {
        return first;
    }
    
    public U getSecond() {
        return second;
    }
    
    public void setFirst(T first) {
        this.first = first;
    }
    
    public void setSecond(U second) {
        this.second = second;
    }
}
```

## 2. 泛型方法

```java
public class GenericMethod {
    // 泛型方法
    public static <T> T getMiddle(T[] array) {
        return array[array.length / 2];
    }
    
    // 多个类型参数的泛型方法
    public static <T, U> U convert(T value, Class<U> targetClass) {
        // 简化的转换逻辑
        return (U) value;
    }
    
    // 泛型静态方法
    public static <T extends Comparable<T>> T max(T[] array) {
        if (array == null || array.length == 0) {
            return null;
        }
        T max = array[0];
        for (T item : array) {
            if (item.compareTo(max) > 0) {
                max = item;
            }
        }
        return max;
    }
    
    public static void main(String[] args) {
        String[] strings = {"apple", "banana", "orange"};
        System.out.println("中间元素: " + getMiddle(strings));
        
        Integer[] numbers = {1, 5, 3, 9, 2};
        System.out.println("最大值: " + max(numbers));
    }
}
```

## 3. 类型边界

### extends关键字

```java
// 类型上界
class NumberBox<T extends Number> {
    private T number;
    
    public NumberBox(T number) {
        this.number = number;
    }
    
    public T getNumber() {
        return number;
    }
    
    // 可以使用Number的方法
    public double doubleValue() {
        return number.doubleValue();
    }
}

public class BoundedGenerics {
    public static void main(String[] args) {
        NumberBox<Integer> intBox = new NumberBox<>(100);
        NumberBox<Double> doubleBox = new NumberBox<>(3.14);
        // NumberBox<String> stringBox = new NumberBox<>("Hello");  // 错误
        
        System.out.println(intBox.doubleValue());      // 100.0
        System.out.println(doubleBox.doubleValue());   // 3.14
    }
}
```

### 多个边界

```java
interface Comparable<T> {
    int compareTo(T other);
}

// 多个边界：必须同时是Number和Comparable
class BoundedType<T extends Number & Comparable<T>> {
    private T value;
    
    public BoundedType(T value) {
        this.value = value;
    }
    
    public T getValue() {
        return value;
    }
}
```

## 4. 通配符

### 上界通配符（? extends）

```java
import java.util.ArrayList;
import java.util.List;

public class WildcardExtends {
    // 使用上界通配符：可以接受Number及其子类
    public static double sum(List<? extends Number> numbers) {
        double sum = 0;
        for (Number num : numbers) {
            sum += num.doubleValue();
        }
        return sum;
    }
    
    public static void main(String[] args) {
        List<Integer> intList = new ArrayList<>();
        intList.add(1);
        intList.add(2);
        intList.add(3);
        
        List<Double> doubleList = new ArrayList<>();
        doubleList.add(1.5);
        doubleList.add(2.5);
        doubleList.add(3.5);
        
        System.out.println("整数和: " + sum(intList));     // 6.0
        System.out.println("浮点数和: " + sum(doubleList)); // 7.5
    }
}
```

### 下界通配符（? super）

```java
import java.util.ArrayList;
import java.util.List;

public class WildcardSuper {
    // 使用下界通配符：可以接受Integer及其父类
    public static void addNumbers(List<? super Integer> list) {
        list.add(1);
        list.add(2);
        list.add(3);
    }
    
    public static void main(String[] args) {
        List<Number> numberList = new ArrayList<>();
        List<Object> objectList = new ArrayList<>();
        
        addNumbers(numberList);
        addNumbers(objectList);
        
        // List<String> stringList = new ArrayList<>();
        // addNumbers(stringList);  // 错误：String不是Integer的父类
    }
}
```

### 无界通配符（?）

```java
import java.util.ArrayList;
import java.util.List;

public class UnboundedWildcard {
    // 无界通配符：可以接受任何类型
    public static void printList(List<?> list) {
        for (Object obj : list) {
            System.out.print(obj + " ");
        }
        System.out.println();
    }
    
    public static void main(String[] args) {
        List<String> stringList = new ArrayList<>();
        stringList.add("Hello");
        stringList.add("World");
        
        List<Integer> intList = new ArrayList<>();
        intList.add(1);
        intList.add(2);
        
        printList(stringList);  // 可以打印String列表
        printList(intList);     // 可以打印Integer列表
    }
}
```

## 5. 类型擦除

```java
import java.lang.reflect.Field;
import java.lang.reflect.Type;

public class TypeErasure {
    public static void main(String[] args) {
        // 类型擦除：运行时泛型信息被擦除
        List<String> stringList = new ArrayList<>();
        List<Integer> intList = new ArrayList<>();
        
        // 运行时类型相同
        System.out.println(stringList.getClass() == intList.getClass());  // true
        
        // 泛型信息在编译时检查，运行时擦除
        // 以下代码在运行时等价于：
        // List stringList = new ArrayList();
        // List intList = new ArrayList();
    }
}
```

## 6. 泛型限制

```java
public class GenericLimitations {
    // 限制1：不能创建泛型数组
    // T[] array = new T[10];  // 错误
    
    // 限制2：不能实例化泛型类型
    // T obj = new T();  // 错误
    
    // 限制3：不能使用基本类型作为类型参数
    // List<int> list = new ArrayList<>();  // 错误，需要使用包装类
    // List<Integer> list = new ArrayList<>();  // 正确
    
    // 限制4：不能抛出或捕获泛型类的实例
    // catch (T e) { }  // 错误
    
    // 限制5：不能重载参数类型擦除后相同的方法
    // public void method(List<String> list) { }
    // public void method(List<Integer> list) { }  // 错误：擦除后签名相同
}
```

## 7. 综合示例

### 泛型栈实现

```java
import java.util.ArrayList;
import java.util.List;

class Stack<T> {
    private List<T> elements = new ArrayList<>();
    
    public void push(T item) {
        elements.add(item);
    }
    
    public T pop() {
        if (isEmpty()) {
            throw new RuntimeException("栈为空");
        }
        return elements.remove(elements.size() - 1);
    }
    
    public T peek() {
        if (isEmpty()) {
            throw new RuntimeException("栈为空");
        }
        return elements.get(elements.size() - 1);
    }
    
    public boolean isEmpty() {
        return elements.isEmpty();
    }
    
    public int size() {
        return elements.size();
    }
}

public class GenericStack {
    public static void main(String[] args) {
        Stack<String> stringStack = new Stack<>();
        stringStack.push("Hello");
        stringStack.push("World");
        System.out.println("栈顶: " + stringStack.peek());
        System.out.println("出栈: " + stringStack.pop());
        
        Stack<Integer> intStack = new Stack<>();
        intStack.push(10);
        intStack.push(20);
        System.out.println("栈顶: " + intStack.peek());
        System.out.println("出栈: " + intStack.pop());
    }
}
```

## 练习

1. 实现一个泛型的Pair类，支持两个不同类型的值
2. 创建一个泛型的LinkedList类
3. 实现一个泛型的工具方法，计算数组的最大值和最小值
4. 使用泛型实现一个简单的缓存类
5. 实现一个泛型的二叉树类

