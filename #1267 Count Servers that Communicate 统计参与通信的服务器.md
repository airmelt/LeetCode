# 1267 Count Servers that Communicate 统计参与通信的服务器

__Description:__

You are given a map of a server center, represented as a m * n integer matrix grid, where 1 means that on that cell there is a server and 0 means that it is no server. Two servers are said to communicate if they are on the same row or on the same column.

Return the number of servers that communicate with any other server.

__Example:__

Example 1:

![Servers 1](https://assets.leetcode.com/uploads/2019/11/14/untitled-diagram-6.jpg)

Input: grid = [[1,0],[0,1]]
Output: 0
Explanation: No servers can communicate with others.

Example 2:

![Servers 2](https://assets.leetcode.com/uploads/2019/11/13/untitled-diagram-4.jpg)

Input: grid = [[1,0],[1,1]]
Output: 3
Explanation: All three servers can communicate with at least one other server.

Example 3:

![Servers 3](https://assets.leetcode.com/uploads/2019/11/14/untitled-diagram-1-3.jpg)

Input: grid = [[1,1,0,0],[0,0,1,0],[0,0,1,0],[0,0,0,1]]
Output: 4
Explanation: The two servers in the first row can communicate with each other. The two servers in the third column can communicate with each other. The server at right bottom corner can't communicate with any other server.

__Constraints:__

m == grid.length
n == grid[i].length
1 <= m <= 250
1 <= n <= 250
grid[i][j] == 0 or 1

__题目描述：__

这里有一幅服务器分布图，服务器的位置标识在 m * n 的整数矩阵网格 grid 中，1 表示单元格上有服务器，0 表示没有。

如果两台服务器位于同一行或者同一列，我们就认为它们之间可以进行通信。

请你统计并返回能够与至少一台其他服务器进行通信的服务器的数量。

__示例：__

示例 1：

![服务器 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/11/24/untitled-diagram-6.jpg)

输入：grid = [[1,0],[0,1]]
输出：0
解释：没有一台服务器能与其他服务器进行通信。

示例 2：

![服务器 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/11/24/untitled-diagram-4-1.jpg)

输入：grid = [[1,0],[1,1]]
输出：3
解释：所有这些服务器都至少可以与一台别的服务器进行通信。

示例 3：

![服务器 3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/11/24/untitled-diagram-1-3.jpg)

输入：grid = [[1,1,0,0],[0,0,1,0],[0,0,1,0],[0,0,0,1]]
输出：4
解释：第一行的两台服务器互相通信，第三列的两台服务器互相通信，但右下角的服务器无法与其他服务器通信。

__提示：__

m == grid.length
n == grid[i].length
1 <= m <= 250
1 <= n <= 250
grid[i][j] == 0 or 1

__思路：__

模拟
先按行和列统计服务器的数量
然后再查找当前行或列上除了本身还有其他服务器的服务器
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int countServers(vector<vector<int>>& grid) 
    {
        int result = 0, n = grid.size();
        vector<int> row(n), col(n);
        for (int i = 0; i < n; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                if (grid[i][j]) 
                {
                    ++row[i];
                    ++col[j];
                }
            }
        }
        for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) if (grid[i][j] and (row[i] > 1 or col[j] > 1)) ++result;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countServers(int[][] grid) {
        int result = 0, n = grid.length, row[] = new int[n], col[] = new int[n];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] != 0) {
                    ++row[i];
                    ++col[j];
                }
            }
        }
        for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) if (grid[i][j] != 0 && (row[i] > 1 || col[j] > 1)) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
        def countServers(self, grid: List[List[int]]) -> int:
            return sum(grid[i][j] and ((count_row := [sum(row) for row in grid])[i] > 1 or (count_col := [sum(col) for col in zip(*grid)])[j] > 1) for i in range(len(grid)) for j in range(len(grid[0])))
```
