# 945 Minimum Increment to Make Array Unique 使数组唯一的最小增量

__Description__:
You are given an integer array nums. In one move, you can pick an index i where 0 <= i < nums.length and increment nums[i] by 1.

Return the minimum number of moves to make every value in nums unique.

__Example:__

Example 1:

Input: nums = [1,2,2]
Output: 1
Explanation: After 1 move, the array could be [1, 2, 3].

Example 2:

Input: nums = [3,2,1,2,1,7]
Output: 6
Explanation: After 6 moves, the array could be [3, 4, 1, 2, 5, 7].
It can be shown with 5 or less moves that it is impossible for the array to have all unique values.

__Constraints:__

1 <= nums.length <= 10^5
0 <= nums[i] <= 10^5

__题目描述__:
给你一个整数数组 nums 。每次 move 操作将会选择任意一个满足 0 <= i < nums.length 的下标 i，并将 nums[i] 递增 1。

返回使 nums 中的每个值都变成唯一的所需要的最少操作次数。

__示例 :__

示例 1：

输入：nums = [1,2,2]
输出：1
解释：经过一次 move 操作，数组将变为 [1, 2, 3]。

示例 2：

输入：nums = [3,2,1,2,1,7]
输出：6
解释：经过 6 次 move 操作，数组将变为 [3, 4, 1, 2, 5, 7]。
可以看出 5 次或 5 次以下的 move 操作是不能让数组的每个值唯一的。

__提示:__
1 <= nums.length <= 10^5
0 <= nums[i] <= 10^5

__思路__:

1. 排序
排序之后只要使得后一个数比前一个数大 1 即可
时间复杂度为 O(nlgn), 空间复杂度为 O(lgn)
2. 计数
因为数组中的数范围为 [0, 100000]
假设所有数字都是 100000, 开一个 200005 的数组完全可以放下所有的数
这个时候只要将重复的数字在数组中往后移, 每次遍历最多 200000 个位置即可
最差情况是数组中全部是 100000
时间复杂度为 O(m), 空间复杂度为 O(m), m 为数组中的最大值

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minIncrementForUnique(vector<int>& nums) 
    {
        int result = 0, pos[200005]{ 0 };
        for (const auto& num : nums) ++pos[num];
        for (int i = 0; i < 200004; i++)
        {
            if (pos[i] <= 1) continue;
            result += pos[i] - 1;
            pos[i + 1] += pos[i] - 1;
            pos[i] = 1;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minIncrementForUnique(int[] nums) {
        int n = nums.length, pos[] = new int[200005], result = 0;
        for (int num : nums) ++pos[num];
        for (int i = 0; i < 200004; i++) {
            if (pos[i] <= 1) continue;
            result += pos[i] - 1;
            pos[i + 1] += pos[i] - 1;
            pos[i] = 1;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minIncrementForUnique(self, nums: List[int]) -> int:
        nums.sort()
        result, n = 0, len(nums)
        for i in range(1, n):
            if nums[i] <= nums[i - 1]:
                result += nums[i - 1] - nums[i] + 1
                nums[i] = nums[i - 1] + 1
        return result
```
