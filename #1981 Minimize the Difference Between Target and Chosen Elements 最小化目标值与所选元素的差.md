# 1981 Minimize the Difference Between Target and Chosen Elements 最小化目标值与所选元素的差

__Description:__

You are given an `m x n` integer matrix `mat` and an integer `target`.

Choose one integer from __each row__ in the matrix such that the __absolute difference__ between `target` and the __sum__ of the chosen elements is __minimized__.

Return the minimum absolute difference.

The __absolute difference__ between two numbers `a` and `b` is the absolute value of `a - b`.

__Example:__

Example 1:

![1981-1](https://assets.leetcode.com/uploads/2021/08/03/matrix1.png)

```text
Input: mat = [[1,2,3],[4,5,6],[7,8,9]], target = 13
Output: 0
Explanation: One possible choice is to:
```

- Choose 1 from the first row.
- Choose 5 from the second row.
- Choose 7 from the third row.
The sum of the chosen elements is 13, which equals the target, so the absolute difference is 0.

Example 2:

![1981-2](https://assets.leetcode.com/uploads/2021/08/03/matrix1-1.png)

```text
Input: mat = [[1],[2],[3]], target = 100
Output: 94
Explanation: The best possible choice is to:
```

- Choose 1 from the first row.
- Choose 2 from the second row.
- Choose 3 from the third row.
The sum of the chosen elements is 6, and the absolute difference is 94.

Example 3:

![1981-3](https://assets.leetcode.com/uploads/2021/08/03/matrix1-3.png)

```text
Input: mat = [[1,2,9,8,7]], target = 6
Output: 1
Explanation: The best choice is to choose 7 from the first row.
The absolute difference is 1.
```

__Constraints:__

- `m == mat.length`
- `n == mat[i].length`
- `1 <= m, n <= 70`
- `1 <= mat[i][j] <= 70`
- `1 <= target <= 800`

__题目描述:__

给你一个大小为 `m x n` 的整数矩阵 `mat` 和一个整数 `target` 。

从矩阵的 __每一行__ 中选择一个整数，你的目标是 __最小化__ 所有选中元素之 __和__ 与目标值 `target` 的 __绝对差__ 。

返回 最小的绝对差 。

`a` 和 `b` 两数字的 __绝对差__ 是 `a - b` 的绝对值。

__示例:__

示例 1：

![1981-4](https://assets.leetcode.com/uploads/2021/08/03/matrix1.png)

```text
输入：mat = [[1,2,3],[4,5,6],[7,8,9]], target = 13
输出：0
解释：一种可能的最优选择方案是：
```

- 第一行选出 1
- 第二行选出 5
- 第三行选出 7
所选元素的和是 13 ，等于目标值，所以绝对差是 0 。

示例 2：

![1981-5](https://assets.leetcode.com/uploads/2021/08/03/matrix1-1.png)

```text
输入：mat = [[1],[2],[3]], target = 100
输出：94
解释：唯一一种选择方案是：
```

- 第一行选出 1
- 第二行选出 2
- 第三行选出 3
所选元素的和是 6 ，绝对差是 94 。

示例 3：

![1981-6](https://assets.leetcode.com/uploads/2021/08/03/matrix1-3.png)

```text
输入：mat = [[1,2,9,8,7]], target = 6
输出：1
解释：最优的选择方案是选出第一行的 7 。
绝对差是 1 。
```

__提示：__

- `m == mat.length`
- `n == mat[i].length`
- `1 <= m, n <= 70`
- `1 <= mat[i][j] <= 70`
- `1 <= target <= 800`

__思路:__

```text
1. 动态规划
从每一行选出所有值的和, 用一个集合保存
遍历集合再加上当前行的值, 更新集合
时间复杂度为 O(M ^ 2NC), 空间复杂度为 O(MC), 其中 C 为最大值
2. 剪枝
不需要计算大于 target 的值
时间复杂度为 O(MNC), 空间复杂度为 O(C), C 为 target 的值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimizeTheDifference(vector<vector<int>>& mat, int target) 
    {
        int m = mat.size(), n = mat[0].size(), s = 0, result = INT_MAX;
        vector<int> f = {1};
        for (int i = 0; i < m; i++) 
        {
            int best = *max_element(mat[i].begin(), mat[i].end());
            vector<int> g(s + best + 1);
            for (int x: mat[i]) for (int j = x; j <= s + x; ++j) g[j] |= f[j - x];
            f = move(g);
            s += best;
        }
        for (int i = 0; i <= s; i++) if (f[i] and abs(i - target) < result) result = abs(i - target);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimizeTheDifference(int[][] mat, int target) {
        int best = 0, m = mat.length, n = mat[0].length, result = 0x3f3f3f3f, f[][] = new int[m][4910];
        for (int j : mat[0]) f[0][j] = 1;
        for (int i = 1; i < m; i++) for (int j = 0; j < 4910; j++) for (int k : mat[i]) if (j - k >= 0 && f[i - 1][j - k] > 0) f[i][j] = f[i - 1][j - k];
        for (int i = 0; i < 4910; i++) if (f[m - 1][i] > 0) result = Math.min(Math.abs(i - target), result);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimizeTheDifference(self, mat: List[List[int]], target: int) -> int:
        m, n, f = len(mat), len(mat[0]), {0}
        for i in range(m):
            g = set()
            for x in mat[i]:
                for j in f:
                    g.add(j + x)
            f = g
        return min(abs(x - target) for x in f)
```
