# 3098 Find the Sum of Subsequence Powers 求出所有子序列的能量和

__Description:__

You are given an integer array `nums` of length `n`, and a __positive__ integer `k`.

The power of a subsequence is defined as the minimum absolute difference between any two elements in the subsequence.

Return _the __sum__ of __powers__ of __all__ subsequences of_ `nums` _which have length_ ___equal to___ `k`.

Since the answer may be large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,4], k = 3

Output: 4

Explanation:
```

There are 4 subsequences in `nums` which have length 3: `[1,2,3]`, `[1,3,4]`, `[1,2,4]`, and `[2,3,4]`. The sum of powers is `|2 - 3| + |3 - 4| + |2 - 1| + |3 - 4| = 4`.

Example 2:

```text
Input: nums = [2,2], k = 2

Output: 0

Explanation:
```

The only subsequence in `nums` which has length 2 is `[2,2]`. The sum of powers is `|2 - 2| = 0`.

Example 3:

```text
Input: nums = [4,3,-1], k = 2

Output: 10

Explanation:
```

There are 3 subsequences in `nums` which have length 2: `[4,3]`, `[4,-1]`, and `[3,-1]`. The sum of powers is `|4 - 3| + |4 - (-1)| + |3 - (-1)| = 10`.

__Constraints:__

- `2 <= n == nums.length <= 50`
- `-10 ^ 8 <= nums[i] <= 10 ^ 8`
- `2 <= k <= n`

__题目描述:__

给你一个长度为 `n` 的整数数组 `nums` 和一个 __正__ 整数 `k` 。

一个 子序列 的 能量 定义为子序列中 任意 两个元素的差值绝对值的 最小值 。

请你返回 `nums` 中长度 __等于__ `k` 的 __所有__ 子序列的 __能量和__ 。

由于答案可能会很大，将答案对 `10 ^ 9 + 7` __取余__ 后返回。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,4], k = 3

输出：4

解释：
```

`nums` 中总共有 4 个长度为 3 的子序列: `[1,2,3]` ， `[1,3,4]` ， `[1,2,4]` 和 `[2,3,4]` 。能量和为 `|2 - 3| + |3 - 4| + |2 - 1| + |3 - 4| = 4` 。

示例 2：

```text
输入：nums = [2,2], k = 2

输出：0

解释：
```

`nums` 中唯一一个长度为 2 的子序列是 `[2,2]` 。能量和为 `|2 - 2| = 0` 。

示例 3：

```text
输入：nums = [4,3,-1], k = 2

输出：10

解释：
```

`nums` 总共有 3 个长度为 2 的子序列: `[4,3]` ， `[4,-1]` 和 `[3,-1]` 。能量和为 `|4 - 3| + |4 - (-1)| + |3 - (-1)| = 10` 。

__提示：__

- `2 <= n == nums.length <= 50`
- `-10 ^ 8 <= nums[i] <= 10 ^ 8`
- `2 <= k <= n`

__思路:__

```text
动态规划
由于子序列和数组顺序无关, 可以将数组排序
设 dp[i][j][diff] 表示 nums[:i] 中长度为 j 的子序列的最大差值为 diff 时的数量
那么枚举 [0, i - 1] 的 a
假设上一个子序列长度为 j - 1, 枚举长度 [2, k]
令 next_diff = min(diff, nums[i] - nums[a])
则有转移方程 dp[i][j][next_diff] += dp[a][j - 1][diff]
所以需要记录所有的 diff 的情况
这里使用哈希表记录
最后将所有 dp[i][k] 加起来即可
时间复杂度为 O(KN ^ 4), 空间复杂度为 O(KN ^ 3)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int sumOfPowers(vector<int>& nums, int k) 
    {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        long long MOD = 1e9 + 7LL, result = 0LL;
        unordered_map<int, long long> dp[51][51];
        for (int i = 0; i < n; i++)
        {
            dp[i][1][MOD] = 1LL;
            for (int j = 2; j <= k; j++) for (int a = 0; a < i; a++) for (const auto& [diff, c] : dp[a][j - 1]) dp[i][j][min(diff, nums[i] - nums[a])] = (dp[i][j][min(diff, nums[i] - nums[a])] + c) % MOD;
        }
        for (int i = 0; i < n; i++) for (const auto& [diff, c] : dp[i][k]) result = (result + diff * c) % MOD;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int sumOfPowers(int[] nums, int k) {
        Arrays.sort(nums);
        int n = nums.length;
        long MOD = 1_000_000_007L, result = 0L;
        Map<Integer, Long>[][] dp = new HashMap[51][51];
        for (int i = 0; i < 51; i++) for (int j = 0; j < 51; j++) dp[i][j] = new HashMap<>();
        for (int i = 0; i < n; i++)
        {
            dp[i][1].put((int)MOD, 1L);
            for (int j = 2; j <= k; j++) for (int a = 0; a < i; a++) for (var entry : dp[a][j - 1].entrySet()) dp[i][j].put(Math.min(entry.getKey(), nums[i] - nums[a]), (dp[i][j].getOrDefault(Math.min(entry.getKey(), nums[i] - nums[a]), 0L) + entry.getValue()) % MOD);
        }
        for (int i = 0; i < n; i++) for (var entry : dp[i][k].entrySet()) result = (result + entry.getKey() * entry.getValue()) % MOD;
        return (int)result;
    }
}
```

__Python__:

```Python
class Solution:
    def sumOfPowers(self, nums: List[int], k: int) -> int:
        nums.sort()
        n, MOD = len(nums), 10 ** 9 + 7
        dp = [[Counter() for _ in range(k + 1)] for _ in range(n)]
        for i, num in enumerate(nums):
            dp[i][1][inf] = 1
            for j in range(2, k + 1):
                for a in range(i):
                    for diff, c in dp[a][j - 1].items():
                        dp[i][j][min(diff, num - nums[a])] += c
        return sum(diff * c for i in range(n) for diff, c in dp[i][k].items()) % MOD
```
