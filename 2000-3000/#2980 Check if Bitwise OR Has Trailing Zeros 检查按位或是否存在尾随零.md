# 2980 Check if Bitwise OR Has Trailing Zeros 检查按位或是否存在尾随零

__Description:__

You are given an array of __positive__ integers `nums`.

You have to check if it is possible to select __two or more__ elements in the array such that the bitwise `OR` of the selected elements has __at least__ one trailing zero in its binary representation.

For example, the binary representation of `5`, which is `"101"`, does not have any trailing zeros, whereas the binary representation of `4`, which is `"100"`, has two trailing zeros.

Return `true` _if it is possible to select two or more elements whose bitwise_ `OR` _has trailing zeros, return_ `false` _otherwise_.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,4,5]
Output: true
Explanation: If we select the elements 2 and 4, their bitwise OR is 6, which has the binary representation "110" with one trailing zero.
```

Example 2:

```text
Input: nums = [2,4,8,16]
Output: true
Explanation: If we select the elements 2 and 4, their bitwise OR is 6, which has the binary representation "110" with one trailing zero.
Other possible ways to select elements to have trailing zeroes in the binary representation of their bitwise OR are: (2, 8), (2, 16), (4, 8), (4, 16), (8, 16), (2, 4, 8), (2, 4, 16), (2, 8, 16), (4, 8, 16), and (2, 4, 8, 16).
```

Example 3:

```text
Input: nums = [1,3,5,7,9]
Output: false
Explanation: There is no possible way to select two or more elements to have trailing zeros in the binary representation of their bitwise OR.
```

__Constraints:__

- `2 <= nums.length <= 100`
- `1 <= nums[i] <= 100`

__题目描述:__

给你一个 __正整数__ 数组 `nums` 。

你需要检查是否可以从数组中选出 __两个或更多__ 元素，满足这些元素的按位或运算（ `OR`）结果的二进制表示中 __至少__ 存在一个尾随零。

例如，数字 `5` 的二进制表示是 `"101"`，不存在尾随零，而数字 `4` 的二进制表示是 `"100"`，存在两个尾随零。

如果可以选择两个或更多元素，其按位或运算结果存在尾随零，返回 `true`；否则，返回 `false` 。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,4,5]
输出：true
解释：如果选择元素 2 和 4，按位或运算结果是 6，二进制表示为 "110" ，存在一个尾随零。
```

示例 2：

```text
输入：nums = [2,4,8,16]
输出：true
解释：如果选择元素 2 和 4，按位或运算结果是 6，二进制表示为 "110"，存在一个尾随零。
其他按位或运算结果存在尾随零的可能选择方案包括：(2, 8), (2, 16), (4, 8), (4, 16), (8, 16), (2, 4, 8), (2, 4, 16), (2, 8, 16), (4, 8, 16), 以及 (2, 4, 8, 16) 。
```

示例 3：

```text
输入：nums = [1,3,5,7,9]
输出：false
解释：不存在按位或运算结果存在尾随零的选择方案。
```

__提示：__

- `2 <= nums.length <= 100`
- `1 <= nums[i] <= 100`

__思路:__

```text
模拟
检查数组中是否有至少两个偶数
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool hasTrailingZeros(vector<int>& nums) 
    {
        return count_if(nums.begin(), nums.end(), [](int num) { return !(num & 1); }) > 1;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean hasTrailingZeros(int[] nums) {
        return Arrays.stream(nums).filter(num -> (num & 1) == 0).count() > 1;
    }
}
```

__Python__:

```Python
class Solution:
    def hasTrailingZeros(self, nums: List[int]) -> bool:
        return sum(not num & 1 for num in nums) > 1
```
