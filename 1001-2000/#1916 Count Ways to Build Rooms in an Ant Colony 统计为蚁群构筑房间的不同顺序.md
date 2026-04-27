# 1916 Count Ways to Build Rooms in an Ant Colony 统计为蚁群构筑房间的不同顺序

__Description:__

You are an ant tasked with adding `n` new rooms numbered `0` to `n-1` to your colony. You are given the expansion plan as a __0-indexed__ integer array of length `n`, `prevRoom`, where `prevRoom[i]` indicates that you must build room `prevRoom[i]` before building room `i`, and these two rooms must be connected __directly__. Room `0` is already built, so `prevRoom[0] = -1`. The expansion plan is given such that once all the rooms are built, every room will be reachable from room `0`.

You can only build one room at a time, and you can travel freely between rooms you have already built only if they are connected. You can choose to build any room as long as its previous room is already built.

Return _the __number of different orders__ you can build all the rooms in_. Since the answer may be large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

![1916-1](https://assets.leetcode.com/uploads/2021/06/19/d1.JPG)

```text
Input: prevRoom = [-1,0,1]
Output: 1
Explanation: There is only one way to build the additional rooms: 0 → 1 → 2
```

Example 2:

![1916-2](https://assets.leetcode.com/uploads/2021/06/19/d2.JPG)

```text
Input: prevRoom = [-1,0,0,1,2]
Output: 6
Explanation:
The 6 ways are:
0 → 1 → 3 → 2 → 4
0 → 2 → 4 → 1 → 3
0 → 1 → 2 → 3 → 4
0 → 1 → 2 → 4 → 3
0 → 2 → 1 → 3 → 4
0 → 2 → 1 → 4 → 3
```

__Constraints:__

- `n == prevRoom.length`
- `2 <= n <= 10 ^ 5`
- `prevRoom[0] == -1`
- `0 <= prevRoom[i] < n` for all `1 <= i < n`
- Every room is reachable from room `0` once all the rooms are built.

__题目描述:__

你是一只蚂蚁，负责为蚁群构筑 `n` 间编号从 `0` 到 `n-1` 的新房间。给你一个 __下标从 0 开始__ 且长度为 `n` 的整数数组 `prevRoom` 作为扩建计划。其中， `prevRoom[i]` 表示在构筑房间 `i` 之前，你必须先构筑房间 `prevRoom[i]` ，并且这两个房间必须 __直接__ 相连。房间 `0` 已经构筑完成，所以 `prevRoom[0] = -1` 。扩建计划中还有一条硬性要求，在完成所有房间的构筑之后，从房间 `0` 可以访问到每个房间。

你一次只能构筑 __一个__ 房间。你可以在 __已经构筑好的__ 房间之间自由穿行，只要这些房间是 __相连的__ 。如果房间 `prevRoom[i]` 已经构筑完成，那么你就可以构筑房间 `i`。

返回你构筑所有房间的 __不同顺序的数目__ 。由于答案可能很大，请返回对 `10 ^ 9 + 7` __取余__ 的结果。

__示例:__

示例 1：

![1916-3](https://assets.leetcode.com/uploads/2021/06/19/d1.JPG)

```text
输入: `prevRoom` = [-1,0,1]
输出: 1
解释: 仅有一种方案可以完成所有房间的构筑:0 → 1 → 2
```

示例 2：

![1916-4](https://assets.leetcode.com/uploads/2021/06/19/d2.JPG)

```text
输入: `prevRoom` = [-1,0,0,1,2]
输出: 6
解释:
有 6 种不同顺序:
0 → 1 → 3 → 2 → 4
0 → 2 → 4 → 1 → 3
0 → 1 → 2 → 3 → 4
0 → 1 → 2 → 4 → 3
0 → 2 → 1 → 3 → 4
0 → 2 → 1 → 4 → 3
```

__提示：__

- `n == prevRoom.length`
- `2 <= n <= 10 ^ 5`
- `prevRoom[0] == -1`
- 对于所有的 `1 <= i < n` ，都有 `0 <= prevRoom[i] < n`
- 题目保证所有房间都构筑完成后，从房间 `0` 可以访问到每个房间

__思路:__

```text
动态规划
设 dp[i] 表示第 i 个结点的构造树
cnt[i] 表示第 i 个结点的子树的结点个数
则由乘法原理 dp[i] = dp[j] * dp[k] * ..., 其中 j, k, ... 为 i 的子结点
上述式子没有考虑到结点之间的排序, 由于各个结点之间是等效的
所以可以还需要乘上 cnt[i] / (cnt[j]! * cnt[k]! * ...)
为了快速计算阶乘以及其取模, 可以使用快速幂和逆元
令 fac[i] 表示 i 的阶乘, inv[i] 表示 i 的逆元
fac[0] = inv[0] = 1
fac[i] = fac[i - 1] * i % MOD
inv[i] = quick_pow(fac[i], MOD - 2, MOD)
则 dp[i] = dp[j] * dp[k] * ... * fac[cnt[i]] % MOD * inv[cnt[j]] % MOD * inv[cnt[k]] % MOD * ...
最后答案为 dp[0]
遍历 prevRoom, 构造有向图, 从 0 开始 dfs 即可
时间复杂度为 O(NlogN), 空间复杂度为 O(N), 其中 N 为结点个数, 主要时间复杂度来源于快速幂处理逆元
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int waysToBuildRooms(vector<int>& prevRoom) 
    {
        int n = prevRoom.size(), MOD = 1e9 + 7;
        auto quick_pow = [&](auto a, auto n) 
        {
            int result = 1;
            while (n) 
            {
                if (n & 1) result = (long long)result * a % MOD;
                a = (long long)a * a % MOD;
                n >>= 1;
            }
            return result;
        };
        vector<int> fac(n), inv(n), dp(n), cnt(n);
        vector<vector<int>> graph(n);
        fac.front() = inv.front() = 1;
        for (int i = 1; i < n; i++) 
        {
            fac[i] = (long long)fac[i - 1] * i % MOD;
            inv[i] = quick_pow(fac[i], MOD - 2);
            graph[prevRoom[i]].emplace_back(i);
        }
        function<void(int)> dfs = [&](auto u) 
        {
            dp[u] = 1;
            for (int v : graph[u]) 
            {
                dfs(v);
                dp[u] = (long long)dp[u] * dp[v] % MOD * inv[cnt[v]] % MOD;
                cnt[u] += cnt[v];
            }
            dp[u] = (long long)dp[u] * fac[cnt[u]] % MOD;
            ++cnt[u];
        };
        
        dfs(0);
        return dp.front();
    }
};
```

__Java__:

```Java
class Solution {
    public int waysToBuildRooms(int[] prevRoom) {
        int n = prevRoom.length;
        long MOD = 1_000_000_007, fac[] = new long[n], inv[] = new long[n], dp[] = new long[n], count[] = new long[n];
        Map<Integer, Set<Integer>> graph = new HashMap<>();
        fac[0] = inv[0] = 1L;
        for (int i = 1; i < n; i++) {
            fac[i] = fac[i - 1] * i % MOD;
            inv[i] = quickPow(fac[i], MOD - 2, MOD);
            graph.putIfAbsent(prevRoom[i], new HashSet<>());
            graph.get(prevRoom[i]).add(i);
        }
        dfs(0, dp, count, fac, inv, graph, MOD);
        return (int)dp[0];
    }

    private long quickPow(long a, long n, long MOD) {
        long result = 1L;
        while (n > 0) {
            if ((n & 1) == 1) result = result * a % MOD;
            a = a * a % MOD;
            n >>>= 1;
        }
        return result;
    }

    private void dfs(int u, long[] dp, long[] count, long[] fac, long[] inv, Map<Integer, Set<Integer>> graph, long MOD) {
        dp[u] = 1;
        for (int v : graph.getOrDefault(u, new HashSet<>())) {
            dfs(v, dp, count, fac, inv, graph, MOD);
            dp[u] = dp[u] * dp[v] % MOD * inv[(int)count[v]] % MOD;
            count[u] += count[v];
        }
        dp[u] = dp[u] * fac[(int)count[u]] % MOD;
        ++count[u];
    }
}
```

__Python__:

```Python
class Solution:
    def waysToBuildRooms(self, prevRoom: List[int]) -> int:
        MOD, fac, inv, graph, dp, count = 10 ** 9 + 7, [1] + [0] * ((n := len(prevRoom)) - 1), [1] + [0] * (n - 1), defaultdict(set), [0] * n, [0] * n
        for i in range(1, n):
            fac[i] = fac[i - 1] * i % MOD
            inv[i] = pow(fac[i], MOD - 2, MOD)
            graph[prevRoom[i]].add(i)
        print(fac, inv)
        def dfs(u: int) -> NoReturn:
            dp[u] = 1
            for v in graph[u]:
                dfs(v)
                dp[u] = dp[u] * dp[v] * inv[count[v]] % MOD
                count[u] += count[v]
            dp[u] = dp[u] * fac[count[u]] % MOD
            count[u] += 1
        dfs(0)
        return dp[0]
```
