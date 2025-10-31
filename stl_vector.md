# vector

C风格数组int arr[5];需要固定长度数组，无法做到
int arr[num];
一维数组时还可以使用int *ptr = new int[n];
二维数组时却不能int **ptr = new int[n][y];

使用vector现代化一点解决数组问题吧以后

## 语法

### 访问

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









