# vector

C风格数组int arr[5];需要固定长度数组，无法做到
int arr[num];
一维数组时还可以使用int *ptr = new int[n];
二维数组时却不能int **ptr = new int[n][y];

使用vector现代化一点解决数组问题吧以后

## 语法

### 访问

``` CPP
//声明初始化
std::vector<int>(column);

```
这一部分是一个 临时对象（匿名 vector），
表示：

“创建一个长度为 column 的 std::vector<int>，
其中每个元素都被值初始化为 0。”

这源于 std::vector 的构造函数：
``` CPP
explicit vector(size_type count);

```
所以
``` cpp
std::vector<int> v(4);
//等价于
v = [0, 0, 0, 0];
```




访问：

``` CPP
arr.at(2) = 99; // 若越界会抛出异常
arr[i] = 10;

```




### 动态增删元素

| 操作     | C 风格     | vector 风格           |
| ------ | -------- | ------------------- |
| 添加末尾元素 | ❌ 需要重新分配 | `arr.push_back(x);` |
| 删除末尾元素 | ❌        | `arr.pop_back();`   |
| 获取当前长度 | ❌ 需要单独变量 | `arr.size();`       |
| 清空     | 手动释放     | `arr.clear();`      |


``` cpp
std::vector<int> nums;
nums.push_back(10);
nums.push_back(20);
nums.push_back(30);

std::cout << "长度: " << nums.size() << std::endl; // 输出 3
```

### 遍历方式

``` cpp
// 传统方式
for (size_t i = 0; i < nums.size(); ++i)
    std::cout << nums[i] << " ";

// C++11 范围 for
for (int x : nums)
    std::cout << x << " ";
```

### 二维vector

#### 说明
🧠 1️⃣ 先看 std::vector<int>(column) 是什么？
``` cpp
std::vector<int>(column)
```

这一部分是一个 临时对象（匿名 vector），
表示：

“创建一个长度为 column 的 std::vector<int>，
其中每个元素都被值初始化为 0。”

这源于 std::vector 的构造函数：
``` cpp
explicit vector(size_type count);
```

文档说明：

Constructs the container with count default-inserted elements.
Each element is value-initialized (for int → 0).

🔹 所以：
``` cpp
std::vector<int> v(4);
```

等价于：
``` cpp
v = [0, 0, 0, 0];
```
🧠 2️⃣ 再看 space.push_back(...)

假设：
``` cpp
std::vector<std::vector<int>> space;
```

那么：
``` cpp
space.push_back(std::vector<int>(column));
```

意思是：

“在 space 的末尾，压入一个新的元素，
这个元素是一个长度为 column、内容全为 0 的 vector<int>。”

#### 示例
``` cpp
int row = 3, col = 4;
std::vector<std::vector<int>> mat(row, std::vector<int>(col, 0));

// 赋值
mat[1][2] = 5;

// 打印
for (const auto& row_vec : mat) {
    for (int v : row_vec)
        std::cout << v << " ";
    std::cout << "\n";
}

//or
int row, column;
    std::cin >> row >> column;
    std::vector<std::vector<int>> squal(row, std::vector<int>(column));

```
- 自动释放，无需 delete。
- 支持不同长度的行（即“非矩形二维数组”）。


``` CPP
std::vector<std::vector<int>> space;
for (int i = 0; i < row; ++i)
    space.push_back(std::vector<int>(column));
//等价于
std::vector<std::vector<int>> space(
    row,                    // count → 创建 row 个元素
    std::vector<int>(column) // value → 每个元素都是长度为 column 的 vector<int>
);

//可以和string搭配使用实现char的二维数组
std::vector<std::string> grid(H);
    for (int i = 0; i < H; ++i) {
        std::cin >> grid[i];    //输入地图 一行一个string 不用知道具体W大小
    }

```
“创建一个名为 space 的二维数组，共有 row 行，
每一行是一个长度为 column 的 std::vector<int>。”










