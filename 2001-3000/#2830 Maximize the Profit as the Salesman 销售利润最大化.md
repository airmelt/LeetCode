# 2830 Maximize the Profit as the Salesman 销售利润最大化

__Description:__

You are given an integer `n` representing the number of houses on a number line, numbered from `0` to `n - 1`.

Additionally, you are given a 2D integer array `offers` where `offers[i] = [start_i, end_i, gold_i]`, indicating that `i ^ th` buyer wants to buy all the houses from `start_i` to `end_i` for `gold_i` amount of gold.

As a salesman, your goal is to maximize your earnings by strategically selecting and selling houses to buyers.

Return the maximum amount of gold you can earn.

Note that different buyers can't buy the same house, and some houses may remain unsold.

__Example:__

Example 1:

```text
Input: n = 5, offers = [[0,0,1],[0,2,2],[1,3,2]]
Output: 3
Explanation: There are 5 houses numbered from 0 to 4 and there are 3 purchase offers.
We sell houses in the range [0,0] to 1st buyer for 1 gold and houses in the range [1,3] to 3rd buyer for 2 golds.
It can be proven that 3 is the maximum amount of gold we can achieve.
```

Example 2:

```text
Input: n = 5, offers = [[0,0,1],[0,2,10],[1,3,2]]
Output: 10
Explanation: There are 5 houses numbered from 0 to 4 and there are 3 purchase offers.
We sell houses in the range [0,2] to 2nd buyer for 10 golds.
It can be proven that 10 is the maximum amount of gold we can achieve.
```

__Constraints:__

- `1 <= n <= 10 ^ 5`
- `1 <= offers.length <= 10 ^ 5`
- `offers[i].length == 3`
- `0 <= start_i <= end_i <= n - 1`
- `1 <= gold_i <= 10 ^ 3`

__题目描述:__

给你一个整数 `n` 表示数轴上的房屋数量，编号从 `0` 到 `n - 1` 。

另给你一个二维整数数组 `offers` ，其中 `offers[i] = [start_i, end_i, gold_i]` 表示第 `i` 个买家想要以 `gold_i` 枚金币的价格购买从 `start_i` 到 `end_i` 的所有房屋。

作为一名销售，你需要有策略地选择并销售房屋使自己的收入最大化。

返回你可以赚取的金币的最大数目。

注意 同一所房屋不能卖给不同的买家，并且允许保留一些房屋不进行出售。

__示例:__

示例 1：

```text
输入：n = 5, offers = [[0,0,1],[0,2,2],[1,3,2]]
输出：3
解释：
有 5 所房屋，编号从 0 到 4 ，共有 3 个购买要约。
将位于 [0,0] 范围内的房屋以 1 金币的价格出售给第 1 位买家，并将位于 [1,3] 范围内的房屋以 2 金币的价格出售给第 3 位买家。
可以证明我们最多只能获得 3 枚金币。
```

示例 2：

```text
输入：n = 5, offers = [[0,0,1],[0,2,10],[1,3,2]]
输出：10
解释：有 5 所房屋，编号从 0 到 4 ，共有 3 个购买要约。
将位于 [0,2] 范围内的房屋以 10 金币的价格出售给第 2 位买家。
可以证明我们最多只能获得 10 枚金币。
```

__提示：__

- `1 <= n <= 10 ^ 5`
- `1 <= offers.length <= 10 ^ 5`
- `offers[i].length == 3`
- `0 <= start_i <= end_i <= n - 1`
- `1 <= gold_i <= 10 ^ 3`

__思路:__

```text
动态规划
设 dp[i + 1] 表示以 i 结尾的最大盈利
如果不卖那么 dp[i + 1] = dp[i]
如果卖那么在所有能到达的选个最大的 dp[i + 1] = dp[start] + gold for start, gold in g
初始化 dp[0] = 0
最后返回 dp[n]
时间复杂度为 O(N + M), 空间复杂度为 O(N + M), M 为 offers 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximizeTheProfit(int n, vector<vector<int>>& offers) 
    {
        vector<vector<pair<int, int>>> graph(n);
        for (const auto& offer : offers) graph[offer[1]].emplace_back(offer.front(), offer.back());
        vector<int> dp(n + 1);
        for (int end = 0; end < n; end++) 
        {
            dp[end + 1] = dp[end];
            for (const auto& [start, gold] : graph[end]) dp[end + 1] = max(dp[end + 1], dp[start] + gold);
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int maximizeTheProfit(int n, List<List<Integer>> offers) {
        List<int[]>[] graph = new ArrayList[n];
        Arrays.setAll(graph, i -> new ArrayList<int[]>());
        for (var offer : offers) graph[offer.get(1)].add(new int[]{ offer.get(0), offer.get(2) });
        var dp = new int[n + 1];
        for (int end = 0; end < n; end++) {
            dp[end + 1] = dp[end];
            for (var g : graph[end]) dp[end + 1] = Math.max(dp[end + 1], dp[g[0]] + g[1]);
        }
        return dp[n];
    }
}
```

__Python__:

```Python
class Solution:
    def maximizeTheProfit(self, n: int, offers: List[List[int]]) -> int:
        graph, dp = [[] for _ in range(n)], [0] * (n + 1)
        for start, end, gold in offers:
            graph[end].append((start, gold))
        for end, g in enumerate(graph):
            dp[end + 1] = max(dp[end], max((dp[start] + gold for start, gold in g), default=0))
        return dp[n]
```
