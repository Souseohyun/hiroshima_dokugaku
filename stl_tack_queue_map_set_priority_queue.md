## queue

一种常见的先进先出的数据结构，常用于：

> 走迷宫，最短路径，BFS等



需要注意的可能是：
STL 的设计理念是：

> “如果你想取值，就先访问；如果你想删除，就单独删除。”

因为：

<pre class="overflow-visible!" data-start="280" data-end="328"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-cpp"><span><span>auto</span><span> x = q.</span><span>pop</span><span>();  </span><span>// ❌ 错误！pop()没有返回值</span><span>
</span></span></code></div></div></pre>

会带来临时对象复制和不必要的复杂性，所以 C++ STL 明确规定：

> `pop()` 只做删除动作，不返回被删除的元素。

### 实际示例：

```cpp
std::queue<int> q;
q.push(10);
q.push(20);

int x = q.front();  // x = 10
q.pop();            // 删除10

std::cout << x << "\n";   // 输出10
std::cout << q.front() << "\n"; // 输出20
```

！！在调用 `front()` 或 `pop()` 前，最好先判断是否为空：

```cpp
if (!q.empty()) {
    auto val = q.front();
    q.pop();
}
```




### 小结表：

| 操作            | 返回值   | 功能         |
| ----------------- | ---------- | -------------- |
| `q.push(x)` | 无       | 入队尾       |
| `q.front()` | 元素引用 | 查看队首元素 |
| `q.back()`  | 元素引用 | 查看队尾元素 |
| `q.pop()`   | 无       | 删除队首元素 |
| `q.empty()` | bool     | 判断是否为空 |
| `q.size()`  | size\_t  | 当前元素数   |

---

## stack

先进后出的数据结构，常用于

> 括号匹配，DFS，函数调用栈

### 基础操作

```CPP
std::stack<int> st;
st.push(10);       // 压入栈顶
st.push(20);
st.top();          // 查看栈顶（返回引用）
st.pop();          // 弹出栈顶（无返回值）
st.empty();        // 是否为空
st.size();         // 元素个数
```

### 代码示例

```cpp
std::string s = "([]{})";
std::stack<char> st;
for (char c : s) {
    if (c=='(' || c=='[' || c=='{') st.push(c);
    else {
        if (st.empty()) return false;
        st.pop();
    }
}
```

## map

键值映射，key-value
常用：

> 词频统计，查表关系

### 基础操作

```cpp
std::map<std::string, int> mp;
mp["apple"] = 5;
mp["banana"] = 3;

std::cout << mp["apple"];     // 5
mp.erase("banana");
```

## set

用于快速查找是否存在某个值。

> 去重、记录访问状态。

### 基础操作

```cpp
std::set<int> s;
s.insert(5);
s.insert(1);
s.insert(5);  // 自动忽略重复
s.count(1);   // 是否存在 → 1
s.erase(5);
```

## priority_queue

优先队列（堆），自动按优先级排序取最大（或最小）元素。

> Dijkstra、A\*、调度算法。

### 基础操作

```cpp
std::priority_queue<int> pq;  // 默认大顶堆
pq.push(3);
pq.push(10);
pq.push(5);
std::cout << pq.top(); // 10
pq.pop();              // 删除最大值

//最小堆
std::priority_queue<int, std::vector<int>, std::greater<int>> min_pq;
```
