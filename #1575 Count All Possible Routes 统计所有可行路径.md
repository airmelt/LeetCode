# 1575 Count All Possible Routes 统计所有可行路径

__Description:__

You are given an array of __distinct__ positive integers locations where `locations[i]` represents the position of city `i`. You are also given integers `start`, `finish` and `fuel` representing the starting city, ending city, and the initial amount of fuel you have, respectively.

At each step, if you are at city `i`, you can pick any city `j` such that `j != i` and `0 <= j < locations.length` and move to city `j`. Moving from city `i` to city `j` reduces the amount of fuel you have by `|locations[i] - locations[j]|`. Please notice that `|x|` denotes the absolute value of `x`.

Notice that `fuel` __cannot__ become negative at any point in time, and that you are __allowed__ to visit any city more than once (including `start` and `finish`).

Return _the count of all possible routes from_ `start` _to_ `finish`. Since the answer may be too large, return it modulo `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: locations = [2,3,6,8,4], start = 1, finish = 3, fuel = 5
Output: 4
Explanation: The following are all possible routes, each uses 5 units of fuel:
1 -> 3
1 -> 2 -> 3
1 -> 4 -> 3
1 -> 4 -> 2 -> 3
```

Example 2:

```text
Input: locations = [4,3,1], start = 1, finish = 0, fuel = 6
Output: 5
Explanation: The following are all possible routes:
1 -> 0, used fuel = 1
1 -> 2 -> 0, used fuel = 5
1 -> 2 -> 1 -> 0, used fuel = 5
1 -> 0 -> 1 -> 0, used fuel = 3
1 -> 0 -> 1 -> 0 -> 1 -> 0, used fuel = 5
```

Example 3:

```text
Input: locations = [5,2,1], start = 0, finish = 2, fuel = 3
Output: 0
Explanation: It is impossible to get from 0 to 2 using only 3 units of fuel since the shortest route needs 4 units of fuel.
```

__Constraints:__

- `2 <= locations.length <= 100`
- `1 <= locations[i] <= 10 ^ 9`
- All integers in `locations` are __distinct__.
- `0 <= start, finish < locations.length`
- `1 <= fuel <= 200`

__题目描述:__

给你一个 __互不相同__ 的整数数组，其中 `locations[i]` 表示第 `i` 个城市的位置。同时给你 `start`， `finish` 和 `fuel` 分别表示出发城市、目的地城市和你初始拥有的汽油总量

每一步中，如果你在城市 `i` ，你可以选择任意一个城市 `j` ，满足  `j != i` 且 `0 <= j < locations.length` ，并移动到城市 `j` 。从城市 `i` 移动到 `j` 消耗的汽油量为 `|locations[i] - locations[j]|`， `|x|` 表示 `x` 的绝对值。

请注意， `fuel` 任何时刻都 __不能__ 为负，且你 __可以__ 经过任意城市超过一次（包括 `start` 和 `finish` ）。

请你返回从 `start` 到 `finish` 所有可能路径的数目。

由于答案可能很大， 请将它对 `10 ^ 9 + 7` 取余后返回。

__示例:__

示例 1：

```text
输入：locations = [2,3,6,8,4], start = 1, finish = 3, fuel = 5
输出：4
解释：以下为所有可能路径，每一条都用了 5 单位的汽油：
1 -> 3
1 -> 2 -> 3
1 -> 4 -> 3
1 -> 4 -> 2 -> 3
```

示例 2：

```text
输入：locations = [4,3,1], start = 1, finish = 0, fuel = 6
输出：5
解释：以下为所有可能的路径：
1 -> 0，使用汽油量为 fuel = 1
1 -> 2 -> 0，使用汽油量为 fuel = 5
1 -> 2 -> 1 -> 0，使用汽油量为 fuel = 5
1 -> 0 -> 1 -> 0，使用汽油量为 fuel = 3
1 -> 0 -> 1 -> 0 -> 1 -> 0，使用汽油量为 fuel = 5
```

示例 3：

```text
输入：locations = [5,2,1], start = 0, finish = 2, fuel = 3
输出：0
解释：没有办法只用 3 单位的汽油从 0 到达 2 。因为最短路径需要 4 单位的汽油。
```

