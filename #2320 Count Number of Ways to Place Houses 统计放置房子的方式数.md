# 2320 Count Number of Ways to Place Houses 统计放置房子的方式数

__Description:__

There is a street with `n * 2` __plots__, where there are `n` plots on each side of the street. The plots on each side are numbered from `1` to `n`. On each plot, a house can be placed.

Return _the number of ways houses can be placed such that no two houses are adjacent to each other on the same side of the street_. Since the answer may be very large, return it __modulo__ `10 ^ 9 + 7`.

Note that if a house is placed on the `i ^ th` plot on one side of the street, a house can also be placed on the `i ^ th` plot on the other side of the street.

__Example:__

Example 1:

```text
Input: n = 1
Output: 4
Explanation: 
Possible arrangements:
1. All plots are empty.
2. A house is placed on one side of the street.
3. A house is placed on the other side of the street.
4. Two houses are placed, one on each side of the street.
```

Example 2:

![2320-1](https://assets.leetcode.com/uploads/2022/05/12/arrangements.png)

```text
Input: n = 2
Output: 9
Explanation: The 9 possible arrangements are shown in the diagram above.
```

__Constraints:__

- `1 <= n <= 10 ^ 4`

__题目描述:__

一条街道上共有 `n * 2` 个 __地块__ ，街道的两侧各有 `n` 个地块。每一边的地块都按从 `1` 到 `n` 编号。每个地块上都可以放置一所房子。

现要求街道同一侧不能存在两所房子相邻的情况，请你计算并返回放置房屋的方式数目。由于答案可能很大，需要对 `10 ^ 9 + 7` 取余后再返回。

注意，如果一所房子放置在这条街某一侧上的第 `i` 个地块，不影响在另一侧的第 `i` 个地块放置房子。

__示例:__

示例 1：

```text
输入：n = 1
输出：4
解释：
可能的放置方式：
1. 所有地块都不放置房子。
2. 一所房子放在街道的某一侧。
3. 一所房子放在街道的另一侧。
4. 放置两所房子，街道两侧各放置一所。
```

示例 2：

![2320-2](https://assets.leetcode.com/uploads/2022/05/12/arrangements.png)

```text
输入：n = 2
输出：9
解释：如上图所示，共有 9 种可能的放置方式。
```

__提示：__

- `1 <= n <= 10 ^ 4`

__思路:__

```text
动态规划
先考虑同一侧的情况
设 dp[i] 表示放置 i 个地块的方式数目
dp[1] = 2, 因为可以放置或不放置
dp[2] = 3, 因为可以放置在 0, 1 或者不放置
dp[i] = dp[i - 1] + dp[i - 2], i >= 3
因为如果第 i 块地块放置房子，则第 i - 1 块地块不能放置房子, 再加上第 i - 2 块地块放置房子的方式数目即可
对于两侧是对称的, 所以最终的方式数目为 dp[n] * dp[n]
由于 dp[i] 只与 dp[i - 1] 和 dp[i - 2] 有关, 所以可以使用滚动数组进行优化
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countHousePlacements(int n) 
    {
        long long a = 1LL, b = 1LL, t = 0LL, MOD = 1e9 + 7;
        for (int i = 0; i < n; i++) 
        {
            t = a + b;
            b = a;
            a = t % MOD;
        }
        return a * a % MOD;
    }
};
```

__Java__:

```Java
class Solution {
    public int countHousePlacements(int n) {
        long a = 1L, b = 1L, t = 0L, MOD = 1_000_000_007L;
        for (int i = 0; i < n; i++) {
            t = a + b;
            b = a;
            a = t % MOD;
        }
        return (int)(a * a % MOD);
    }
}
```

__Python__:

```Python
class Solution:
    def countHousePlacements(self, n: int, a: int=2, b: int=1) -> int:
        return a ** 2 % (10 ** 9 + 7) if n == 1 else self.countHousePlacements(n - 1, a + b, a)
```
