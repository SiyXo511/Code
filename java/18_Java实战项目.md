# Java实战项目

本教程将提供几个完整的Java实战项目，包括学生管理系统、图书管理系统等。

## 1. 学生管理系统（完整版）

```java
import java.util.*;
import java.io.*;

class Student implements Serializable {
    private static final long serialVersionUID = 1L;
    
    private int id;
    private String name;
    private int age;
    private double score;
    
    public Student(int id, String name, int age, double score) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.score = score;
    }
    
    // Getters and Setters
    public int getId() { return id; }
    public void setId(int id) { this.id = id; }
    
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    
    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }
    
    public double getScore() { return score; }
    public void setScore(double score) { this.score = score; }
    
    @Override
    public String toString() {
        return String.format("ID: %d, 姓名: %s, 年龄: %d, 分数: %.2f", 
                           id, name, age, score);
    }
    
    public String toFileString() {
        return id + " " + name + " " + age + " " + score;
    }
    
    public static Student fromFileString(String line) {
        Scanner scanner = new Scanner(line);
        int id = scanner.nextInt();
        String name = scanner.next();
        int age = scanner.nextInt();
        double score = scanner.nextDouble();
        scanner.close();
        return new Student(id, name, age, score);
    }
}

class StudentManager {
    private List<Student> students;
    private String filename;
    private int nextId;
    
    public StudentManager(String filename) {
        this.filename = filename;
        this.students = new ArrayList<>();
        this.nextId = 1;
        loadFromFile();
        updateNextId();
    }
    
    private void updateNextId() {
        nextId = students.stream()
                        .mapToInt(Student::getId)
                        .max()
                        .orElse(0) + 1;
    }
    
    public void addStudent(String name, int age, double score) {
        Student student = new Student(nextId++, name, age, score);
        students.add(student);
        saveToFile();
        System.out.println("学生添加成功！ID: " + student.getId());
    }
    
    public void deleteStudent(int id) {
        if (students.removeIf(s -> s.getId() == id)) {
            saveToFile();
            System.out.println("学生删除成功！");
        } else {
            System.out.println("未找到ID为 " + id + " 的学生！");
        }
    }
    
    public Student findStudent(int id) {
        return students.stream()
                      .filter(s -> s.getId() == id)
                      .findFirst()
                      .orElse(null);
    }
    
    public void updateStudent(int id, String name, int age, double score) {
        Student student = findStudent(id);
        if (student != null) {
            student.setName(name);
            student.setAge(age);
            student.setScore(score);
            saveToFile();
            System.out.println("学生信息更新成功！");
        } else {
            System.out.println("未找到ID为 " + id + " 的学生！");
        }
    }
    
    public void displayAll() {
        if (students.isEmpty()) {
            System.out.println("暂无学生信息！");
            return;
        }
        
        System.out.println("\n========== 所有学生 ==========");
        System.out.printf("%-10s%-20s%-10s%-10s%n", "ID", "姓名", "年龄", "分数");
        System.out.println("----------------------------------------");
        students.forEach(s -> System.out.printf("%-10d%-20s%-10d%-10.2f%n",
                          s.getId(), s.getName(), s.getAge(), s.getScore()));
    }
    
    public void sortByScore() {
        students.sort((a, b) -> Double.compare(b.getScore(), a.getScore()));
        System.out.println("按分数排序完成！");
    }
    
    public void statistics() {
        if (students.isEmpty()) {
            System.out.println("暂无学生信息！");
            return;
        }
        
        double sum = students.stream().mapToDouble(Student::getScore).sum();
        double average = sum / students.size();
        double max = students.stream().mapToDouble(Student::getScore).max().orElse(0);
        double min = students.stream().mapToDouble(Student::getScore).min().orElse(0);
        
        System.out.println("\n========== 统计信息 ==========");
        System.out.println("学生总数: " + students.size());
        System.out.printf("平均分: %.2f%n", average);
        System.out.printf("最高分: %.2f%n", max);
        System.out.printf("最低分: %.2f%n", min);
    }
    
    private void loadFromFile() {
        File file = new File(filename);
        if (!file.exists()) {
            return;
        }
        
        try (Scanner scanner = new Scanner(file)) {
            while (scanner.hasNextLine()) {
                String line = scanner.nextLine();
                if (!line.trim().isEmpty()) {
                    students.add(Student.fromFileString(line));
                }
            }
        } catch (FileNotFoundException e) {
            System.out.println("文件未找到: " + e.getMessage());
        }
    }
    
    private void saveToFile() {
        try (PrintWriter pw = new PrintWriter(new FileWriter(filename))) {
            for (Student student : students) {
                pw.println(student.toFileString());
            }
        } catch (IOException e) {
            System.out.println("保存失败: " + e.getMessage());
        }
    }
}

public class StudentManagementSystem {
    public static void main(String[] args) {
        StudentManager manager = new StudentManager("students.txt");
        Scanner scanner = new Scanner(System.in);
        
        while (true) {
            showMenu();
            System.out.print("请选择操作: ");
            int choice = scanner.nextInt();
            scanner.nextLine();  // 清除换行符
            
            if (choice == 0) {
                System.out.println("感谢使用！再见！");
                break;
            }
            
            switch (choice) {
                case 1: {
                    System.out.print("请输入姓名: ");
                    String name = scanner.nextLine();
                    System.out.print("请输入年龄: ");
                    int age = scanner.nextInt();
                    System.out.print("请输入分数: ");
                    double score = scanner.nextDouble();
                    manager.addStudent(name, age, score);
                    break;
                }
                case 2: {
                    System.out.print("请输入要删除的学生ID: ");
                    int id = scanner.nextInt();
                    manager.deleteStudent(id);
                    break;
                }
                case 3: {
                    System.out.print("请输入要查找的学生ID: ");
                    int id = scanner.nextInt();
                    Student student = manager.findStudent(id);
                    if (student != null) {
                        System.out.println("找到学生: " + student);
                    } else {
                        System.out.println("未找到该学生！");
                    }
                    break;
                }
                case 4: {
                    System.out.print("请输入要修改的学生ID: ");
                    int id = scanner.nextInt();
                    scanner.nextLine();
                    System.out.print("请输入新姓名: ");
                    String name = scanner.nextLine();
                    System.out.print("请输入新年龄: ");
                    int age = scanner.nextInt();
                    System.out.print("请输入新分数: ");
                    double score = scanner.nextDouble();
                    manager.updateStudent(id, name, age, score);
                    break;
                }
                case 5:
                    manager.displayAll();
                    break;
                case 6:
                    manager.sortByScore();
                    manager.displayAll();
                    break;
                case 7:
                    manager.statistics();
                    break;
                default:
                    System.out.println("无效的选择！");
            }
            
            System.out.println("\n按回车继续...");
            scanner.nextLine();
        }
        
        scanner.close();
    }
    
    private static void showMenu() {
        System.out.println("\n========== 学生管理系统 ==========");
        System.out.println("1. 添加学生");
        System.out.println("2. 删除学生");
        System.out.println("3. 查找学生");
        System.out.println("4. 修改学生信息");
        System.out.println("5. 显示所有学生");
        System.out.println("6. 按分数排序");
        System.out.println("7. 统计信息");
        System.out.println("0. 退出");
        System.out.println("===================================");
    }
}
```

