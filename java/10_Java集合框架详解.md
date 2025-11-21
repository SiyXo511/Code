# Java集合框架详解

本教程将深入学习Java集合框架，包括Collection接口、List、Set、Map的详细用法。

## 1. Collection接口

```java
import java.util.*;

public class CollectionInterface {
    public static void main(String[] args) {
        Collection<String> collection = new ArrayList<>();
        
        // 基本操作
        collection.add("Apple");
        collection.add("Banana");
        collection.add("Orange");
        
        System.out.println("大小: " + collection.size());
        System.out.println("是否为空: " + collection.isEmpty());
        System.out.println("包含Apple: " + collection.contains("Apple"));
        
        // 批量操作
        Collection<String> other = new ArrayList<>();
        other.add("Grape");
        other.add("Mango");
        
        collection.addAll(other);
        System.out.println("添加后大小: " + collection.size());
        
        // 移除
        collection.remove("Banana");
        collection.removeAll(Arrays.asList("Orange", "Grape"));
        
        // 遍历
        for (String item : collection) {
            System.out.print(item + " ");
        }
        System.out.println();
        
        // 转换为数组
        String[] array = collection.toArray(new String[0]);
        
        // 清空
        collection.clear();
    }
}
```

## 2. List接口详解

### ArrayList vs LinkedList

```java
import java.util.*;

public class ListComparison {
    public static void main(String[] args) {
        // ArrayList：基于数组，随机访问快
        List<Integer> arrayList = new ArrayList<>();
        
        // LinkedList：基于链表，插入删除快
        List<Integer> linkedList = new LinkedList<>();
        
        // 添加元素
        for (int i = 0; i < 100000; i++) {
            arrayList.add(i);
            linkedList.add(i);
        }
        
        // 随机访问
        long start = System.currentTimeMillis();
        int value = arrayList.get(50000);  // 很快
        long end = System.currentTimeMillis();
        System.out.println("ArrayList随机访问: " + (end - start) + "ms");
        
        // LinkedList随机访问较慢
        start = System.currentTimeMillis();
        value = linkedList.get(50000);  // 较慢
        end = System.currentTimeMillis();
        System.out.println("LinkedList随机访问: " + (end - start) + "ms");
    }
}
```

### List特有方法

```java
import java.util.*;

public class ListMethods {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        
        // 添加元素
        list.add("Apple");
        list.add("Banana");
        list.add("Orange");
        
        // 在指定位置插入
        list.add(1, "Grape");
        
        // 获取元素
        System.out.println("索引1的元素: " + list.get(1));
        
        // 设置元素
        list.set(0, "Mango");
        
        // 查找索引
        System.out.println("Orange的索引: " + list.indexOf("Orange"));
        System.out.println("Orange的最后索引: " + list.lastIndexOf("Orange"));
        
        // 子列表
        List<String> subList = list.subList(1, 3);
        System.out.println("子列表: " + subList);
        
        // 排序
        Collections.sort(list);
        System.out.println("排序后: " + list);
        
        // 反转
        Collections.reverse(list);
        System.out.println("反转后: " + list);
        
        // ListIterator
        ListIterator<String> iterator = list.listIterator();
        while (iterator.hasNext()) {
            String item = iterator.next();
            if (item.equals("Banana")) {
                iterator.set("New Banana");
            }
        }
    }
}
```

## 3. Set接口详解

### HashSet特性

```java
import java.util.*;

public class HashSetDemo {
    public static void main(String[] args) {
        // HashSet：基于哈希表，无序、不重复
        Set<String> set = new HashSet<>();
        
        set.add("Apple");
        set.add("Banana");
        set.add("Apple");  // 重复，不会添加
        set.add("Orange");
        
        System.out.println("大小: " + set.size());  // 3
        
        // 遍历（无序）
        for (String item : set) {
            System.out.print(item + " ");
        }
        System.out.println();
        
        // LinkedHashSet：保持插入顺序
        Set<String> linkedSet = new LinkedHashSet<>();
        linkedSet.add("Apple");
        linkedSet.add("Banana");
        linkedSet.add("Orange");
        
        System.out.println("LinkedHashSet:");
        for (String item : linkedSet) {
            System.out.print(item + " ");
        }
        System.out.println();
    }
}
```

