# 485 Max Consecutive Ones 最大连续1的个数

__Description__:
Given a binary array, find the maximum number of consecutive 1s in this array.

__Example:__

Example 1:
Input: [1,1,0,1,1,1]
Output: 3
Explanation: The first two digits or the last three digits are consecutive 1s.
    The maximum number of consecutive 1s is 3.

__Note:__

The input array will only contain 0 and 1.
The length of input array is a positive integer and will not exceed 10,000

__题目描述__:
给定一个二进制数组， 计算其中最大连续1的个数。

__示例 :__

示例 1:

输入: [1,1,0,1,1,1]
输出: 3
解释: 开头的两位和最后的三位都是连续1，所以最大连续1的个数是 3.

__注意：__

输入的数组只包含 0 和1。
输入数组的长度是正整数，且不超过 10,000。

__思路__:

设计两个指针, 一个指针用来记录连续 1出现的个数, 另一个指针用来记录最大的连续 1出现的个数
注意因为有全部为 1的情况, 在循环外面需要判断一次大小
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findMaxConsecutiveOnes(vector<int>& nums) 
    {
        int count = 0, result = 0;
        for (int num : nums) 
        {
            if (num == 1) count++;
            else 
            {
                if (count > result) result = count;
                count = 0;
            }
        }
        if (count > result) result = count;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int count = 0, result = 0;
        for (int num : nums) {
            if (num == 1) count++;
            else {
                if (count > result) result = count;
                count = 0;
            }
        }
        if (count > result) result = count;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findMaxConsecutiveOnes(self, nums: List[int]) -> int:
        return len(max(''.join(map(str, nums)).split('0')))
```