__提示：__

- `2 <= locations.length <= 100`
- `1 <= locations[i] <= 10 ^ 9`
- 所有 `locations` 中的整数 __互不相同__ 。
- `0 <= start, finish < locations.length`
- `1 <= fuel <= 200`

__思路:__

```text
1. 记忆化搜索
记录 memo[cur][cur_fuel] 表示到 cur 地点还剩 cur_fuel 的种类数
初始化为 -1
如果当前燃料不足, 即小于 0, 返回 0, 表示不能到达
如果 cur == finish, 表示已经抵达终点, 方式数自增
遍历 locations, 所有与当前 cur 访问的不同的点都可以访问, 并记录消耗的燃料
时间复杂度为 O(MN ^ 2), 空间复杂度为 O(MN), M 为 fuel
2. 优化的动态规划
记录 dpL[city][used] 表示从起点到达城市 city, 消耗的汽油量为 used 并且最后一次为负方向, dpR 定义类似, 方向不同
先将 locations 排序, 方便进行状态转移
dpL 的状态转移方程 dpL[city][used] = dpR[city + 1][used - dist(city, city + 1)] + dpL[city + 1][used - dist(city, city + 1)] * 2
时间复杂度为 O(MN), 空间复杂度为 O(MN), M 为 fuel
```

__代码:__

__C++__:

```C++
class Solution {
public:
    int countRoutes(vector<int>& locations, int start, int finish, int fuel) {
        int MOD = 1e9 + 7, n = locations.size(), start_pos = locations[start], finish_pos = locations[finish], result = 0;
        sort(locations.begin(), locations.end());
        for (int i = 0; i < n; i++) 
        {
            if (start_pos == locations[i]) start = i;
            if (finish_pos == locations[i]) finish = i;
        }
        vector<vector<int>> dpL(n, vector<int>(fuel + 1)), dpR(n, vector<int>(fuel + 1));
        dpL[start].front() = dpR[start].front() = 1;
        for (int used = 0; used <= fuel; ++used) 
        {
            for (int city = n - 2; ~city; city--) if (int delta = locations[city + 1] - locations[city]; used >= delta) dpL[city][used] = ((used == delta ? 0 : dpL[city + 1][used - delta]) * 2 % MOD + dpR[city + 1][used - delta]) % MOD;
            for (int city = 1; city < n; city++) if (int delta = locations[city] - locations[city - 1]; used >= delta) dpR[city][used] = ((used == delta ? 0 : dpR[city - 1][used - delta]) * 2 % MOD + dpL[city - 1][used - delta]) % MOD;
        }
        for (int used = 0; used <= fuel; ++used) 
        {
            result += (dpL[finish][used] + dpR[finish][used]) % MOD;
            result %= MOD;
        }
        return finish == start ? (result + MOD - 1) % MOD : result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countRoutes(int[] locations, int start, int finish, int fuel) {
        int[][] memo = new int[locations.length][fuel + 1];
        for (int[] arr : memo) Arrays.fill(arr, -1);
        return dfs(locations, start, finish, fuel, memo);
    }

    int dfs(int[] locations, int start, int finish, int fuel, int[][] memo) {
        if (fuel < 0) return 0;
        if (memo[start][fuel] != -1) return memo[start][fuel];
        int result = 0, n = locations.length, MOD = 1_000_000_007;
        if (start == finish) ++result;
        for (int i = n - 1; i > -1; i--) {
            if (start == i) continue;
            result += dfs(locations, i, finish, fuel - Math.abs(locations[i] - locations[start]), memo);
            result %= MOD;
        }
        return memo[start][fuel] = result;
    }
}
```

__Python__:

```Python
class Solution:
    def countRoutes(self, locations: List[int], start: int, finish: int, fuel: int) -> int:
        @lru_cache(None)
        def dfs(cur: int, cur_fuel: int) -> int:
            if cur_fuel < 0:
                return 0
            result, n = cur == finish, len(locations)
            for i in range(n):
                if i != cur:
                    result += dfs(i, cur_fuel - abs(locations[cur] - locations[i]))
            return result
        return dfs(start, fuel) % (10 ** 9 + 7)
```
