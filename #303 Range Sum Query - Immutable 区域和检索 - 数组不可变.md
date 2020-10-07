__Description__:
Given a 2D matrix matrix, find the sum of the elements inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).

![The above rectangle (with the red border) is defined by (row1, col1) = (2, 1) and (row2, col2) = (4, 3), which contains sum = 8.](https://upload-images.jianshu.io/upload_images/16639143-9a40febb0b8d6666.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

__Example:__
Given matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
sumRegion(1, 1, 2, 2) -> 11
sumRegion(1, 2, 2, 4) -> 12

__Note:__
You may assume that the matrix does not change.
There are many calls to sumRegion function.
You may assume that row1 ≤ row2 and col1 ≤ col2.

__题目描述__:
给定一个二维矩阵，计算其子矩形范围内元素的总和，该子矩阵的左上角为 (row1, col1) ，右下角为 (row2, col2)。

![上图子矩阵左上角 (row1, col1) = (2, 1) ，右下角(row2, col2) = (4, 3)，该子矩形内元素的总和为 8。](https://upload-images.jianshu.io/upload_images/16639143-9a40febb0b8d6666.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

__示例 :__

给定 matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
sumRegion(1, 1, 2, 2) -> 11
sumRegion(1, 2, 2, 4) -> 12

__说明:__

你可以假设矩阵不可变。
会多次调用 sumRegion 方法。
你可以假设 row1 ≤ row2 且 col1 ≤ col2。

__思路__:
动态规划
参考[LeetCode #303 Range Sum Query - Immutable 区域和检索 - 数组不可变](https://www.jianshu.com/p/a20ea63b10d1)
在二维数组, dp[i + 1][j + 1]的前缀和表示为matrix[i][j]左上角元素之和
如
```
  [3, 0, 1]               [3, 3, 4]
  [5, 6, 3]      ->       [8, 14, 18] 
  [1, 2, 0]               [9, 17, 21]
```
dp[i + 1][j + 1] = dp[i][j + 1] + dp[i + 1][j] + matrix[i][j] - dp[i][j]
sumRegion(row1, col1, row2, col2) = dp[row2 + 1][col2 + 1] - dp[row2 + 1][col1] - dp[row1][col2 + 1] + dp[row1][col1]
这个和概率论里的联合分布有点像
时间复杂度O(1), 每次调用 sumRegion(row1, col1, row2, col2), 预处理时间复杂度O(mn), 空间复杂度O(mn)

__代码__:
__C++__:
```C++
class NumMatrix 
{
private:
    vector<vector<int>> dp;
public:
    NumMatrix(vector<vector<int>>& matrix) 
    {
        if (matrix.empty() or matrix[0].empty()) return;
        dp.resize(matrix.size() + 1);
        for (int i = 0; i < matrix.size() + 1; i++) dp[i].resize(matrix[0].size() + 1, 0);
        for (int i = 0; i < matrix.size(); i++) for (int j = 0; j < matrix[0].size(); j++) dp[i + 1][j + 1] = dp[i][j + 1] + dp[i + 1][j] + matrix[i][j] - dp[i][j];
    }
    
    int sumRegion(int row1, int col1, int row2, int col2) 
    {
        return dp[row2 + 1][col2 + 1] - dp[row1][col2 + 1] - dp[row2 + 1][col1] + dp[row1][col1];
    }
};

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix* obj = new NumMatrix(matrix);
 * int param_1 = obj->sumRegion(row1,col1,row2,col2);
 */
```

__Java__:
```Java
class NumMatrix {

    private int[][] dp;

    public NumMatrix(int[][] matrix) {
        if (matrix.length == 0 || matrix[0].length == 0) return;
        dp = new int[matrix.length + 1][matrix[0].length + 1];
        for (int r = 0; r < matrix.length; r++) for (int c = 0; c < matrix[0].length; c++) dp[r + 1][c + 1] = dp[r + 1][c] + dp[r][c + 1] + matrix[r][c] - dp[r][c];
    }

    public int sumRegion(int row1, int col1, int row2, int col2) {
        return dp[row2 + 1][col2 + 1] - dp[row1][col2 + 1] - dp[row2 + 1][col1] + dp[row1][col1];
    }
}

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix obj = new NumMatrix(matrix);
 * int param_1 = obj.sumRegion(row1,col1,row2,col2);
 */
```

__Python__:
```Python
class NumMatrix:

    def __init__(self, matrix: List[List[int]]):
        if not matrix or not matrix[0]:
            return
        self.dp = [[0] * (len(matrix[0]) + 1) for _ in range(len(matrix) + 1)]
        for i in range(len(matrix)):
            for j in range(len(matrix[i])):
                self.dp[i + 1][j + 1] = self.dp[i][j + 1] + self.dp[i + 1][j] + matrix[i][j] - self.dp[i][j]

    def sumRegion(self, row1: int, col1: int, row2: int, col2: int) -> int:
        return self.dp[row2 + 1][col2 + 1] - self.dp[row1][col2 + 1] - self.dp[row2 + 1][col1] + self.dp[row1][col1];


# Your NumMatrix object will be instantiated and called as such:
# obj = NumMatrix(matrix)
# param_1 = obj.sumRegion(row1,col1,row2,col2)
```