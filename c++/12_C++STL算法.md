# C++ STL算法

本教程将学习C++标准模板库中的算法，包括排序、查找、变换、数值算法等。

## 1. 迭代器

### 迭代器类型

```cpp
#include <iostream>
#include <vector>
#include <list>
#include <map>
using namespace std;

int main() {
    vector<int> vec{1, 2, 3, 4, 5};
    
    // 输入迭代器
    auto it1 = vec.begin();
    cout << *it1 << endl;
    
    // 前向迭代器
    list<int> lst{10, 20, 30};
    auto it2 = lst.begin();
    ++it2;
    cout << *it2 << endl;
    
    // 双向迭代器
    auto it3 = lst.rbegin();  // 反向迭代器
    cout << *it3 << endl;
    
    // 随机访问迭代器
    auto it4 = vec.begin();
    it4 += 3;
    cout << *it4 << endl;
    
    return 0;
}
```

### 迭代器操作

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> vec{1, 2, 3, 4, 5};
    
    // 遍历
    for (auto it = vec.begin(); it != vec.end(); ++it) {
        cout << *it << " ";
    }
    cout << endl;
    
    // 反向遍历
    for (auto it = vec.rbegin(); it != vec.rend(); ++it) {
        cout << *it << " ";
    }
    cout << endl;
    
    // 距离
    auto dist = distance(vec.begin(), vec.end());
    cout << "距离: " << dist << endl;
    
    // 前进
    auto it = vec.begin();
    advance(it, 3);
    cout << *it << endl;
    
    // 下一元素
    auto next_it = next(vec.begin(), 2);
    cout << *next_it << endl;
    
    // 前一元素
    auto prev_it = prev(vec.end(), 2);
    cout << *prev_it << endl;
    
    return 0;
}
```

## 2. 查找算法

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <list>
using namespace std;

int main() {
    vector<int> vec{1, 2, 3, 4, 5, 3, 7, 3};
    
    // find: 查找第一个匹配的元素
    auto it1 = find(vec.begin(), vec.end(), 3);
    if (it1 != vec.end()) {
        cout << "找到3，位置: " << distance(vec.begin(), it1) << endl;
    }
    
    // find_if: 使用谓词查找
    auto it2 = find_if(vec.begin(), vec.end(), 
                       [](int x) { return x > 5; });
    if (it2 != vec.end()) {
        cout << "找到大于5的元素: " << *it2 << endl;
    }
    
    // count: 计数
    int cnt = count(vec.begin(), vec.end(), 3);
    cout << "3的个数: " << cnt << endl;
    
    // count_if: 使用谓词计数
    int cnt2 = count_if(vec.begin(), vec.end(),
                        [](int x) { return x % 2 == 0; });
    cout << "偶数的个数: " << cnt2 << endl;
    
    // binary_search: 二分查找（需要有序）
    vector<int> sorted{1, 2, 3, 4, 5, 6, 7, 8};
    bool found = binary_search(sorted.begin(), sorted.end(), 5);
    cout << "找到5: " << found << endl;
    
    // lower_bound: 第一个不小于值的元素
    auto it3 = lower_bound(sorted.begin(), sorted.end(), 5);
    cout << "lower_bound(5): " << *it3 << endl;
    
    // upper_bound: 第一个大于值的元素
    auto it4 = upper_bound(sorted.begin(), sorted.end(), 5);
    cout << "upper_bound(5): " << *it4 << endl;
    
    return 0;
}
```

