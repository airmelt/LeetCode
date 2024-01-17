# 1959 Minimum Total Space Wasted With K Resizing Operations K 次调整数组大小浪费的最小总空间

__Description:__

You are currently designing a dynamic array. You are given a __0-indexed__ integer array `nums`, where `nums[i]` is the number of elements that will be in the array at time `i`. In addition, you are given an integer `k`, the __maximum__ number of times you can __resize__ the array (to __any__ size).

The size of the array at time `t`, `sizet`, must be at least `nums[t]` because there needs to be enough space in the array to hold all the elements. The __space wasted__ at time `t` is defined as `sizet - nums[t]`, and the __total__ space wasted is the __sum__ of the space wasted across every time `t` where `0 <= t < nums.length`.

Return _the __minimum__ __total space wasted__ if you can resize the array at most_ `k` _times_.

Note: The array can have any size at the start and does not count towards the number of resizing operations.

__Example:__

Example 1:

```text
Input: nums = [10,20], k = 0
Output: 10
Explanation: size = [20,20].
We can set the initial size to be 20.
The total wasted space is (20 - 10) + (20 - 20) = 10.
```

Example 2:

```text
Input: nums = [10,20,30], k = 1
Output: 10
Explanation: size = [20,20,30].
We can set the initial size to be 20 and resize to 30 at time 2. 
The total wasted space is (20 - 10) + (20 - 20) + (30 - 30) = 10.
```

Example 3:

```text
Input: nums = [10,20,15,30,20], k = 2
Output: 15
Explanation: size = [10,20,20,30,30].
We can set the initial size to 10, resize to 20 at time 1, and resize to 30 at time 3.
The total wasted space is (10 - 10) + (20 - 20) + (20 - 15) + (30 - 30) + (30 - 20) = 15.
```

__Constraints:__

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 10 ^ 6`
- `0 <= k <= nums.length - 1`

__题目描述:__

你正在设计一个动态数组。给你一个下标从 __0__ 开始的整数数组 `nums` ，其中 `nums[i]` 是 `i` 时刻数组中的元素数目。除此以外，你还有一个整数 `k` ，表示你可以 __调整__ 数组大小的 __最多__ 次数（每次都可以调整成 __任意__ 大小）。

`t` 时刻数组的大小 `sizet` 必须大于等于 `nums[t]` ，因为数组需要有足够的空间容纳所有元素。 `t` 时刻 __浪费的空间__ 为 `sizet - nums[t]` ，__总__ 浪费空间为满足 `0 <= t < nums.length` 的每一个时刻 `t` 浪费的空间 __之和__ 。

在调整数组大小不超过 `k` 次的前提下，请你返回 __最小总浪费空间__ 。

注意：数组最开始时可以为 任意大小 ，且 不计入 调整大小的操作次数。

__示例:__

示例 1：

```text
输入：nums = [10,20], k = 0
输出：10
解释：size = [20,20].
我们可以让数组初始大小为 20 。
总浪费空间为 (20 - 10) + (20 - 20) = 10 。
```

示例 2：

```text
输入：nums = [10,20,30], k = 1
输出：10
解释：size = [20,20,30].
我们可以让数组初始大小为 20 ，然后时刻 2 调整大小为 30 。
总浪费空间为 (20 - 10) + (20 - 20) + (30 - 30) = 10 。
```

示例 3：

```text
输入：nums = [10,20,15,30,20], k = 2
输出：15
解释：size = [10,20,20,30,30].
我们可以让数组初始大小为 10 ，时刻 1 调整大小为 20 ，时刻 3 调整大小为 30 。
总浪费空间为 (10 - 10) + (20 - 20) + (20 - 15) + (30 - 30) + (30 - 20) = 15 。
```

__提示：__

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 10 ^ 6`
- `0 <= k <= nums.length - 1`

__思路:__

```text
动态规划
题意实际上是要将数组最多分成 k + 1 段, 并求出每一段的最大值和长度的乘积再减去这一段的总和
设 dp[i][j] 表示将数组 nums[0..i] 最多分成 j + 1 段的最小总浪费空间
最终答案为 dp[n - 1][k + 1]
dp[i][j] = min(dp[i][j], dp[p - 1][j - 1] + values[p][i]), 其中 values[p][i] 表示 nums[p..i] 的最大值乘以长度减去 nums[p..i] 的总和, 其中 0 <= p <= i
初始化 dp[i][j] = inf
计算 dp[-1][j] 时, 取 0, 看作 0 个元素分成 j 组
预处理 values 数组, 使得计算 values[p][i] 的时间复杂度为 O(1)
时间复杂度为 O(KN ^ 2), 空间复杂度为 O(N(N + K))
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minSpaceWastedKResizing(vector<int>& nums, int k) 
    {
        int n = nums.size(), INF = 0x3f3f3f3f;
        vector<vector<int>> values(n, vector<int>(n)), dp(n, vector<int>(k + 2, INF));
        for (int i = 0; i < n; i++) for (int j = i, total = nums[i], best = nums[i]; j < n; j++, total += j < n ? nums[j] : 0, best = max(best, j < n ? nums[j] : 0)) values[i][j] = best * (j - i + 1) - total;
        for (int i = 0; i < n; i++) for (int p = 0; p < i + 1; p++) for (int j = 1; j < k + 2; j++) dp[i][j] = min(dp[i][j], (!p ? 0 : dp[p - 1][j - 1]) + values[p][i]);
        return dp.back().back();
    }
};
```

__Java__:

```Java
class Solution {
    public int minSpaceWastedKResizing(int[] nums, int k) {
        int n = nums.length, INF = 0x3f3f3f3f, values[][] = new int[n][n], dp[][] = new int[n][k + 2];
        for (int i = 0; i < n; i++) Arrays.fill(dp[i], INF);
        for (int i = 0; i < n; i++) for (int j = i, total = nums[i], best = nums[i]; j < n; j++, total += j < n ? nums[j] : 0, best = Math.max(best, j < n ? nums[j] : 0)) values[i][j] = best * (j - i + 1) - total;
        for (int i = 0; i < n; i++) for (int p = 0; p < i + 1; p++) for (int j = 1; j < k + 2; j++) dp[i][j] = Math.min(dp[i][j], (p == 0 ? 0 : dp[p - 1][j - 1]) + values[p][i]);
        return dp[n - 1][k + 1];
    }
}
```

__Python__:

```Python
class Solution:
    def minSpaceWastedKResizing(self, nums: List[int], k: int) -> int:
        n, pre = len(nums), [0] + list(accumulate(nums))
        max_values, dp = [[0] * i + [max(nums[i:j + 1]) for j in range(i, n)] for i in range(n)], [[inf] * (k + 2) for _ in range(n)]
        values = [[0] * i + [max_values[i][j] * (j - i + 1) - pre[j + 1] + pre[i] for j in range(i, n)] for i in range(n)] 
        for i in range(n):
            for j in range(1, k + 2):
                for p in range(i + 1):
                    dp[i][j] = min(dp[i][j], (0 if p == 0 else dp[p - 1][j - 1]) + values[p][i])
        return dp[-1][-1]
```