### TreeSet特性

```java
import java.util.*;

public class TreeSetDemo {
    public static void main(String[] args) {
        // TreeSet：基于红黑树，有序、不重复
        Set<Integer> treeSet = new TreeSet<>();
        
        treeSet.add(5);
        treeSet.add(2);
        treeSet.add(8);
        treeSet.add(1);
        treeSet.add(3);
        
        // 遍历（有序：升序）
        System.out.println("TreeSet（升序）:");
        for (Integer value : treeSet) {
            System.out.print(value + " ");
        }
        System.out.println();
        
        // 降序排列
        TreeSet<Integer> descSet = new TreeSet<>((a, b) -> b - a);
        descSet.addAll(treeSet);
        
        System.out.println("TreeSet（降序）:");
        for (Integer value : descSet) {
            System.out.print(value + " ");
        }
        System.out.println();
        
        // 范围操作
        System.out.println("小于5的元素: " + treeSet.headSet(5));
        System.out.println("大于等于5的元素: " + treeSet.tailSet(5));
        System.out.println("范围[2,8): " + treeSet.subSet(2, 8));
    }
}
```

## 4. Map接口详解

### HashMap详细用法

```java
import java.util.*;

public class HashMapDemo {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        
        // 添加键值对
        map.put("Alice", 95);
        map.put("Bob", 87);
        map.put("Charlie", 92);
        map.put("Alice", 98);  // 更新值
        
        // 访问值
        System.out.println("Alice的分数: " + map.get("Alice"));
        System.out.println("David的分数: " + map.get("David"));  // null
        
        // 获取默认值
        int score = map.getOrDefault("David", 0);
        System.out.println("David的分数（默认）: " + score);
        
        // 检查键和值
        System.out.println("包含Alice: " + map.containsKey("Alice"));
        System.out.println("包含95: " + map.containsValue(95));
        
        // 获取所有键
        Set<String> keys = map.keySet();
        System.out.println("所有键: " + keys);
        
        // 获取所有值
        Collection<Integer> values = map.values();
        System.out.println("所有值: " + values);
        
        // 获取所有键值对
        Set<Map.Entry<String, Integer>> entries = map.entrySet();
        for (Map.Entry<String, Integer> entry : entries) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }
        
        // Java 8+: Lambda表达式遍历
        map.forEach((key, value) -> 
            System.out.println(key + ": " + value));
        
        // 替换
        map.replace("Bob", 90);
        System.out.println("替换后Bob的分数: " + map.get("Bob"));
        
        // 移除
        map.remove("Charlie");
        System.out.println("移除后大小: " + map.size());
        
        // 清空
        map.clear();
    }
}
```

### TreeMap详细用法

```java
import java.util.*;

public class TreeMapDemo {
    public static void main(String[] args) {
        // TreeMap：基于红黑树，按键排序
        Map<String, Integer> map = new TreeMap<>();
        
        map.put("Charlie", 92);
        map.put("Alice", 95);
        map.put("Bob", 87);
        
        // 遍历（按键排序）
        System.out.println("TreeMap（按键排序）:");
        map.forEach((key, value) -> 
            System.out.println(key + ": " + value));
        
        // 降序排列
        Map<String, Integer> descMap = new TreeMap<>((a, b) -> b.compareTo(a));
        descMap.putAll(map);
        
        System.out.println("\n降序排列:");
        descMap.forEach((key, value) -> 
            System.out.println(key + ": " + value));
        
        // 范围操作
        SortedMap<String, Integer> subMap = ((TreeMap<String, Integer>) map)
            .subMap("Alice", "Charlie");
        System.out.println("\n子映射[Alice, Charlie):");
        subMap.forEach((key, value) -> 
            System.out.println(key + ": " + value));
    }
}
```

## 5. Collections工具类

