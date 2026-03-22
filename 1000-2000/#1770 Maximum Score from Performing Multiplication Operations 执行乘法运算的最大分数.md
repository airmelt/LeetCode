# 1770 Maximum Score from Performing Multiplication Operations 执行乘法运算的最大分数

__Description:__

You are given two __0-indexed__ integer arrays `nums` and `multipliers` of size `n` and `m` respectively, where `n >= m`.

You begin with a score of `0`. You want to perform __exactly__ `m` operations. On the `i ^ th` operation (__0-indexed__) you will:

- Choose one integer `x` from __either the start or the end__ of the array `nums`.
- Add `multipliers[i] * x` to your score.
- Remove `x` from `nums`.
- Note that `multipliers[0]` corresponds to the first operation, `multipliers[1]` to the second operation, and so on.

Return _the __maximum__ score after performing_ `m` _operations._

__Example:__

Example 1:

```text
Input: nums = [1,2,3], multipliers = [3,2,1]
Output: 14
Explanation: An optimal solution is as follows:
- Choose from the end, [1,2,3], adding 3 * 3 = 9 to the score.
- Choose from the end, [1,2], adding 2 * 2 = 4 to the score.
- Choose from the end, [1], adding 1 * 1 = 1 to the score.
The total score is 9 + 4 + 1 = 14.
```

Example 2:

```text
Input: nums = [-5,-3,-3,-2,7,1], multipliers = [-10,-5,3,4,6]
Output: 102
Explanation: An optimal solution is as follows:
- Choose from the start, [-5,-3,-3,-2,7,1], adding -5 * -10 = 50 to the score.
- Choose from the start, [-3,-3,-2,7,1], adding -3 * -5 = 15 to the score.
- Choose from the start, [-3,-2,7,1], adding -3 * 3 = -9 to the score.
- Choose from the end, [-2,7,1], adding 1 * 4 = 4 to the score.
- Choose from the end, [-2,7], adding 7 * 6 = 42 to the score. 
The total score is 50 + 15 - 9 + 4 + 42 = 102.
```

__Constraints:__

- `n == nums.length`
- `m == multipliers.length`
- `1 <= m <= 300`
- `m <= n <= 10 ^ 5` ``
- `-1000 <= nums[i], multipliers[i] <= 1000`

__题目描述:__

给你两个长度分别 `n` 和 `m` 的整数数组 `nums` 和 `multipliers` ，其中 `n >= m` ，数组下标 __从 1 开始__ 计数。

初始时，你的分数为 `0` 。你需要执行恰好 `m` 步操作。在第 `i` 步操作（__从 1 开始__ 计数）中，需要:

- 选择数组 `nums` __开头处或者末尾处__ 的整数 `x` 。
- 你获得 `multipliers[i] * x` 分，并累加到你的分数中。
- 将 `x` 从数组 `nums` 中移除。

在执行 `m` 步操作后，返回 __最大__ 分数。

__示例:__

示例 1：

```text
输入：nums = [1,2,3], multipliers = [3,2,1]
输出：14
解释：一种最优解决方案如下：
- 选择末尾处的整数 3 ，[1,2,3] ，得 3 * 3 = 9 分，累加到分数中。
- 选择末尾处的整数 2 ，[1,2] ，得 2 * 2 = 4 分，累加到分数中。
- 选择末尾处的整数 1 ，[1] ，得 1 * 1 = 1 分，累加到分数中。
总分数为 9 + 4 + 1 = 14 。
```

示例 2：

```text
输入：nums = [-5,-3,-3,-2,7,1], multipliers = [-10,-5,3,4,6]
输出：102
解释：一种最优解决方案如下：
- 选择开头处的整数 -5 ，[-5,-3,-3,-2,7,1] ，得 -5 * -10 = 50 分，累加到分数中。
- 选择开头处的整数 -3 ，[-3,-3,-2,7,1] ，得 -3 * -5 = 15 分，累加到分数中。
- 选择开头处的整数 -3 ，[-3,-2,7,1] ，得 -3 * 3 = -9 分，累加到分数中。
- 选择末尾处的整数 1 ，[-2,7,1] ，得 1 * 4 = 4 分，累加到分数中。
- 选择末尾处的整数 7 ，[-2,7] ，得 7 * 6 = 42 分，累加到分数中。
总分数为 50 + 15 - 9 + 4 + 42 = 102 。
```

