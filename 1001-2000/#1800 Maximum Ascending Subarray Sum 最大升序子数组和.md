# 1800 Maximum Ascending Subarray Sum 最大升序子数组和

__Description:__

Given an array of positive integers `nums`, return the _maximum possible sum of an __ascending__ subarray in_ `nums`.

A subarray is defined as a contiguous sequence of numbers in an array.

A subarray `[numsl, numsl+1, ..., numsr-1, numsr]` is __ascending__ if for all `i` where `l <= i < r`, `numsi < numsi+1`. Note that a subarray of size `1` is __ascending__.

__Example:__

Example 1:

```text
Input: nums = [10,20,30,5,10,50]
Output: 65
Explanation: [5,10,50] is the ascending subarray with the maximum sum of 65.
```

Example 2:

```text
Input: nums = [10,20,30,40,50]
Output: 150
Explanation: [10,20,30,40,50] is the ascending subarray with the maximum sum of 150.
```

Example 3:

```text
Input: nums = [12,17,15,13,10,11,12]
Output: 33
Explanation: [10,11,12] is the ascending subarray with the maximum sum of 33.
```

__Constraints:__

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`

__题目描述:__

给你一个正整数组成的数组 `nums` ，返回 `nums` 中一个 __升序__ 子数组的最大可能元素和。

子数组是数组中的一个连续数字序列。

已知子数组 `[numsl, numsl+1, ..., numsr-1, numsr]` ，若对所有 `i`（ `l <= i < r`）， `numsi < numsi+1` 都成立，则称这一子数组为 __升序__ 子数组。注意，大小为 `1` 的子数组也视作 __升序__ 子数组。

__示例:__

示例 1：

```text
输入：nums = [10,20,30,5,10,50]
输出：65
解释：[5,10,50] 是元素和最大的升序子数组，最大元素和为 65 。
```

示例 2：

```text
输入：nums = [10,20,30,40,50]
输出：150
解释：[10,20,30,40,50] 是元素和最大的升序子数组，最大元素和为 150 。
```

示例 3：

```text
输入：nums = [12,17,15,13,10,11,12]
输出：33
解释：[10,11,12] 是元素和最大的升序子数组，最大元素和为 33 。
```

示例 4：

```text
输入：nums = [100,10,1]
输出：100
```

__提示：__

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`

__思路:__

```text
模拟
用一个变量记录当前连续升序的子数组之和
如果后一个数大于前一个数就累加
否则更新成当前元素
结果和当前子数组之和比较再更新
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxAscendingSum(vector<int>& nums) 
    {
        int result = nums.front(), cur = nums.front(), n = nums.size();
        for (int i = 1; i < n; i++) 
        {
            cur = nums[i] > nums[i - 1] ? cur + nums[i] : nums[i];
            result = max(result, cur);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxAscendingSum(int[] nums) {
        int result = nums[0], cur = nums[0], n = nums.length;
        for (int i = 1; i < n; i++) {
            cur = nums[i] > nums[i - 1] ? cur + nums[i] : nums[i];
            result = Math.max(result, cur);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxAscendingSum(self, nums: List[int]) -> int:
        result, cur = nums[0], nums[0]
        for last, num in pairwise(nums):
            cur = cur + num if last < num else num
            result = max(result, cur)
        return result
```
