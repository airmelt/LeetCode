# 2732 Find a Good Subset of the Matrix 找到矩阵中的好子集

__Description:__

You are given a __0-indexed__ `m x n` binary matrix `grid`.

Let us call a non-empty subset of rows good if the sum of each column of the subset is at most half of the length of the subset.

More formally, if the length of the chosen subset of rows is `k`, then the sum of each column should be at most `floor(k / 2)`.

Return an integer array that contains row indices of a good subset sorted in ascending order.

If there are multiple good subsets, you can return any of them. If there are no good subsets, return an empty array.

A __subset__ of rows of the matrix `grid` is any matrix that can be obtained by deleting some (possibly none or all) rows from `grid`.

__Example:__

Example 1:

```text
Input: grid = [[0,1,1,0],[0,0,0,1],[1,1,1,1]]
Output: [0,1]
Explanation: We can choose the 0th and 1st rows to create a good subset of rows.
The length of the chosen subset is 2.
```

- The sum of the 0th column is 0 + 0 = 0, which is at most half of the length of the subset.
- The sum of the 1st column is 1 + 0 = 1, which is at most half of the length of the subset.
- The sum of the 2nd column is 1 + 0 = 1, which is at most half of the length of the subset.
- The sum of the 3rd column is 0 + 1 = 1, which is at most half of the length of the subset.

Example 2:

```text
Input: grid = [[0]]
Output: [0]
Explanation: We can choose the 0th row to create a good subset of rows.
The length of the chosen subset is 1.
```

- The sum of the 0th column is 0, which is at most half of the length of the subset.

Example 3:

```text
Input: grid = [[1,1,1],[1,1,1]]
Output: []
Explanation: It is impossible to choose any subset of rows to create a good subset.
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m <= 10 ^ 4`
- `1 <= n <= 5`
- `grid[i][j]` is either `0` or `1`.

__题目描述:__

给你一个下标从 __0__ 开始大小为 `m x n` 的二进制矩阵 `grid` 。

从原矩阵中选出若干行构成一个行的 非空 子集，如果子集中任何一列的和至多为子集大小的一半，那么我们称这个子集是 好子集。

更正式的，如果选出来的行子集大小（即行的数量）为 k，那么每一列的和至多为 `floor(k / 2)` 。

请你返回一个整数数组，它包含好子集的行下标，请你将其 升序 返回。

如果有多个好子集，你可以返回任意一个。如果没有好子集，请你返回一个空数组。

一个矩阵 `grid` 的行 __子集__ ，是删除 `grid` 中某些（也可能不删除）行后，剩余行构成的元素集合。

__示例:__

示例 1：

```text
输入：grid = [[0,1,1,0],[0,0,0,1],[1,1,1,1]]
输出：[0,1]
解释：我们可以选择第 0 和第 1 行构成一个好子集。
选出来的子集大小为 2 。
```

- 第 0 列的和为 0 + 0 = 0 ，小于等于子集大小的一半。
- 第 1 列的和为 1 + 0 = 1 ，小于等于子集大小的一半。
- 第 2 列的和为 1 + 0 = 1 ，小于等于子集大小的一半。
- 第 3 列的和为 0 + 1 = 1 ，小于等于子集大小的一半。

示例 2：

```text
输入：grid = [[0]]
输出：[0]
解释：我们可以选择第 0 行构成一个好子集。
选出来的子集大小为 1 。
```

- 第 0 列的和为 0 ，小于等于子集大小的一半。

示例 3：

```text
输入：grid = [[1,1,1],[1,1,1]]
输出：[]
解释：没有办法得到一个好子集。
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m <= 10 ^ 4`
- `1 <= n <= 5`
- `grid[i][j]` 要么是 `0` ，要么是 `1` 。

__思路:__

