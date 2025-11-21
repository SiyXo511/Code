# Java文件操作

本教程将学习Java中的文件操作，包括File类、字节流、字符流、NIO等。

## 1. File类

### 文件和目录操作

```java
import java.io.File;
import java.io.IOException;

public class FileDemo {
    public static void main(String[] args) {
        // 创建File对象
        File file = new File("test.txt");
        File dir = new File("test_dir");
        
        // 文件信息
        System.out.println("文件名: " + file.getName());
        System.out.println("路径: " + file.getPath());
        System.out.println("绝对路径: " + file.getAbsolutePath());
        System.out.println("是否存在: " + file.exists());
        System.out.println("是文件: " + file.isFile());
        System.out.println("是目录: " + file.isDirectory());
        System.out.println("大小: " + file.length() + " 字节");
        
        // 创建文件
        try {
            if (file.createNewFile()) {
                System.out.println("文件创建成功");
            }
        } catch (IOException e) {
            System.out.println("文件创建失败: " + e.getMessage());
        }
        
        // 创建目录
        if (dir.mkdir()) {
            System.out.println("目录创建成功");
        }
        
        // 删除文件
        // if (file.delete()) {
        //     System.out.println("文件删除成功");
        // }
        
        // 列出目录内容
        File currentDir = new File(".");
        String[] files = currentDir.list();
        if (files != null) {
            System.out.println("\n当前目录内容:");
            for (String fileName : files) {
                System.out.println(fileName);
            }
        }
    }
}
```

### 目录遍历

```java
import java.io.File;

public class DirectoryTraversal {
    public static void main(String[] args) {
        File dir = new File(".");
        
        // 遍历目录
        listFiles(dir, 0);
    }
    
    static void listFiles(File dir, int level) {
        if (!dir.isDirectory()) {
            return;
        }
        
        File[] files = dir.listFiles();
        if (files == null) {
            return;
        }
        
        for (File file : files) {
            // 打印缩进
            for (int i = 0; i < level; i++) {
                System.out.print("  ");
            }
            
            if (file.isDirectory()) {
                System.out.println("[目录] " + file.getName());
                listFiles(file, level + 1);  // 递归遍历
            } else {
                System.out.println("[文件] " + file.getName() 
                                 + " (" + file.length() + " 字节)");
            }
        }
    }
}
```

## 2. 字节流

### FileInputStream和FileOutputStream

```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class ByteStreamDemo {
    public static void main(String[] args) {
        String filename = "test.txt";
        
        // 写入文件（字节流）
        try (FileOutputStream fos = new FileOutputStream(filename)) {
            String data = "Hello, Java File I/O!";
            byte[] bytes = data.getBytes();
            fos.write(bytes);
            System.out.println("文件写入成功");
        } catch (IOException e) {
            System.out.println("写入失败: " + e.getMessage());
        }
        
        // 读取文件（字节流）
        try (FileInputStream fis = new FileInputStream(filename)) {
            int byteData;
            System.out.print("文件内容: ");
            while ((byteData = fis.read()) != -1) {
                System.out.print((char) byteData);
            }
            System.out.println();
        } catch (IOException e) {
            System.out.println("读取失败: " + e.getMessage());
        }
        
        // 读取到字节数组
        try (FileInputStream fis = new FileInputStream(filename)) {
            byte[] buffer = new byte[1024];
            int bytesRead = fis.read(buffer);
            if (bytesRead > 0) {
                String content = new String(buffer, 0, bytesRead);
                System.out.println("内容: " + content);
            }
        } catch (IOException e) {
            System.out.println("读取失败: " + e.getMessage());
        }
    }
}
```

### 文件复制

```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class FileCopy {
    public static void copyFile(String source, String dest) {
        try (FileInputStream fis = new FileInputStream(source);
             FileOutputStream fos = new FileOutputStream(dest)) {
            
            byte[] buffer = new byte[1024];
            int bytesRead;
            
            while ((bytesRead = fis.read(buffer)) != -1) {
                fos.write(buffer, 0, bytesRead);
            }
            
            System.out.println("文件复制成功: " + source + " -> " + dest);
        } catch (IOException e) {
            System.out.println("复制失败: " + e.getMessage());
        }
    }
    
    public static void main(String[] args) {
        copyFile("source.txt", "dest.txt");
    }
}
```

## 3. 字符流

### FileReader和FileWriter

