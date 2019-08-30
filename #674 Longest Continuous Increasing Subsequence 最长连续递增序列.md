__Description__:
Given an unsorted array of integers, find the length of longest continuous increasing subsequence (subarray).

__Example:__
Example 1:

Input: [1,3,5,4,7]
Output: 3
Explanation: The longest continuous increasing subsequence is [1,3,5], its length is 3. 
Even though [1,3,5,7] is also an increasing subsequence, it's not a continuous one where 5 and 7 are separated by 4. 

Example 2:

Input: [2,2,2,2,2]
Output: 1
Explanation: The longest continuous increasing subsequence is [2], its length is 1. 

__Note:__
Length of the array will not exceed 10,000.

__题目描述__:
给定一个未经排序的整数数组，找到最长且连续的的递增序列。

__示例 :__
示例 1:

输入: [1,3,5,4,7]
输出: 3
解释: 最长连续递增序列是 [1,3,5], 长度为3。
尽管 [1,3,5,7] 也是升序的子序列, 但它不是连续的，因为5和7在原数组里被4隔开。
 
示例 2:

输入: [2,2,2,2,2]
输出: 1
解释: 最长连续递增序列是 [2], 长度为1。
注意：数组长度不会超过10000。

__思路__:
要求最长连续子数组, 遍历数组, 只要不满足递增条件, 重新从 1开始计数, 返回最大的计数值即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        int result = nums.size() < 2 ? nums.size() : 0, temp = 1;
        for (int i = 1; i < nums.size(); i++) {
            temp = nums[i] > nums[i - 1] ? temp + 1 : 1;
            result = max(temp, result);
        }
        return result;
    }
};
```

__Java__:
```
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        int result = nums.length < 2 ? nums.length : 0, temp = 1;
        for (int i = 1; i < nums.length; i++) {
            temp = nums[i] > nums[i - 1] ? temp + 1 : 1;
            result = Math.max(temp, result);
        }
        return result;
    }
}
```

__Python__:
```
class Solution:
    def findLengthOfLCIS(self, nums: List[int]) -> int:
        result, temp = 0 if len(nums) > 1 else len(nums), 1
        for i in range(1, len(nums)):
            temp = temp + 1 if nums[i] > nums[i - 1] else 1
            result = max(result, temp)
        return result
```