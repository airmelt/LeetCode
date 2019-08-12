__Description__:
Given an integer array, you need to find one continuous subarray that if you only sort this subarray in ascending order, then the whole array will be sorted in ascending order, too.

You need to find the shortest such subarray and output its length.

__Example:__
Example 1:
Input: [2, 6, 4, 8, 10, 9, 15]
Output: 5
Explanation: You need to sort [6, 4, 8, 10, 9] in ascending order to make the whole array sorted in ascending order.

__Note:__
Then length of the input array is in range [1, 10,000].
The input array may contain duplicates, so ascending order here means <=.

__题目描述__:
给定一个整数数组，你需要寻找一个连续的子数组，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。

你找到的子数组应是最短的，请输出它的长度。

__示例 :__
示例 1:

输入: [2, 6, 4, 8, 10, 9, 15]
输出: 5
解释: 你只需要对 [6, 4, 8, 10, 9] 进行升序排序，那么整个表都会变为升序排序。

__说明 :__

输入的数组长度范围在 [1, 10,000]。
输入的数组可能包含重复元素 ，所以升序的意思是<=。

__思路__:
1. 排序之后比较位置差别
时间复杂度O(nlgn), 空间复杂度O(1)
2. 设置两个指针分别指向数组的最大值和最小值, 初始值设为 0和 -1保证数组有序的时候输出 0
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    int findUnsortedSubarray(vector<int>& nums) {
        int len = nums.size(), start = 0, end = -1, min_nums = nums[len - 1], max_nums = nums[0];
        for (int i = 0, pos = 0; i < len; i++) {
            pos = len - 1 - i;
            max_nums = max(max_nums, nums[i]);
            min_nums = min(min_nums, nums[pos]);
            if (nums[i] < max_nums) end = i;
            if (nums[pos] > min_nums) start = pos;
        }
        return end - start + 1;
    }
};
```

__Java__:
```
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int len = nums.length, start = 0, end = -1, min = nums[len - 1], max = nums[0];
        for (int i = 0, pos = 0; i < len; i++) {
            pos = len - 1 - i;
            max = Math.max(max, nums[i]);
            min = Math.min(min, nums[pos]);
            if (nums[i] < max) end = i;
            if (nums[pos] > min) start = pos;
        }
        return end - start + 1;
    }
}
```

__Python__:
```
class Solution:
    def findUnsortedSubarray(self, nums: List[int]) -> int:
        return len([i for i, (a, b) in enumerate(zip(nums, sorted(nums))) if a != b]) and [i for i, (a, b) in enumerate(zip(nums, sorted(nums))) if a != b][-1] - [i for i, (a, b) in enumerate(zip(nums, sorted(nums))) if a != b][0] + 1
```
