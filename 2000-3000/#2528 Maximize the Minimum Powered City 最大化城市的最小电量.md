# 2528 Maximize the Minimum Powered City 最大化城市的最小电量

__Description:__

You are given a __0-indexed__ integer array `stations` of length `n`, where `stations[i]` represents the number of power stations in the `i ^ th` city.

Each power station can provide power to every city in a fixed __range__. In other words, if the range is denoted by `r`, then a power station at city `i` can provide power to all cities `j` such that `|i - j| <= r` and `0 <= i, j <= n - 1`.

- Note that `|x|` denotes __absolute__ value. For example, `|7 - 5| = 2` and `|3 - 10| = 7`.

The power of a city is the total number of power stations it is being provided power from.

The government has sanctioned building `k` more power stations, each of which can be built in any city, and have the same range as the pre-existing ones.

Given the two integers `r` and `k`, return _the __maximum possible minimum power__ of a city, if the additional power stations are built optimally._

__Note__ that you can build the `k` power stations in multiple cities.

__Example:__

Example 1:

```text
Input: stations = [1,2,4,5,0], r = 1, k = 2
Output: 5
Explanation: 
One of the optimal ways is to install both the power stations at city 1. 
So stations will become [1,4,4,5,0].
```

- City 0 is provided by 1 + 4 = 5 power stations.
- City 1 is provided by 1 + 4 + 4 = 9 power stations.
- City 2 is provided by 4 + 4 + 5 = 13 power stations.
- City 3 is provided by 5 + 4 = 9 power stations.
- City 4 is provided by 5 + 0 = 5 power stations.

So the minimum power of a city is 5.

Since it is not possible to obtain a larger power, we return 5.

Example 2:

```text
Input: stations = [4,4,4,4], r = 0, k = 3
Output: 4
Explanation: 
It can be proved that we cannot make the minimum power of a city greater than 4.
```

__Constraints:__

