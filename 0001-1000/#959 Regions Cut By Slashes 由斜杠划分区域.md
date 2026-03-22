# 959 Regions Cut By Slashes 由斜杠划分区域

__Description__:
An n x n grid is composed of 1 x 1 squares where each 1 x 1 square consists of a '/', '\', or blank space ' '. These characters divide the square into contiguous regions.

Given the grid grid represented as a string array, return the number of regions.

Note that backslash characters are escaped, so a '\' is represented as '\\'.

__Example:__

Example 1:

![Regions 1](https://assets.leetcode.com/uploads/2018/12/15/1.png)

Input: grid = [" /","/ "]
Output: 2

Example 2:

![Regions 2](https://assets.leetcode.com/uploads/2018/12/15/2.png)

Input: grid = [" /","  "]
Output: 1

Example 3:

![Regions 3](https://assets.leetcode.com/uploads/2018/12/15/4.png)

Input: grid = ["/\\","\\/"]
Output: 5
Explanation: Recall that because \ characters are escaped, "\\/" refers to \/, and "/\\" refers to /\.

__Constraints:__

n == grid.length == grid[i].length
1 <= n <= 30
grid[i][j] is either '/', '\', or ' '.

__题目描述__:
在由 1 x 1 方格组成的 N x N 网格 grid 中，每个 1 x 1 方块由 /、\ 或空格构成。这些字符会将方块划分为一些共边的区域。

（请注意，反斜杠字符是转义的，因此 \ 用 "\\" 表示。）。

返回区域的数目。

__示例 :__

示例 1：

输入：
[
  " /",
  "/ "
]
输出：2
解释：2x2 网格如下：

![区域 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/15/1.png)

示例 2：

输入：
[
  " /",
  "  "
]
输出：1
解释：2x2 网格如下：

![区域 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/15/2.png)

示例 3：

输入：
[
  "\\/",
  "/\\"
]
输出：4
解释：（回想一下，因为 \ 字符是转义的，所以 "\\/" 表示 \/，而 "/\\" 表示 /\。）
2x2 网格如下：

![区域 3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/15/3.png)

示例 4：

输入：
[
  "/\\",
  "\\/"
]
输出：5
解释：（回想一下，因为 \ 字符是转义的，所以 "/\\" 表示 /\，而 "\\/" 表示 \/。）
2x2 网格如下：

![区域 4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/15/4.png)

示例 5：

输入：
[
  "//",
  "/ "
]
输出：3
解释：2x2 网格如下：

![区域 5](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/15/5.png)

__提示:__

1 <= grid.length == grid[0].length <= 30
grid[i][j] 是 '/'、'\'、或 ' '。

__思路__:

1. 并查集
观察一个 1 X 1 的小方块, 它可以被两个对角线分成 4 个区域
规定北方开始顺时针分别为 0, 1, 2, 3
如果是 '\', 则 01 和 23 分别相连
如果是 '//', 则 03 和 12 分别相连
否则 0123 均可以连在一起
如果小方块不是第一行, 那么可以和上一行的南方(2) 相连
如果小方块不是第一列, 那么可以和上一列的东方(1) 相连
最后返回并查集中的树的数目即可
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n ^ 2)
2. DFS
如果将 1 X 1 的小方块分成 9 格, 那么对角线占据其中 3 格, 而且可以分开两个区域
将这个题目转化成 [LeetCode #200 Number of Islands 岛屿数量](https://www.jianshu.com/p/4756ec92cbf1)
先将对角线标记成 1
遍历不为 1 的方块, 将这个块联通的区域全部标记为 1 并增加一个岛屿数量
岛屿的数量即为区域的数量
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    void dfs(vector<vector<int>>& g, int i, int j, int n)
    {
        if (i > -1 and i < n and j > -1 and j < n and !g[i][j])
        {
            g[i][j] = 1;
            dfs(g, i + 1, j, n);
            dfs(g, i - 1, j, n);
            dfs(g, i, j + 1, n);
            dfs(g, i, j - 1, n);
        }
    }
    
    int regionsBySlashes(vector<string>& grid) 
    {
        int result = 0, n = grid.size(), m = n * 3;
        vector<vector<int>> g(m, vector<int>(m, 0));
        for (int i = 0; i < n; i++)
        {
            for(int j = 0; j < n; j++)
            {
                if (grid[i][j] == '/') g[i * 3 + 2][j * 3] = g[i * 3 + 1][j * 3 + 1] = g[i * 3][j * 3 + 2] = 1;
                else if (grid[i][j] == '\\') g[i * 3][j * 3] = g[i * 3 + 1][j * 3 + 1] = g[i * 3 + 2][j * 3 + 2] = 1;
            }
        }
        for (int i = 0; i < m; i++)
        {
            for (int j = 0; j < m; j++)
            {
                if(!g[i][j])
                {
                    dfs(g, i, j, m);
                    ++result;
                }
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int regionsBySlashes(String[] grid) {
        int result = 0, n = grid.length, m = n * 3, g[][] = new int[m][m];
        for (int i = 0; i < n; i++){
            for(int j = 0; j < n; j++) {
                if (grid[i].charAt(j) == '/') g[i * 3 + 2][j * 3] = g[i * 3 + 1][j * 3 + 1] = g[i * 3][j * 3 + 2] = 1;
                else if (grid[i].charAt(j) == '\\') g[i * 3][j * 3] = g[i * 3 + 1][j * 3 + 1] = g[i * 3 + 2][j * 3 + 2] = 1;
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < m; j++) {
                if(g[i][j] == 0) {
                    dfs(g, i, j, m);
                    ++result;
                }
            }
        }
        return result;
    }
    
    private void dfs(int[][] g, int i, int j, int n) {
        if (i >= 0 && i < n && j >= 0 && j < n && g[i][j] == 0) {
            g[i][j] = 1;
            dfs(g, i + 1, j, n);
            dfs(g, i - 1, j, n);
            dfs(g, i, j + 1, n);
            dfs(g, i, j - 1, n);
        }
    }
}
```

__Python__:

```Python
class UF:
    def __init__(self, n: int) -> None:
        self.parent = [i for i in range(n)]
        self.weight = [1] * n
        self.count = n

    def union(self, p: int, q: int) -> None:
        """
        连接两个点
        :param p: 一个节点
        :param q: 另一个节点
        :return:
        """
        p = self.find(p)
        q = self.find(q)
        if p == q:
            return
        if self.weight[p] > self.weight[q]:
            self.parent[q] = p
            self.weight[p] += self.weight[q]
        else:
            self.parent[p] = q
            self.weight[q] += self.weight[p]
        self.count -= 1

    def connected(self, p: int, q: int) -> bool:
        """
        检查两个点是否在同一分量
        :param p: 一个节点
        :param q: 另一个节点
        :return: 返回两个点是否在同一个分量
        """
        return self.find(p) == self.find(q)

    def find(self, p: int) -> int:
        """
        查找根节点, 并进行路径压缩
        :param p: 一个节点
        :return: 根节点
        """
        while self.parent[p] != p:
            self.parent[p] = self.parent[self.parent[p]]
            p = self.parent[p]
        return p
class Solution:
    def regionsBySlashes(self, grid: List[str]) -> int:
        uf = UF((n := len(grid)) ** 2 << 2)
        for i in range(n):
            for j in range(n):
                start = (i * n + j) << 2
                if (c := grid[i][j]) == ' ':
                    uf.union(start, start + 1)
                    uf.union(start, start + 2)
                    uf.union(start, start + 3)
                elif c == '/':
                    uf.union(start, start + 3)
                    uf.union(start + 1, start + 2)
                elif c == '\\':
                    uf.union(start, start + 1)
                    uf.union(start + 2, start + 3)
                if i:
                    uf.union(start, start + 2 - (n << 2))
                if j:
                    uf.union(start + 3, start - 3)
        return uf.count
```
