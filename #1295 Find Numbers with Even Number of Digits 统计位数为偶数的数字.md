# 1295 Find Numbers with Even Number of Digits 统计位数为偶数的数字

__Description__:
Given an array nums of integers, return how many of them contain an even number of digits.

__Example:__

Example 1:

Input: nums = [12,345,2,6,7896]
Output: 2
Explanation:
12 contains 2 digits (even number of digits).
345 contains 3 digits (odd number of digits).
2 contains 1 digit (odd number of digits).
6 contains 1 digit (odd number of digits).
7896 contains 4 digits (even number of digits).
Therefore only 12 and 7896 contain an even number of digits.

Example 2:

Input: nums = [555,901,482,1771]
Output: 1
Explanation:
Only 1771 contains an even number of digits.

__Constraints:__

1 <= nums.length <= 500
1 <= nums[i] <= 10^5

__题目描述__:
给你一个整数数组 nums，请你返回其中位数为 偶数 的数字的个数。

__示例 :__

示例 1：

输入：nums = [12,345,2,6,7896]
输出：2
解释：
12 是 2 位数字（位数为偶数）
345 是 3 位数字（位数为奇数）
2 是 1 位数字（位数为奇数）
6 是 1 位数字 位数为奇数）
7896 是 4 位数字（位数为偶数）  
因此只有 12 和 7896 是位数为偶数的数字

示例 2：

输入：nums = [555,901,482,1771]
输出：1
解释：
只有 1771 是位数为偶数的数字。

__提示：__

1 <= nums.length <= 500
1 <= nums[i] <= 10^5

__思路__:

用对数求数字的位数, 然后和 1取 &找偶数即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findNumbers(vector<int>& nums) 
    {
        int result = 0;
        for (auto num : nums) if (!((int)(log10(num) + 1) & 1)) ++result;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findNumbers(int[] nums) {
        int result = 0;
        for (int num : nums) if (((int)(Math.log10(num) + 1) & 1) == 0) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findNumbers(self, nums: List[int]) -> int:
        return sum((1 for i in nums if not int(math.log10(i) + 1) & 1))
```
