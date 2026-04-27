# 1284 Minimum Number of Flips to Convert Binary Matrix to Zero Matrix 转化为全零矩阵的最少反转次数

__Description:__

Given a m x n binary matrix mat. In one step, you can choose one cell and flip it and all the four neighbors of it if they exist (Flip is changing 1 to 0 and 0 to 1). A pair of cells are called neighbors if they share one edge.

Return the minimum number of steps required to convert mat to a zero matrix or -1 if you cannot.

A binary matrix is a matrix with all cells equal to 0 or 1 only.

A zero matrix is a matrix with all cells equal to 0.

__Example:__

Example 1:

![matrix](https://assets.leetcode.com/uploads/2019/11/28/matrix.png)

Input: mat = [[0,0],[0,1]]
Output: 3
Explanation: One possible solution is to flip (1, 0) then (0, 1) and finally (1, 1) as shown.

Example 2:

Input: mat = [[0]]
Output: 0
Explanation: Given matrix is a zero matrix. We do not need to change it.

Example 3:

Input: mat = [[1,0,0],[1,0,0]]
Output: -1
Explanation: Given matrix cannot be a zero matrix.

__Constraints:__

m == mat.length
n == mat[i].length
1 <= m, n <= 3
mat[i][j] is either 0 or 1.

__题目描述：__

给你一个 m x n 的二进制矩阵 mat。每一步，你可以选择一个单元格并将它反转（反转表示 0 变 1 ，1 变 0 ）。如果存在和它相邻的单元格，那么这些相邻的单元格也会被反转。相邻的两个单元格共享同一条边。

请你返回将矩阵 mat 转化为全零矩阵的最少反转次数，如果无法转化为全零矩阵，请返回 -1 。

二进制矩阵 的每一个格子要么是 0 要么是 1 。

全零矩阵 是所有格子都为 0 的矩阵。

__示例：__

示例 1：

![矩阵](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/12/13/matrix.png)

输入：mat = [[0,0],[0,1]]
输出：3
解释：一个可能的解是反转 (1, 0)，然后 (0, 1) ，最后是 (1, 1) 。

示例 2：

输入：mat = [[0]]
输出：0
解释：给出的矩阵是全零矩阵，所以你不需要改变它。

示例 3：

输入：mat = [[1,0,0],[1,0,0]]
输出：-1
解释：该矩阵无法转变成全零矩阵

__提示：__

m == mat.length
n == mat[0].length
1 <= m <= 3
1 <= n <= 3
mat[i][j] 是 0 或 1 。

__思路：__

DFS ➕ 状态压缩
对每一格都有翻转或者不翻转两种选择
遍历所有格子, 每个格子都尝试两种选择, 记录下到达终点时数组全部为 0 的翻转次数
时间复杂度为 O(mn2 ^ mn), 空间复杂度为 O(mn)

__代码__:

__C++__:

```C++
class Solution 
{
private:
    vector<vector<int>> dirs{{0, 0}, {0, 1}, {0, -1}, {1, 0}, {-1, 0}};
    int m, n;
    
    void flip(vector<vector<int>>& mat, int x, int y)
    {
        for (const auto& dir : dirs) if (x + dir.front() > -1 and x + dir.front() < m and y + dir.back() > -1 and y + dir.back() < n) mat[x + dir.front()][y + dir.back()] ^= 1;
    }
    
    int dfs(vector<vector<int>>& mat, int state)
    {
        if (state == m * n) 
        {
            int cur = 0;
            for (const auto& row : mat) cur += accumulate(row.begin(), row.end(), 0);
            return cur ? 0x3f3f3f3f : 0;
        }
        int x = state / n, y = state % n, record = dfs(mat, state + 1);
        flip(mat, x, y);
        record = min(record, dfs(mat, state + 1) + 1);
        flip(mat, x, y);
        return record;
    }
public:
    int minFlips(vector<vector<int>>& mat) 
    {
        m = mat.size();
        n = mat.front().size();
        int result = dfs(mat, 0);
        return result < 0x3f3f3f3f ? result : -1;
    }
};
```

__Java__:

```Java
class Solution {
    private int m, n, dirs[][] = new int[][]{{0, 0}, {0, 1}, {0, -1}, {1, 0}, {-1, 0}};
    public int minFlips(int[][] mat) {
        m = mat.length;
        n = mat[0].length;
        int result = dfs(mat, 0);
        return result < 0x3f3f3f3f ? result : -1;
    }
    
    private void flip(int[][] mat, int x, int y) {
        for (int[] dir : dirs) if (x + dir[0] > -1 && x + dir[0] < m && y + dir[1] > -1 && y + dir[1] < n) mat[x + dir[0]][y + dir[1]] ^= 1;
    }
    
    private int dfs(int[][] mat, int state) {
        if (state == m * n) {
            int cur = 0;
            for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) cur += mat[i][j];
            if (cur == 0) return cur;
            return 0x3f3f3f3f;
        }
        int x = state / n, y = state % n, record = dfs(mat, state + 1);
        flip(mat, x, y);
        record = Math.min(dfs(mat, state + 1) + 1, record);
        flip(mat, x, y);
        return record;
    }
}
```

__Python__:

```Python
class Solution:
    def minFlips(self, mat: List[List[int]]) -> int:
        def flip(x: int, y: int) -> None:
            for dx, dy in d:
                if -1 < x + dx < m and -1 < y + dy < n:
                    mat[x + dx][y + dy] ^= 1
            
        def dfs(state: int) -> int:
            if state == m * n:
                return 0 if not sum(sum(i) for i in mat) else float("inf")
            x, y = state // n, state % n
            record = dfs(state + 1)
            flip(x, y)
            record = min(record, 1 + dfs(state + 1))
            flip(x, y)
            return record

        m, n, d = len(mat), len(mat[0]), [(0, 0), (0, 1), (0, -1), (1, 0), (-1, 0)]
        return result if (result := dfs(0)) < float("inf") else -1
```
