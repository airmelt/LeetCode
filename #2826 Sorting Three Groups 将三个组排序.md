# 2826 Sorting Three Groups 将三个组排序

__Description:__

You are given an integer array `nums`. Each element in `nums` is 1, 2 or 3. In each operation, you can remove an element from `nums`. Return the __minimum__ number of operations to make `nums` __non-decreasing__.

__Example:__

Example 1:

```text
Input: nums = [2,1,3,2,1]

Output: 3

Explanation:

One of the optimal solutions is to remove nums[0], nums[2] and nums[3].
```

Example 2:

```text
Input: nums = [1,3,2,1,3,3]


Output: 2

Explanation:

One of the optimal solutions is to remove nums[1] and nums[2].
```

Example 3:

```text
Input: nums = [2,2,2,2,3,3]

Output: 0

Explanation:

nums is already non-decreasing.
```

__Constraints:__

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 3`

__题目描述:__

给你一个整数数组 `nums` 。 `nums` 的每个元素是 1，2 或 3。在每次操作中，你可以删除 `nums` 中的一个元素。返回使 nums 成为 __非递减__ 顺序所需操作数的 __最小值__。

__示例:__

示例 1：

```text
输入：nums = [2,1,3,2,1]
输出：3
解释：
其中一个最优方案是删除 nums[0]，nums[2] 和 nums[3]。
```

示例 2：

```text
输入：nums = [1,3,2,1,3,3]
输出：2
解释：
其中一个最优方案是删除 nums[1] 和 nums[2]。
```

示例 3：

```text
输入：nums = [2,2,2,2,3,3]
输出：0
解释：
nums 已是非递减顺序的。
```

__提示：__

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 3`

__进阶:__
你可以使用 `O(n)` 时间复杂度以内的算法解决吗？

__思路:__

```text
动态规划
设 dp[i] 表示以 i 结尾的最长递增子序列
那么结果为 n - max(dp)
dp[i] = max(dp[:i + 1]) + 1
比如 2 结尾的可以由 1 或者 2 转移过来
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumOperations(vector<int>& nums) 
    {
        int n = nums.size(), dp[4]{ 0 };
        for (const auto& num : nums) dp[num] = max({ dp[1], num > 1 ? dp[2] : 0, num > 2 ? dp[3] : 0 }) + 1;
        return n - max({ dp[1], dp[2], dp[3] });
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumOperations(List<Integer> nums) {
        int n = nums.size(), dp[] = new int[4];
        for (int num : nums) dp[num] = Math.max(Math.max(dp[1], num > 1 ? dp[2] : 0), num > 2 ? dp[3] : 0) + 1;
        return n - Math.max(dp[1], Math.max(dp[2], dp[3]));
    }
}
```

__Python__:

```Python
class Solution:
    def minimumOperations(self, nums: List[int]) -> int:
        dp = [0] * 4
        for num in nums:
            dp[num] = max(dp[:num + 1]) + 1
        return len(nums) - max(dp)
```
