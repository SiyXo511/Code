# Java反射和注解

本教程将学习Java中的反射（Reflection）和注解（Annotation），这些是Java的高级特性。

## 1. 反射基础

### Class类

```java
import java.lang.reflect.*;

public class ReflectionBasics {
    public static void main(String[] args) throws ClassNotFoundException {
        // 获取Class对象的三种方式
        // 1. 通过类名.class
        Class<String> cls1 = String.class;
        
        // 2. 通过对象.getClass()
        String str = "Hello";
        Class<?> cls2 = str.getClass();
        
        // 3. 通过Class.forName()
        Class<?> cls3 = Class.forName("java.lang.String");
        
        System.out.println(cls1 == cls2);  // true
        System.out.println(cls2 == cls3);  // true
        
        // 获取类信息
        System.out.println("类名: " + cls1.getName());
        System.out.println("简单类名: " + cls1.getSimpleName());
        System.out.println("包名: " + cls1.getPackage().getName());
    }
}
```

### 获取类的成员

```java
import java.lang.reflect.*;

class Person {
    private String name;
    public int age;
    protected String email;
    
    public Person() {}
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    private void privateMethod() {}
}

public class ReflectionMembers {
    public static void main(String[] args) {
        Class<Person> cls = Person.class;
        
        // 获取所有字段
        System.out.println("=== 字段 ===");
        Field[] fields = cls.getDeclaredFields();
        for (Field field : fields) {
            System.out.println("字段: " + field.getName() 
                             + ", 类型: " + field.getType());
        }
        
        // 获取所有方法
        System.out.println("\n=== 方法 ===");
        Method[] methods = cls.getDeclaredMethods();
        for (Method method : methods) {
            System.out.println("方法: " + method.getName() 
                             + ", 参数: " + method.getParameterCount());
        }
        
        // 获取所有构造函数
        System.out.println("\n=== 构造函数 ===");
        Constructor<?>[] constructors = cls.getDeclaredConstructors();
        for (Constructor<?> constructor : constructors) {
            System.out.println("构造函数: " + constructor.getName() 
                             + ", 参数: " + constructor.getParameterCount());
        }
    }
}
```

## 2. 反射操作

### 创建对象

```java
import java.lang.reflect.*;

public class ReflectionCreateObject {
    public static void main(String[] args) {
        try {
            Class<Person> cls = Person.class;
            
            // 使用默认构造函数
            Person person1 = cls.newInstance();
            System.out.println("创建对象1: " + person1);
            
            // 使用带参数的构造函数
            Constructor<Person> constructor = cls.getConstructor(String.class, int.class);
            Person person2 = constructor.newInstance("张三", 25);
            System.out.println("创建对象2: " + person2.getName() + ", " + person2.getAge());
            
        } catch (Exception e) {
            System.out.println("创建对象失败: " + e.getMessage());
        }
    }
}
```

### 访问和修改字段

```java
import java.lang.reflect.*;

public class ReflectionFields {
    public static void main(String[] args) {
        try {
            Person person = new Person("李四", 30);
            Class<?> cls = person.getClass();
            
            // 获取字段
            Field nameField = cls.getDeclaredField("name");
            Field ageField = cls.getField("age");  // public字段可以直接获取
            
            // 设置可访问性（访问private字段）
            nameField.setAccessible(true);
            
            // 获取字段值
            String name = (String) nameField.get(person);
            int age = ageField.getInt(person);
            
            System.out.println("原值 - 姓名: " + name + ", 年龄: " + age);
            
            // 修改字段值
            nameField.set(person, "王五");
            ageField.setInt(person, 35);
            
            System.out.println("修改后 - 姓名: " + person.getName() 
                             + ", 年龄: " + person.getAge());
            
        } catch (Exception e) {
            System.out.println("访问字段失败: " + e.getMessage());
        }
    }
}
```

### 调用方法

```java
import java.lang.reflect.*;

public class ReflectionMethods {
    public static void main(String[] args) {
        try {
            Person person = new Person();
            Class<?> cls = person.getClass();
            
            // 获取方法
            Method setNameMethod = cls.getMethod("setName", String.class);
            Method getNameMethod = cls.getMethod("getName");
            Method privateMethod = cls.getDeclaredMethod("privateMethod");
            
            // 调用公共方法
            setNameMethod.invoke(person, "赵六");
            String name = (String) getNameMethod.invoke(person);
            System.out.println("姓名: " + name);
            
            // 调用私有方法（需要设置可访问性）
            privateMethod.setAccessible(true);
            privateMethod.invoke(person);
            
        } catch (Exception e) {
            System.out.println("调用方法失败: " + e.getMessage());
        }
    }
}
```

## 3. 注解基础

### 内置注解

```java
import java.lang.annotation.*;

// 自定义注解
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@interface MyAnnotation {
    String value() default "";
    int count() default 1;
}

class TestClass {
    @Override  // 标记方法重写
    public String toString() {
        return "TestClass";
    }
    
    @Deprecated  // 标记已废弃
    public void oldMethod() {
        System.out.println("旧方法");
    }
    
    @SuppressWarnings("unchecked")  // 抑制警告
    public void suppressWarning() {
        @SuppressWarnings("rawtypes")
        List list = new ArrayList();
    }
    
    @MyAnnotation(value = "测试", count = 5)
    public void annotatedMethod() {
        System.out.println("带注解的方法");
    }
}

public class AnnotationBasics {
    public static void main(String[] args) {
        TestClass obj = new TestClass();
        obj.oldMethod();  // 会有警告（已废弃）
    }
}
```

