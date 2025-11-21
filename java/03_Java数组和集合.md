# Java数组和集合

本教程将学习Java中的数组和集合框架，包括数组、ArrayList、HashMap等常用数据结构。

## 1. 数组

### 一维数组

```java
public class ArrayDemo {
    public static void main(String[] args) {
        // 数组声明和初始化
        int[] arr1 = new int[5];              // 默认值为0
        int[] arr2 = {1, 2, 3, 4, 5};         // 初始化列表
        int[] arr3 = new int[]{1, 2, 3};      // 另一种初始化方式
        
        // 访问元素
        System.out.println(arr2[0]);  // 第一个元素
        System.out.println(arr2[4]);  // 最后一个元素
        
        // 数组长度
        System.out.println("数组长度: " + arr2.length);
        
        // 遍历数组
        // 传统for循环
        for (int i = 0; i < arr2.length; i++) {
            System.out.print(arr2[i] + " ");
        }
        System.out.println();
        
        // 增强for循环
        for (int value : arr2) {
            System.out.print(value + " ");
        }
        System.out.println();
    }
}
```

### 二维数组

```java
public class TwoDArray {
    public static void main(String[] args) {
        // 二维数组
        int[][] matrix = {
            {1, 2, 3, 4},
            {5, 6, 7, 8},
            {9, 10, 11, 12}
        };
        
        // 访问元素
        System.out.println(matrix[0][0]);  // 1
        System.out.println(matrix[1][2]);  // 7
        
        // 遍历二维数组
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[i].length; j++) {
                System.out.print(matrix[i][j] + "\t");
            }
            System.out.println();
        }
        
        // 增强for循环
        for (int[] row : matrix) {
            for (int value : row) {
                System.out.print(value + "\t");
            }
            System.out.println();
        }
    }
}
```

### 数组排序和查找

```java
import java.util.Arrays;

public class ArrayOperations {
    public static void main(String[] args) {
        int[] arr = {5, 2, 8, 1, 9, 3};
        
        // 排序（升序）
        Arrays.sort(arr);
        System.out.println("排序后: " + Arrays.toString(arr));
        
        // 查找（需要先排序）
        int index = Arrays.binarySearch(arr, 5);
        System.out.println("5的位置: " + index);
        
        // 填充
        int[] arr2 = new int[5];
        Arrays.fill(arr2, 10);
        System.out.println("填充后: " + Arrays.toString(arr2));
        
        // 复制
        int[] arr3 = Arrays.copyOf(arr, arr.length);
        System.out.println("复制: " + Arrays.toString(arr3));
        
        // 比较
        boolean equal = Arrays.equals(arr, arr3);
        System.out.println("是否相等: " + equal);
    }
}
```

## 2. List集合

### ArrayList

```java
import java.util.ArrayList;
import java.util.List;

public class ArrayListDemo {
    public static void main(String[] args) {
        // 创建ArrayList
        List<Integer> list1 = new ArrayList<>();
        ArrayList<String> list2 = new ArrayList<>();
        
        // 添加元素
        list1.add(10);
        list1.add(20);
        list1.add(30);
        list1.add(1, 15);  // 在索引1处插入
        
        // 访问元素
        System.out.println(list1.get(0));     // 10
        System.out.println(list1.get(1));     // 15
        
        // 修改元素
        list1.set(0, 100);
        
        // 删除元素
        list1.remove(1);         // 按索引删除
        list1.remove(Integer.valueOf(30));  // 按值删除
        
        // 大小
        System.out.println("大小: " + list1.size());
        System.out.println("是否为空: " + list1.isEmpty());
        
        // 包含
        System.out.println("包含20: " + list1.contains(20));
        
        // 遍历
        // 增强for循环
        for (Integer value : list1) {
            System.out.print(value + " ");
        }
        System.out.println();
        
        // 迭代器
        for (int i = 0; i < list1.size(); i++) {
            System.out.print(list1.get(i) + " ");
        }
        System.out.println();
        
        // 转换为数组
        Integer[] array = list1.toArray(new Integer[0]);
        System.out.println(Arrays.toString(array));
    }
}
```

