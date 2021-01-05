# 268 Missing Number 缺失数字

__Description__:
Given an array containing n distinct numbers taken from 0, 1, 2, ..., n, find the one that is missing from the array.

**Example :**

Example 1:
Input: [3,0,1]
Output: 2

Example 2:
Input: [9,6,4,2,3,5,7,0,1]
Output: 8

__Note:__
Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?

__题目描述__:
给定一个包含 0, 1, 2, ..., n 中 n 个数的序列，找出 0 .. n 中没有出现在序列中的那个数。

**示例 :**

示例 1:
输入: [3,0,1]
输出: 2

示例 2:
输入: [9,6,4,2,3,5,7,0,1]
输出: 8

__说明：__
你的算法应具有线性时间复杂度。你能否仅使用额外常数空间来实现?

__思路__:

1. 参考[LeetCode #136 Single Number 只出现一次的数字](https://www.jianshu.com/p/d8050ac9d91d)的思路, 利用异或
2. 求和, 与通项公式求取的值相差的数就是丢失的数
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int missingNumber(vector<int>& nums) 
    {
        return ((nums.size() * nums.size() + nums.size()) >> 1) - accumulate(nums.begin(), nums.end(), 0);
    }
};
```

__Java__:

```Java
class Solution {
    public int missingNumber(int[] nums) {
        int result = nums.length;
        for (int i = 0; i < nums.length; i++) {
            result ^= i;
            result ^= nums[i];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        return ((len(nums) ** 2 + len(nums)) >> 1) - sum(nums)
```