### 自定义注解

```java
import java.lang.annotation.*;

// 元注解：注解的注解
@Target({ElementType.TYPE, ElementType.METHOD, ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@interface Author {
    String name();
    String date();
    int version() default 1;
}

@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@interface Range {
    int min() default 0;
    int max() default 100;
}

// 使用注解
@Author(name = "张三", date = "2024-01-01", version = 1)
class MyClass {
    @Range(min = 0, max = 150)
    private int age;
    
    @Range(min = 1, max = 100)
    private int score;
    
    @Author(name = "李四", date = "2024-01-02")
    public void method() {
        System.out.println("方法");
    }
}

public class CustomAnnotation {
    public static void main(String[] args) {
        // 使用反射读取注解
        Class<MyClass> cls = MyClass.class;
        
        // 读取类注解
        Author classAnnotation = cls.getAnnotation(Author.class);
        if (classAnnotation != null) {
            System.out.println("作者: " + classAnnotation.name());
            System.out.println("日期: " + classAnnotation.date());
            System.out.println("版本: " + classAnnotation.version());
        }
        
        // 读取字段注解
        try {
            Field ageField = cls.getDeclaredField("age");
            Range range = ageField.getAnnotation(Range.class);
            if (range != null) {
                System.out.println("年龄范围: " + range.min() + "-" + range.max());
            }
        } catch (NoSuchFieldException e) {
            e.printStackTrace();
        }
    }
}
```

## 4. 注解处理

### 运行时处理注解

```java
import java.lang.reflect.*;
import java.lang.annotation.*;

@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@interface NotNull {
    String message() default "字段不能为空";
}

@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@interface Email {
    String message() default "必须是有效的邮箱格式";
}

class User {
    @NotNull(message = "用户名不能为空")
    private String username;
    
    @Email
    private String email;
    
    public User(String username, String email) {
        this.username = username;
        this.email = email;
    }
}

class Validator {
    public static void validate(Object obj) throws IllegalAccessException {
        Class<?> cls = obj.getClass();
        Field[] fields = cls.getDeclaredFields();
        
        for (Field field : fields) {
            field.setAccessible(true);
            
            // 检查@NotNull注解
            NotNull notNull = field.getAnnotation(NotNull.class);
            if (notNull != null) {
                Object value = field.get(obj);
                if (value == null || value.toString().isEmpty()) {
                    throw new IllegalArgumentException(
                        field.getName() + ": " + notNull.message());
                }
            }
            
            // 检查@Email注解
            Email email = field.getAnnotation(Email.class);
            if (email != null) {
                Object value = field.get(obj);
                if (value != null && !value.toString().contains("@")) {
                    throw new IllegalArgumentException(
                        field.getName() + ": " + email.message());
                }
            }
        }
    }
}

public class AnnotationProcessing {
    public static void main(String[] args) {
        try {
            User user1 = new User("张三", "zhangsan@example.com");
            Validator.validate(user1);
            System.out.println("user1验证通过");
            
            User user2 = new User("", "invalid-email");
            Validator.validate(user2);  // 会抛出异常
        } catch (Exception e) {
            System.out.println("验证失败: " + e.getMessage());
        }
    }
}
```

## 5. 综合示例

### 简单的ORM框架（使用反射和注解）

```java
import java.lang.annotation.*;
import java.lang.reflect.*;
import java.sql.*;
import java.util.*;

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@interface Table {
    String name();
}

@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@interface Column {
    String name();
    boolean primaryKey() default false;
}

@Table(name = "users")
class UserEntity {
    @Column(name = "id", primaryKey = true)
    private int id;
    
    @Column(name = "username")
    private String username;
    
    @Column(name = "email")
    private String email;
    
    // Getters and Setters
    public int getId() { return id; }
    public void setId(int id) { this.id = id; }
    public String getUsername() { return username; }
    public void setUsername(String username) { this.username = username; }
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
}

class SimpleORM {
    public static <T> T mapToObject(ResultSet rs, Class<T> cls) 
            throws Exception {
        T obj = cls.newInstance();
        Field[] fields = cls.getDeclaredFields();
        
        for (Field field : fields) {
            Column column = field.getAnnotation(Column.class);
            if (column != null) {
                field.setAccessible(true);
                Object value = rs.getObject(column.name());
                field.set(obj, value);
            }
        }
        
        return obj;
    }
    
    public static <T> List<T> query(Connection conn, Class<T> cls) 
            throws Exception {
        Table table = cls.getAnnotation(Table.class);
        if (table == null) {
            throw new IllegalArgumentException("类必须使用@Table注解");
        }
        
        String sql = "SELECT * FROM " + table.name();
        List<T> results = new ArrayList<>();
        
        try (Statement stmt = conn.createStatement();
             ResultSet rs = stmt.executeQuery(sql)) {
            
            while (rs.next()) {
                T obj = mapToObject(rs, cls);
                results.add(obj);
            }
        }
        
        return results;
    }
}

public class ReflectionORM {
    public static void main(String[] args) {
        // 示例：使用反射和注解实现简单的ORM
        System.out.println("使用反射和注解实现ORM");
    }
}
```

## 练习

1. 实现一个注解处理器，自动生成类的toString方法
2. 创建一个依赖注入框架的基础实现
3. 实现一个配置文件的注解驱动加载
4. 使用反射实现一个通用的对象克隆工具
5. 创建一个基于注解的验证框架

