__Description__:
Given an array with n integers, your task is to check if it could become non-decreasing by modifying at most 1 element.

We define an array is non-decreasing if array[i] <= array[i + 1] holds for every i (1 <= i < n).

__Example:__
Example 1:

Input: [4,2,3]
Output: True
Explanation: You could modify the first 4 to 1 to get a non-decreasing array.
Example 2:

Input: [4,2,1]
Output: False
Explanation: You can't get a non-decreasing array by modify at most one element.

__Note:__
The n belongs to [1, 10,000].

__题目描述__:
给定一个长度为 n 的整数数组，你的任务是判断在最多改变 1 个元素的情况下，该数组能否变成一个非递减数列。

我们是这样定义一个非递减数列的： 对于数组中所有的 i (1 <= i < n)，满足 array[i] <= array[i + 1]。

__示例 :__
示例 1:

输入: [4,2,3]
输出: True
解释: 你可以通过把第一个4变成1来使得它成为一个非递减数列。
示例 2:

输入: [4,2,1]
输出: False
解释: 你不能在只改变一个元素的情况下将其变为非递减数列。

__说明:__
n 的范围为 [1, 10,000]。

__思路__:
1. 如果数组不是递增数列, 有两种情况:
[1, 4, 2, 3]或者[1, 2, 2, 1], 可以看出来比较 i - 2和 i位置元素的大小, 分别更改成 [1, 2, 2, 3]和[1, 2, 2, 2]即可
2. 用双指针搜索, 如果满足题意, 那么指针最多只能相差 1, 注意判断边界条件即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    bool checkPossibility(vector<int>& nums) {
        int i = 0, j = nums.size() - 1;
        while (i < j && nums[i] <= nums[i + 1]) i++;
        while (i < j && nums[j] >= nums[j - 1]) j--;
        return i == j || (i == j - 1 && ((i > 0 && (nums[i - 1] < nums[j])) || ((j < nums.size() - 1 && (nums[i] < nums[j + 1]))) || (i == 0 || j == nums.size() - 1)));
    }
};
```

__Java__:
```
class Solution {
    public boolean checkPossibility(int[] nums) {
        int count = 0;
        for (int i = 1; i < nums.length && count < 2; i++) {
            if (nums[i - 1] <= nums[i]) continue;
            count++;
            if (i - 2 >= 0 && nums[i - 2] > nums[i]) nums[i] = nums[i - 1];
            else nums[i - 1] = nums[i];
        }
        return count <= 1;
    }
}
```

__Python__:
```
class Solution:
    def checkPossibility(self, nums: List[int]) -> bool:
        count = 0
        for i in range(1, len(nums)):
            if nums[i - 1] <= nums[i]:
                continue
            count += 1
            if count >= 2:
                return False
            if i - 2 >= 0 and nums[i - 2] > nums[i]:
                nums[i] = nums[i - 1]
            else:
                nums[i - 1] = nums[i]
        return count <= 1
```