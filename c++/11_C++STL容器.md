# C++ STL容器

本教程将学习C++标准模板库（STL）中的容器，包括序列容器、关联容器、无序容器等。

## 1. 序列容器

### vector

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    // 创建vector
    vector<int> vec1;                    // 空vector
    vector<int> vec2(5);                 // 5个元素，值为0
    vector<int> vec3(5, 10);             // 5个元素，值为10
    vector<int> vec4{1, 2, 3, 4, 5};    // 初始化列表
    
    // 添加元素
    vec1.push_back(10);
    vec1.push_back(20);
    vec1.push_back(30);
    
    // 访问元素
    cout << vec1[0] << endl;        // 使用下标（不检查边界）
    cout << vec1.at(1) << endl;     // 使用at（检查边界）
    cout << vec1.front() << endl;   // 第一个元素
    cout << vec1.back() << endl;    // 最后一个元素
    
    // 容量操作
    cout << "大小: " << vec1.size() << endl;
    cout << "容量: " << vec1.capacity() << endl;
    vec1.reserve(100);  // 预留容量
    vec1.shrink_to_fit();  // 收缩容量
    
    // 修改元素
    vec1.insert(vec1.begin() + 1, 15);  // 在位置1插入15
    vec1.erase(vec1.begin());           // 删除第一个元素
    vec1.pop_back();                    // 删除最后一个元素
    
    // 遍历
    for (int x : vec1) {
        cout << x << " ";
    }
    cout << endl;
    
    // 使用迭代器
    for (auto it = vec1.begin(); it != vec1.end(); ++it) {
        cout << *it << " ";
    }
    cout << endl;
    
    return 0;
}
```

### deque

```cpp
#include <iostream>
#include <deque>
using namespace std;

int main() {
    deque<int> dq;
    
    // 两端操作
    dq.push_back(10);
    dq.push_front(5);
    dq.push_back(20);
    dq.push_front(1);
    
    // 访问元素
    cout << "第一个: " << dq.front() << endl;
    cout << "最后一个: " << dq.back() << endl;
    
    // 删除元素
    dq.pop_front();
    dq.pop_back();
    
    // 遍历
    for (int x : dq) {
        cout << x << " ";
    }
    cout << endl;
    
    return 0;
}
```

### list

```cpp
#include <iostream>
#include <list>
using namespace std;

int main() {
    list<int> lst{1, 2, 3, 4, 5};
    
    // 添加元素
    lst.push_back(6);
    lst.push_front(0);
    
    // 插入元素
    auto it = lst.begin();
    advance(it, 3);
    lst.insert(it, 99);
    
    // 删除元素
    lst.remove(3);  // 删除所有值为3的元素
    lst.erase(it);  // 删除迭代器指向的元素
    
    // 排序
    lst.sort();
    
    // 反转
    lst.reverse();
    
    // 合并
    list<int> lst2{10, 20, 30};
    lst.merge(lst2);
    
    // 遍历
    for (int x : lst) {
        cout << x << " ";
    }
    cout << endl;
    
    return 0;
}
```

### forward_list（C++11）

```cpp
#include <iostream>
#include <forward_list>
using namespace std;

int main() {
    forward_list<int> flst{1, 2, 3, 4, 5};
    
    // 只有前向迭代器
    // flst.push_back(6);  // 错误：没有push_back
    
    flst.push_front(0);
    flst.insert_after(flst.begin(), 99);
    
    // 删除
    flst.erase_after(flst.begin());
    
    // 遍历
    for (int x : flst) {
        cout << x << " ";
    }
    cout << endl;
    
    return 0;
}
```

### array（C++11）

```cpp
#include <iostream>
#include <array>
using namespace std;

int main() {
    // C++11: array模板类
    array<int, 5> arr{1, 2, 3, 4, 5};
    
    // 访问元素
    cout << arr[0] << endl;
    cout << arr.at(1) << endl;
    cout << arr.front() << endl;
    cout << arr.back() << endl;
    
    // 填充
    arr.fill(10);
    
    // 大小（编译时常量）
    cout << "大小: " << arr.size() << endl;
    
    // 遍历
    for (int x : arr) {
        cout << x << " ";
    }
    cout << endl;
    
    return 0;
}
```

## 2. 关联容器

### set

```cpp
#include <iostream>
#include <set>
using namespace std;

