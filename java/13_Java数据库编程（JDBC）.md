# Java数据库编程（JDBC）

本教程将学习Java中的数据库编程，包括JDBC基础、连接数据库、执行SQL语句等。

## 1. JDBC基础

### 连接数据库

```java
import java.sql.*;

public class JDBCDemo {
    public static void main(String[] args) {
        // 数据库连接信息（MySQL示例）
        String url = "jdbc:mysql://localhost:3306/testdb";
        String username = "root";
        String password = "password";
        
        Connection connection = null;
        
        try {
            // 加载数据库驱动
            Class.forName("com.mysql.cj.jdbc.Driver");
            
            // 建立连接
            connection = DriverManager.getConnection(url, username, password);
            System.out.println("数据库连接成功");
            
            // 执行SQL语句
            Statement statement = connection.createStatement();
            ResultSet resultSet = statement.executeQuery("SELECT * FROM users");
            
            // 处理结果集
            while (resultSet.next()) {
                int id = resultSet.getInt("id");
                String name = resultSet.getString("name");
                int age = resultSet.getInt("age");
                System.out.println("ID: " + id + ", 姓名: " + name + ", 年龄: " + age);
            }
            
            // 关闭资源
            resultSet.close();
            statement.close();
        } catch (ClassNotFoundException e) {
            System.out.println("驱动未找到: " + e.getMessage());
        } catch (SQLException e) {
            System.out.println("数据库错误: " + e.getMessage());
        } finally {
            // 关闭连接
            if (connection != null) {
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

### 使用try-with-resources

```java
import java.sql.*;

public class JDBCWithResources {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/testdb";
        String username = "root";
        String password = "password";
        
        try (Connection connection = DriverManager.getConnection(url, username, password);
             Statement statement = connection.createStatement();
             ResultSet resultSet = statement.executeQuery("SELECT * FROM users")) {
            
            System.out.println("数据库连接成功");
            
            while (resultSet.next()) {
                System.out.println("ID: " + resultSet.getInt("id") 
                                 + ", 姓名: " + resultSet.getString("name"));
            }
        } catch (SQLException e) {
            System.out.println("数据库错误: " + e.getMessage());
        }
        // 资源自动关闭
    }
}
```

## 2. 执行SQL语句

### INSERT操作

```java
import java.sql.*;

public class InsertDemo {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/testdb";
        String username = "root";
        String password = "password";
        
        try (Connection connection = DriverManager.getConnection(url, username, password);
             Statement statement = connection.createStatement()) {
            
            // 插入数据
            String sql = "INSERT INTO users (name, age, email) VALUES "
                       + "('张三', 25, 'zhangsan@example.com')";
            
            int rowsAffected = statement.executeUpdate(sql);
            System.out.println("插入 " + rowsAffected + " 行");
            
            // 获取生成的ID（自增主键）
            ResultSet generatedKeys = statement.getGeneratedKeys();
            if (generatedKeys.next()) {
                int id = generatedKeys.getInt(1);
                System.out.println("生成的ID: " + id);
            }
            
        } catch (SQLException e) {
            System.out.println("数据库错误: " + e.getMessage());
        }
    }
}
```

### UPDATE和DELETE操作

```java
import java.sql.*;

public class UpdateDeleteDemo {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/testdb";
        String username = "root";
        String password = "password";
        
        try (Connection connection = DriverManager.getConnection(url, username, password);
             Statement statement = connection.createStatement()) {
            
            // 更新数据
            String updateSql = "UPDATE users SET age = 30 WHERE name = '张三'";
            int rowsUpdated = statement.executeUpdate(updateSql);
            System.out.println("更新 " + rowsUpdated + " 行");
            
            // 删除数据
            String deleteSql = "DELETE FROM users WHERE age < 20";
            int rowsDeleted = statement.executeUpdate(deleteSql);
            System.out.println("删除 " + rowsDeleted + " 行");
            
        } catch (SQLException e) {
            System.out.println("数据库错误: " + e.getMessage());
        }
    }
}
```

## 3. PreparedStatement

### 使用PreparedStatement（推荐）

```java
import java.sql.*;

