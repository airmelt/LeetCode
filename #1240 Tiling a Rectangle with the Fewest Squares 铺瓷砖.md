# 1240 Tiling a Rectangle with the Fewest Squares 铺瓷砖

__Description:__

Given a rectangle of size n x m, return the minimum number of integer-sided squares that tile the rectangle.

__Example:__

Example 1:

![Tiles 1](https://assets.leetcode.com/uploads/2019/10/17/sample_11_1592.png)

Input: n = 2, m = 3
Output: 3
Explanation: 3 squares are necessary to cover the rectangle.
2 (squares of 1x1)
1 (square of 2x2)

Example 2:

![Tiles 2](https://assets.leetcode.com/uploads/2019/10/17/sample_22_1592.png)

Input: n = 5, m = 8
Output: 5

Example 3:

![Tiles 3](https://assets.leetcode.com/uploads/2019/10/17/sample_33_1592.png)

Input: n = 11, m = 13
Output: 6

__Constraints:__

1 <= n, m <= 13

__题目描述：__

你是一位施工队的工长，根据设计师的要求准备为一套设计风格独特的房子进行室内装修。

房子的客厅大小为 n x m，为保持极简的风格，需要使用尽可能少的 正方形 瓷砖来铺盖地面。

假设正方形瓷砖的规格不限，边长都是整数。

请你帮设计师计算一下，最少需要用到多少块方形瓷砖？

__示例：__

示例 1：

![瓷砖 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/10/25/sample_11_1592.png)

输入：n = 2, m = 3
输出：3
解释：3 块地砖就可以铺满卧室。
     2 块 1x1 地砖
     1 块 2x2 地砖

示例 2：

![瓷砖 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/10/25/sample_22_1592.png)

输入：n = 5, m = 8
输出：5

示例 3：

![瓷砖 3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/10/25/sample_33_1592.png)

输入：n = 11, m = 13
输出：6

__提示：__

1 <= n <= 13
1 <= m <= 13

__思路：__

回溯
本题是一道 NP 问题, 只能使用回溯的方法搜索所有子空间
如果当前搜索值大于结果, 可以进行剪枝加快搜索
否则如果当前已经没有空位, 就更新结果
或者用一个正方形填充再回溯
时间复杂度为 O(2 ^ mn), 空间复杂度为 O(mn)

__代码__:

__C++__:

```C++
class Solution 
{
private:
    int result = 14 * 14;
    
    void dfs(int n, int m, int cur, vector<vector<bool>>& visited)
    {
        if (cur >= result) return;
        bool full = true;
        int row = 0, col = 0;
        for (int i = 0; i < n and full; i++) 
        {
            for (int j = 0; j < m and full; j++) 
            {
                if (!visited[i][j])
                {
                    full = false;
                    row = i;
                    col = j;
                }
            }
        }
        if (full) return (void)(result = min(result, cur));
        for (int len = min(n - row, m - col); len > 0; len--) 
        {
            full = false;
            for (int i = row; i < row + len and !full; i++) for (int j = col; j < col + len and !full; j++) if (visited[i][j] == 1) full = true;
            if (full) continue;
            for (int i = row; i < row + len; i++) for (int j = col; j < col + len; j++) visited[i][j] = 1;
            dfs(n, m, cur + 1, visited);
            for (int i = row; i < row + len; i++) for (int j = col; j < col + len; j++) visited[i][j] = 0;
        }
    }
public:
    int tilingRectangle(int n, int m) 
    {
        vector<vector<bool>> visited(n, vector<bool>(m));
        if (n == m) return 1;
        dfs(n, m, 0, visited);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private int result = 14 * 14;
    
    public int tilingRectangle(int n, int m) {
        int[][] visited = new int[n][m];
        if (n == m) return 1;
        dfs(n, m, 0, visited);
        return result;
    }
    
    private void dfs(int n, int m, int cur, int[][] visited) {
        if (cur >= result) return;
        boolean full = true;
        int row = 0, col = 0;
        for (int i = 0; i < n && full; i++) {
            for (int j = 0; j < m && full; j++) {
                if (visited[i][j] == 0) {
                    full = false;
                    row = i;
                    col = j;
                }
            }
        }
        if (full) {
            result = Math.min(result, cur);
            return;
        }
        for (int len = Math.min(n - row, m - col); len > 0; len--) {
            full = false;
            for (int i = row; i < row + len && !full; i++) for (int j = col; j < col + len && !full; j++) if (visited[i][j] == 1) full = true;
            if (full) continue;
            for (int i = row; i < row + len; i++) for (int j = col; j < col + len; j++) visited[i][j] = 1;
            dfs(n, m, cur + 1, visited);
            for (int i = row; i < row + len; i++) for (int j = col; j < col + len; j++) visited[i][j] = 0;
        }
    }
}
```

__Python__:

```Python
class Solution:
    def tilingRectangle(self, n: int, m: int) -> int:
        if n == m:
            return 1
        visited, result = [[False] * m for _ in range(n)], float('inf')
        
        def dfs(cur: int) -> None:
            nonlocal result
            if cur >= result:
                return
            full = True
            for i in range(n):
                if not full:
                    break
                for j in range(m):
                    if not visited[i][j]:
                        full, row, col = False, i, j
                        break
            if full:
                result = min(result, cur)
                return
            for l in range(min(n - row, m - col), 0, -1):
                full = False
                for i in range(row, row + l):
                    if full:
                        break
                    for j in range(col, col + l):
                        if visited[i][j]:
                            full = True
                            break
                if full:
                    continue
                for i in range(row, row + l):
                    for j in range(col, col + l):
                        visited[i][j] = 1
                dfs(cur + 1)
                for i in range(row, row + l):
                    for j in range(col, col + l):
                        visited[i][j] = 0
        
        dfs(0)
        return result
```
