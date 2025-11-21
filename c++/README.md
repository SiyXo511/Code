# C++学习教程

本目录包含系统化的C++学习教程，从基础到进阶，帮助你掌握C++编程。

## 教程列表

### 基础篇

1. **01_C++基础语法.md**
   - 第一个C++程序
   - 变量和数据类型
   - 常量（const、constexpr）
   - 输入输出（cin、cout）
   - 运算符
   - 类型转换
   - 字符串（string类）
   - 注释

2. **02_C++控制流.md**
   - if条件语句
   - switch语句
   - for循环
   - while和do-while循环
   - break和continue
   - 范围for循环（C++11）
   - 嵌套循环
   - 综合示例

3. **03_C++函数.md**
   - 函数定义和调用
   - 参数传递（值、指针、引用）
   - 默认参数
   - 函数重载
   - 内联函数
   - 递归函数
   - 函数指针
   - Lambda表达式（C++11）
   - 可变参数函数

4. **04_C++数组和字符串.md**
   - 一维数组和二维数组
   - vector容器
   - C风格字符串
   - string类
   - 数组和函数
   - 多维vector
   - 综合示例

### 进阶篇

5. **05_C++指针和引用.md**
   - 指针基础
   - 指针和数组
   - 指针和函数
   - 引用基础
   - 指针和引用的区别
   - 智能指针（unique_ptr、shared_ptr、weak_ptr）

6. **06_C++面向对象编程.md**
   - 类和对象
   - 构造函数和析构函数
   - 成员函数
   - 访问控制（public、private、protected）
   - this指针
   - 静态成员
   - 友元函数和友元类

7. **07_C++继承和多态.md**
   - 继承基础
   - 访问继承
   - 虚函数和多态
   - 抽象类
   - 虚析构函数
   - 多重继承
   - 菱形继承问题

8. **08_C++运算符重载.md**
   - 运算符重载基础
   - 常用运算符重载
   - 友元运算符
   - 输入输出运算符重载
   - 下标运算符重载
   - 函数调用运算符重载

### 高级篇

9. **09_C++模板编程.md**
   - 函数模板
   - 类模板
   - 模板特化
   - 模板参数
   - 可变参数模板（C++11）
   - 模板元编程

10. **10_C++异常处理.md**
    - 异常处理基础
    - try-catch块
    - 抛出异常
    - 标准异常类
    - 自定义异常
    - 异常安全

11. **11_C++STL容器.md**
    - vector、list、deque、forward_list、array
    - set、map、multiset、multimap
    - unordered_set、unordered_map
    - stack、queue、priority_queue
    - 容器选择指南

12. **12_C++STL算法.md**
    - 迭代器类型和操作
    - 查找算法（find、binary_search等）
    - 排序算法（sort、partial_sort等）
    - 变换算法（transform、for_each）
    - 移除算法（remove、unique）
    - 数值算法（accumulate、partial_sum）
    - 集合算法（set_union、set_intersection）
    - 比较算法（equal、mismatch）
    - 排列算法（next_permutation）

13. **13_C++文件操作.md**（待创建）
    - 文件流（fstream）
    - 文件读写
    - 文件定位
    - 二进制文件
    - 文件操作示例

14. **14_C++现代特性（C++11）.md**（待创建）
    - auto关键字
    - 范围for循环
    - nullptr
    - 右值引用和移动语义
    - Lambda表达式
    - 智能指针

14. **14_C++现代特性（C++11）.md**
    - auto关键字
    - nullptr
    - 范围for循环
    - 初始化列表
    - 右值引用和移动语义
    - Lambda表达式
    - 智能指针

15. **15_C++现代特性（C++14_17_20）.md**
    - C++14特性（泛型Lambda、变量模板、make_unique）
    - C++17特性（结构化绑定、if constexpr、fold表达式、optional、variant、filesystem）
    - C++20特性（concepts、ranges、协程）

16. **16_C++多线程编程.md**
    - 线程创建和管理
    - 互斥量和锁（mutex、lock_guard、unique_lock）
    - 条件变量
    - 原子操作
    - async和future
    - promise和future
    - 综合示例