### LinkedList

```java
import java.util.LinkedList;
import java.util.List;

public class LinkedListDemo {
    public static void main(String[] args) {
        List<String> list = new LinkedList<>();
        
        // 添加元素
        list.add("Apple");
        list.add("Banana");
        list.addFirst("Orange");  // 添加到开头
        list.addLast("Grape");    // 添加到末尾
        
        // 访问元素
        System.out.println("第一个: " + list.getFirst());
        System.out.println("最后一个: " + list.getLast());
        
        // 删除元素
        list.removeFirst();  // 删除第一个
        list.removeLast();   // 删除最后一个
        
        // 遍历
        for (String item : list) {
            System.out.print(item + " ");
        }
        System.out.println();
    }
}
```

## 3. Set集合

### HashSet

```java
import java.util.HashSet;
import java.util.Set;

public class HashSetDemo {
    public static void main(String[] args) {
        // HashSet：无序、不重复
        Set<String> set = new HashSet<>();
        
        // 添加元素
        set.add("Apple");
        set.add("Banana");
        set.add("Apple");  // 重复元素，不会添加
        set.add("Orange");
        
        System.out.println("大小: " + set.size());  // 3
        
        // 包含
        System.out.println("包含Apple: " + set.contains("Apple"));
        
        // 删除
        set.remove("Banana");
        
        // 遍历（无序）
        for (String item : set) {
            System.out.print(item + " ");
        }
        System.out.println();
        
        // 清空
        set.clear();
    }
}
```

### TreeSet

```java
import java.util.TreeSet;
import java.util.Set;

public class TreeSetDemo {
    public static void main(String[] args) {
        // TreeSet：有序、不重复（自然排序）
        Set<Integer> set = new TreeSet<>();
        
        set.add(5);
        set.add(2);
        set.add(8);
        set.add(1);
        set.add(3);
        
        // 遍历（有序）
        for (Integer value : set) {
            System.out.print(value + " ");
        }
        System.out.println();
        
        // 降序排列
        Set<Integer> descSet = new TreeSet<>((a, b) -> b - a);
        descSet.addAll(set);
        System.out.println("降序: " + descSet);
    }
}
```

## 4. Map集合

### HashMap

```java
import java.util.HashMap;
import java.util.Map;

public class HashMapDemo {
    public static void main(String[] args) {
        // HashMap：键值对，无序
        Map<String, Integer> map = new HashMap<>();
        
        // 添加键值对
        map.put("Alice", 95);
        map.put("Bob", 87);
        map.put("Charlie", 92);
        map.put("Alice", 98);  // 更新值
        
        // 访问值
        System.out.println("Alice的分数: " + map.get("Alice"));
        System.out.println("Bob的分数: " + map.get("Bob"));
        
        // 获取默认值
        int score = map.getOrDefault("David", 0);
        System.out.println("David的分数: " + score);
        
        // 包含
        System.out.println("包含Alice: " + map.containsKey("Alice"));
        System.out.println("包含95: " + map.containsValue(95));
        
        // 删除
        map.remove("Bob");
        
        // 大小
        System.out.println("大小: " + map.size());
        
        // 遍历
        // 方式1：键集合
        for (String key : map.keySet()) {
            System.out.println(key + ": " + map.get(key));
        }
        
        // 方式2：值集合
        for (Integer value : map.values()) {
            System.out.print(value + " ");
        }
        System.out.println();
        
        // 方式3：键值对（推荐）
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }
        
        // Java 8+: Lambda表达式
        map.forEach((key, value) -> 
            System.out.println(key + ": " + value));
    }
}
```

### TreeMap

```java
import java.util.TreeMap;
import java.util.Map;

public class TreeMapDemo {
    public static void main(String[] args) {
        // TreeMap：键值对，有序（按键排序）
        Map<String, Integer> map = new TreeMap<>();
        
        map.put("Charlie", 92);
        map.put("Alice", 95);
        map.put("Bob", 87);
        
        // 遍历（按键排序）
        map.forEach((key, value) -> 
            System.out.println(key + ": " + value));
        
        // 降序排列
        Map<String, Integer> descMap = new TreeMap<>((a, b) -> b.compareTo(a));
        descMap.putAll(map);
        System.out.println("\n降序:");
        descMap.forEach((key, value) -> 
            System.out.println(key + ": " + value));
    }
}
```

