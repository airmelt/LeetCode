# 2218 Maximum Value of K Coins From Piles 从栈中取出 K 个硬币的最大面值和

__Description:__

There are `n` __piles__ of coins on a table. Each pile consists of a __positive number__ of coins of assorted denominations.

In one move, you can choose any coin on top of any pile, remove it, and add it to your wallet.

Given a list `piles`, where `piles[i]` is a list of integers denoting the composition of the `i ^ th` pile from __top to bottom__, and a positive integer `k`, return _the __maximum total value__ of coins you can have in your wallet if you choose __exactly___ `k` _coins optimally_.

__Example:__

Example 1:

![2218-1](https://assets.leetcode.com/uploads/2019/11/09/e1.png)

```text
Input: piles = [[1,100,3],[7,8,9]], k = 2
Output: 101
Explanation:
The above diagram shows the different ways we can choose k coins.
The maximum total we can obtain is 101.
```

Example 2:

```text
Input: piles = [[100],[100],[100],[100],[100],[100],[1,1,1,1,1,1,700]], k = 7
Output: 706
Explanation:
The maximum total can be obtained if we choose all coins from the last pile.
```

__Constraints:__

- `n == piles.length`
- `1 <= n <= 1000`
- `1 <= piles[i][j] <= 10 ^ 5`
- `1 <= k <= sum(piles[i].length) <= 2000`

__题目描述:__

一张桌子上总共有 `n` 个硬币 _栈_ 。每个栈有 __正整数__ 个带面值的硬币。

每一次操作中，你可以从任意一个栈的 顶部 取出 1 个硬币，从栈中移除它，并放入你的钱包里。

给你一个列表 `piles` ，其中 `piles[i]` 是一个整数数组，分别表示第 `i` 个栈里 __从顶到底__ 的硬币面值。同时给你一个正整数 `k` ，请你返回在 __恰好__ 进行 `k` 次操作的前提下，你钱包里硬币面值之和 __最大为多少__ 。

__示例:__

示例 1：

![2218-2](https://assets.leetcode.com/uploads/2019/11/09/e1.png)

```text
输入：piles = [[1,100,3],[7,8,9]], k = 2
输出：101
解释：
上图展示了几种选择 k 个硬币的不同方法。
我们可以得到的最大面值为 101 。
```

示例 2：

```text
输入：piles = [[100],[100],[100],[100],[100],[100],[1,1,1,1,1,1,700]], k = 7
输出：706
解释：
如果我们所有硬币都从最后一个栈中取，可以得到最大面值和。
```

__提示：__

- `n == piles.length`
- `1 <= n <= 1000`
- `1 <= piles[i][j] <= 10 ^ 5`
- `1 <= k <= sum(piles[i].length) <= 2000`

__思路:__

```text
前缀和 ➕ 动态规划
计算每个栈的前缀和
设 dp[i] 为取 i 个硬币的最大面值和
dp[i] = max(dp[i], dp[i - j - 1] + pile[j])
其中 0 <= i <= min(pre + n, k), 0 <= j < min(n, i)
pre 为前 i 个栈的累积硬币数, 即 pre = sum(pile.size())
时间复杂度为 O(KMN), 空间复杂度为 O(K), N 为栈的平均长度, M 为栈的个数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxValueOfCoins(vector<vector<int>>& piles, int k) 
    {
        vector<int> dp(k + 1);
        int pre = 0;
        for (auto& pile : piles)
        {
            int n = pile.size();
            for (int i = 1; i < n; i++) pile[i] += pile[i - 1];
            pre = min(pre + n, k);
            for (int i = pre; i > -1; i--) for (int j = 0, m = min(n, i); j < m; j++) dp[i] = max(dp[i], dp[i - j - 1] + pile[j]);
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int maxValueOfCoins(List<List<Integer>> piles, int k) {
        int dp[] = new int[k + 1], pre = 0;
        for (List<Integer> pile : piles) {
            int n = pile.size();
            for (int i = 1; i < n; i++) pile.set(i, pile.get(i - 1) + pile.get(i));
            pre = Math.min(pre + n, k);
            for (int i = pre; i > -1; i--) for (int j = 0, m = Math.min(n, i); j < m; j++) dp[i] = Math.max(dp[i], dp[i - j - 1] + pile.get(j));
        }
        return dp[k];
    }
}
```

__Python__:

```Python
class Solution:
    def maxValueOfCoins(self, piles: List[List[int]], k: int) -> int:
        dp, pre = [0] * (k + 1), 0
        for pile in piles:
            for i in range(1, len(pile)):
                pile[i] += pile[i - 1]
            pre = min(pre + len(pile), k)
            for j in range(pre, 0, -1):
                dp[j] = max(dp[j], max(dp[j - w - 1] + pile[w] for w in range(min(len(pile), j))))
        return dp[k]
```