### 应用篇

17. **17_C++设计模式.md**
    - 单例模式（线程安全版本）
    - 工厂模式（简单工厂、抽象工厂）
    - 观察者模式
    - 策略模式
    - 适配器模式
    - 装饰器模式
    - 命令模式

18. **18_C++实战项目.md**
    - 学生管理系统（完整实现）
    - 图书馆管理系统（简化版）
    - 项目结构组织
    - CMake构建系统
    - 综合项目示例

## 学习方法

1. **按顺序学习**：建议按照编号顺序学习，每个教程都是在前面的基础上构建的
2. **动手实践**：每个教程都包含大量代码示例，建议自己编写和运行这些代码
3. **完成练习**：每个教程末尾都有练习题，完成它们可以加深理解
4. **阅读源码**：理解示例代码的逻辑，尝试修改和扩展

## 编译和运行

### 使用G++编译器

```bash
# 编译
g++ program.cpp -o program

# 运行
./program
```

### 使用C++标准

```bash
# 使用C++11标准
g++ -std=c++11 program.cpp -o program

# 使用C++14标准
g++ -std=c++14 program.cpp -o program

# 使用C++17标准
g++ -std=c++17 program.cpp -o program

# 使用C++20标准
g++ -std=c++20 program.cpp -o program
```

## 常用编译选项

- `-std=c++11/14/17/20`: 指定C++标准版本
- `-Wall`: 显示所有警告
- `-g`: 包含调试信息
- `-O2`: 优化级别2
- `-o`: 指定输出文件名
- `-I`: 指定头文件搜索路径
- `-L`: 指定库文件搜索路径
- `-l`: 链接库

## 开发工具推荐

### 编译器
- **GCC/G++** (GNU Compiler Collection) - Linux/Mac/Windows
- **Clang/Clang++** - 跨平台编译器
- **MSVC** - Microsoft Visual C++（Windows）
- **MinGW** - Windows下的GCC

### IDE/编辑器
- **Visual Studio** - Windows上功能强大的IDE
- **Visual Studio Code** - 轻量级编辑器
- **Code::Blocks** - 跨平台IDE
- **CLion** - JetBrains的C/C++ IDE
- **Qt Creator** - Qt框架的IDE

### 调试工具
- **GDB** - GNU调试器
- **Valgrind** - 内存检查工具（Linux）
- **AddressSanitizer** - 地址检查器

## 推荐资源

- [C++标准文档](https://www.iso.org/standard/79358.html)
- [cppreference.com](https://en.cppreference.com/) - C++参考手册
- [GCC文档](https://gcc.gnu.org/onlinedocs/)
- [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)

## 学习路径

```
基础语法 → 控制流 → 函数 → 数组和字符串
    ↓
指针 → 引用 → 面向对象编程 → 模板
    ↓
STL → 异常处理 → 文件操作 → 高级特性
```

## 注意事项

1. **内存管理**：现代C++推荐使用智能指针，避免手动管理内存
2. **RAII原则**：资源获取即初始化，利用构造函数和析构函数管理资源
3. **使用现代C++**：尽量使用C++11/14/17/20的新特性
4. **代码规范**：遵循C++编码规范，保持代码清晰易读
5. **避免C风格**：尽量使用C++的标准库而不是C风格代码

## 常见问题

### Q: C++和C有什么区别？
A: 
- C++是C的超集，增加了面向对象、模板、异常处理等特性
- C++有更丰富的标准库（STL）
- C++支持函数重载、命名空间等特性
- C++有更严格的类型检查

### Q: 如何选择C++标准版本？
A: 
- C++11是基础，大部分现代特性都在C++11中
- C++14和C++17增加了更多便利特性
- C++20引入了很多新特性（concepts、ranges等）
- 根据项目需求选择合适的标准

### Q: 什么时候使用指针，什么时候使用引用？
A: 
- 引用必须初始化，不能重新绑定，不能为NULL
- 指针可以指向NULL，可以重新赋值
- 函数参数优先使用引用（避免复制）
- 需要表示"可选"时使用指针

祝你学习愉快！

