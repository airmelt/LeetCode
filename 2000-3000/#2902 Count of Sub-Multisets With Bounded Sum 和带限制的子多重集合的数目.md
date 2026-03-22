# 2902 Count of Sub-Multisets With Bounded Sum 和带限制的子多重集合的数目

__Description:__

You are given a __0-indexed__ array `nums` of non-negative integers, and two integers `l` and `r`.

Return _the __count of sub-multisets__ within_ `nums` _where the sum of elements in each subset falls within the inclusive range of_ `[l, r]`.

Since the answer may be large, return it modulo `10 ^ 9 + 7`.

A __sub-multiset__ is an __unordered__ collection of elements of the array in which a given value `x` can occur `0, 1, ..., occ[x]` times, where `occ[x]` is the number of occurrences of `x` in the array.

Note that:

- Two __sub-multisets__ are the same if sorting both sub-multisets results in identical multisets.
- The sum of an __empty__ multiset is `0`.

__Example:__

Example 1:

```text
Input: nums = [1,2,2,3], l = 6, r = 6
Output: 1
Explanation: The only subset of nums that has a sum of 6 is {1, 2, 3}.
```

Example 2:

```text
Input: nums = [2,1,4,2,7], l = 1, r = 5
Output: 7
Explanation: The subsets of nums that have a sum within the range [1, 5] are {1}, {2}, {4}, {2, 2}, {1, 2}, {1, 4}, and {1, 2, 2}.
```

Example 3:

```text
Input: nums = [1,2,1,3,5,2], l = 3, r = 5
Output: 9
Explanation: The subsets of nums that have a sum within the range [3, 5] are {3}, {5}, {1, 2}, {1, 3}, {2, 2}, {2, 3}, {1, 1, 2}, {1, 1, 3}, and {1, 2, 2}.
```

__Constraints:__

- `1 <= nums.length <= 2 * 10 ^ 4`
- `0 <= nums[i] <= 2 * 10 ^ 4`
- Sum of `nums` does not exceed `2 * 10 ^ 4`.
- `0 <= l <= r <= 2 * 10 ^ 4`

__题目描述:__

给你一个下标从 __0__ 开始的非负整数数组 `nums` 和两个整数 `l` 和 `r` 。

请你返回 `nums` 中子多重集合的和在闭区间 `[l, r]` 之间的 __子多重集合的数目__ 。

由于答案可能很大，请你将答案对 `10 ^ 9 + 7` 取余后返回。

__子多重集合__ 指的是从数组中选出一些元素构成的 __无序__ 集合，每个元素 `x` 出现的次数可以是 `0, 1, ..., occ[x]` 次，其中 `occ[x]` 是元素 `x` 在数组中的出现次数。

注意：

- 如果两个子多重集合中的元素排序后一模一样，那么它们两个是相同的 __子多重集合__ 。
- __空__ 集合的和是 `0` 。

__示例:__

示例 1：

```text
输入：nums = [1,2,2,3], l = 6, r = 6
输出：1
解释：唯一和为 6 的子集合是 {1, 2, 3} 。
```

示例 2：

```text
输入：nums = [2,1,4,2,7], l = 1, r = 5
输出：7
解释：和在闭区间 [1, 5] 之间的子多重集合为 {1} ，{2} ，{4} ，{2, 2} ，{1, 2} ，{1, 4} 和 {1, 2, 2} 。
```

示例 3：

```text
输入：nums = [1,2,1,3,5,2], l = 3, r = 5
输出：9
解释：和在闭区间 [3, 5] 之间的子多重集合为 {3} ，{5} ，{1, 2} ，{1, 3} ，{2, 2} ，{2, 3} ，{1, 1, 2} ，{1, 1, 3} 和 {1, 2, 2} 。
```

__提示：__

- `1 <= nums.length <= 2 * 10 ^ 4`
- `0 <= nums[i] <= 2 * 10 ^ 4`
- `nums` 的和不超过 `2 * 10 ^ 4` 。
- `0 <= l <= r <= 2 * 10 ^ 4`

