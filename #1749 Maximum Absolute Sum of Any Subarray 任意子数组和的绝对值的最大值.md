# 1749 Maximum Absolute Sum of Any Subarray 任意子数组和的绝对值的最大值

__Description:__

You are given an integer array `nums`. The __absolute sum__ of a subarray `[numsl, numsl+1, ..., numsr-1, numsr]` is `abs(numsl + numsl+1 + ... + numsr-1 + numsr)`.

Return _the __maximum__ absolute sum of any __(possibly empty)__ subarray of_ `nums`.

Note that `abs(x)` is defined as follows:

- If `x` is a negative integer, then `abs(x) = -x`.
- If `x` is a non-negative integer, then `abs(x) = x`.

__Example:__

Example 1:

```text
Input: nums = [1,-3,2,3,-4]
Output: 5
Explanation: The subarray [2,3] has absolute sum = abs(2+3) = abs(5) = 5.
```

Example 2:

```text
Input: nums = [2,-5,1,-4,3,-2]
Output: 8
Explanation: The subarray [-5,1,-4] has absolute sum = abs(-5+1-4) = abs(-8) = 8.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `-10 ^ 4 <= nums[i] <= 10 ^ 4`

__题目描述:__

给你一个整数数组 `nums` 。一个子数组 `[numsl, numsl+1, ..., numsr-1, numsr]` 的 __和的绝对值__ 为 `abs(numsl + numsl+1 + ... + numsr-1 + numsr)` 。

请你找出 `nums` 中 __和的绝对值__ 最大的任意子数组（ _可能为空_ ），并返回该 __最大值__ 。

`abs(x)` 定义如下:

- 如果 `x` 是负整数，那么 `abs(x) = -x` 。
- 如果 `x` 是非负整数，那么 `abs(x) = x` 。

__示例:__

示例 1：

```text
输入：nums = [1,-3,2,3,-4]
输出：5
解释：子数组 [2,3] 和的绝对值最大，为 abs(2+3) = abs(5) = 5 。
```

示例 2：

```text
输入：nums = [2,-5,1,-4,3,-2]
输出：8
解释：子数组 [-5,1,-4] 和的绝对值最大，为 abs(-5+1-4) = abs(-8) = 8 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `-10 ^ 4 <= nums[i] <= 10 ^ 4`

__思路:__

```text
前缀和 ➕ 贪心
记录数组的前缀和
如果前缀和大于 0 将正数的最大值记录下来
如果前缀和小于 0 将负数的最小值记录下来
最后返回正数的最大值减去负数的最小值
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxAbsoluteSum(vector<int>& nums) 
    {
        int s = 0, plus = 0, minus = 0;
        for (const auto& num : nums) 
        {
            s += num;
            if (s > 0) plus = max(plus, s);
            else minus = min(minus, s);
        }
        return plus - minus;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxAbsoluteSum(int[] nums) {
        int s = 0, plus = 0, minus = 0;
        for (int num : nums) {
            s += num;
            if (s > 0) plus = Math.max(plus, s);
            else minus = Math.min(minus, s);
        }
        return plus - minus;
    }
}
```

__Python__:

```Python
class Solution:
    def maxAbsoluteSum(self, nums: List[int]) -> int:
        s, plus, minus = 0, 0, 0
        for num in nums:
            s += num
            plus, minus = max(s, plus), min(s, minus)
        return plus - minus
```
