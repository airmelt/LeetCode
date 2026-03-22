# 2861 Maximum Number of Alloys 最大合金数

__Description:__

You are the owner of a company that creates alloys using various types of metals. There are `n` different types of metals available, and you have access to `k` machines that can be used to create alloys. Each machine requires a specific amount of each metal type to create an alloy.

For the `i ^ th` machine to create an alloy, it needs `composition[i][j]` units of metal of type `j`. Initially, you have `stock[i]` units of metal type `i`, and purchasing one unit of metal type `i` costs `cost[i]` coins.

Given integers `n`, `k`, `budget`, a __1-indexed__ 2D array `composition`, and __1-indexed__ arrays `stock` and `cost`, your goal is to __maximize__ the number of alloys the company can create while staying within the budget of `budget` coins.

All alloys must be created with the same machine.

Return the maximum number of alloys that the company can create.

__Example:__

Example 1:

```text
Input: n = 3, k = 2, budget = 15, composition = [[1,1,1],[1,1,10]], stock = [0,0,0], cost = [1,2,3]
Output: 2
Explanation: It is optimal to use the 1st machine to create alloys.
To create 2 alloys we need to buy the:
```

- 2 units of metal of the 1st type.
- 2 units of metal of the 2nd type.
- 2 units of metal of the 3rd type.

```text
In total, we need 2 * 1 + 2 * 2 + 2 * 3 = 12 coins, which is smaller than or equal to budget = 15.
Notice that we have 0 units of metal of each type and we have to buy all the required units of metal.
It can be proven that we can create at most 2 alloys.
```

Example 2:

```text
Input: n = 3, k = 2, budget = 15, composition = [[1,1,1],[1,1,10]], stock = [0,0,100], cost = [1,2,3]
Output: 5
Explanation: It is optimal to use the 2nd machine to create alloys.
To create 5 alloys we need to buy:
```

- 5 units of metal of the 1st type.
- 5 units of metal of the 2nd type.
- 0 units of metal of the 3rd type.

```text
In total, we need 5 * 1 + 5 * 2 + 0 * 3 = 15 coins, which is smaller than or equal to budget = 15.
It can be proven that we can create at most 5 alloys.
```

Example 3:

```text
Input: n = 2, k = 3, budget = 10, composition = [[2,1],[1,2],[1,1]], stock = [1,1], cost = [5,5]
Output: 2
Explanation: It is optimal to use the 3rd machine to create alloys.
To create 2 alloys we need to buy the:
```

- 1 unit of metal of the 1st type.
- 1 unit of metal of the 2nd type.

```text
In total, we need 1 * 5 + 1 * 5 = 10 coins, which is smaller than or equal to budget = 10.
It can be proven that we can create at most 2 alloys.
```

__Constraints:__

- `1 <= n, k <= 100`
- `0 <= budget <= 10 ^ 8`
- `composition.length == k`
- `composition[i].length == n`
- `1 <= composition[i][j] <= 100`
- `stock.length == cost.length == n`
- `0 <= stock[i] <= 10 ^ 8`
- `1 <= cost[i] <= 100`

__题目描述:__

假设你是一家合金制造公司的老板，你的公司使用多种金属来制造合金。现在共有 `n` 种不同类型的金属可以使用，并且你可以使用 `k` 台机器来制造合金。每台机器都需要特定数量的每种金属来创建合金。

对于第 `i` 台机器而言，创建合金需要 `composition[i][j]` 份 `j` 类型金属。最初，你拥有 `stock[x]` 份 `x` 类型金属，而每购入一份 `x` 类型金属需要花费 `cost[x]` 的金钱。

给你整数 `n`、 `k`、 `budget`，下标从 __1__ 开始的二维数组 `composition`，两个下标从 __1__ 开始的数组 `stock` 和 `cost`，请你在预算不超过 `budget` 金钱的前提下，__最大化__ 公司制造合金的数量。

所有合金都需要由同一台机器制造。

返回公司可以制造的最大合金数。

__示例:__

示例 1：

```text
输入：n = 3, k = 2, budget = 15, composition = [[1,1,1],[1,1,10]], stock = [0,0,0], cost = [1,2,3]
输出：2
解释：最优的方法是使用第 1 台机器来制造合金。
要想制造 2 份合金，我们需要购买：
```

- 2 份第 1 类金属。
- 2 份第 2 类金属。
- 2 份第 3 类金属。