- `n == stations.length`
- `1 <= n <= 10 ^ 5`
- `0 <= stations[i] <= 10 ^ 5`
- `0 <= r <= n - 1`
- `0 <= k <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始长度为 `n` 的整数数组 `stations` ，其中 `stations[i]` 表示第 `i` 座城市的供电站数目。

每个供电站可以在一定 __范围__ 内给所有城市提供电力。换句话说，如果给定的范围是 `r` ，在城市 `i` 处的供电站可以给所有满足 `|i - j| <= r` 且 `0 <= i, j <= n - 1` 的城市 `j` 供电。

- `|x|` 表示 `x` 的 __绝对值__ 。比方说， `|7 - 5| = 2` ， `|3 - 10| = 7` 。

一座城市的 电量 是所有能给它供电的供电站数目。

政府批准了可以额外建造 `k` 座供电站，你需要决定这些供电站分别应该建在哪里，这些供电站与已经存在的供电站有相同的供电范围。

给你两个整数 `r` 和 `k` ，如果以最优策略建造额外的发电站，返回所有城市中，最小电量的最大值是多少。

这 `k` 座供电站可以建在多个城市。

__示例:__

示例 1：

```text
输入：stations = [1,2,4,5,0], r = 1, k = 2
输出：5
解释：
最优方案之一是把 2 座供电站都建在城市 1 。
每座城市的供电站数目分别为 [1,4,4,5,0] 。
```

- 城市 0 的供电站数目为 1 + 4 = 5 。
- 城市 1 的供电站数目为 1 + 4 + 4 = 9 。
- 城市 2 的供电站数目为 4 + 4 + 5 = 13 。
- 城市 3 的供电站数目为 5 + 4 = 9 。
- 城市 4 的供电站数目为 5 + 0 = 5 。

供电站数目最少是 5 。

无法得到更优解，所以我们返回 5 。

示例 2：

```text
输入：stations = [4,4,4,4], r = 0, k = 3
输出：4
解释：
无论如何安排，总有一座城市的供电站数目是 4 ，所以最优解是 4 。
```

__提示：__

- `n == stations.length`
- `1 <= n <= 10 ^ 5`
- `0 <= stations[i] <= 10 ^ 5`
- `0 <= r <= n - 1`
- `0 <= k <= 10 ^ 9`

__思路:__

```text
二分查找 ➕ 差分数组
需要求最小的最大适合用二分查找
先计算每个城市的电量
使用前缀和计算每个城市的电量
二分查找的左边界为 min(stations)
右边界为 min(stations) + k
都是闭区间
由于增加变电站会使得区间的电量增加
使用差分数组来计算每个城市的电量
累计差分数组的值
如果当前城市的电量小于 mid
那么就需要增加电量
贪心的把电量增加在 min(i + r, n - 1) 的位置
那么贪心的边界就是 i + (r << 1) + 1
时间复杂度为 O(NlogK), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maxPower(vector<int>& stations, int r, int k) 
    {
        int n = stations.size();
        vector<long long> pre(n + 1), powers(n);
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + stations[i];
        for (int i = 0; i < n; i++) powers[i] = pre[min(i + r + 1, n)] - pre[max(i - r, 0)];
        long left = 0LL, right = pre[n] + k, result = 0LL, mid = 0LL;
        auto check = [&](long long mid) -> bool
        {
            vector<long long> diff(n);
            long long s = 0LL, cur = 0LL, m = 0LL;
            for (int i = 0; i < n; i++) 
            {
                s += diff[i];
                if ((m = mid - powers[i] - s) > 0LL) 
                {
                    if ((cur += m) > k) return false;
                    s += m;
                    if ((i + (r << 1) + 1) < n) diff[i + (r << 1) + 1] -= m;
                }
            }
            return true;
        };
        while (left <= right) 
        {
            if (check(mid = left + ((right - left) >> 1LL))) 
            {
                result = max(result, mid);
                left = mid + 1LL;
            } 
            else right = mid - 1LL;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long maxPower(int[] stations, int r, int k) {
        int n = stations.length;
        long pre[] = new long[n + 1], powers[] = new long[n];
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + stations[i];
        for (int i = 0; i < n; i++) powers[i] = pre[Math.min(i + r + 1, n)] - pre[Math.max(i - r, 0)];
        long left = 0L, right = pre[n] + k, result = 0L, mid = 0L;
        while (left <= right) {
            if (check((mid = left + ((right - left) >>> 1)), powers, k, r, n)) {
                result = Math.max(result, mid);
                left = mid + 1L;
            } else right = mid - 1L;
        }
        return result;
    }

    private boolean check(long mid, long[] powers, int k, int r, int n) {
        long diff[] = new long[n], s = 0L, cur = 0L, m = 0L;
        for (int i = 0; i < n; i++) {
            s += diff[i];
            if ((m = mid - powers[i] - s) > 0L) {
                if ((cur += m) > k) return false;
                s += m;
                if ((i + (r << 1) + 1) < n) diff[i + (r << 1) + 1] -= m;
            }
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def maxPower(self, stations: List[int], r: int, k: int) -> int:
        n, pre = len(stations), [0] + list(accumulate(stations))
        stations = [pre[min(i + r + 1, n)] - pre[max(i - r, 0)] for i in range(n)]
        right, result = (left := min(stations)) + k, left

        def check(mid: int) -> bool:
            diff, cur, s = [0] * n, 0, 0
            for i, power in enumerate(stations):
                s += diff[i]
                if (m := mid - power - s) > 0:
                    if (cur := cur + m) > k:
                        return False
                    s += m
                    if i + (r << 1) + 1 < n:
                        diff[i + (r << 1) + 1] -= m
            return True
        while left <= right:
            if check(mid := left + ((right - left) >> 1)):
                result, left = max(result, mid), mid + 1
            else:
                right = mid - 1
        return result
```
