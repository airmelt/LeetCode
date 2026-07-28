# 3116 Kth Smallest Amount With Single Denomination Combination 单面值组合的第 K 小金额

__Description:__

You are given an integer array `coins` representing coins of different denominations and an integer `k`.

You have an infinite number of coins of each denomination. However, you are not allowed to combine coins of different denominations.

Return the `k ^ th` __smallest__ amount that can be made using these coins.

__Example:__

Example 1:

```text
Input: coins = [3,6,9], k = 3

Output: 9

Explanation: The given coins can make the following amounts:
Coin 3 produces multiples of 3: 3, 6, 9, 12, 15, etc.
Coin 6 produces multiples of 6: 6, 12, 18, 24, etc.
Coin 9 produces multiples of 9: 9, 18, 27, 36, etc.
All of the coins combined produce: 3, 6, 9, 12, 15, etc.
```

Example 2:

```text
Input: coins = [5,2], k = 7

Output: 12

Explanation: The given coins can make the following amounts:
Coin 5 produces multiples of 5: 5, 10, 15, 20, etc.
Coin 2 produces multiples of 2: 2, 4, 6, 8, 10, 12, etc.
All of the coins combined produce: 2, 4, 5, 6, 8, 10, 12, 14, 15, etc.
```

__Constraints:__

- `1 <= coins.length <= 15`
- `1 <= coins[i] <= 25`
- `1 <= k <= 2 * 10 ^ 9`
- `coins` contains pairwise distinct integers.

__题目描述:__

给你一个整数数组 `coins` 表示不同面额的硬币，另给你一个整数 `k` 。

你有无限量的每种面额的硬币。但是，你 不能 组合使用不同面额的硬币。

返回使用这些硬币能制造的 __第__ `k ^ th` __小__ 金额。

__示例:__

示例 1：

```text
输入： coins = [3,6,9], k = 3

输出： 9

解释：给定的硬币可以制造以下金额：
3元硬币产生3的倍数：3, 6, 9, 12, 15等。
6元硬币产生6的倍数：6, 12, 18, 24等。
9元硬币产生9的倍数：9, 18, 27, 36等。
所有硬币合起来可以产生：3, 6, 9, 12, 15等。
```

示例 2：

```text
输入：coins = [5,2], k = 7

输出：12

解释：给定的硬币可以制造以下金额：
5元硬币产生5的倍数：5, 10, 15, 20等。
2元硬币产生2的倍数：2, 4, 6, 8, 10, 12等。
所有硬币合起来可以产生：2, 4, 5, 6, 8, 10, 12, 14, 15等。
```

__提示：__

- `1 <= coins.length <= 15`
- `1 <= coins[i] <= 25`
- `1 <= k <= 2 * 10 ^ 9`
- `coins` 包含两两不同的整数。

__思路:__

```text
二分查找
当 coins 最小值为 1 时, left 最小为 k
right 最大为 min(coins) * k
所以开区间为 (k - 1, min(coins) * k + 1)
求出每一个 lcm
使用 lcm_list[bit | mask] = lcm(lcm_list[mask], coins[i]) 计算
如果当前为偶数个数字组成的 lcm 就需要减去 mid / lcm_list[i]
否则就加上
最后比较总和是否不小于 k
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long findKthSmallest(vector<int>& coins, int k) 
    {
        long long left = k - 1LL, right = *min_element(coins.begin(), coins.end()) * (long long)k + 1LL, mid = -1L;
        int n = coins.size(), total = 1 << n;
        vector<long long> lcm_list(total, 1LL);
        for (int i = 0; i < n; i++) for (int bit = 1 << i, mask = 0; mask < bit; mask++) lcm_list[bit | mask] = lcm(lcm_list[mask], coins[i]);
        auto check = [&](const auto& mid) -> bool
        {
            long long cur = 0LL;
            for (int i = 1; i < total; i++) cur += __builtin_popcount(i) & 1 ? mid / lcm_list[i] : -mid / lcm_list[i];
            return cur >= k;
        };
        while (left + 1 < right)
        {
            if (check(mid = left + right >> 1LL)) right = mid;
            else left = mid;
        }
        return right;
    }
};
```

__Java__:

```Java
class Solution {
    public long findKthSmallest(int[] coins, int k) {
        long left = k - 1L, right = Arrays.stream(coins).min().getAsInt() * (long)k + 1L, mid = -1L, lcmList[] = new long[1 << coins.length];
        lcmList[0] = 1L;
        for (int i = 0, n = coins.length; i < n; i++) for (int bit = 1 << i, mask = 0; mask < bit; mask++) lcmList[mask | bit] = lcm(lcmList[mask], coins[i]);
        while (left + 1 < right) {
            if (check(mid = ((left + right) >>> 1L), lcmList, k)) right = mid;
            else left = mid;
        }
        return right;
    }

    private boolean check(long mid, long[] lcmList, int k) {
        long cur = 0L;
        for (int i = 1, total = lcmList.length; i < total; i++) cur += (Integer.bitCount(i) & 1) == 1 ? mid / lcmList[i] : -mid / lcmList[i];
        return cur >= k;
    }

    private long lcm(long a, long b) {
        return a * b / gcd(a, b);
    }

    private long gcd(long a, long b) {
        return b == 0L ? a : gcd(b, a % b);
    }
}
```

__Python__:

```Python
class Solution:
    def findKthSmallest(self, coins: List[int], k: int) -> int:
        return bisect_left(range(k * min(coins)), True, k, key=lambda mid: sum(mid // lcm(*comb) * pow(-1, r + 1) for r in range(1, len(coins) + 1) for comb in combinations(coins, r)) >= k)
```
