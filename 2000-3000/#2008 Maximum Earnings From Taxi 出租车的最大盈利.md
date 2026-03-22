# 2008 Maximum Earnings From Taxi 出租车的最大盈利

__Description:__

There are `n` points on a road you are driving your taxi on. The `n` points on the road are labeled from `1` to `n` in the direction you are going, and you want to drive from point `1` to point `n` to make money by picking up passengers. You cannot change the direction of the taxi.

The passengers are represented by a __0-indexed__ 2D integer array `rides`, where `rides[i] = [starti, endi, tipi]` denotes the `i ^ th` passenger requesting a ride from point `starti` to point `endi` who is willing to give a `tipi` dollar tip.

For __each__ passenger `i` you pick up, you __earn__ `endi - starti + tipi` dollars. You may only drive _at most one_ passenger at a time.

Given `n` and `rides`, return _the __maximum__ number of dollars you can earn by picking up the passengers optimally._

Note: You may drop off a passenger and pick up a different passenger at the same point.

__Example:__

Example 1:

```text
Input: n = 5, rides = [[2,5,4],[1,5,1]]
Output: 7
Explanation: We can pick up passenger 0 to earn 5 - 2 + 4 = 7 dollars.
```

Example 2:

```text
Input: n = 20, rides = [[1,6,1],[3,10,2],[10,12,3],[11,12,2],[12,15,2],[13,18,1]]
Output: 20
Explanation: We will pick up the following passengers:
```

- Drive passenger 1 from point 3 to point 10 for a profit of 10 - 3 + 2 = 9 dollars.
- Drive passenger 2 from point 10 to point 12 for a profit of 12 - 10 + 3 = 5 dollars.
- Drive passenger 5 from point 13 to point 18 for a profit of 18 - 13 + 1 = 6 dollars.
We earn 9 + 5 + 6 = 20 dollars in total.

__Constraints:__

- `1 <= n <= 10 ^ 5`
- `1 <= rides.length <= 3 * 10 ^ 4`
- `rides[i].length == 3`
- `1 <= starti < endi <= n`
- `1 <= tipi <= 10 ^ 5`

__题目描述:__

你驾驶出租车行驶在一条有 `n` 个地点的路上。这 `n` 个地点从近到远编号为 `1` 到 `n` ，你想要从 `1` 开到 `n` ，通过接乘客订单盈利。你只能沿着编号递增的方向前进，不能改变方向。

乘客信息用一个下标从 __0__ 开始的二维数组 `rides` 表示，其中 `rides[i] = [starti, endi, tipi]` 表示第 `i` 位乘客需要从地点 `starti` 前往 `endi` ，愿意支付 `tipi` 元的小费。

__每一位__ 你选择接单的乘客 `i` ，你可以 __盈利__ `endi - starti + tipi` 元。你同时 __最多__ 只能接一个订单。

给你 `n` 和 `rides` ，请你返回在最优接单方案下，你能盈利 __最多__ 多少元。

注意：你可以在一个地点放下一位乘客，并在同一个地点接上另一位乘客。

__示例:__

示例 1：

```text
输入：n = 5, rides = [[2,5,4],[1,5,1]]
输出：7
解释：我们可以接乘客 0 的订单，获得 5 - 2 + 4 = 7 元。
```

示例 2：

```text
输入：n = 20, rides = [[1,6,1],[3,10,2],[10,12,3],[11,12,2],[12,15,2],[13,18,1]]
输出：20
解释：我们可以接以下乘客的订单：
```

- 将乘客 1 从地点 3 送往地点 10 ，获得 10 - 3 + 2 = 9 元。
- 将乘客 2 从地点 10 送往地点 12 ，获得 12 - 10 + 3 = 5 元。
- 将乘客 5 从地点 13 送往地点 18 ，获得 18 - 13 + 1 = 6 元。
我们总共获得 9 + 5 + 6 = 20 元。

__提示：__

- `1 <= n <= 10 ^ 5`
- `1 <= rides.length <= 3 * 10 ^ 4`
- `rides[i].length == 3`
- `1 <= starti < endi <= n`
- `1 <= tipi <= 10 ^ 5`

__思路:__

```text
动态规划
设 dp[i] 表示到达第 i 个地点时的最大盈利
对于每个到达点 i，我们可以选择接或不接
如果不接客，则 dp[i] = dp[i - 1]
如果接客，则 dp[i] = max(dp[i], dp[start] + end - start + tip)
可以将 end - start + tip 作为一个整体
将所有到达点为 end 的乘客分组
遍历每个到达点 i，更新 dp[i] = max(dp[i], dp[start] + end - start + tip)
最后返回 dp[n]
时间复杂度为 O(M + N), 空间复杂度为 O(M + N), 其中 M 为 rides 的长度, N 为 n
```

__代码:__

__C++__:

```C++
class Solution {
public:
    long long maxTaxiEarnings(int n, vector<vector<int>>& rides) {
        vector<vector<pair<int, int>>> groups(n + 1);
        for (const auto& ride : rides) groups[ride[1]].emplace_back(make_pair(ride[0], ride[1] - ride[0] + ride[2]));
        vector<long long> dp(n + 1);
        for (int i = 2; i <= n; i++) 
        {
            dp[i] = dp[i - 1];
            for (const auto& ride : groups[i]) dp[i] = max(dp[i], dp[ride.first] + ride.second);
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public long maxTaxiEarnings(int n, int[][] rides) {
        Map<Integer, List<int[]>> groups = new HashMap<>();
        for (int[] ride : rides) groups.computeIfAbsent(ride[1], k -> new ArrayList<>()).add(new int[]{ ride[0], ride[1], ride[2] });
        long[] dp = new long[n + 1];
        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i - 1];
            if (groups.get(i) != null) for (int[] ride : groups.get(i)) dp[i] = Math.max(dp[i], dp[ride[0]] + ride[1] - ride[0] + ride[2]);
        }
        return dp[n];
    }
}
```

__Python__:

```Python
class Solution:
    def maxTaxiEarnings(self, n: int, rides: List[List[int]]) -> int:
        groups = defaultdict(list)
        for start, end, tip in rides:
            groups[end].append((start, end - start + tip))
        @lru_cache(None)
        def dfs(i: int) -> int:
            return 0 if i == 1 else max(dfs(i - 1), max((dfs(s) + t for s, t in groups[i]), default=0))
        return dfs(n)
```