__思路:__

```text
动态规划
本质上是一个多重背包, 看做 k 个物品每个物品可以选 1 次
设 dp[i][j] 表示选 i 种物品值之和为 j 的种类数
那么 dp[i][j] 对于某个数 k, 假设有 v 个
dp[i][j] = dp[i - 1][j - k] + dp[i - 1][j - k * 2] + ... + dp[i - 1][j - k * v]
又 dp[i][j - k] = dp[i - 1][j - k * 2] + dp[i - 1][j - k * 3] + ... + dp[i - 1][j - k * (v + 1)]
所以 dp[i][j] = dp[i - 1][j - k] + dp[i][j - k] - dp[i - 1][j - k * (v + 1)], 若 j - k * (v + 1) < 0, dp[i - 1][j - k * (v + 1)] 为 0
或者可以用同余前缀和优化
设 pre[p] = dp[i - 1][p] + dp[i - 1][p - k] + ...
则 pre[p] = dp[i - 1][p] + pre[p - k]
所以 dp[i - 1][p] = pre[p] - pre[p - k]
可以直接在 dp 上原地计算
先累加 [k, cur] 上所有的 dp[i - 1][j - p * k]
然后反向遍历减去 [k * (v + 1), cur] 上所有的 pre[p - k]
即 dp[j] -= dp[j - k * (v + 1)]
时间复杂度为 O(MN), 空间复杂度为 O(M), 其中 M 为 sum(nums), N 为 len(nums)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countSubMultisets(vector<int>& nums, int l, int r) 
    {
        int MOD = 1e9 + 7, s = min(accumulate(nums.begin(), nums.end(), 0), r), cur = 0, result = 0;
        if (l > s) return 0;
        vector<int> dp(s + 1);
        unordered_map<int, int> c;
        for (const auto& num : nums) ++c[num];
        dp.front() = c[0] + 1;
        c.erase(0);
        for (const auto& [k, v] : c) 
        {
            cur = min(cur + k * v, s);
            for (int j = k; j <= cur; j++) dp[j] = (dp[j] + dp[j - k]) % MOD;
            for (int j = cur; j >= k * (v + 1); j--) dp[j] = (dp[j] - dp[j - k * (v + 1)] + MOD) % MOD;
        }
        for (int i = l; i <= s; i++) result = (result + dp[i]) % MOD;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countSubMultisets(List<Integer> nums, int l, int r) {
        int MOD = 1_000_000_007, s = Math.min(nums.stream().mapToInt(v -> v).sum(), r), dp[] = new int[s + 1], cur = 0, result = 0;
        if (l > s) {
            return 0;
        }
        var c = new HashMap<Integer, Integer>();
        for (int num : nums) c.merge(num, 1, Integer::sum);
        dp[0] = c.getOrDefault(0, 0) + 1;
        c.remove(0);
        for (var entry : c.entrySet()) {
            cur = Math.min(cur + entry.getKey() * entry.getValue(), s);
            for (int j = entry.getKey(); j <= cur; j++) dp[j] = (dp[j] + dp[j - entry.getKey()]) % MOD;
            for (int j = cur; j >= entry.getKey() * (entry.getValue() + 1); j--) dp[j] = (dp[j] - dp[j - entry.getKey() * (entry.getValue() + 1)] + MOD) % MOD;
        }
        for (int i = l; i <= s; i++) result = (result + dp[i]) % MOD;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countSubMultisets(self, nums: List[int], l: int, r: int) -> int:
        if l > (s := sum(nums)):
            return 0
        dp, cur, n = [(c := Counter(nums))[0] + 1] + [0] * (r := min(r, s)), 0, len(nums)
        del c[0]
        for k, v in c.items():
            next_dp, cur = dp[:], min(cur + k * v, r)
            for j in range(k, cur + 1):
                next_dp[j] += next_dp[j - k]
                if j >= (v + 1) * k:
                    next_dp[j] -= dp[j - (v + 1) * k]
            dp = next_dp
        return sum(dp[l:]) % (10 ** 9 + 7)
```
