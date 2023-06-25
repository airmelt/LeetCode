# 1681 Minimum Incompatibility 最小不兼容性

__Description:__

You are given an integer array `nums`​​​ and an integer `k`. You are asked to distribute this array into `k` subsets of __equal size__ such that there are no two equal elements in the same subset.

A subset's incompatibility is the difference between the maximum and minimum elements in that array.

Return _the __minimum possible sum of incompatibilities__ of the_ `k` _subsets after distributing the array optimally, or return_ `-1` _if it is not possible._

A subset is a group integers that appear in the array with no particular order.

__Example:__

Example 1:

```text
Input: nums = [1,2,1,4], k = 2
Output: 4
Explanation: The optimal distribution of subsets is [1,2] and [1,4].
The incompatibility is (2-1) + (4-1) = 4.
Note that [1,1] and [2,4] would result in a smaller sum, but the first subset contains 2 equal elements.
```

Example 2:

```text
Input: nums = [6,3,8,1,3,1,2,2], k = 4
Output: 6
Explanation: The optimal distribution of subsets is [1,2], [2,3], [6,8], and [1,3].
The incompatibility is (2-1) + (3-2) + (8-6) + (3-1) = 6.
```

Example 3:

```text
Input: nums = [5,3,3,6,3,3], k = 3
Output: -1
Explanation: It is impossible to distribute nums into 3 subsets where no two elements are equal in the same subset.
```

__Constraints:__

- `1 <= k <= nums.length <= 16`
- `nums.length` is divisible by `k`
- `1 <= nums[i] <= nums.length`

__题目描述:__

给你一个整数数组 `nums`​​​ 和一个整数 `k` 。你需要将这个数组划分到 `k` 个相同大小的子集中，使得同一个子集里面没有两个相同的元素。

一个子集的 不兼容性 是该子集里面最大值和最小值的差。

请你返回将数组分成 `k` 个子集后，各子集 __不兼容性__ 的 __和__ 的 __最小值__ ，如果无法分成分成 `k` 个子集，返回 `-1` 。

子集的定义是数组中一些数字的集合，对数字顺序没有要求。

__示例:__

示例 1：

```text
输入：nums = [1,2,1,4], k = 2
输出：4
解释：最优的分配是 [1,2] 和 [1,4] 。
不兼容性和为 (2-1) + (4-1) = 4 。
注意到 [1,1] 和 [2,4] 可以得到更小的和，但是第一个集合有 2 个相同的元素，所以不可行。
```

示例 2：

```text
输入：nums = [6,3,8,1,3,1,2,2], k = 4
输出：6
解释：最优的子集分配为 [1,2]，[2,3]，[6,8] 和 [1,3] 。
不兼容性和为 (2-1) + (3-2) + (8-6) + (3-1) = 6 。
```

示例 3：

```text
输入：nums = [5,3,3,6,3,3], k = 3
输出：-1
解释：没办法将这些数字分配到 3 个子集且满足每个子集里没有相同数字。
```

__提示：__

- `1 <= k <= nums.length <= 16`
- `nums.length` 能被 `k` 整除。
- `1 <= nums[i] <= nums.length`

__思路:__