```java
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class CharacterStreamDemo {
    public static void main(String[] args) {
        String filename = "text.txt";
        
        // 写入文件（字符流）
        try (FileWriter fw = new FileWriter(filename)) {
            fw.write("Hello, Java字符流！\n");
            fw.write("这是第二行\n");
            fw.write("这是第三行\n");
            System.out.println("文件写入成功");
        } catch (IOException e) {
            System.out.println("写入失败: " + e.getMessage());
        }
        
        // 读取文件（字符流）
        try (FileReader fr = new FileReader(filename)) {
            int charData;
            System.out.print("文件内容: ");
            while ((charData = fr.read()) != -1) {
                System.out.print((char) charData);
            }
            System.out.println();
        } catch (IOException e) {
            System.out.println("读取失败: " + e.getMessage());
        }
        
        // 读取到字符数组
        try (FileReader fr = new FileReader(filename)) {
            char[] buffer = new char[1024];
            int charsRead = fr.read(buffer);
            if (charsRead > 0) {
                String content = new String(buffer, 0, charsRead);
                System.out.println("内容:\n" + content);
            }
        } catch (IOException e) {
            System.out.println("读取失败: " + e.getMessage());
        }
    }
}
```

### BufferedReader和BufferedWriter

```java
import java.io.*;

public class BufferedStreamDemo {
    public static void main(String[] args) {
        String filename = "buffered.txt";
        
        // 使用缓冲写入
        try (BufferedWriter bw = new BufferedWriter(new FileWriter(filename))) {
            bw.write("第一行");
            bw.newLine();  // 换行
            bw.write("第二行");
            bw.newLine();
            bw.write("第三行");
            System.out.println("缓冲写入成功");
        } catch (IOException e) {
            System.out.println("写入失败: " + e.getMessage());
        }
        
        // 使用缓冲读取
        try (BufferedReader br = new BufferedReader(new FileReader(filename))) {
            String line;
            int lineNumber = 1;
            System.out.println("文件内容:");
            while ((line = br.readLine()) != null) {
                System.out.println(lineNumber + ": " + line);
                lineNumber++;
            }
        } catch (IOException e) {
            System.out.println("读取失败: " + e.getMessage());
        }
    }
}
```

## 4. PrintWriter和Scanner

### PrintWriter

```java
import java.io.*;

public class PrintWriterDemo {
    public static void main(String[] args) {
        String filename = "print.txt";
        
        try (PrintWriter pw = new PrintWriter(new FileWriter(filename))) {
            pw.println("Hello, PrintWriter!");
            pw.println(100);
            pw.println(3.14);
            pw.printf("格式化输出: %s, %d, %.2f%n", "Hello", 100, 3.14);
            System.out.println("PrintWriter写入成功");
        } catch (IOException e) {
            System.out.println("写入失败: " + e.getMessage());
        }
    }
}
```

### Scanner读取文件

```java
import java.io.*;
import java.util.Scanner;

public class ScannerFileDemo {
    public static void main(String[] args) {
        String filename = "data.txt";
        
        // 写入数据
        try (PrintWriter pw = new PrintWriter(new FileWriter(filename))) {
            pw.println("张三 25 95.5");
            pw.println("李四 30 87.0");
            pw.println("王五 28 92.5");
        } catch (IOException e) {
            System.out.println("写入失败: " + e.getMessage());
            return;
        }
        
        // 使用Scanner读取文件
        try (Scanner scanner = new Scanner(new File(filename))) {
            while (scanner.hasNextLine()) {
                String line = scanner.nextLine();
                Scanner lineScanner = new Scanner(line);
                
                String name = lineScanner.next();
                int age = lineScanner.nextInt();
                double score = lineScanner.nextDouble();
                
                System.out.printf("姓名: %s, 年龄: %d, 分数: %.2f%n", 
                                 name, age, score);
                
                lineScanner.close();
            }
        } catch (FileNotFoundException e) {
            System.out.println("文件未找到: " + e.getMessage());
        }
    }
}
```

## 5. 对象序列化

### Serializable接口

```java
import java.io.*;

// 可序列化的类
class Student implements Serializable {
    private static final long serialVersionUID = 1L;
    
    private int id;
    private String name;
    private int age;
    
    public Student(int id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }
    
    @Override
    public String toString() {
        return "Student{id=" + id + ", name='" + name + "', age=" + age + "}";
    }
}

public class SerializationDemo {
    public static void main(String[] args) {
        String filename = "student.ser";
        
        // 序列化（写入对象）
        Student student = new Student(1, "张三", 25);
        try (ObjectOutputStream oos = new ObjectOutputStream(
                new FileOutputStream(filename))) {
            oos.writeObject(student);
            System.out.println("对象序列化成功");
        } catch (IOException e) {
            System.out.println("序列化失败: " + e.getMessage());
        }
        
        // 反序列化（读取对象）
        try (ObjectInputStream ois = new ObjectInputStream(
                new FileInputStream(filename))) {
            Student readStudent = (Student) ois.readObject();
            System.out.println("对象反序列化成功");
            System.out.println(readStudent);
        } catch (IOException | ClassNotFoundException e) {
            System.out.println("反序列化失败: " + e.getMessage());
        }
    }
}
```

## 6. NIO（Java 7+）

### Files类

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.StandardOpenOption;
import java.util.List;