## 3. 排序算法

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> vec{5, 2, 8, 1, 9, 3};
    
    // sort: 排序
    sort(vec.begin(), vec.end());
    for (int x : vec) {
        cout << x << " ";
    }
    cout << endl;
    
    // 自定义排序
    sort(vec.begin(), vec.end(), greater<int>());
    for (int x : vec) {
        cout << x << " ";
    }
    cout << endl;
    
    // 使用Lambda
    sort(vec.begin(), vec.end(), 
         [](int a, int b) { return a < b; });
    
    // partial_sort: 部分排序
    vector<int> vec2{5, 2, 8, 1, 9, 3, 7, 4, 6};
    partial_sort(vec2.begin(), vec2.begin() + 3, vec2.end());
    cout << "前3个最小的: ";
    for (int i = 0; i < 3; i++) {
        cout << vec2[i] << " ";
    }
    cout << endl;
    
    // nth_element: 找到第n个元素
    vector<int> vec3{5, 2, 8, 1, 9, 3};
    nth_element(vec3.begin(), vec3.begin() + 2, vec3.end());
    cout << "第3小的元素: " << vec3[2] << endl;
    
    // is_sorted: 检查是否已排序
    bool sorted = is_sorted(vec.begin(), vec.end());
    cout << "是否已排序: " << sorted << endl;
    
    return 0;
}
```

## 4. 变换算法

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <numeric>
using namespace std;

int main() {
    vector<int> vec{1, 2, 3, 4, 5};
    vector<int> result;
    
    // transform: 变换每个元素
    transform(vec.begin(), vec.end(), back_inserter(result),
              [](int x) { return x * 2; });
    
    for (int x : result) {
        cout << x << " ";
    }
    cout << endl;
    
    // transform: 二元操作
    vector<int> vec1{1, 2, 3};
    vector<int> vec2{4, 5, 6};
    vector<int> sum;
    
    transform(vec1.begin(), vec1.end(), vec2.begin(),
              back_inserter(sum), plus<int>());
    
    for (int x : sum) {
        cout << x << " ";
    }
    cout << endl;
    
    // for_each: 对每个元素执行操作
    for_each(vec.begin(), vec.end(),
             [](int& x) { x *= 10; });
    
    for (int x : vec) {
        cout << x << " ";
    }
    cout << endl;
    
    return 0;
}
```

## 5. 移除算法

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> vec{1, 2, 3, 2, 4, 2, 5};
    
    // remove: 移除元素（不改变大小）
    auto new_end = remove(vec.begin(), vec.end(), 2);
    vec.erase(new_end, vec.end());  // 实际删除
    
    for (int x : vec) {
        cout << x << " ";
    }
    cout << endl;
    
    // remove_if: 使用谓词移除
    vector<int> vec2{1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    auto new_end2 = remove_if(vec2.begin(), vec2.end(),
                              [](int x) { return x % 2 == 0; });
    vec2.erase(new_end2, vec2.end());
    
    for (int x : vec2) {
        cout << x << " ";
    }
    cout << endl;
    
    // unique: 移除相邻重复元素
    vector<int> vec3{1, 1, 2, 2, 3, 3, 4, 4};
    auto unique_end = unique(vec3.begin(), vec3.end());
    vec3.erase(unique_end, vec3.end());
    
    for (int x : vec3) {
        cout << x << " ";
    }
    cout << endl;
    
    return 0;
}
```

## 6. 数值算法

```cpp
#include <iostream>
#include <vector>
#include <numeric>
#include <algorithm>
using namespace std;

int main() {
    vector<int> vec{1, 2, 3, 4, 5};
    
    // accumulate: 累加
    int sum = accumulate(vec.begin(), vec.end(), 0);
    cout << "和: " << sum << endl;
    
    // 自定义累加操作
    int product = accumulate(vec.begin(), vec.end(), 1,
                            multiplies<int>());
    cout << "乘积: " << product << endl;
    
    // partial_sum: 部分和
    vector<int> prefix_sum;
    partial_sum(vec.begin(), vec.end(), back_inserter(prefix_sum));
    for (int x : prefix_sum) {
        cout << x << " ";
    }
    cout << endl;
    
    // adjacent_difference: 相邻差
    vector<int> diff;
    adjacent_difference(vec.begin(), vec.end(), back_inserter(diff));
    for (int x : diff) {
        cout << x << " ";
    }
    cout << endl;
    
    // inner_product: 内积
    vector<int> vec1{1, 2, 3};
    vector<int> vec2{4, 5, 6};
    int dot = inner_product(vec1.begin(), vec1.end(), vec2.begin(), 0);
    cout << "内积: " << dot << endl;
    
    return 0;
}
```

## 7. 集合算法

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <iterator>
using namespace std;

int main() {
    vector<int> vec1{1, 2, 3, 4, 5};
    vector<int> vec2{3, 4, 5, 6, 7};
    vector<int> result;
    
    // set_union: 并集
    set_union(vec1.begin(), vec1.end(),
              vec2.begin(), vec2.end(),
              back_inserter(result));
    cout << "并集: ";
    for (int x : result) {
        cout << x << " ";
    }
    cout << endl;
    
    result.clear();
    
    // set_intersection: 交集
    set_intersection(vec1.begin(), vec1.end(),
                     vec2.begin(), vec2.end(),
                     back_inserter(result));
    cout << "交集: ";
    for (int x : result) {
        cout << x << " ";
    }
    cout << endl;
    
    result.clear();
    
    // set_difference: 差集
    set_difference(vec1.begin(), vec1.end(),
                   vec2.begin(), vec2.end(),
                   back_inserter(result));
    cout << "差集: ";
    for (int x : result) {
        cout << x << " ";
    }
    cout << endl;
    
    return 0;
}
```