```text
动态规划 ➕ 状态压缩
设 dp[mask] 表示取出的状态为 mask 时能够获得的最小不兼容性
则 dp[mask] = dp[mask ^ sub] + subs[sub], 其中 subs 表示子集 sub 的兼容性取值
先计算符合条件的子集, 需要从原数组中取出 n / k 个不相同的数, 然后记录这 n / k 个数的最大值和最小值之差
初始化 dp[0] = 0, 表示 mask 为 0(取 0 个元素) 时的最小不兼容性为 0
然后取 mask 的子集 sub 如果 sub 在 subs 中出现过, 说明 sub 是一个合法的子集, 如果 mask ^ sub 在 dp 中出现过, 说明 mask ^ sub 是一个合法的状态, 那么就可以更新 dp[mask], 如果 dp[mask] != -1, 或者用哈希表记录 dp[mask] 的值, dp.count(mask) != 0, 那么就可以更新 dp[mask] = min(dp[mask], dp[mask ^ sub] + subs[sub]), 否则 dp[mask] = dp[mask ^ sub] + subs[sub]
时间复杂度为 O(3 ^ N), 空间复杂度为 O(2 ^ N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumIncompatibility(vector<int>& nums, int k) 
    {
        int n = nums.size(), total = 1 << n, m = n / k;
        vector<int> subs(1 << n, -1), dp(1 << n, -1);
        for (int sub = 1; sub < total; sub++) 
        {
            if (__builtin_popcount(sub) == m) 
            {
                set<int> freq;
                bool flag = true;
                for (int j = 0; j < n; j++) 
                {
                    if (sub & (1 << j)) 
                    {
                        if (freq.count(nums[j]))
                        {
                            flag = false;
                            break;
                        }
                        freq.insert(nums[j]);
                    }
                }
                if (flag) subs[sub] = *freq.rbegin() - *freq.begin();
            }
        }
        dp.front() = 0;
        for (int mask = 1; mask < total; mask++) if (!(__builtin_popcount(mask) % m)) for (int sub = mask; sub; sub = (sub - 1) & mask) if (subs[sub] != -1 and dp[mask ^ sub] != -1) dp[mask] = dp[mask] == -1 ? dp[mask ^ sub] + subs[sub] : min(dp[mask], dp[mask ^ sub] + subs[sub]);
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumIncompatibility(int[] nums, int k) {
        int n = nums.length, total = 1 << n, m = n / k;
        if (n == k) return 0;
        Map<Integer, Integer> subs = new HashMap<>(), dp = new HashMap<>();
        for (int sub = 1; sub < total; sub++) {
            if (Integer.bitCount(sub) == m) {
                Set<Integer> freq = new HashSet<>();
                boolean flag = true;
                for (int j = 0; j < n; j++) {
                    if ((sub & (1 << j)) != 0) {
                        if (freq.contains(nums[j])) {
                            flag = false;
                            break;
                        }
                        freq.add(nums[j]);
                    }
                }
                if (flag) subs.put(sub, freq.stream().mapToInt(v -> v).max().getAsInt() - freq.stream().mapToInt(v -> v).min().getAsInt());
            }
        }
        dp.put(0, 0);
        for (int mask = 1; mask < total; mask++) if (Integer.bitCount(mask) % m == 0) for (int sub = mask; sub > 0; sub = (sub - 1) & mask) if (subs.containsKey(sub) && dp.containsKey(mask ^ sub)) dp.put(mask, dp.containsKey(mask) ? Math.min(dp.get(mask), dp.get(mask ^ sub) + subs.get(sub)) : dp.get(mask ^ sub) + subs.get(sub));
        return dp.getOrDefault(total - 1, -1);
    }
}
```

__Python__:

```Python
class Solution:
    def minimumIncompatibility(self, nums: List[int], k: int) -> int:
        if (n := len(nums)) == k:
            return 0
        if k == 1:
            return -1 if len(set(nums)) != n else max(nums) - min(nums)
        subs, dp = defaultdict(int), {0: 0}
        for sub in range(1, 1 << n):
            if bin(sub).count('1') == (m := n // k):
                freq, flag = set(), True
                for j in range(n):
                    if sub & (1 << j):
                        if nums[j] in freq:
                            flag = False
                            break
                        freq.add(nums[j])
                if flag:
                    subs[sub] = max(freq) - min(freq)
        for mask in range(1, 1 << n):
            if not bin(mask).count("1") % m:
                sub = mask
                while sub > 0:
                    if sub in subs and mask ^ sub in dp:
                        dp[mask] = (dp[mask ^ sub] + subs[sub]) if mask not in dp else min(dp[mask], dp[mask ^ sub] + subs[sub])
                    sub = (sub - 1) & mask
        return -1 if (1 << n) - 1 not in dp else dp[(1 << n) - 1]
```
