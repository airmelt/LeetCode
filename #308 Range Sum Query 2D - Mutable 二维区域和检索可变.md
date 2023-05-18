# 308 Range Sum Query 2D - Mutable 二维区域和检索可变

__Description:__

Given a 2D matrix `matrix`, handle multiple queries of the following types:

Implement the NumMatrix class:

- `NumMatrix(int[][] matrix)` Initializes the object with the integer matrix `matrix`.
- `void update(int row, int col, int val)` __Updates__ the value of `matrix[row][col]` to be `val`.
- `int sumRegion(int row1, int col1, int row2, int col2)` Returns the __sum__ of the elements of `matrix` inside the rectangle defined by its __upper left corner__ `(row1, col1)` and __lower right corner__ `(row2, col2)`.

__Example:__

Example 1:

![308-1](https://assets.leetcode.com/uploads/2021/03/14/summut-grid.jpg)

```text
Input
["NumMatrix", "sumRegion", "update", "sumRegion"]
[[[[3, 0, 1, 4, 2], [5, 6, 3, 2, 1], [1, 2, 0, 1, 5], [4, 1, 0, 1, 7], [1, 0, 3, 0, 5]]], [2, 1, 4, 3], [3, 2, 2], [2, 1, 4, 3]]
Output
[null, 8, null, 10]
```

Explanation

```Java
NumMatrix numMatrix = new NumMatrix([[3, 0, 1, 4, 2], [5, 6, 3, 2, 1], [1, 2, 0, 1, 5], [4, 1, 0, 1, 7], [1, 0, 3, 0, 5]]);
numMatrix.sumRegion(2, 1, 4, 3); // return 8 (i.e. sum of the left red rectangle)
numMatrix.update(3, 2, 2);       // matrix changes from left image to right image
numMatrix.sumRegion(2, 1, 4, 3); // return 10 (i.e. sum of the right red rectangle)
```

__Constraints:__

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 200`
- `-1000 <= matrix[i][j] <= 1000`
- `0 <= row < m`
- `0 <= col < n`
- `-1000 <= val <= 1000`
- `0 <= row1 <= row2 < m`
- `0 <= col1 <= col2 < n`
- At most `5000` calls will be made to `sumRegion` and `update`.

__题目描述:__

给你一个二维矩阵 `matrix` ，处理以下类型的多个查询:

实现 `NumMatrix` 类:

- `NumMatrix(int[][] matrix)` 用整数矩阵 `matrix` 初始化对象。
- `void update(int row, int col, int val)` __更新__ `matrix[row][col]` 的值到 `val` 。
- `int sumRegion(int row1, int col1, int row2, int col2)` 返回矩阵 `matrix` 中指定矩形区域元素的 __和__ ，该区域由 __左上角__ `(row1, col1)` 和 __右下角__ `(row2, col2)` 界定。

__示例:__

示例 1：

![308-2](https://assets.leetcode.com/uploads/2021/03/14/summut-grid.jpg)

```text
输入
["NumMatrix", "sumRegion", "update", "sumRegion"]
[[[[3, 0, 1, 4, 2], [5, 6, 3, 2, 1], [1, 2, 0, 1, 5], [4, 1, 0, 1, 7], [1, 0, 3, 0, 5]]], [2, 1, 4, 3], [3, 2, 2], [2, 1, 4, 3]]
输出
[null, 8, null, 10]
```

解释

```Java
NumMatrix numMatrix = new NumMatrix([[3, 0, 1, 4, 2], [5, 6, 3, 2, 1], [1, 2, 0, 1, 5], [4, 1, 0, 1, 7], [1, 0, 3, 0, 5]]);
numMatrix.sumRegion(2, 1, 4, 3); // 返回 8 (即, 左侧红色矩形的和)
numMatrix.update(3, 2, 2);       // 矩阵从左图变为右图
numMatrix.sumRegion(2, 1, 4, 3); // 返回 10 (即，右侧红色矩形的和)
```

__提示：__

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 200`
- `-10 ^ 5 <= matrix[i][j] <= 10 ^ 5`
- `0 <= row < m`
- `0 <= col < n`
- `-10 ^ 5 <= val <= 10 ^ 5`
- `0 <= row1 <= row2 < m`
- `0 <= col1 <= col2 < n`
- 最多调用 `10 ^ 4` 次 `sumRegion` 和 `update` 方法

__思路:__

```text
1. 前缀和
对每一行分别求前缀和
每次更新值的时候, 将当前行的前缀和更新
求区域和的时候求出所有前缀和的和
NumMatrix() 时间复杂度为 O(MN), update() 时间复杂度为 O(N), sumRegion() 时间复杂度为 O(M), 空间复杂度为 O(MN)
2. 树状数组
将二维数组平铺成一维数组
其他类似一维数组的树状数组
NumMatrix() 时间复杂度为 O(MN), update() 时间复杂度为 O(logN), sumRegion() 时间复杂度为 O(logN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class BitTree
{
public:
    vector<int> tree;
    int n;
    BitTree() {}

    BitTree(int n)
    {
        tree.resize(n + 1, 0);
        this -> n = n;
    }

    int lowbit(int x)
    {
        return x & (-x);
    }

    void update(int x, int val)
    {
        while(x <= n)
        {
            tree[x] += val;
            x += lowbit(x);
        }
    }

    int query(int x)
    {
        int result = 0;
        while (x > 0)
        {
            result += tree[x];
            x -= lowbit(x);
        }
        return result;
    }
};

class NumMatrix 
{
public:
    vector<vector<int>> matrix;
    int m, n;
    BitTree tree;

    NumMatrix(vector<vector<int>>& matrix) 
    {
        this -> matrix = matrix;
        this -> m = matrix.size();
        this -> n = matrix.front().size();
        int total = m * n;
        this -> tree = BitTree(total);
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) tree.update(i * n + j + 1, matrix[i][j]);
    }
    
    void update(int row, int col, int val) 
    {
        tree.update(row * n + col + 1, val - matrix[row][col]);
        matrix[row][col] = val;
    }
    
    int sumRegion(int row1, int col1, int row2, int col2) 
    {
        int result = 0;
        for (int i = row1; i <= row2; i++) result += tree.query(i * n + col2 + 1) - tree.query(i * n + col1);
        return result;
    }
};

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix* obj = new NumMatrix(matrix);
 * obj->update(row,col,val);
 * int param_2 = obj->sumRegion(row1,col1,row2,col2);
 */
```

__Java__:

```Java
class NumMatrix {
    private int[][] matrix, pre;

    public NumMatrix(int[][] matrix) {
        this.matrix = matrix;
        int m = matrix.length, n = matrix[0].length;
        pre = new int[m][n + 1];
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) pre[i][j + 1] = pre[i][j] + matrix[i][j];
    }
    
    public void update(int row, int col, int val) {
        matrix[row][col] = val;
        for (int j = 0, n = matrix[0].length; j < n; j++) pre[row][j + 1] = pre[row][j] + matrix[row][j];
    }
    
    public int sumRegion(int row1, int col1, int row2, int col2) {
        int result = 0;
        for (int i = row1; i <= row2; i++) result += pre[i][col2 + 1] - pre[i][col1];
        return result;
    }
}

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix obj = new NumMatrix(matrix);
 * obj.update(row,col,val);
 * int param_2 = obj.sumRegion(row1,col1,row2,col2);
 */
```

__Python__:

```Python
class NumMatrix:

    def __init__(self, matrix: List[List[int]]):
        self.matrix = matrix
        self.pre = [list(accumulate([0] + row)) for row in matrix]


    def update(self, row: int, col: int, val: int) -> None:
        self.matrix[row][col] = val
        self.pre[row] = list(accumulate([0] + self.matrix[row]))


    def sumRegion(self, row1: int, col1: int, row2: int, col2: int) -> int:
        return sum(self.pre[r][col2 + 1] - self.pre[r][col1] for r in range(row1, row2 + 1))



# Your NumMatrix object will be instantiated and called as such:
# obj = NumMatrix(matrix)
# obj.update(row,col,val)
# param_2 = obj.sumRegion(row1,col1,row2,col2)
```
