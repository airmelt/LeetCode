# 1260 Shift 2D Grid 二维网格迁移

__Description__:
Given a 2D grid of size m x n and an integer k. You need to shift the grid k times.

In one shift operation:

Element at grid[i][j] becomes at grid[i][j + 1].
Element at grid[i][n - 1] becomes at grid[i + 1][0].
Element at grid[n - 1][n - 1] becomes at grid[0][0].
Return the 2D grid after applying shift operation k times.

__Example:__

Example 1:
![Matrix 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/11/16/e1-1.png)
Input: grid = [[1,2,3],[4,5,6],[7,8,9]], k = 1
Output: [[9,1,2],[3,4,5],[6,7,8]]

Example 2:
![Matrix 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/11/16/e2-1.png)
Input: grid = [[3,8,1,9],[19,7,2,5],[4,6,11,10],[12,0,21,13]], k = 4
Output: [[12,0,21,13],[3,8,1,9],[19,7,2,5],[4,6,11,10]]

Example 3:

Input: grid = [[1,2,3],[4,5,6],[7,8,9]], k = 9
Output: [[1,2,3],[4,5,6],[7,8,9]]

__Constraints:__

m == grid.length
n == grid[i].length
1 <= m <= 50
1 <= n <= 50
-1000 <= grid[i][j] <= 1000
0 <= k <= 100

__题目描述__:
给你一个 n 行 m 列的二维网格 grid 和一个整数 k。你需要将 grid 迁移 k 次。

每次「迁移」操作将会引发下述活动：

位于 grid[i][j] 的元素将会移动到 grid[i][j + 1]。
位于 grid[i][m - 1] 的元素将会移动到 grid[i + 1][0]。
位于 grid[n - 1][m - 1] 的元素将会移动到 grid[0][0]。
请你返回 k 次迁移操作后最终得到的 二维网格。

__示例 :__

示例 1：
![矩阵1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/11/16/e1-1.png)
输入：grid = [[1,2,3],[4,5,6],[7,8,9]], k = 1
输出：[[9,1,2],[3,4,5],[6,7,8]]

示例 2：
![矩阵2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/11/16/e2-1.png)
输入：grid = [[3,8,1,9],[19,7,2,5],[4,6,11,10],[12,0,21,13]], k = 4
输出：[[12,0,21,13],[3,8,1,9],[19,7,2,5],[4,6,11,10]]

示例 3：

输入：grid = [[1,2,3],[4,5,6],[7,8,9]], k = 9
输出：[[1,2,3],[4,5,6],[7,8,9]]

__提示：__

1 <= grid.length <= 50
1 <= grid[i].length <= 50
-1000 <= grid[i][j] <= 1000
0 <= k <= 100

__思路__:

1. 展开成 1维数组, 移动之后拼接成 2维数组
时间复杂度O(mn), 空间复杂度O(mn)
2. 计算出结果二维数组的下标, 按下标插入
时间复杂度O(mn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> shiftGrid(vector<vector<int>>& grid, int k) 
    {
        int n = grid.size(), m = grid[0].size();
        vector<vector<int>> result(n, vector<int>(m, 0));
        for (int i = 0; i < n; i++) for (int j = 0; j < m; j++) result[((i * m) + j + k) % (n * m) / m][((i * m) + j + k) % (n * m) % m] = grid[i][j];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<Integer>> shiftGrid(int[][] grid, int k) {
        int n = grid.length, m = grid[0].length;
        int temp[] = new int[m * n];
        List<List<Integer>> result = new ArrayList<>(n);
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                k %= (m * n);
                temp[k++] = grid[i][j];
            }
        }
        k = 0;
        for (int i = 0; i < n; i++) {
            List<Integer> list = new ArrayList<>(m);
            for (int j = 0; j < m; j++) list.add(temp[k++]);
            result.add(list);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def shiftGrid(self, grid: List[List[int]], k: int) -> List[List[int]]:
        n, m = len(grid), len(grid[0])
        temp, result = [0] * (m * n), grid[:]
        for i in range(n):
            for j in range(m):
                k %= m * n
                temp[k] = grid[i][j]
                k += 1
        for i in range(n):
            for j in range(m):
                result[i][j] = temp.pop(0)
        return result
```