```java
import java.util.*;

public class CollectionsUtils {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<>(Arrays.asList(5, 2, 8, 1, 9, 3));
        
        // 排序
        Collections.sort(list);
        System.out.println("排序: " + list);
        
        // 反转
        Collections.reverse(list);
        System.out.println("反转: " + list);
        
        // 打乱
        Collections.shuffle(list);
        System.out.println("打乱: " + list);
        
        // 查找（需要先排序）
        Collections.sort(list);
        int index = Collections.binarySearch(list, 5);
        System.out.println("5的位置: " + index);
        
        // 最大值和最小值
        System.out.println("最大值: " + Collections.max(list));
        System.out.println("最小值: " + Collections.min(list));
        
        // 填充
        List<Integer> filled = new ArrayList<>(Arrays.asList(0, 0, 0, 0, 0));
        Collections.fill(filled, 100);
        System.out.println("填充: " + filled);
        
        // 替换
        List<String> strList = new ArrayList<>(Arrays.asList("a", "b", "a", "c"));
        Collections.replaceAll(strList, "a", "A");
        System.out.println("替换: " + strList);
        
        // 频率
        int frequency = Collections.frequency(strList, "A");
        System.out.println("A的频率: " + frequency);
        
        // 复制
        List<Integer> src = Arrays.asList(1, 2, 3, 4, 5);
        List<Integer> dest = new ArrayList<>(Arrays.asList(0, 0, 0, 0, 0));
        Collections.copy(dest, src);
        System.out.println("复制: " + dest);
        
        // 创建不可变集合
        List<String> unmodifiable = Collections.unmodifiableList(
            new ArrayList<>(Arrays.asList("a", "b", "c")));
        // unmodifiable.add("d");  // 错误：不能修改
        
        // 创建同步集合
        List<String> synchronized = Collections.synchronizedList(
            new ArrayList<>());
        synchronized.add("Thread-safe");
    }
}
```

## 6. 迭代器详解

```java
import java.util.*;

public class IteratorDemo {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>(Arrays.asList("Apple", "Banana", "Orange"));
        
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
        
        // ListIterator：支持双向遍历和修改
        List<String> list2 = new ArrayList<>(Arrays.asList("a", "b", "c"));
        ListIterator<String> listIterator = list2.listIterator();
        
        // 正向遍历
        System.out.println("\n正向遍历:");
        while (listIterator.hasNext()) {
            System.out.print(listIterator.next() + " ");
            listIterator.set(listIterator.next().toUpperCase());  // 可以修改
        }
        
        // 反向遍历
        System.out.println("\n反向遍历:");
        while (listIterator.hasPrevious()) {
            System.out.print(listIterator.previous() + " ");
        }
    }
}
```

## 7. 综合示例

### 学生成绩管理系统

```java
import java.util.*;

class Student {
    private String name;
    private int score;
    
    public Student(String name, int score) {
        this.name = name;
        this.score = score;
    }
    
    public String getName() { return name; }
    public int getScore() { return score; }
    
    @Override
    public String toString() {
        return name + ": " + score;
    }
}

public class StudentGradeSystem {
    public static void main(String[] args) {
        // 使用Map存储学生成绩
        Map<String, Student> students = new HashMap<>();
        
        students.put("001", new Student("Alice", 95));
        students.put("002", new Student("Bob", 87));
        students.put("003", new Student("Charlie", 92));
        students.put("004", new Student("David", 78));
        students.put("005", new Student("Eve", 90));
        
        // 显示所有学生
        System.out.println("所有学生成绩:");
        students.forEach((id, student) -> 
            System.out.println(id + ": " + student));
        
        // 按分数排序（使用TreeMap）
        Map<String, Student> sortedByScore = new TreeMap<>(
            (id1, id2) -> students.get(id2).getScore() - students.get(id1).getScore());
        sortedByScore.putAll(students);
        
        System.out.println("\n按分数排序:");
        sortedByScore.forEach((id, student) -> 
            System.out.println(student));
        
        // 统计
        double average = students.values().stream()
                                 .mapToInt(Student::getScore)
                                 .average()
                                 .orElse(0.0);
        System.out.println("\n平均分: " + average);
        
        // 找出高分学生（>=90）
        System.out.println("\n高分学生（>=90）:");
        students.values().stream()
                .filter(s -> s.getScore() >= 90)
                .forEach(System.out::println);
    }
}
```

## 练习

1. 使用ArrayList实现一个动态数组类
2. 使用HashMap实现一个字典系统
3. 使用TreeSet实现一个有序的去重功能
4. 实现一个统计文本中单词出现次数的程序
5. 使用集合实现一个简单的任务管理系统

