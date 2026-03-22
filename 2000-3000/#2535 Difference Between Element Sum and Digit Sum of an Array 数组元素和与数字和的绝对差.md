# 2535 Difference Between Element Sum and Digit Sum of an Array 数组元素和与数字和的绝对差

__Description:__

You are given a positive integer array `nums`.

- The __element sum__ is the sum of all the elements in `nums`.
- The __digit sum__ is the sum of all the digits (not necessarily distinct) that appear in `nums`.

Return _the __absolute__ difference between the __element sum__ and __digit sum__ of_ `nums`.

__Note__ that the absolute difference between two integers `x` and `y` is defined as `|x - y|`.

__Example:__

Example 1:

```text
Input: nums = [1,15,6,3]
Output: 9
Explanation: 
The element sum of nums is 1 + 15 + 6 + 3 = 25.
The digit sum of nums is 1 + 1 + 5 + 6 + 3 = 16.
The absolute difference between the element sum and digit sum is |25 - 16| = 9.
```

Example 2:

```text
Input: nums = [1,2,3,4]
Output: 0
Explanation:
The element sum of nums is 1 + 2 + 3 + 4 = 10.
The digit sum of nums is 1 + 2 + 3 + 4 = 10.
The absolute difference between the element sum and digit sum is |10 - 10| = 0.
```

__Constraints:__

- `1 <= nums.length <= 2000`
- `1 <= nums[i] <= 2000`

__题目描述:__

给你一个正整数数组 `nums` 。

- __元素和__ 是 `nums` 中的所有元素相加求和。
- __数字和__ 是 `nums` 中每一个元素的每一数位（重复数位需多次求和）相加求和。

返回 元素和 与 数字和 的绝对差。

__注意:__

两个整数 `x` 和 `y` 的绝对差定义为 `|x - y|` 。

__示例:__

示例 1：

```text
输入：nums = [1,15,6,3]
输出：9
解释：
nums 的元素和是 1 + 15 + 6 + 3 = 25 。
nums 的数字和是 1 + 1 + 5 + 6 + 3 = 16 。
元素和与数字和的绝对差是 |25 - 16| = 9 。
```

示例 2：

```text
输入：nums = [1,2,3,4]
输出：0
解释：
nums 的元素和是 1 + 2 + 3 + 4 = 10 。
nums 的数字和是 1 + 2 + 3 + 4 = 10 。
元素和与数字和的绝对差是 |10 - 10| = 0 。
```

__提示：__

- `1 <= nums.length <= 2000`
- `1 <= nums[i] <= 2000`

__思路:__

```text
模拟
用数组的和减去数组中每个数的数字和
数组的和一定大于等于数字和
数字和可以用对 10 取模和除法来求
时间复杂度为 O(NlogM), 空间复杂度为 O(1), 其中 M 为 nums[i] 的最大值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int differenceOfSum(vector<int>& nums) 
    {
        int result = accumulate(nums.begin(), nums.end(), 0);
        for (int num : nums) 
        {
            while (num) 
            {
                result -= num % 10;
                num /= 10;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int differenceOfSum(int[] nums) {
        int result = Arrays.stream(nums).sum();
        for (int num : nums) {
            while (num > 0) {
                result -= num % 10;
                num /= 10;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def differenceOfSum(self, nums: List[int]) -> int:
        return sum(nums) - sum(sum(map(int, str(num))) for num in nums)
```