__提示：__

- `n == nums.length`
- `m == multipliers.length`
- `1 <= m <= 10 ^ 3`
- `m <= n <= 10 ^ 5` ``
- `-1000 <= nums[i], multipliers[i] <= 1000`

__思路:__

```text
动态规划
dp[i][j] 表示从 nums 的开头取 i 个数, 结尾取 j 个数的最大分数
初始化 dp[0][0] = 0 表示取 0 个数的最大分数为 0
dp[i][0] 只能由 dp[i - 1][0] 转移而来, dp[0][j] 只能由 dp[0][j - 1] 转移而来
dp[i][j] 可以由 dp[i - 1][j] 或 dp[i][j - 1] 转移而来
转移方程: 
dp[i][0] = dp[i - 1][0] + nums[i - 1] * multipliers[i - 1]
dp[0][j] = dp[0][j - 1] + nums[n - j] * multipliers[j - 1]
dp[i][j] = max(dp[i - 1][j] + nums[i - 1] * multipliers[i + j - 1],dp[i][j - 1] + nums[n - j] * multipliers[i + j - 1])
最后返回dp[i][j] 中满足 i + j = m 中的最大值
注意到 dp[i] 只与 dp[i - 1] 有关
可以使用滚动数组将空间复杂度优化为 O(m)
定义 dp[i] 表示从 nums 的开头取 i 个数, 结尾取 m - i 个数的最大分数
dp[0] = 0
定义 cur[i] 表示从 nums 的开头取 i + 1 个数, 结尾取 m - i - 1 个数的最大分数
cur[0] = dp[0] + nums[i - 1] * multipliers[i - 1]
cur[j] = max(dp[j] + nums[n - i + j] * multipliers[i - 1], dp[j - 1] + nums[j - 1] * multipliers[i - 1]), j < i
cur[i] = dp[i - 1] + nums[i - 1] * multipliers[i - 1]
时间复杂度为 O(M ^ 2), 空间复杂度为 O(M)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumScore(vector<int>& nums, vector<int>& multipliers) 
    {
        int n = nums.size(), m = multipliers.size();
        vector<int> dp(m + 1);
        for (int i = 1; i <= m; i++)
        {
            vector<int> cur(m + 1);
            cur.front() = dp.front() + nums[n - i] * multipliers[i - 1];
            for (int j = 1; j < i; j++) cur[j] = max(dp[j] + nums[n - i + j] * multipliers[i - 1], dp[j - 1] + nums[j - 1] * multipliers[i - 1]);
            cur[i] = dp[i - 1] + nums[i - 1] * multipliers[i - 1];
            dp = move(cur);
        }
        return *max_element(dp.begin(), dp.end());
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumScore(int[] nums, int[] multipliers) {
        int n = nums.length, m = multipliers.length, dp[] = new int[m + 1], result = Integer.MIN_VALUE;
        for (int i = 1; i <= m; i++) {
            int[] cur = new int[m + 1];
            cur[0] = dp[0] + nums[n - i] * multipliers[i - 1];
            for (int j = 1; j < i; j++) cur[j] = Math.max(dp[j - 1] + nums[j - 1] * multipliers[i - 1], dp[j] + nums[n - i + j] * multipliers[i - 1]);
            cur[i] = dp[i - 1] + nums[i - 1] * multipliers[i - 1];
            dp = cur;
        }
        for (int i = 0; i < m; i++) result = Math.max(result, dp[i]);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumScore(self, nums: List[int], multipliers: List[int]) -> int:
        @lru_cache(len(multipliers))
        def dfs(left, right, idx) -> int:
            if idx >= len(multipliers):
                return 0
            multiplier = multipliers[idx]
            idx += 1
            return max(nums[left] * multiplier + dfs(left + 1, right, idx), nums[right] * multiplier + dfs(left, right - 1, idx))
        return dfs(0, len(nums) - 1, 0)
```