public class PreparedStatementDemo {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/testdb";
        String username = "root";
        String password = "password";
        
        try (Connection connection = DriverManager.getConnection(url, username, password)) {
            
            // 插入数据（使用PreparedStatement，防止SQL注入）
            String insertSql = "INSERT INTO users (name, age, email) VALUES (?, ?, ?)";
            try (PreparedStatement pstmt = connection.prepareStatement(insertSql)) {
                pstmt.setString(1, "李四");
                pstmt.setInt(2, 30);
                pstmt.setString(3, "lisi@example.com");
                
                int rowsAffected = pstmt.executeUpdate();
                System.out.println("插入 " + rowsAffected + " 行");
            }
            
            // 查询数据
            String selectSql = "SELECT * FROM users WHERE age > ?";
            try (PreparedStatement pstmt = connection.prepareStatement(selectSql)) {
                pstmt.setInt(1, 25);
                
                ResultSet rs = pstmt.executeQuery();
                while (rs.next()) {
                    System.out.println("ID: " + rs.getInt("id") 
                                     + ", 姓名: " + rs.getString("name")
                                     + ", 年龄: " + rs.getInt("age"));
                }
            }
            
        } catch (SQLException e) {
            System.out.println("数据库错误: " + e.getMessage());
        }
    }
}
```

## 4. 事务处理

```java
import java.sql.*;

public class TransactionDemo {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/testdb";
        String username = "root";
        String password = "password";
        
        Connection connection = null;
        
