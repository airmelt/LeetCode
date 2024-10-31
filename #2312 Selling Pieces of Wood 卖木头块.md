# 2312 Selling Pieces of Wood 卖木头块

__Description:__

You are given two integers `m` and `n` that represent the height and width of a rectangular piece of wood. You are also given a 2D integer array `prices`, where `prices[i] = [hi, wi, price_i]` indicates you can sell a rectangular piece of wood of height `hi` and width `wi` for `price_i` dollars.

To cut a piece of wood, you must make a vertical or horizontal cut across the __entire__ height or width of the piece to split it into two smaller pieces. After cutting a piece of wood into some number of smaller pieces, you can sell pieces according to `prices`. You may sell multiple pieces of the same shape, and you do not have to sell all the shapes. The grain of the wood makes a difference, so you __cannot__ rotate a piece to swap its height and width.

Return _the __maximum__ money you can earn after cutting an_ `m x n` _piece of wood_.

Note that you can cut the piece of wood as many times as you want.

__Example:__

Example 1:

![2312-1](https://assets.leetcode.com/uploads/2022/04/27/ex1.png)

```text
Input: m = 3, n = 5, prices = [[1,4,2],[2,2,7],[2,1,3]]
Output: 19
Explanation: The diagram above shows a possible scenario. It consists of:
```

- 2 pieces of wood shaped 2 x 2, selling for a price of 2 * 7 = 14.
- 1 piece of wood shaped 2 x 1, selling for a price of 1 * 3 = 3.
- 1 piece of wood shaped 1 x 4, selling for a price of 1 * 2 = 2.

This obtains a total of 14 + 3 + 2 = 19 money earned.
It can be shown that 19 is the maximum amount of money that can be earned.

Example 2:

![2312-2](https://assets.leetcode.com/uploads/2022/04/27/ex2new.png)

```text
Input: m = 4, n = 6, prices = [[3,2,10],[1,4,2],[4,1,3]]
Output: 32
Explanation: The diagram above shows a possible scenario. It consists of:
```

- 3 pieces of wood shaped 3 x 2, selling for a price of 3 * 10 = 30.
- 1 piece of wood shaped 1 x 4, selling for a price of 1 * 2 = 2.

This obtains a total of 30 + 2 = 32 money earned.
It can be shown that 32 is the maximum amount of money that can be earned.
Notice that we cannot rotate the 1 x 4 piece of wood to obtain a 4 x 1 piece of wood.

__Constraints:__

- `1 <= m, n <= 200`
- `1 <= prices.length <= 2 * 10 ^ 4`
- `prices[i].length == 3`
- `1 <= hi <= m`
- `1 <= wi <= n`
- `1 <= price_i <= 10 ^ 6`
- All the shapes of wood `(hi, wi)` are pairwise __distinct__.

__题目描述:__

给你两个整数 `m` 和 `n` ，分别表示一块矩形木块的高和宽。同时给你一个二维整数数组 `prices` ，其中 `prices[i] = [hi, wi, price_i]` 表示你可以以 `price_i` 元的价格卖一块高为 `hi` 宽为 `wi` 的矩形木块。

每一次操作中，你必须按下述方式之一执行切割操作，以得到两块更小的矩形木块：

- 沿垂直方向按高度 __完全__ 切割木块，或
- 沿水平方向按宽度 __完全__ 切割木块

在将一块木块切成若干小木块后，你可以根据 `prices` 卖木块。你可以卖多块同样尺寸的木块。你不需要将所有小木块都卖出去。你 __不能__ 旋转切好后木块来交换它的高度值和宽度值。

请你返回切割一块大小为 `m x n` 的木块后，能得到的 __最多__ 钱数。

注意你可以切割木块任意次。

__示例:__

示例 1：

![2312-3](https://assets.leetcode.com/uploads/2022/04/27/ex1.png)

```text
输入：m = 3, n = 5, prices = [[1,4,2],[2,2,7],[2,1,3]]
输出：19
解释：上图展示了一个可行的方案。包括：
- 2 块 2 x 2 的小木块，售出 2 * 7 = 14 元。
- 1 块 2 x 1 的小木块，售出 1 * 3 = 3 元。
- 1 块 1 x 4 的小木块，售出 1 * 2 = 2 元。
总共售出 14 + 3 + 2 = 19 元。
19 元是最多能得到的钱数。
```

示例 2：

![2312-4](https://assets.leetcode.com/uploads/2022/04/27/ex2new.png)

```text
输入：m = 4, n = 6, prices = [[3,2,10],[1,4,2],[4,1,3]]
输出：32
解释：上图展示了一个可行的方案。包括：
- 3 块 3 x 2 的小木块，售出 3 * 10 = 30 元。
- 1 块 1 x 4 的小木块，售出 1 * 2 = 2 元。
总共售出 30 + 2 = 32 元。
32 元是最多能得到的钱数。
注意我们不能旋转 1 x 4 的木块来得到 4 x 1 的木块。
```

__提示：__

- `1 <= m, n <= 200`
- `1 <= prices.length <= 2 * 10 ^ 4`
- `prices[i].length == 3`
- `1 <= hi <= m`
- `1 <= wi <= n`
- `1 <= price_i <= 10 ^ 6`
- 所有 `(hi, wi)` __互不相同__ 。

__思路:__

```text
动态规划
dp[i][j] 表示切割到 grid[i][j] 的木块能得到的最多钱数
先把 prices 中的数据填入 dp 中作为初始值
dp[i][j] = max(dp[i][j], dp[i][k] + dp[i][j - k], dp[k][j] + dp[i - k][j]), 其中 1 <= k < j 或 1 <= k < i
要么横着切, 要么竖着切
当超过一半的时候, 切的方式与另一半是对称的
所以只用考虑前一半的切法
可以优化一半的计算
时间复杂度为 O(MN(M + N)), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long sellingWood(int m, int n, vector<vector<int>>& prices) 
    {
        vector<vector<long long>> dp(m + 1, vector<long long>(n + 1));
        for (const auto& price : prices) dp[price[0]][price[1]] = price[2];
        for (int i = 1; i <= m; i++) 
        {
            for (int j = 1; j <= n; j++) 
            {
                for (int k = 1; k <= i >> 1; k++) dp[i][j] = max(dp[k][j] + dp[i - k][j], dp[i][j]);
                for (int k = 1; k <= j >> 1; k++) dp[i][j] = max(dp[i][k] + dp[i][j - k], dp[i][j]);
            }
        }
        return dp.back().back();
    }
};
```

__Java__:

```Java
class Solution {
    public long sellingWood(int m, int n, int[][] prices) {
        long[][] dp = new long[m + 1][n + 1];
        for (int[] price : prices) dp[price[0]][price[1]] = price[2];
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                for (int k = 1; k < i; k++) dp[i][j] = Math.max(dp[k][j] + dp[i - k][j], dp[i][j]);
                for (int k = 1; k < j; k++) dp[i][j] = Math.max(dp[i][k] + dp[i][j - k], dp[i][j]);
            }
        }
        return dp[m][n];
    }
}
```

__Python__:

```Python
class Solution:
    def sellingWood(self, m: int, n: int, prices: List[List[int]]) -> int:
        dp = [[0] * (n + 1) for _ in range(m + 1)]
        for h, w, p in prices:
            dp[h][w] = p
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                dp[i][j] = max(dp[i][j], max((dp[i][k] + dp[i][j - k] for k in range(1, j // 2 + 1)), default=0), max((dp[k][j] + dp[i - k][j] for k in range(1, i // 2 + 1)), default=0))
        return dp[m][n]
```
