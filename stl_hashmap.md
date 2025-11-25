# `std::unordered_map`（哈希表）


`unordered_map` 是 C++ STL 的哈希表，特点是：

* ​**O(1) 平均时间复杂度**​：插入、查找、删除都是常数时间
* **键值对（key-value）结构**
* **键不允许重复**
* 内部使用 **hash** 存储（无序排列）

👉 适用于：统计、快速查找、计数器、映射表等。

---

# 1. 基础定义与初始化

```cpp
#include <unordered_map>

std::unordered_map<std::string, int> score;
```

也可以初始化：

```cpp
std::unordered_map<char, int> count = {
    {'a', 1},
    {'b', 2}
};
```


# 2. 插入（insert / operator[] / emplace）

### ① 用 `[]` 插入（最常用）

```cpp
score["Alice"] = 90;
score["Bob"] = 85;
```

* 如果 key 不存在，会创建一个新元素
* 如果 key 已存在，会覆盖值
* 如果用在映射到数字时，也会自动初始化为 0

### ② `insert`（不会覆盖已有 key）

```cpp
score.insert({"Tom", 70});
```

### ③ `emplace`（原地构造）

```cpp
score.emplace("Mike", 88);
```

# 3. 查找（count / find / operator[]）

### ① 判断 key 是否存在

```cpp
if (score.count("Alice")) {
    std::cout << "Alice exists\n";
}
```

### ② 使用 find 获取迭代器

```cpp
auto it = score.find("Bob");
if (it != score.end()) {
    std::cout << it->first << ": " << it->second << "\n";
}
```

### ③ 注意：`operator[]` 会创建不存在的 key

```cpp
int x = score["XYZ"];  // 会自动创建 key "XYZ"，值初始化为 0
```


这是很多新手踩过的坑。

---

# 4. 删除元素

```cpp
score.erase("Bob");
```

根据 key 删除，找不到不会崩溃。

# 5. 遍历（range-based for）

```cpp
for (const auto& p : score) {
    std::cout << p.first << " => " << p.second << "\n";
}
```

因为内部是哈希表，遍历顺序是 **无序** 的。

---

# 6. 实战例子：统计字符频率（最典型用途）

```cpp
std::string s = "abaccdeff";

std::unordered_map<char, int> freq;

for (char c : s) {
    freq[c]++;
}

for (auto &p : freq) {
    std::cout << p.first << ": " << p.second << "\n";
}

```

输出示例：

```shell
a: 2
b: 1
c: 2
d: 1
e: 1
f: 2
```

# 7. unordered\_map vs map（你必须知道的差别）

| 特点      | unordered\_map | map              |
| ----------- | ---------------- | ------------------ |
| 底层结构  | 哈希表         | 红黑树           |
| 查找/插入 | O(1) 平均      | O(logN)          |
| 有序性    | ❌ 无序        | ✔️ 按 key 排序 |
| 适用场景  | 高速查找       | 需要保持顺序     |
| 内存占用  | 较高           | 较低             |

大多数情况你都应该选 `unordered_map`。

---

# 8. nested：二维哈希表

可以建立映射到 vector、map、pair：

```cpp
std::unordered_map<int, std::vector<int>> graph;
graph[1].push_back(2);
graph[1].push_back(3);

```

或：

```cpp
std::unordered_map<std::string, std::pair<int,int>> pos;
pos["start"] = {0, 0};
pos["end"]   = {5, 7};

```

# 9. C 与 C++ 的对比

| C 风格              | C++ unordered\_map |
| --------------------- | -------------------- |
| 没有哈希表这种结构  | 内置，泛型，O(1)   |
| 要自己写链表 + hash | 直接用，安全简单   |
| 容易内存泄露        | 自动管理           |

C++ 的 STL 极大提高安全性与可靠性。

---

# 总结

`unordered_map` 用于：

* ​**计数**​（字符频率、单词计数）
* ​**快速查找**​（ID → 名字）
* ​**记录访问状态**​（visited set）
* **构建邻接表（图论）**
* **实现缓存（cache）**

是竞争编程、项目开发、面试算法中出现频率最高的容器之一。