public class NIOFilesDemo {
    public static void main(String[] args) {
        Path path = Paths.get("nio_test.txt");
        
        // 写入文件
        try {
            String content = "Hello, NIO!";
            Files.write(path, content.getBytes(), StandardOpenOption.CREATE);
            System.out.println("文件写入成功");
        } catch (IOException e) {
            System.out.println("写入失败: " + e.getMessage());
        }
        
        // 读取文件（所有行）
        try {
            List<String> lines = Files.readAllLines(path);
            System.out.println("文件内容:");
            for (String line : lines) {
                System.out.println(line);
            }
        } catch (IOException e) {
            System.out.println("读取失败: " + e.getMessage());
        }
        
        // 读取文件（所有字节）
        try {
            byte[] bytes = Files.readAllBytes(path);
            String content = new String(bytes);
            System.out.println("内容: " + content);
        } catch (IOException e) {
            System.out.println("读取失败: " + e.getMessage());
        }
        
        // 检查文件是否存在
        System.out.println("文件存在: " + Files.exists(path));
        
        // 获取文件大小
        try {
            long size = Files.size(path);
            System.out.println("文件大小: " + size + " 字节");
        } catch (IOException e) {
            System.out.println("获取大小失败: " + e.getMessage());
        }
        
        // 复制文件
        try {
            Path dest = Paths.get("nio_test_copy.txt");
            Files.copy(path, dest);
            System.out.println("文件复制成功");
        } catch (IOException e) {
            System.out.println("复制失败: " + e.getMessage());
        }
        
        // 删除文件
        try {
            Files.deleteIfExists(Paths.get("nio_test_copy.txt"));
            System.out.println("文件删除成功");
        } catch (IOException e) {
            System.out.println("删除失败: " + e.getMessage());
        }
    }
}
```

## 7. 综合示例

### 学生信息管理系统（文件版本）

```java
import java.io.*;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

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
    
    public int getId() { return id; }
    public String getName() { return name; }
    public int getAge() { return age; }
    public double getScore() { return score; }
    
    @Override
    public String toString() {
        return String.format("%d %s %d %.2f", id, name, age, score);
    }
}

class StudentFileManager {
    private String filename;
    
    public StudentFileManager(String filename) {
        this.filename = filename;
    }
    
    // 从文件加载学生列表
    public List<Student> loadStudents() {
        List<Student> students = new ArrayList<>();
        
        try (Scanner scanner = new Scanner(new File(filename))) {
            while (scanner.hasNextLine()) {
                String line = scanner.nextLine();
                if (!line.isEmpty()) {
                    Scanner lineScanner = new Scanner(line);
                    int id = lineScanner.nextInt();
                    String name = lineScanner.next();
                    int age = lineScanner.nextInt();
                    double score = lineScanner.nextDouble();
                    
                    students.add(new Student(id, name, age, score));
                    lineScanner.close();
                }
            }
        } catch (FileNotFoundException e) {
            System.out.println("文件未找到，将创建新文件");
        }
        
        return students;
    }
    
    // 保存学生列表到文件
    public void saveStudents(List<Student> students) {
        try (PrintWriter pw = new PrintWriter(new FileWriter(filename))) {
            for (Student student : students) {
                pw.println(student.toString());
            }
            System.out.println("保存成功");
        } catch (IOException e) {
            System.out.println("保存失败: " + e.getMessage());
        }
    }
}

public class StudentFileSystem {
    public static void main(String[] args) {
        StudentFileManager manager = new StudentFileManager("students.txt");
        List<Student> students = manager.loadStudents();
        
        Scanner scanner = new Scanner(System.in);
        
        while (true) {
            System.out.println("\n========== 学生管理系统 ==========");
            System.out.println("1. 添加学生");
            System.out.println("2. 显示所有学生");
            System.out.println("3. 保存到文件");
            System.out.println("0. 退出");
            System.out.print("请选择: ");
            
            int choice = scanner.nextInt();
            
            if (choice == 0) {
                manager.saveStudents(students);  // 退出前保存
                break;
            }
            
            switch (choice) {
                case 1:
                    System.out.print("输入学生信息（ID 姓名 年龄 分数）: ");
                    int id = scanner.nextInt();
                    String name = scanner.next();
                    int age = scanner.nextInt();
                    double score = scanner.nextDouble();
                    students.add(new Student(id, name, age, score));
                    System.out.println("添加成功");
                    break;
                    
                case 2:
                    System.out.println("所有学生:");
                    for (Student s : students) {
                        System.out.println(s);
                    }
                    break;
                    
                case 3:
                    manager.saveStudents(students);
                    break;
            }
        }
        
        scanner.close();
    }
}
```

## 练习

1. 编写一个程序，统计文本文件中单词的个数
2. 实现一个文件复制程序（支持大文件）
3. 创建一个程序，读取CSV文件并显示数据
4. 实现一个简单的日志记录系统（写入文件）
5. 编写一个程序，将对象数据保存到文件并读取