## 8. 比较算法

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> vec1{1, 2, 3, 4, 5};
    vector<int> vec2{1, 2, 3, 4, 5};
    vector<int> vec3{1, 2, 3, 4, 6};
    
    // equal: 判断是否相等
    bool eq1 = equal(vec1.begin(), vec1.end(), vec2.begin());
    cout << "vec1 == vec2: " << eq1 << endl;
    
    bool eq2 = equal(vec1.begin(), vec1.end(), vec3.begin());
    cout << "vec1 == vec3: " << eq2 << endl;
    
    // lexicographical_compare: 字典序比较
    bool lex = lexicographical_compare(vec1.begin(), vec1.end(),
                                       vec3.begin(), vec3.end());
    cout << "vec1 < vec3: " << lex << endl;
    
    // mismatch: 找出第一个不匹配的位置
    auto pair = mismatch(vec1.begin(), vec1.end(), vec3.begin());
    if (pair.first != vec1.end()) {
        cout << "第一个不匹配: " << *pair.first 
             << " vs " << *pair.second << endl;
    }
    
    return 0;
}
```

## 9. 排列算法

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> vec{1, 2, 3};
    
    // next_permutation: 下一个排列
    cout << "所有排列:" << endl;
    do {
        for (int x : vec) {
            cout << x << " ";
        }
        cout << endl;
    } while (next_permutation(vec.begin(), vec.end()));
    
    // prev_permutation: 上一个排列
    vector<int> vec2{3, 2, 1};
    do {
        for (int x : vec2) {
            cout << x << " ";
        }
        cout << endl;
    } while (prev_permutation(vec2.begin(), vec2.end()));
    
    return 0;
}
```

## 10. 综合示例

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <numeric>
#include <string>
using namespace std;

// 使用STL算法处理数据
void processData() {
    vector<int> numbers{5, 2, 8, 1, 9, 3, 7, 4, 6, 2};
    
    // 排序
    sort(numbers.begin(), numbers.end());
    cout << "排序后: ";
    for (int x : numbers) {
        cout << x << " ";
    }
    cout << endl;
    
    // 去重
    numbers.erase(unique(numbers.begin(), numbers.end()), numbers.end());
    cout << "去重后: ";
    for (int x : numbers) {
        cout << x << " ";
    }
    cout << endl;
    
    // 统计
    int sum = accumulate(numbers.begin(), numbers.end(), 0);
    double avg = static_cast<double>(sum) / numbers.size();
    cout << "平均值: " << avg << endl;
    
    // 查找
    auto max_it = max_element(numbers.begin(), numbers.end());
    auto min_it = min_element(numbers.begin(), numbers.end());
    cout << "最大值: " << *max_it << endl;
    cout << "最小值: " << *min_it << endl;
    
    // 变换
    vector<int> doubled;
    transform(numbers.begin(), numbers.end(), back_inserter(doubled),
              [](int x) { return x * 2; });
    cout << "翻倍后: ";
    for (int x : doubled) {
        cout << x << " ";
    }
    cout << endl;
}

int main() {
    processData();
    return 0;
}
```

## 练习

1. 实现一个函数，统计容器中满足条件的元素个数
2. 使用STL算法实现一个去重函数
3. 实现一个函数，查找容器中的第k大元素
4. 使用STL算法实现一个排序去重的功能
5. 比较不同查找算法的性能