## 2. 图书管理系统

```java
import java.util.*;

class Book {
    private int id;
    private String title;
    private String author;
    private boolean available;
    
    public Book(int id, String title, String author) {
        this.id = id;
        this.title = title;
        this.author = author;
        this.available = true;
    }
    
    public int getId() { return id; }
    public String getTitle() { return title; }
    public String getAuthor() { return author; }
    public boolean isAvailable() { return available; }
    
    public void borrow() { available = false; }
    public void returnBook() { available = true; }
    
    @Override
    public String toString() {
        return String.format("ID: %d, 书名: %s, 作者: %s, 状态: %s",
                           id, title, author, available ? "可借" : "已借出");
    }
}

class Library {
    private Map<Integer, Book> books;
    private int nextId;
    
    public Library() {
        this.books = new HashMap<>();
        this.nextId = 1;
    }
    
    public void addBook(String title, String author) {
        Book book = new Book(nextId++, title, author);
        books.put(book.getId(), book);
        System.out.println("图书添加成功！ID: " + book.getId());
    }
    
    public void borrowBook(int id) {
        Book book = books.get(id);
        if (book != null) {
            if (book.isAvailable()) {
                book.borrow();
                System.out.println("借书成功！");
            } else {
                System.out.println("该书已被借出！");
            }
        } else {
            System.out.println("未找到该书！");
        }
    }
    
    public void returnBook(int id) {
        Book book = books.get(id);
        if (book != null) {
            book.returnBook();
            System.out.println("还书成功！");
        } else {
            System.out.println("未找到该书！");
        }
    }
    
    public void displayAll() {
        if (books.isEmpty()) {
            System.out.println("暂无图书！");
            return;
        }
        
        System.out.println("\n========== 所有图书 ==========");
        books.values().forEach(System.out::println);
    }
    
    public void searchByTitle(String keyword) {
        boolean found = false;
        for (Book book : books.values()) {
            if (book.getTitle().contains(keyword)) {
                System.out.println(book);
                found = true;
            }
        }
        if (!found) {
            System.out.println("未找到相关图书！");
        }
    }
}

public class LibraryManagementSystem {
    public static void main(String[] args) {
        Library library = new Library();
        Scanner scanner = new Scanner(System.in);
        
        // 添加示例图书
        library.addBook("Java编程思想", "Bruce Eckel");
        library.addBook("算法导论", "Thomas H. Cormen");
        library.addBook("设计模式", "Gang of Four");
        
        while (true) {
            System.out.println("\n========== 图书管理系统 ==========");
            System.out.println("1. 添加图书");
            System.out.println("2. 借书");
            System.out.println("3. 还书");
            System.out.println("4. 显示所有图书");
            System.out.println("5. 搜索图书");
            System.out.println("0. 退出");
            System.out.print("请选择操作: ");
            
            int choice = scanner.nextInt();
            scanner.nextLine();
            
            if (choice == 0) {
                break;
            }
            
            switch (choice) {
                case 1: {
                    System.out.print("请输入书名: ");
                    String title = scanner.nextLine();
                    System.out.print("请输入作者: ");
                    String author = scanner.nextLine();
                    library.addBook(title, author);
                    break;
                }
                case 2: {
                    System.out.print("请输入图书ID: ");
                    int id = scanner.nextInt();
                    library.borrowBook(id);
                    break;
                }
                case 3: {
                    System.out.print("请输入图书ID: ");
                    int id = scanner.nextInt();
                    library.returnBook(id);
                    break;
                }
                case 4:
                    library.displayAll();
                    break;
                case 5: {
                    System.out.print("请输入关键词: ");
                    String keyword = scanner.nextLine();
                    library.searchByTitle(keyword);
                    break;
                }
            }
        }
        
        scanner.close();
    }
}
```

