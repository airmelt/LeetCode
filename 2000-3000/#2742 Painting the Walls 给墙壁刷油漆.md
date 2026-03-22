# 2742 Painting the Walls 给墙壁刷油漆

__Description:__

You are given two __0-indexed__ integer arrays, `cost` and `time`, of size `n` representing the costs and the time taken to paint `n` different walls respectively. There are two painters available:

- A __paid painter__ that paints the `i ^ th` wall in `time[i]` units of time and takes `cost[i]` units of money.
- A __free painter__ that paints __any__ wall in `1` unit of time at a cost of `0`. But the free painter can only be used if the paid painter is already __occupied__.

Return _the minimum amount of money required to paint the_ `n` _walls._

__Example:__

Example 1:

```text
Input: cost = [1,2,3,2], time = [1,2,3,2]
Output: 3
Explanation: The walls at index 0 and 1 will be painted by the paid painter, and it will take 3 units of time; meanwhile, the free painter will paint the walls at index 2 and 3, free of cost in 2 units of time. Thus, the total cost is 1 + 2 = 3.
```

Example 2:

```text
Input: cost = [2,3,4,2], time = [1,1,1,1]
Output: 4
Explanation: The walls at index 0 and 3 will be painted by the paid painter, and it will take 2 units of time; meanwhile, the free painter will paint the walls at index 1 and 2, free of cost in 2 units of time. Thus, the total cost is 2 + 2 = 4.
```

__Constraints:__

- `1 <= cost.length <= 500`
- `cost.length == time.length`
- `1 <= cost[i] <= 10 ^ 6`
- `1 <= time[i] <= 500`

__题目描述:__

给你两个长度为 `n` 下标从 __0__ 开始的整数数组 `cost` 和 `time` ，分别表示给 `n` 堵不同的墙刷油漆需要的开销和时间。你有两名油漆匠:

- 一位需要 __付费__ 的油漆匠，刷第 `i` 堵墙需要花费 `time[i]` 单位的时间，开销为 `cost[i]` 单位的钱。
- 一位 __免费__ 的油漆匠，刷 __任意__ 一堵墙的时间为 `1` 单位，开销为 `0` 。但是必须在付费油漆匠 __工作__ 时，免费油漆匠才会工作。

请你返回刷完 `n` 堵墙最少开销为多少。

__示例:__

示例 1：

```text
输入：cost = [1,2,3,2], time = [1,2,3,2]
输出：3
解释：下标为 0 和 1 的墙由付费油漆匠来刷，需要 3 单位时间。同时，免费油漆匠刷下标为 2 和 3 的墙，需要 2 单位时间，开销为 0 。总开销为 1 + 2 = 3 。
```

示例 2：

```text
输入：cost = [2,3,4,2], time = [1,1,1,1]
输出：4
解释：下标为 0 和 3 的墙由付费油漆匠来刷，需要 2 单位时间。同时，免费油漆匠刷下标为 1 和 2 的墙，需要 2 单位时间，开销为 0 。总开销为 2 + 2 = 4 。
```

__提示：__

- `1 <= cost.length <= 500`
- `cost.length == time.length`
- `1 <= cost[i] <= 10 ^ 6`
- `1 <= time[i] <= 500`

__思路:__

```text
动态规划
设 dp[i][j] 表示使用 i 个油漆工, 其中使用 j 个时间的总花费
dp[i][j] = min(dp[i - 1][j - time[i] - 1] + cost[i], dp[i - 1][j])
如果使用免费的, dp[i][j] = dp[i - 1][j]
如果不使用免费的 dp[i][j] = dp[i - 1][j - time[i] - 1] + cost[i], 这是因为, 一个要钱的油漆工能够提供 time[i] 的时间需要 cost[i] 的消费, 免费的需要 1 个单位的时间, 付费的总时间需要不少于 j 的数量
最后返回 dp[n - 1][n]
由于上面的计算只和相邻两行有关, 可以用滚动数组优化空间复杂度
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int paintWalls(vector<int>& cost, vector<int>& time) 
    {
        int n = cost.size(), inf = 0x3f3f3f3f;
        vector<int> dp(n + 1, inf);
        dp.front() = 0;
        for (int i = 0; i < n; i++) for (int j = n; j > 0; j--) dp[j] = min(dp[j], dp[max(j - time[i] - 1, 0)] + cost[i]);
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int paintWalls(int[] cost, int[] time) {
        int n = cost.length, dp[] = new int[n + 1], inf = 0x3f3f3f3f;
        for (int i = 1; i <= n; i++) dp[i] = inf;
        for (int i = 0; i < n; i++) for (int j = n; j > 0; j--) dp[j] = Math.min(dp[j], dp[Math.max(j - time[i] - 1, 0)] + cost[i]);
        return dp[n];
    }
}
```

__Python__:

```Python
class Solution:
    def paintWalls(self, cost: List[int], time: List[int]) -> int:
        @lru_cache(None)
        def dfs(i: int, j: int) -> int:
            return 0 if j > i else inf if i < 0 else min(dfs(i - 1, j + time[i]) + cost[i], dfs(i - 1, j - 1))
        return dfs(len(cost) - 1, 0)
```