## 5. 集合工具类

### Collections

```java
import java.util.*;

public class CollectionsDemo {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<>();
        list.add(5);
        list.add(2);
        list.add(8);
        list.add(1);
        list.add(9);
        list.add(3);
        
        // 排序
        Collections.sort(list);
        System.out.println("排序: " + list);
        
        // 反转
        Collections.reverse(list);
        System.out.println("反转: " + list);
        
        // 打乱
        Collections.shuffle(list);
        System.out.println("打乱: " + list);
        
        // 查找
        Collections.sort(list);
        int index = Collections.binarySearch(list, 5);
        System.out.println("5的位置: " + index);
        
        // 最大值和最小值
        System.out.println("最大值: " + Collections.max(list));
        System.out.println("最小值: " + Collections.min(list));
        
        // 填充
        Collections.fill(list, 0);
        System.out.println("填充后: " + list);
        
        // 替换
        List<String> strList = Arrays.asList("a", "b", "a", "c");
        Collections.replaceAll(strList, "a", "A");
        System.out.println("替换后: " + strList);
        
        // 频率
        int frequency = Collections.frequency(strList, "A");
        System.out.println("A的频率: " + frequency);
    }
}
```

## 6. 迭代器

```java
import java.util.*;

public class IteratorDemo {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("Apple");
        list.add("Banana");
        list.add("Orange");
        
        // Iterator遍历
        Iterator<String> iterator = list.iterator();
        while (iterator.hasNext()) {
            String item = iterator.next();
            System.out.print(item + " ");
            // 可以在遍历时删除
            if (item.equals("Banana")) {
                iterator.remove();
            }
        }
        System.out.println("\n删除后: " + list);
        
        // ListIterator（支持双向遍历）
        ListIterator<String> listIterator = list.listIterator();
        
        // 正向遍历
        while (listIterator.hasNext()) {
            System.out.print(listIterator.next() + " ");
        }
        System.out.println();
        
        // 反向遍历
        while (listIterator.hasPrevious()) {
            System.out.print(listIterator.previous() + " ");
        }
        System.out.println();
    }
}
```

## 7. 综合示例

### 学生成绩管理系统

```java
import java.util.*;

public class StudentGradeSystem {
    public static void main(String[] args) {
        // 使用HashMap存储学生成绩
        Map<String, Integer> grades = new HashMap<>();
        
        // 添加学生成绩
        grades.put("Alice", 95);
        grades.put("Bob", 87);
        grades.put("Charlie", 92);
        grades.put("David", 78);
        grades.put("Eve", 90);
        
        // 显示所有学生
        System.out.println("所有学生成绩:");
        grades.forEach((name, score) -> 
            System.out.println(name + ": " + score));
        
        // 统计
        double average = grades.values().stream()
                               .mapToInt(Integer::intValue)
                               .average()
                               .orElse(0.0);
        System.out.println("\n平均分: " + average);
        
        // 按分数排序（使用TreeMap）
        Map<String, Integer> sortedByScore = new TreeMap<>((a, b) -> 
            grades.get(b).compareTo(grades.get(a)));
        sortedByScore.putAll(grades);
        
        System.out.println("\n按分数排序:");
        sortedByScore.forEach((name, score) -> 
            System.out.println(name + ": " + score));
        
        // 找出高分学生（分数>=90）
        System.out.println("\n高分学生（>=90）:");
        grades.entrySet().stream()
              .filter(entry -> entry.getValue() >= 90)
              .forEach(entry -> 
                  System.out.println(entry.getKey() + ": " + entry.getValue()));
    }
}
```

## 练习

1. 使用ArrayList实现一个动态数组类
2. 使用HashMap实现一个字典系统
3. 使用Set实现一个去重功能
4. 实现一个统计文本中单词出现次数的程序
5. 使用集合实现一个简单的任务管理系统

