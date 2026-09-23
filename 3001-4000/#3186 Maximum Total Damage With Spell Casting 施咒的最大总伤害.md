# 3186 Maximum Total Damage With Spell Casting 施咒的最大总伤害

__Description:__

A magician has various spells.

You are given an array `power`, where each element represents the damage of a spell. Multiple spells can have the same damage value.

It is a known fact that if a magician decides to cast a spell with a damage of `power[i]`, they __cannot__ cast any spell with a damage of `power[i] - 2`, `power[i] - 1`, `power[i] + 1`, or `power[i] + 2`.

Each spell can be cast __only once__.

Return the __maximum__ possible _total damage_ that a magician can cast.

__Example:__

Example 1:

```text
Input: power = [1,1,3,4]

Output: 6

Explanation:

The maximum possible damage of 6 is produced by casting spells 0, 1, 3 with damage 1, 1, 4.
```

Example 2:

```text
Input: power = [7,1,6,6]

Output: 13

Explanation:

The maximum possible damage of 13 is produced by casting spells 1, 2, 3 with damage 1, 6, 6.
```

__Constraints:__

- `1 <= power.length <= 10 ^ 5`
- `1 <= power[i] <= 10 ^ 9`

__题目描述:__

一个魔法师有许多不同的咒语。

给你一个数组 `power` ，其中每个元素表示一个咒语的伤害值，可能会有多个咒语有相同的伤害值。

已知魔法师使用伤害值为 `power[i]` 的咒语时，他们就 __不能__ 使用伤害为 `power[i] - 2` ，`power[i] - 1` ，`power[i] + 1` 或者 `power[i] + 2` 的咒语。

每个咒语最多只能被使用 __一次__ 。

请你返回这个魔法师可以达到的伤害值之和的 __最大值__ 。

__示例:__

示例 1：

```text
输入：power = [1,1,3,4]

输出：6

解释：

可以使用咒语 0，1，3，伤害值分别为 1，1，4，总伤害值为 6 。
```

示例 2：

```text
输入：power = [7,1,6,6]

输出：13

解释：

可以使用咒语 1，2，3，伤害值分别为 1，6，6，总伤害值为 13 。
```

__提示：__

- `1 <= hours.length <= 5 * 10 ^ 5`
- `1 <= hours[i] <= 10 ^ 9`

__思路:__

```text
动态规划
设 dp[i + 1] 表示使用不大于能量 i 一共可以获得的最大伤害
那么如果不使用能量 i 的卷轴, dp[i + 1] = dp[i]
如果使用能量 i 的卷轴, 最多还能使用能量 i - 3 的卷轴 p, dp[i + 1] = dp[i - 3] + p * c[p], 其中 c[p] 是 卷轴 p 的个数, 例如示例 1 种卷轴 1 有两个所以 c[1] = 2
记录所有卷轴的数量并从小到大排序伤害的大小
初始化 dp[0] = 0, 不选择卷轴的时候伤害为 0
然后从最小的开始遍历, 更新 dp[i + 1] = max(dp[i], dp[j] + p * c[p]), 其中 j 为不大于 i - 3 的卷轴, 初始化 j = 0
时间复杂度为 O(NlogN), 空间复杂度为 O(N), 瓶颈在排序, 双指针更新 dp 时间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maximumTotalDamage(vector<int>& power) 
    {
        unordered_map<int, int> c;
        for (const auto& p : power) ++c[p];
        vector<pair<int, int>> a(c.begin(), c.end());
        sort(a.begin(), a.end());
        vector<long long> dp(a.size() + 1);
        for (int i = 0, j = 0, p = 0, n = a.size(); i < n; i++) 
        {
            while (a[j].first < a[i].first - 2) ++j;
            dp[i + 1] = max(dp[i], dp[j] + 1LL * a[i].first * c[a[i].first]);
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public long maximumTotalDamage(int[] power) {
        var c = new HashMap<Integer, Integer>();
        for (int p : power) c.merge(p, 1, Integer::sum);
        int n = c.size(), a[] = new int[n], idx = 0;
        for (int k : c.keySet()) a[idx++] = k;
        Arrays.sort(a);
        long[] dp = new long[n + 1];
        for (int i = 0, j = 0; i < n; i++) {
            while (a[j] < a[i] - 2) ++j;
            dp[i + 1] = Math.max(dp[i], dp[j] + (long)a[i] * c.get(a[i]));
        }
        return dp[n];
    }
}
```

__Python__:

```Python
class Solution:
    def maximumTotalDamage(self, power: List[int]) -> int:
        dp, j = [0] * (len(a := sorted(c := Counter(power))) + 1), 0
        for i, p in enumerate(a):
            while a[j] < p - 2:
                j += 1
            dp[i + 1] = max(dp[i], dp[j] + p * c[p])
        return dp[-1]
```
