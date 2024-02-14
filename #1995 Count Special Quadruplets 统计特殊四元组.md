# 1995 Count Special Quadruplets 统计特殊四元组

__Description:__

Given a __0-indexed__ integer array `nums`, return _the number of __distinct__ quadruplets_ `(a, b, c, d)` _such that:_

- `nums[a] + nums[b] + nums[c] == nums[d]`, and
- `a < b < c < d`

__Example:__

Example 1:

```text
Input: nums = [1,2,3,6]
Output: 1
Explanation: The only quadruplet that satisfies the requirement is (0, 1, 2, 3) because 1 + 2 + 3 == 6.
```

Example 2:

```text
Input: nums = [3,3,6,4,5]
Output: 0
Explanation: There are no such quadruplets in [3,3,6,4,5].
```

Example 3:

```text
Input: nums = [1,1,1,3,5]
Output: 4
Explanation: The 4 quadruplets that satisfy the requirement are:
```

- (0, 1, 2, 3): 1 + 1 + 1 == 3
- (0, 1, 3, 4): 1 + 1 + 3 == 5
- (0, 2, 3, 4): 1 + 1 + 3 == 5
- (1, 2, 3, 4): 1 + 1 + 3 == 5

__Constraints:__

- `4 <= nums.length <= 50`
- `1 <= nums[i] <= 100`

__题目描述:__

给你一个 __下标从 0 开始__ 的整数数组 `nums` ，返回满足下述条件的 __不同__ 四元组 `(a, b, c, d)` 的 __数目__ :

- `nums[a] + nums[b] + nums[c] == nums[d]` ，且
- `a < b < c < d`

__示例:__

示例 1：

```text
输入：nums = [1,2,3,6]
输出：1
解释：满足要求的唯一一个四元组是 (0, 1, 2, 3) 因为 1 + 2 + 3 == 6 。
```

示例 2：

```text
输入：nums = [3,3,6,4,5]
输出：0
解释：[3,3,6,4,5] 中不存在满足要求的四元组。
```

示例 3：

```text
输入：nums = [1,1,1,3,5]
输出：4
解释：满足要求的 4 个四元组如下：
```

- (0, 1, 2, 3): 1 + 1 + 1 == 3
- (0, 1, 3, 4): 1 + 1 + 3 == 5
- (0, 2, 3, 4): 1 + 1 + 3 == 5
- (1, 2, 3, 4): 1 + 1 + 3 == 5

__提示：__

- `4 <= nums.length <= 50`
- `1 <= nums[i] <= 100`

__思路:__

```text
1. 暴力法
由于 N <= 50, 可以使用暴力法求解
时间复杂度为 O(N ^ 4), 空间复杂度为 O(1)
2. 哈希表
将 nums[a] + nums[b] + nums[c] = nums[d] 移项
可得 nums[a] + nums[b] = nums[d] - nums[c]
枚举 c(b + 1), 记录 nums[d] - nums[c] 的个数, 由于可能存在负数, 可以将其加上一个较大的数, 使其变为正数
然后枚举 a([0, b)), 计算 nums[a] + nums[b] 的个数
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N ^ 2)
3. 动态规划
设 dp[i][j][k] 表示前 i 个数中选和为 j 使用 k 个数的方案数
dp[0][0][0] = 1 表示不选任何数和为 0 的方案数为 1
若不选 nums[i - 1], 有 dp[i - 1][j][k] 种方案
若选 nums[i - 1], 有 dp[i - 1][j - nums[i - 1]][k - 1] 种方案
由于 dp[i] 只与 dp[i - 1] 有关, 可以使用滚动数组优化空间
时间复杂度为 O(NMK), 空间复杂度为 O(MK), K = 4 为选数的个数, M = 110 为 nums[i] 的最大值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countQuadruplets(vector<int>& nums) 
    {
        int n = nums.size(), result = 0;
        vector<vector<int>> dp(110, vector<int>(4));
        dp.front().front() = 1;
        for (int i = 1; i <= n; i++) 
        {
            result += dp[nums[i - 1]][3];
            for (int j = 109; j > -1; j--) for (int k = 3; k > 0; k--) if (j - nums[i - 1] > -1) dp[j][k] += dp[j - nums[i - 1]][k - 1];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countQuadruplets(int[] nums) {
        int n = nums.length, result = 0, count[] = new int[10010];
        for (int b = n - 3; b >= 1; b--) {
            for (int d = b + 2; d < n; d++) ++count[nums[d] - nums[b + 1] + 200];
            for (int a = 0; a < b; a++) result += count[nums[a] + nums[b] + 200];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countQuadruplets(self, nums: List[int]) -> int:
        return sum(nums[i] + nums[j] + nums[k] == nums[t] for i in range(len(nums)) for j in range(i + 1, len(nums)) for k in range(j + 1, len(nums)) for t in range(k + 1, len(nums)))
```
