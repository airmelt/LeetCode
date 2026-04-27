# 2965 Find Missing and Repeated Values 找出缺失和重复的数字

__Description:__

You are given a __0-indexed__ 2D integer matrix `grid` of size `n * n` with values in the range `[1, n ^ 2]`. Each integer appears __exactly once__ except `a` which appears __twice__ and `b` which is __missing__. The task is to find the repeating and missing numbers `a` and `b`.

Return _a __0-indexed__ integer array_ `ans` _of size_ `2` _where_ `ans[0]` _equals to_ `a` _and_ `ans[1]` _equals to_ `b`_._

__Example:__

Example 1:

```text
Input: grid = [[1,3],[2,2]]
Output: [2,4]
Explanation: Number 2 is repeated and number 4 is missing so the answer is [2,4].
```

Example 2:

```text
Input: grid = [[9,1,7],[8,9,2],[3,4,6]]
Output: [9,5]
Explanation: Number 9 is repeated and number 5 is missing so the answer is [9,5].
```

__Constraints:__

- `2 <= n == grid.length == grid[i].length <= 50`
- `1 <= grid[i][j] <= n * n`
- For all `x` that `1 <= x <= n * n` there is exactly one `x` that is not equal to any of the grid members.
- For all `x` that `1 <= x <= n * n` there is exactly one `x` that is equal to exactly two of the grid members.
- For all `x` that `1 <= x <= n * n` except two of them there is exactly one pair of `i, j` that `0 <= i, j <= n - 1` and `grid[i][j] == x`.

__题目描述:__

给你一个下标从 __0__ 开始的二维整数矩阵 `grid`，大小为 `n * n` ，其中的值在 `[1, n ^ 2]` 范围内。除了 `a` 出现 __两次__， `b` __缺失__ 之外，每个整数都 __恰好出现一次__ 。

任务是找出重复的数字 `a` 和缺失的数字 `b` 。

返回一个下标从 0 开始、长度为 `2` 的整数数组 `ans` ，其中 `ans[0]` 等于 `a` ， `ans[1]` 等于 `b` 。

__示例:__

示例 1：

```text
输入：grid = [[1,3],[2,2]]
输出：[2,4]
解释：数字 2 重复，数字 4 缺失，所以答案是 [2,4] 。
```

示例 2：

```text
输入：grid = [[9,1,7],[8,9,2],[3,4,6]]
输出：[9,5]
解释：数字 9 重复，数字 5 缺失，所以答案是 [9,5] 。
```

__提示：__

- `2 <= n == grid.length == grid[i].length <= 50`
- `1 <= grid[i][j] <= n * n`
- 对于所有满足 `1 <= x <= n * n` 的 `x` ，恰好存在一个 `x` 与矩阵中的任何成员都不相等。
- 对于所有满足 `1 <= x <= n * n` 的 `x` ，恰好存在一个 `x` 与矩阵中的两个成员相等。
- 除上述的两个之外，对于所有满足 `1 <= x <= n * n` 的 `x` ，都恰好存在一对 `i, j` 满足 `0 <= i, j <= n - 1` 且 `grid[i][j] == x` 。

__思路:__

```text
1. 计数
遍历矩阵记录下每个数字的出现次数
找到出现次数为 0 和 2 的数字
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N ^ 2)
2. 数学
对于 1 - m 的所有数字之和为 m * (m + 1) / 2
对于 1 - m 的所有数字平方之和为 m * (m + 1) * (2 * m + 1) / 6
这里 m 为 n ^ 2
假设重复数字和缺失的数字为 a, b
设矩阵中所有数字之和为 s1, 平方之和为 s2
则 m * (m + 1) / 2 = s1 - a + b
即 a - b = s - m * (m + 1) / 2, 设 s1 - m * (m + 1) / 2 为 d1
则 m * (m + 1) * (2 * m + 1) / 6 = s2 - a ^ 2 + b ^ 2
即 a ^ 2 - b ^ 2 = s2 - m * (m + 1) * (2 * m + 1) / 6, 设 s1 - m * (m + 1) / 2 为 d2
那么 a - b = d1, a ^ 2 - b ^ 2 = d2, 即 (a + b)(a - b) = d2
a - b != 0, 那么 a + b = d2 / (a - b) = d2 / d1
所以 a = (d2 / d1 + d1) / 2, b = (d2 / d1 - d1) / 2
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> findMissingAndRepeatedValues(vector<vector<int>>& grid) 
    {
        int n = grid.size(), m = n * n + 1;
        vector<int> c(m), result(2);
        for (const auto& row : grid) for (const auto& num : row) ++c[num];
        for (int i = 1; i < m; i++) if (c[i] > 1) result.front() = i;
        for (int i = 1; i < m; i++) if (!c[i]) result.back() = i;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] findMissingAndRepeatedValues(int[][] grid) {
        int n = grid.length, m = n * n + 1, count[] = new int[m], result[] = new int[2];
        for (var row : grid) for (int num : row) ++count[num];
        for (int i = 1; i < m; i++) if (count[i] > 1) result[0] = i;
        for (int i = 1; i < m; i++) if (count[i] < 1) result[1] = i;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findMissingAndRepeatedValues(self, grid: List[List[int]]) -> List[int]:
        return [((d2 := sum(x * x for row in grid for x in row) - (m := len(grid) ** 2) * (m + 1) * ((m << 1) + 1) // 6) // ((d1 := sum(x for row in grid for x in row) - (m * (m + 1) >> 1))) + d1) >> 1, (d2 // d1 - d1) >> 1]
```