```text
总共需要 2 * 1 + 2 * 2 + 2 * 3 = 12 的金钱，小于等于预算 15 。
注意，我们最开始时候没有任何一类金属，所以必须买齐所有需要的金属。
可以证明在示例条件下最多可以制造 2 份合金。
```

示例 2：

```text
输入：n = 3, k = 2, budget = 15, composition = [[1,1,1],[1,1,10]], stock = [0,0,100], cost = [1,2,3]
输出：5
解释：最优的方法是使用第 2 台机器来制造合金。 
要想制造 5 份合金，我们需要购买： 
```

- 5 份第 1 类金属。
- 5 份第 2 类金属。
- 0 份第 3 类金属。

```text
总共需要 5 * 1 + 5 * 2 + 0 * 3 = 15 的金钱，小于等于预算 15 。 
可以证明在示例条件下最多可以制造 5 份合金。
```

示例 3：

```text
输入：n = 2, k = 3, budget = 10, composition = [[2,1],[1,2],[1,1]], stock = [1,1], cost = [5,5]
输出：2
解释：最优的方法是使用第 3 台机器来制造合金。
要想制造 2 份合金，我们需要购买：
```

- 1 份第 1 类金属。
- 1 份第 2 类金属。

```text
总共需要 1 * 5 + 1 * 5 = 10 的金钱，小于等于预算 10 。
可以证明在示例条件下最多可以制造 2 份合金。
```

__提示：__

- `1 <= n, k <= 100`
- `0 <= budget <= 10 ^ 8`
- `composition.length == k`
- `composition[i].length == n`
- `1 <= composition[i][j] <= 100`
- `stock.length == cost.length == n`
- `0 <= stock[i] <= 10 ^ 8`
- `1 <= cost[i] <= 100`

__思路:__

```text
二分查找
合金越少花的钱越少
花费最低为 1
所以最多的合金数不会超过 min(stock) + budget
如果要生产 mid 个金属
分别用 k 台机器中的某台 i 尝试制造
需要金属数量为 [c * mid for c in composition[i]]
这部分需要对应减去已经拥有的金属
累加花费, 花费不超过 budget 时就用当前机器生产就能满足 mid 的合金数量
使用二分查找找到最大能生产的合金数
时间复杂度为 O(KNlogU), 空间复杂度为 O(1), 其中 U = min(stock) + budget
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxNumberOfAlloys(int n, int k, int budget, vector<vector<int>>& composition, vector<int>& stock, vector<int>& cost) 
    {
        int left = 0, right = *min_element(stock.begin(), stock.end()) + budget + 1;
        auto check = [&](const auto& mid) -> bool
        {
            long long cur = LLONG_MAX;
            for (int i = 0; i < k; i++) 
            {
                long long s = 0LL;
                for (int j = 0; j < n; j++) s += cost[j] * max(0LL, composition[i][j] * mid - stock[j]);
                if ((cur = min(cur, s)) <= budget) return true;
            }
            return false;
        };
        while (left + 1 < right) 
        {
            long long mid = left + ((right - left) >> 1LL);
            if (check(mid)) left = mid;
            else right = mid;
        }
        return left;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxNumberOfAlloys(int n, int k, int budget, List<List<Integer>> composition, List<Integer> stock, List<Integer> cost) {
        int left = 0, right = stock.stream().mapToInt(v -> v).min().getAsInt() + budget + 1;
        while (left + 1 < right) {
            int mid = left + ((right - left) >>> 1);
            if (check(mid, n, k, budget, composition, stock, cost)) left = mid;
            else right = mid;
        }
        return left;
    }

    private boolean check(long mid, int n, int k, int budget, List<List<Integer>> composition, List<Integer> stock, List<Integer> cost) {
        long cur = Long.MAX_VALUE;
        for (int i = 0; i < k; i++) {
            long s = 0L;
            for (int j = 0; j < n; j++) s += cost.get(j) * Math.max(0L, composition.get(i).get(j) * mid - stock.get(j));
            if ((cur = Math.min(cur, s)) <= budget) return true;
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def maxNumberOfAlloys(self, n: int, k: int, budget: int, composition: List[List[int]], stock: List[int], cost: List[int]) -> int:
        return bisect_left(range(max(stock) + budget + 1), 1, key=lambda mid: not any(sum(x * y for x, y in zip([max(c * mid - s, 0) for c, s in zip(composition[i], stock)], cost)) <= budget for i in range(k))) - 1
```