## 3. 计算器程序

```java
import java.util.Scanner;
import java.util.Stack;

public class Calculator {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        while (true) {
            System.out.print("请输入表达式（输入'quit'退出）: ");
            String expression = scanner.nextLine();
            
            if ("quit".equalsIgnoreCase(expression)) {
                break;
            }
            
            try {
                double result = evaluate(expression);
                System.out.println("结果: " + result);
            } catch (Exception e) {
                System.out.println("错误: " + e.getMessage());
            }
        }
        
        scanner.close();
    }
    
    // 简单的表达式求值（支持+、-、*、/）
    public static double evaluate(String expression) {
        expression = expression.replaceAll("\\s+", "");
        
        Stack<Double> numbers = new Stack<>();
        Stack<Character> operators = new Stack<>();
        
        for (int i = 0; i < expression.length(); i++) {
            char c = expression.charAt(i);
            
            if (Character.isDigit(c)) {
                StringBuilder num = new StringBuilder();
                while (i < expression.length() && 
                      (Character.isDigit(expression.charAt(i)) || 
                       expression.charAt(i) == '.')) {
                    num.append(expression.charAt(i++));
                }
                i--;
                numbers.push(Double.parseDouble(num.toString()));
            } else if (c == '(') {
                operators.push(c);
            } else if (c == ')') {
                while (operators.peek() != '(') {
                    numbers.push(applyOperator(operators.pop(), 
                                              numbers.pop(), numbers.pop()));
                }
                operators.pop();
            } else if (isOperator(c)) {
                while (!operators.isEmpty() && 
                       precedence(operators.peek()) >= precedence(c)) {
                    numbers.push(applyOperator(operators.pop(), 
                                              numbers.pop(), numbers.pop()));
                }
                operators.push(c);
            }
        }
        
        while (!operators.isEmpty()) {
            numbers.push(applyOperator(operators.pop(), 
                                      numbers.pop(), numbers.pop()));
        }
        
        return numbers.pop();
    }
    
    private static boolean isOperator(char c) {
        return c == '+' || c == '-' || c == '*' || c == '/';
    }
    
    private static int precedence(char op) {
        if (op == '+' || op == '-') return 1;
        if (op == '*' || op == '/') return 2;
        return 0;
    }
    
    private static double applyOperator(char op, double b, double a) {
        switch (op) {
            case '+': return a + b;
            case '-': return a - b;
            case '*': return a * b;
            case '/':
                if (b == 0) throw new ArithmeticException("除数不能为零");
                return a / b;
            default: throw new IllegalArgumentException("无效的运算符");
        }
    }
}
```

## 4. 项目结构示例

```
StudentManagementSystem/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── example/
│   │   │           ├── Student.java
│   │   │           ├── StudentManager.java
│   │   │           └── Main.java
│   │   └── resources/
│   └── test/
│       └── java/
├── pom.xml (Maven项目)
└── README.md
```

## 5. 使用Maven构建项目

```xml
<!-- pom.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <groupId>com.example</groupId>
    <artifactId>student-management</artifactId>
    <version>1.0.0</version>
    
    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>
    
    <dependencies>
        <!-- 添加依赖 -->
    </dependencies>
</project>
```

## 练习

1. 完善学生管理系统，添加导入导出Excel功能
2. 实现一个完整的图书管理系统（带借阅记录）
3. 创建一个简单的文本编辑器
4. 实现一个计算器程序（支持表达式计算）
5. 开发一个简单的游戏（如猜数字、贪吃蛇）