int main() {
    set<int> s{5, 2, 8, 1, 9, 3};
    
    // 插入元素（自动排序）
    s.insert(4);
    s.insert(7);
    
    // 查找元素
    auto it = s.find(5);
    if (it != s.end()) {
        cout << "找到: " << *it << endl;
    }
    
    // 计数（0或1）
    cout << s.count(5) << endl;
    
    // 删除元素
    s.erase(5);
    
    // 遍历（有序）
    for (int x : s) {
        cout << x << " ";
    }
    cout << endl;
    
    // 范围查找
    auto lower = s.lower_bound(3);
    auto upper = s.upper_bound(7);
    
    cout << "范围[3, 7): ";
    for (auto it = lower; it != upper; ++it) {
        cout << *it << " ";
    }
    cout << endl;
    
    return 0;
}
```

### map

```cpp
#include <iostream>
#include <map>
#include <string>
using namespace std;

int main() {
    map<string, int> scores;
    
    // 插入元素
    scores["Alice"] = 95;
    scores["Bob"] = 87;
    scores["Charlie"] = 92;
    scores.insert({"David", 88});
    
    // 访问元素
    cout << "Alice的分数: " << scores["Alice"] << endl;
    cout << "Bob的分数: " << scores.at("Bob") << endl;
    
    // 查找元素
    auto it = scores.find("Alice");
    if (it != scores.end()) {
        cout << it->first << ": " << it->second << endl;
    }
    
    // 检查键是否存在
    if (scores.count("Eve") == 0) {
        cout << "Eve不在map中" << endl;
    }
    
    // 遍历
    for (const auto& pair : scores) {
        cout << pair.first << ": " << pair.second << endl;
    }
    
    // C++17: 结构化绑定
    for (const auto& [name, score] : scores) {
        cout << name << ": " << score << endl;
    }
    
    return 0;
}
```

### multiset和multimap

```cpp
#include <iostream>
#include <set>
#include <map>
using namespace std;

int main() {
    // multiset: 允许重复元素
    multiset<int> ms{1, 2, 2, 3, 3, 3};
    
    ms.insert(2);
    cout << "2的个数: " << ms.count(2) << endl;
    
    // multimap: 允许重复键
    multimap<string, int> mm;
    mm.insert({"Alice", 95});
    mm.insert({"Alice", 97});
    
    // 查找所有相同键的值
    auto range = mm.equal_range("Alice");
    for (auto it = range.first; it != range.second; ++it) {
        cout << it->first << ": " << it->second << endl;
    }
    
    return 0;
}
```

## 3. 无序容器（C++11）

### unordered_set

```cpp
#include <iostream>
#include <unordered_set>
using namespace std;

int main() {
    unordered_set<int> us{5, 2, 8, 1, 9, 3};
    
    // 插入
    us.insert(4);
    
    // 查找（O(1)平均时间复杂度）
    if (us.find(5) != us.end()) {
        cout << "找到5" << endl;
    }
    
    // 统计
    cout << "5的个数: " << us.count(5) << endl;
    
    // 遍历（无序）
    for (int x : us) {
        cout << x << " ";
    }
    cout << endl;
    
    // 桶信息
    cout << "桶数量: " << us.bucket_count() << endl;
    cout << "负载因子: " << us.load_factor() << endl;
    
    return 0;
}
```

### unordered_map

```cpp
#include <iostream>
#include <unordered_map>
#include <string>
using namespace std;

int main() {
    unordered_map<string, int> umap;
    
    // 插入
    umap["Alice"] = 95;
    umap["Bob"] = 87;
    umap.insert({"Charlie", 92});
    
    // 访问
    cout << "Alice: " << umap["Alice"] << endl;
    
    // 遍历
    for (const auto& [name, score] : umap) {
        cout << name << ": " << score << endl;
    }
    
    return 0;
}
```

## 4. 容器适配器

### stack

```cpp
#include <iostream>
#include <stack>
using namespace std;

