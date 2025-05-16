# 2529 Maximum Count of Positive Integer and Negative Integer 正整数和负整数的最大计数

__Description:__

Given an array `nums` sorted in __non-decreasing__ order, return _the maximum between the number of positive integers and the number of negative integers._

- In other words, if the number of positive integers in `nums` is `pos` and the number of negative integers is `neg`, then return the maximum of `pos` and `neg`.

__Note__ that `0` is neither positive nor negative.

__Example:__

Example 1:

```text
Input: nums = [-2,-1,-1,1,2,3]
Output: 3
Explanation: There are 3 positive integers and 3 negative integers. The maximum count among them is 3.
```

Example 2:

```text
Input: nums = [-3,-2,-1,0,0,1,2]
Output: 3
Explanation: There are 2 positive integers and 3 negative integers. The maximum count among them is 3.
```

Example 3:

```text
Input: nums = [5,20,66,1314]
Output: 4
Explanation: There are 4 positive integers and 0 negative integers. The maximum count among them is 4.
```

__Constraints:__

- `1 <= nums.length <= 2000`
- `-2000 <= nums[i] <= 2000`
- `nums` is sorted in a __non-decreasing order__.

__Follow up:__ Can you solve the problem in `O(log(n))` time complexity?

__题目描述:__

给你一个按 __非递减顺序__ 排列的数组 `nums` ，返回正整数数目和负整数数目中的最大值。

- 换句话讲，如果 `nums` 中正整数的数目是 `pos` ，而负整数的数目是 `neg` ，返回 `pos` 和 `neg`二者中的最大值。

__注意:__`0` 既不是正整数也不是负整数。

__示例:__

示例 1：

```text
输入：nums = [-2,-1,-1,1,2,3]
输出：3
解释：共有 3 个正整数和 3 个负整数。计数得到的最大值是 3 。
```

示例 2：

```text
输入：nums = [-3,-2,-1,0,0,1,2]
输出：3
解释：共有 2 个正整数和 3 个负整数。计数得到的最大值是 3 。
```

示例 3：

```text
输入：nums = [5,20,66,1314]
输出：4
解释：共有 4 个正整数和 0 个负整数。计数得到的最大值是 4 。
```

__提示：__

- `1 <= nums.length <= 2000`
- `-2000 <= nums[i] <= 2000`
- `nums` 按 __非递减顺序__ 排列。

__进阶:__

你可以设计并实现时间复杂度为 `O(log(n))` 的算法解决此问题吗？

__思路:__

```text
二分
由于数组是有序的
只需要找到第一个比 0 小的数
和第一个比 0 大的数, 或者第一个比 1 小的数
时间复杂度为 O(logN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumCount(vector<int>& nums) 
    {
        return max(ranges::lower_bound(nums, 0) - nums.begin(), nums.end() - ranges::upper_bound(nums, 0));
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumCount(int[] nums) {
        return (int)Math.max(Arrays.stream(nums).filter(num -> num > 0).count(), Arrays.stream(nums).filter(num -> num < 0).count());
    }
}
```

__Python__:

```Python
class Solution:
    def maximumCount(self, nums: List[int]) -> int:
        return max(bisect_left(nums, 0), len(nums) - bisect_right(nums, 0))
```
