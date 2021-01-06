# 55 Jump Game 跳跃游戏

__Description__:
Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.

__Example:__

Example 1:

Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.

Example 2:

Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.

__Constraints:__

1 <= nums.length <= 3 * 10^4
0 <= nums[i][j] <= 10^5

__题目描述__:
给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个位置。

__示例 :__

示例 1:

输入: [2,3,1,1,4]
输出: true
解释: 我们可以先跳 1 步，从位置 0 到达 位置 1, 然后再从位置 1 跳 3 步到达最后一个位置。

示例 2:

输入: [3,2,1,0,4]
输出: false
解释: 无论怎样，你总会到达索引为 3 的位置。但该位置的最大跳跃长度是 0 ， 所以你永远不可能到达最后一个位置。

__思路__:

贪心法
参考[LeetCode #45 Jump Game II 跳跃游戏 II](https://www.jianshu.com/p/6fa9af3500e1)
每次更新 i + nums[i]表示最远能跳到到位置
只要不能跳到当前位置, 立即返回 false
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool canJump(vector<int>& nums) 
    {
        int reach = 0;
        for (int i = 0; i < nums.size(); i++) 
        {
            if (reach < i) return false;
            reach = max(reach, nums[i] + i);
        }
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canJump(int[] nums) {
        int reach = 0;
        for (int i = 0; i < nums.length; i++) {
            if (reach < i) return false;
            reach = Math.max(reach, nums[i] + i);
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        reach = 0
        for i in range(len(nums)):
            if i > reach:
                return False
            reach = max(i + nums[i], reach)
        return True
```
