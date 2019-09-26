__Description__:
In a given integer array nums, there is always exactly one largest element.

Find whether the largest element in the array is at least twice as much as every other number in the array.

If it is, return the index of the largest element, otherwise return -1.

__Example:__
Example 1:

Input: nums = [3, 6, 1, 0]
Output: 1
Explanation: 6 is the largest integer, and for every other number in the array x,
6 is more than twice as big as x.  The index of value 6 is 1, so we return 1.
 
Example 2:

Input: nums = [1, 2, 3, 4]
Output: -1
Explanation: 4 isn't at least as big as twice the value of 3, so we return -1.
 
__Note:__

nums will have a length in the range [1, 50].
Every nums[i] will be an integer in the range [0, 99].

__题目描述__:
在一个给定的数组nums中，总是存在一个最大元素 。

查找数组中的最大元素是否至少是数组中每个其他数字的两倍。

如果是，则返回最大元素的索引，否则返回-1。

__示例 :__
示例 1:

输入: nums = [3, 6, 1, 0]
输出: 1
解释: 6是最大的整数, 对于数组中的其他整数,
6大于数组中其他元素的两倍。6的索引是1, 所以我们返回1.
 
示例 2:

输入: nums = [1, 2, 3, 4]
输出: -1
解释: 4没有超过3的两倍大, 所以我们返回 -1.
 
__提示:__

nums 的长度范围在[1, 50].
每个 nums[i] 的整数范围在 [0, 100].

__思路__:
1. 参考[LeetCode #414 Third Maximum Number 第三大的数](https://www.jianshu.com/p/7b36be4543da)
一次遍历找到最大值和第二大的值
时间复杂度O(n), 空间复杂度O(1)
2. 先排序, 直接取最大值和第二大的值
时间复杂度O(nlgn), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    int dominantIndex(vector<int>& nums) {
        int result = -1, first = 0, second = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (first < nums[i]) {
                second = first;
                first = nums[i];
                result = i;
            } else if (nums[i] < first && (second < nums[i] || second == first)) second = nums[i];
        }
        return first < 2 * second ? -1 : result;
    }
};
```

__Java__:
```
class Solution {
public:
    int dominantIndex(vector<int>& nums) {
        int result = -1, first = 0, second = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (first < nums[i]) {
                second = first;
                first = nums[i];
                result = i;
            } else if (nums[i] < first && (second < nums[i] || second == first)) second = nums[i];
        }
        return first < 2 * second ? -1 : result;
    }
};
```

__Python__:
```
class Solution:
    def dominantIndex(self, nums: List[int]) -> int:
        return (-1, nums.index(max(nums)))[max(nums) >= ([0] + sorted(nums))[-2] * 2]
```