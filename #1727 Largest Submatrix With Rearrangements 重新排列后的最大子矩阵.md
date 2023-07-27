# 1727 Largest Submatrix With Rearrangements 重新排列后的最大子矩阵

__Description:__

You are given a binary matrix `matrix` of size `m x n`, and you are allowed to rearrange the __columns__ of the `matrix` in any order.

Return _the area of the largest submatrix within_ `matrix` _where __every__ element of the submatrix is_ `1` _after reordering the columns optimally._

__Example:__

Example 1:

![1727-1](https://assets.leetcode.com/uploads/2020/12/29/screenshot-2020-12-30-at-40536-pm.png)

```text
Input: matrix = [[0,0,1],[1,1,1],[1,0,1]]
Output: 4
Explanation: You can rearrange the columns as shown above.
The largest submatrix of 1s, in bold, has an area of 4.
```

Example 2:

![1727-2](https://assets.leetcode.com/uploads/2020/12/29/screenshot-2020-12-30-at-40852-pm.png)

```text
Input: matrix = [[1,0,1,0,1]]
Output: 3
Explanation: You can rearrange the columns as shown above.
The largest submatrix of 1s, in bold, has an area of 3.
```

Example 3:

```text
Input: matrix = [[1,1,0],[1,0,1]]
Output: 2
Explanation: Notice that you must rearrange entire columns, and there is no way to make a submatrix of 1s larger than an area of 2.
```

__Constraints:__

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m * n <= 10 ^ 5`
- `matrix[i][j]` is either `0` or `1`.

__题目描述:__

给你一个二进制矩阵 `matrix` ，它的大小为 `m x n` ，你可以将 `matrix` 中的 __列__ 按任意顺序重新排列。

请你返回最优方案下将 `matrix` 重新排列后，全是 `1` 的子矩阵面积。

__示例:__

示例 1：

![1727-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/17/screenshot-2020-12-30-at-40536-pm.png)

```text
输入：matrix = [[0,0,1],[1,1,1],[1,0,1]]
输出：4
解释：你可以按照上图方式重新排列矩阵的每一列。
最大的全 1 子矩阵是上图中加粗的部分，面积为 4 。
```

示例 2：

![1727-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/17/screenshot-2020-12-30-at-40852-pm.png)

```text
输入：matrix = [[1,0,1,0,1]]
输出：3
解释：你可以按照上图方式重新排列矩阵的每一列。
最大的全 1 子矩阵是上图中加粗的部分，面积为 3 。
```

示例 3：

```text
输入：matrix = [[1,1,0],[1,0,1]]
输出：2
解释：由于你只能整列整列重新排布，所以没有比面积为 2 更大的全 1 子矩形。
```

示例 4：

```text
输入：matrix = [[0,0],[0,0]]
输出：0
解释：由于矩阵中没有 1 ，没有任何全 1 的子矩阵，所以面积为 0 。
```

__提示：__

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m * n <= 10 ^ 5`
- `matrix[i][j]` 要么是 `0` ，要么是 `1` 。

__思路:__

```text
动态规划
记录每一列出现 1 的次数
设 dp[i][j] 表示 matrix[i][j] 向上出现的连续的 1 的最大长度
dp[i][j] = dp[i - 1][j] + 1 if matrix[i][j] else 0
注意到 dp[i] 只与 dp[i - 1] 有关，可以使用滚动数组优化空间
next[i] = dp[i] + 1 if matrix[i][j] else 0
按每行遍历, 并将次数排序, 则最大面积为 max(next[i] * (n - i))
时间复杂度为 O(MNlogM), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int largestSubmatrix(vector<vector<int>>& matrix) 
    {
        int result = 0, m = matrix.size(), n = matrix.front().size(); 
        vector<int> heights(n);
        for (int i = 0; i < m; i++) 
        {
            for (int j = 0; j < n; j++) heights[j] = !matrix[i][j] ? 0 : heights[j] + 1;
            vector<int> cur(heights);
            sort(cur.begin(), cur.end());
            for (int j = 0; j < n; j++) result = max(result, cur[j] * (n - j));
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int largestSubmatrix(int[][] matrix) {
        int result = 0, m = matrix.length, n = matrix[0].length, heights[] = new int[n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) heights[j] = matrix[i][j] == 0 ? 0 : heights[j] + 1;
            int[] cur = heights.clone();
            Arrays.sort(cur);
            for (int j = 0; j < n; j++) result = Math.max(result, cur[j] * (n - j));
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def largestSubmatrix(self, matrix: List[List[int]]) -> int:
        result, m, heights = 0, len(matrix), [0] * (n := len(matrix[0]))
        for i in range(m):
            for j in range(n):
                heights[j] = heights[j] + 1 if matrix[i][j] else 0
            cur = sorted(heights[:])
            result = max(result, max([cur[j] * (n - j) for j in range(n)]))
        return result
```