```text
首先如果一行就能成为好子集
那么每个元素都必须是 0, 检查所有行, 如果有全 0 的行直接输出该行
否则如果 2 行 是好子集, 他们都要求每一列不超过 1
那么这两行合成的数与结果一定要是 0
对于 3 行, 每一列之和也不能超过 1, 与 2 行相同, 所以必然可以从里面选出 2 行满足题意
如果超过 4 行, 假设为 k 行, 每一列的 1 的个数不能超过 k / 2, 并且任意两行的与不能是 0
那么总的 1 的个数不能超过 kn / 2
将这些行按 1 的数量从小到大排序
1 最少的行 1 的数量不能超过 n / 2
如果 n 比 4 小, 那么最多只能有 1 个 1
假设就在第一列, 那么由于任意两行与不为 0, 则第一列必须为全 1, 和每列 1 的个数不超过 k / 2 矛盾
如果 n 比 4 大, 那么第一行最多只能有 2 个 1
不妨设前两列为 1, 则对于其他行第一列和第二列至少都需要 1 个 1
总计有 k + 1 个 1
但是每列最多只能有 k / 2 个 1, 两列最多有 k 个 1, 根据抽屉原理, 矛盾
所以实际上只需要考虑 1 行或者 2 行的情况
1. 位运算
先将每一行转化为一个整数
由于 n 最大为 5, 最多只要 2 ^ 5 的数组就能存储下所有的数字出现的位置
如果两个与运算为 0 的位置都有值, 就返回这两个位置
为了快速计算, 可以只计算该行对于 total = (1 << n) - 1 的补集 cur
这两个数及补集的子集可以使得与运算为 0
再利用 sub = (1 - sub) & cur 来计算子集即可
时间复杂度为 O(MN + 3 ^ N), 空间复杂度为 O(2 ^ N)
2. DP
设 dp[i] 为 max(dp[j]), 其中 j & i = j
初始化 dp[i] -1 为 i 表示 i 未出现过, 或者为方法一计算的位置
对 j 属于 [0, n - 1], 如果 i >> j & 1 != 0 说明 j 不在之前计算过的子集
取 dp[i] = max(dp[i], dp[i ^ (1 << j)])
如果 dp[i] 为 -1, 说明 i 的所有子集都没出现过直接跳过
否则对于 dp[total ^ i] != -1, 即补集存在, 直接返回 i 和 total ^ i
时间复杂度为 O(MN + N * 2 ^ N), 空间复杂度为 O(2 ^ N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> goodSubsetofBinaryMatrix(vector<vector<int>>& grid) 
    {
        int m = grid.size(), n = grid.front().size(), total = (1 << n) - 1;
        vector<int> dp(total + 1, -1);
        for (int i = 0, mask = 0; i < m; i++, mask = 0)
        {
            for (int j = 0; j < n; j++) mask |= grid[i][j] << j;
            if (!mask) return { i };
            dp[mask] = i;
        }
        for (int i = 1; i < total; i++) for (int j = 0; j < n; j++) if (i >> j & 1) if ((dp[i] = max(dp[i], dp[i ^ (1 << j)])) > -1) if (dp[total ^ i] > -1) return { min(dp[i], dp[total ^ i]), max(dp[i], dp[total ^ i]) };
        return {};
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> goodSubsetofBinaryMatrix(int[][] grid) {
        int m = grid.length, n = grid[0].length, d[] = new int[1 << n], total = (1 << n) - 1, sub = 0, cur = 0;
        Arrays.fill(d, -1);
        for (int i = 0, mask = 0; i < m; i++, mask = 0) {
            for (int j = 0; j < n; j++) mask |= grid[i][j] << j;
            if (mask == 0) return List.of(i);
            if (d[mask] == -1) {
                sub = cur = total ^ mask;
                while (sub > 0) {
                    if (d[sub] > -1) return List.of(Math.min(i, d[sub]), Math.max(i, d[sub]));
                    sub = (sub - 1) & cur;
                }
                d[mask] = i;
            }
        }
        return List.of();
    }
}
```

__Python__:

```Python
class Solution:
    def goodSubsetofBinaryMatrix(self, grid: List[List[int]]) -> List[int]:
        return [d[0]] if 0 in (d := dict((int(''.join(map(str, row)), 2), i) for i, row in enumerate(grid))) else c[0] if (c := [sorted((i, j)) for x, i in d.items() for y, j in d.items() if not x & y]) else c
```
