# 1504 Count Submatrices With All Ones 统计全1子矩形

__Description:__

Given an `m x n` binary matrix `mat`, _return the number of __submatrices__ that have all ones_.

__Example:__

Example 1:

![1504-1](https://assets.leetcode.com/uploads/2021/10/27/ones1-grid.jpg)

```text
Input: mat = [[1,0,1],[1,1,0],[1,1,0]]
Output: 13
Explanation: 
There are 6 rectangles of side 1x1.
There are 2 rectangles of side 1x2.
There are 3 rectangles of side 2x1.
There is 1 rectangle of side 2x2. 
There is 1 rectangle of side 3x1.
Total number of rectangles = 6 + 2 + 3 + 1 + 1 = 13.
```

Example 2:

![1504-2](https://assets.leetcode.com/uploads/2021/10/27/ones2-grid.jpg)

```text
Input: mat = [[0,1,1,0],[0,1,1,1],[1,1,1,0]]
Output: 24
Explanation: 
There are 8 rectangles of side 1x1.
There are 5 rectangles of side 1x2.
There are 2 rectangles of side 1x3. 
There are 4 rectangles of side 2x1.
There are 2 rectangles of side 2x2. 
There are 2 rectangles of side 3x1. 
There is 1 rectangle of side 3x2. 
Total number of rectangles = 8 + 5 + 2 + 4 + 2 + 2 + 1 = 24.
```

__Constraints:__

- `1 <= m, n <= 150`
- `mat[i][j]` is either `0` or `1`.

__题目描述:__

给你一个 `m x n` 的二进制矩阵 `mat` ，请你返回有多少个 __子矩形__ 的元素全部都是 1 。

__示例:__

示例 1：

![1504-3](https://assets.leetcode.com/uploads/2021/10/27/ones1-grid.jpg)

```text
输入：mat = [[1,0,1],[1,1,0],[1,1,0]]
输出：13
解释：
有 6 个 1x1 的矩形。
有 2 个 1x2 的矩形。
有 3 个 2x1 的矩形。
有 1 个 2x2 的矩形。
有 1 个 3x1 的矩形。
矩形数目总共 = 6 + 2 + 3 + 1 + 1 = 13 。
```

示例 2：

![1504-4](https://assets.leetcode.com/uploads/2021/10/27/ones2-grid.jpg)

```text
输入：mat = [[0,1,1,0],[0,1,1,1],[1,1,1,0]]
输出：24
解释：
有 8 个 1x1 的子矩形。
有 5 个 1x2 的子矩形。
有 2 个 1x3 的子矩形。
有 4 个 2x1 的子矩形。
有 2 个 2x2 的子矩形。
有 2 个 3x1 的子矩形。
有 1 个 3x2 的子矩形。
矩形数目总共 = 8 + 5 + 2 + 4 + 2 + 2 + 1 = 24 。
```

__提示：__

- `1 <= m, n <= 150`
- `mat[i][j]` 仅包含 `0` 或 `1`

__思路:__

```text
1. 动态规划
用一个数组 record 记录到 mat[i][j] 累计的 '1' 的长度为 record[i][j]
则 if mat[i][j] == 0, record[i][j] = 0
mat[i][j] == 1, record[i][j] = record[i][j - 1] + 1
那么对每一个 mat[i][j] == 1, 从 mat[i][j] 出发, 初始高度为 height = 150(m, n 最大为 150), 从 k = i 开始反向遍历, height = min(height, record[k][j]), 一旦 height 减少到 0, 说明已经没有符合题意的全 1 矩形, 可以提前剪枝
结果累加上 height 即可
时间复杂度为 O(M ^ 2N), 空间复杂度为 O(MN)
2. 单调栈优化
注意到每次反向遍历 k 时进行了大量重复的计算
实际上只需要保留一个单调递减的栈
这样就能保证高度始终最小
时间复杂度为 O(MN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numSubmat(vector<vector<int>>& mat) 
    {
        int m = mat.size(), n = mat.front().size(), record[m][n], result = 0;
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) record[i][j] = mat[i][j] ? (1 + (j ? record[i][j - 1] : 0)) : 0;
        for (int j = 0, total = 0; j < n; j++, total = 0) 
        {
            stack<pair<int, int>> s;
            for (int i = 0, height = 1; i < m; i++, height = 1) 
            {
                while (!s.empty() and s.top().first > record[i][j])
                {
                    total -= s.top().second * (s.top().first - record[i][j]);
                    height += s.top().second;
                    s.pop();
                }
                result += (total += record[i][j]);
                s.push(make_pair(record[i][j], height));
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numSubmat(int[][] mat) {
        int result = 0, m = mat.length, n = mat[0].length, record[][] = new int[m][n];
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) record[i][j] = mat[i][j] == 1 ? (1 + (j > 0 ? record[i][j - 1] : 0)) : 0;
        for (int i = 0; i < m; i++) for (int j = 0, height = 150; j < n; j++, height = 150) for (int k = i; k > -1 && height != 0; k--) result += (height = Math.min(height, record[k][j]));
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numSubmat(self, mat: List[List[int]]) -> int:
        m, n, result = len(mat), len(mat[0]), 0
        record = [[0] * n for _ in range(m)]
        for i in range(m):
            for j in range(n):
                record[i][j] = 1 + (record[i][j - 1] if j else 0) if mat[i][j] else 0
        for j in range(n):
            stack, total = deque(), 0
            for i in range(m):
                height = 1
                while stack and stack[-1][0] > record[i][j]:
                    total -= stack[-1][1] * (stack[-1][0] - record[i][j])
                    height += stack.pop()[1]
                total += record[i][j]
                result += total
                stack.append((record[i][j], height))
        return result
```
