# 1979 Find Greatest Common Divisor of Array 找出数组的最大公约数

__Description:__

Given an integer array `nums`, return _the __greatest common divisor__ of the smallest number and largest number in_ `nums`.

The greatest common divisor of two numbers is the largest positive integer that evenly divides both numbers.

__Example:__

Example 1:

```text
Input: nums = [2,5,6,9,10]
Output: 2
Explanation:
The smallest number in nums is 2.
The largest number in nums is 10.
The greatest common divisor of 2 and 10 is 2.
```

Example 2:

```text
Input: nums = [7,5,6,8,3]
Output: 1
Explanation:
The smallest number in nums is 3.
The largest number in nums is 8.
The greatest common divisor of 3 and 8 is 1.
```

Example 3:

```text
Input: nums = [3,3]
Output: 3
Explanation:
The smallest number in nums is 3.
The largest number in nums is 3.
The greatest common divisor of 3 and 3 is 3.
```

__Constraints:__

- `2 <= nums.length <= 1000`
- `1 <= nums[i] <= 1000`

__题目描述:__

给你一个整数数组 `nums` ，返回数组中最大数和最小数的 __最大公约数__ 。

两个数的 最大公约数 是能够被两个数整除的最大正整数。

__示例:__

示例 1：

```text
输入：nums = [2,5,6,9,10]
输出：2
解释：
nums 中最小的数是 2
nums 中最大的数是 10
2 和 10 的最大公约数是 2
```

示例 2：

```text
输入：nums = [7,5,6,8,3]
输出：1
解释：
nums 中最小的数是 3
nums 中最大的数是 8
3 和 8 的最大公约数是 1
```

示例 3：

```text
输入：nums = [3,3]
输出：3
解释：
nums 中最小的数是 3
nums 中最大的数是 3
3 和 3 的最大公约数是 3
```

__提示：__

- `2 <= nums.length <= 1000`
- `1 <= nums[i] <= 1000`

__思路:__

```text
模拟
先找出数组中的最大值和最小值
然后求最大值和最小值的最大公约数
求 gcd 可以用辗转相除法
时间复杂度为 O(N + logM), 空间复杂度为 O(1), M 为数组元素最大值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findGCD(vector<int>& nums)
    {
        return gcd(*max_element(nums.begin(), nums.end()), *min_element(nums.begin(), nums.end()));
    }
};
```

__Java__:

```Java
class Solution {
    public int findGCD(int[] nums) {
        return gcd(Arrays.stream(nums).max().getAsInt(), Arrays.stream(nums).min().getAsInt());
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

__Python__:

```Python
class Solution:
    def findGCD(self, nums: List[int]) -> int:
        return gcd(max(nums), min(nums))
```
