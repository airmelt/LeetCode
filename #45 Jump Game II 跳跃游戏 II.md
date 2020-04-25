__Description__:
Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

__Example:__

Input: [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2.
    Jump 1 step from index 0 to 1, then 3 steps to the last index.

__Note:__

You can assume that you can always reach the last index.

__题目描述__:
给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

你的目标是使用最少的跳跃次数到达数组的最后一个位置。

__示例 :__

输入: [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。

__说明:__

假设你总是可以到达数组的最后一个位置。

__思路__:
贪心
每一次跳跃都选择能跳到的最远距离
每一步更新 nums[i] + i
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int jump(vector<int>& nums) 
    {
        int result = 0, start = 0, end = 0;
        while (end < nums.size() - 1)
        {
            int best = end;
            for (int i = start; i <= end; i++) if (nums[i] + i > best) best = nums[i] + i;
            start = end + 1;
            end = best;
            ++result;
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int jump(int[] nums) {
        int result = 0, start = 0, end = 0;
        while (end < nums.length - 1) {
            int best = end;
            for (int i = start; i <= end; i++) if (nums[i] + i > best) best = nums[i] + i;
            start = end + 1;
            end = best;
            ++result;
        }
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def jump(self, nums: List[int]) -> int:
        result = start = end = 0
        while end < len(nums) - 1:
            best = end
            for i in range(start, end + 1):
                best = max(nums[i] + i, best)
            start, end, result = end + 1, best, result + 1
        return result
```