int main() {
    stack<int> stk;
    
    // 压栈
    stk.push(10);
    stk.push(20);
    stk.push(30);
    
    // 访问栈顶
    cout << "栈顶: " << stk.top() << endl;
    
    // 出栈
    stk.pop();
    
    // 大小
    cout << "大小: " << stk.size() << endl;
    
    // 检查是否为空
    while (!stk.empty()) {
        cout << stk.top() << " ";
        stk.pop();
    }
    cout << endl;
    
    return 0;
}
```

### queue

```cpp
#include <iostream>
#include <queue>
using namespace std;

int main() {
    queue<int> q;
    
    // 入队
    q.push(10);
    q.push(20);
    q.push(30);
    
    // 访问队首
    cout << "队首: " << q.front() << endl;
    cout << "队尾: " << q.back() << endl;
    
    // 出队
    q.pop();
    
    // 遍历
    while (!q.empty()) {
        cout << q.front() << " ";
        q.pop();
    }
    cout << endl;
    
    return 0;
}
```

### priority_queue

```cpp
#include <iostream>
#include <queue>
#include <vector>
using namespace std;

int main() {
    // 默认最大堆
    priority_queue<int> pq;
    
    pq.push(30);
    pq.push(10);
    pq.push(20);
    
    // 访问最大值
    cout << "最大值: " << pq.top() << endl;
    
    // 出队（删除最大值）
    while (!pq.empty()) {
        cout << pq.top() << " ";
        pq.pop();
    }
    cout << endl;
    
    // 最小堆
    priority_queue<int, vector<int>, greater<int>> min_pq;
    min_pq.push(30);
    min_pq.push(10);
    min_pq.push(20);
    
    cout << "最小值: " << min_pq.top() << endl;
    
    return 0;
}
```

## 5. 容器的选择

```cpp
// 选择容器的指南：

// 1. 需要随机访问？使用vector或deque
vector<int> vec;  // 首选
deque<int> dq;    // 需要两端操作

// 2. 只需要在两端插入/删除？使用deque
deque<int> dq;

// 3. 需要频繁在中间插入/删除？使用list
list<int> lst;

// 4. 需要有序且唯一？使用set
set<int> s;

// 5. 需要键值对且有序？使用map
map<string, int> m;

// 6. 只需要快速查找，不需要有序？使用unordered_set/map
unordered_set<int> us;
unordered_map<string, int> um;

// 7. 需要LIFO？使用stack
stack<int> stk;

// 8. 需要FIFO？使用queue
queue<int> q;

// 9. 需要优先级队列？使用priority_queue
priority_queue<int> pq;
```

## 6. 综合示例

```cpp
#include <iostream>
#include <vector>
#include <map>
#include <set>
#include <algorithm>
using namespace std;

// 学生成绩管理系统
class StudentManager {
private:
    map<int, string> students;  // ID -> Name
    map<int, int> scores;       // ID -> Score
    
public:
    void addStudent(int id, const string& name) {
        students[id] = name;
    }
    
    void setScore(int id, int score) {
        if (students.find(id) != students.end()) {
            scores[id] = score;
        }
    }
    
    void displayRanking() {
        // 按分数排序
        vector<pair<int, int>> ranking;
        for (const auto& [id, score] : scores) {
            ranking.push_back({id, score});
        }
        
        sort(ranking.begin(), ranking.end(),
             [](const pair<int, int>& a, const pair<int, int>& b) {
                 return a.second > b.second;
             });
        
        cout << "排名:" << endl;
        int rank = 1;
        for (const auto& [id, score] : ranking) {
            cout << rank++ << ". " << students[id] 
                 << " (ID: " << id << ") - " << score << "分" << endl;
        }
    }
};

int main() {
    StudentManager manager;
    
    manager.addStudent(1, "Alice");
    manager.addStudent(2, "Bob");
    manager.addStudent(3, "Charlie");
    
    manager.setScore(1, 95);
    manager.setScore(2, 87);
    manager.setScore(3, 92);
    
    manager.displayRanking();
    
    return 0;
}
```

## 练习

1. 使用vector实现一个动态数组类
2. 使用map实现一个字典系统
3. 使用set实现一个去重功能
4. 使用priority_queue实现一个任务调度系统
5. 比较不同容器的性能差异

