# 2733 Neither Minimum nor Maximum 既不是最小值也不是最大值

__Description:__

Given an integer array `nums` containing __distinct__ __positive__ integers, find and return __any__ number from the array that is neither the __minimum__ nor the __maximum__ value in the array, or __`-1`__ if there is no such number.

Return the selected integer.

__Example:__

Example 1:

```text
Input: nums = [3,2,1,4]
Output: 2
Explanation: In this example, the minimum value is 1 and the maximum value is 4. Therefore, either 2 or 3 can be valid answers.
```

Example 2:

```text
Input: nums = [1,2]
Output: -1
Explanation: Since there is no number in nums that is neither the maximum nor the minimum, we cannot select a number that satisfies the given condition. Therefore, there is no answer.
```

Example 3:

```text
Input: nums = [2,1,3]
Output: 2
Explanation: Since 2 is neither the maximum nor the minimum value in nums, it is the only valid answer.
```

__Constraints:__

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`
- All values in `nums` are distinct

__题目描述:__

给你一个整数数组 `nums` ，数组由 __不同正整数__ 组成，请你找出并返回数组中 __任一__ 既不是 __最小值__ 也不是 __最大值__ 的数字，如果不存在这样的数字，返回 __`-1`__ 。

返回所选整数。

__示例:__

示例 1：

```text
输入：nums = [3,2,1,4]
输出：2
解释：在这个示例中，最小值是 1 ，最大值是 4 。因此，2 或 3 都是有效答案。
```

示例 2：

```text
输入：nums = [1,2]
输出：-1
解释：由于不存在既不是最大值也不是最小值的数字，我们无法选出满足题目给定条件的数字。因此，不存在答案，返回 -1 。
```

示例 3：

```text
输入：nums = [2,1,3]
输出：2
解释：2 既不是最小值，也不是最大值，这个示例只有这一个有效答案。
```

__提示：__

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`
- `nums` 中的所有数字互不相同

__思路:__

```text
数组小于 3 的时候直接返回 -1
1. 排序
排序之后选择中间的数
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
2. 数学
注意到 nums 中的数字各不相同
只需要任取 3 个数返回中间值即可
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findNonMinOrMax(vector<int>& nums) 
    {
        return nums.size() < 3 ? -1 : (nums[0] < max(nums[1], nums[2]) and nums[0] > min(nums[2], nums[1]) ? nums[0] : (nums[1] < max(nums[2], nums[0]) and nums[1] > min(nums[2], nums[0]) ? nums[1] : nums[2]));
    }
};
```

__Java__:

```Java
class Solution 
{
public:
    int findNonMinOrMax(vector<int>& nums) 
    {
        return nums.size() < 3 ? -1 : (nums[0] < max(nums[1], nums[2]) and nums[0] > min(nums[2], nums[1]) ? nums[0] : (nums[1] < max(nums[2], nums[0]) and nums[1] > min(nums[2], nums[0]) ? nums[1] : nums[2]));
    }
};
```

__Python__:

```Python
class Solution 
{
public:
    int findNonMinOrMax(vector<int>& nums) 
    {
        return nums.size() < 3 ? -1 : (nums[0] < max(nums[1], nums[2]) and nums[0] > min(nums[2], nums[1]) ? nums[0] : (nums[1] < max(nums[2], nums[0]) and nums[1] > min(nums[2], nums[0]) ? nums[1] : nums[2]));
    }
};
```