        try {
            connection = DriverManager.getConnection(url, username, password);
            connection.setAutoCommit(false);  // 关闭自动提交
            
            try (PreparedStatement pstmt1 = connection.prepareStatement(
                    "UPDATE accounts SET balance = balance - ? WHERE id = ?");
                 PreparedStatement pstmt2 = connection.prepareStatement(
                    "UPDATE accounts SET balance = balance + ? WHERE id = ?")) {
                
                // 转账：从账户1转100到账户2
                pstmt1.setDouble(1, 100.0);
                pstmt1.setInt(2, 1);
                pstmt1.executeUpdate();
                
                pstmt2.setDouble(1, 100.0);
                pstmt2.setInt(2, 2);
                pstmt2.executeUpdate();
                
                // 提交事务
                connection.commit();
                System.out.println("转账成功");
            } catch (SQLException e) {
                // 回滚事务
                connection.rollback();
                System.out.println("转账失败，已回滚: " + e.getMessage());
            }
        } catch (SQLException e) {
            System.out.println("数据库错误: " + e.getMessage());
        } finally {
            if (connection != null) {
                try {
                    connection.setAutoCommit(true);
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

## 5. 连接池（HikariCP示例）

```java
import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;
import java.sql.*;

public class ConnectionPoolDemo {
    public static void main(String[] args) {
        // 配置连接池
        HikariConfig config = new HikariConfig();
        config.setJdbcUrl("jdbc:mysql://localhost:3306/testdb");
        config.setUsername("root");
        config.setPassword("password");
        config.setMaximumPoolSize(10);
        config.setMinimumIdle(5);
        
        // 创建数据源
        HikariDataSource dataSource = new HikariDataSource(config);
        
        try (Connection connection = dataSource.getConnection()) {
            System.out.println("从连接池获取连接成功");
            
            // 使用连接
            Statement statement = connection.createStatement();
            ResultSet rs = statement.executeQuery("SELECT COUNT(*) FROM users");
            
            if (rs.next()) {
                System.out.println("用户总数: " + rs.getInt(1));
            }
            
            rs.close();
            statement.close();
        } catch (SQLException e) {
            System.out.println("数据库错误: " + e.getMessage());
        } finally {
            dataSource.close();
        }
    }
}
```

## 6. 综合示例

### 学生管理系统（JDBC版本）

```java
import java.sql.*;
import java.util.ArrayList;
import java.util.List;

class Student {
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
    public String getName() { return name; }
    public int getAge() { return age; }
    public double getScore() { return score; }
    
    @Override
    public String toString() {
        return String.format("ID: %d, 姓名: %s, 年龄: %d, 分数: %.2f", 
                           id, name, age, score);
    }
}

class StudentDAO {
    private Connection connection;
    
    public StudentDAO(Connection connection) {
        this.connection = connection;
    }
    
    // 添加学生
    public void addStudent(Student student) throws SQLException {
        String sql = "INSERT INTO students (name, age, score) VALUES (?, ?, ?)";
        try (PreparedStatement pstmt = connection.prepareStatement(sql)) {
            pstmt.setString(1, student.getName());
            pstmt.setInt(2, student.getAge());
            pstmt.setDouble(3, student.getScore());
            pstmt.executeUpdate();
        }
    }
    
    // 查询所有学生
    public List<Student> getAllStudents() throws SQLException {
        List<Student> students = new ArrayList<>();
        String sql = "SELECT * FROM students";
        
        try (Statement stmt = connection.createStatement();
             ResultSet rs = stmt.executeQuery(sql)) {
            
            while (rs.next()) {
                students.add(new Student(
                    rs.getInt("id"),
                    rs.getString("name"),
                    rs.getInt("age"),
                    rs.getDouble("score")
                ));
            }
        }
        
        return students;
    }
    
    // 根据ID查询学生
    public Student getStudentById(int id) throws SQLException {
        String sql = "SELECT * FROM students WHERE id = ?";
        
        try (PreparedStatement pstmt = connection.prepareStatement(sql)) {
            pstmt.setInt(1, id);
            
            try (ResultSet rs = pstmt.executeQuery()) {
                if (rs.next()) {
                    return new Student(
                        rs.getInt("id"),
                        rs.getString("name"),
                        rs.getInt("age"),
                        rs.getDouble("score")
                    );
                }
            }
        }
        
        return null;
    }
    
    // 更新学生信息
    public void updateStudent(Student student) throws SQLException {
        String sql = "UPDATE students SET name = ?, age = ?, score = ? WHERE id = ?";
        
        try (PreparedStatement pstmt = connection.prepareStatement(sql)) {
            pstmt.setString(1, student.getName());
            pstmt.setInt(2, student.getAge());
            pstmt.setDouble(3, student.getScore());
            pstmt.setInt(4, student.getId());
            pstmt.executeUpdate();
        }
    }
    
    // 删除学生
    public void deleteStudent(int id) throws SQLException {
        String sql = "DELETE FROM students WHERE id = ?";
        
        try (PreparedStatement pstmt = connection.prepareStatement(sql)) {
            pstmt.setInt(1, id);
            pstmt.executeUpdate();
        }
    }
}

public class StudentJDBCSystem {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/testdb";
        String username = "root";
        String password = "password";
        
        try (Connection connection = DriverManager.getConnection(url, username, password)) {
            StudentDAO dao = new StudentDAO(connection);
            
            // 添加学生
            dao.addStudent(new Student(0, "张三", 25, 95.5));
            dao.addStudent(new Student(0, "李四", 30, 87.0));
            
            // 查询所有学生
            List<Student> students = dao.getAllStudents();
            System.out.println("所有学生:");
            for (Student student : students) {
                System.out.println(student);
            }
            
            // 查询单个学生
            Student student = dao.getStudentById(1);
            if (student != null) {
                System.out.println("\n查询结果: " + student);
            }
            
        } catch (SQLException e) {
            System.out.println("数据库错误: " + e.getMessage());
        }
    }
}
```

## 练习

1. 实现一个完整的CRUD操作类
2. 使用连接池管理数据库连接
3. 实现事务处理功能
4. 创建一个数据库操作的DAO模式示例
5. 实现一个简单的ORM框架基础

