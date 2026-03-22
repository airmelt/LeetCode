# 930 Binary Subarrays With Sum 和相同的二元子数组

__Description__:
Given a binary array nums and an integer goal, return the number of non-empty subarrays with a sum goal.

A subarray is a contiguous part of the array.

__Example:__

Example 1:

Input: nums = [1,0,1,0,1], goal = 2
Output: 4
Explanation: The 4 subarrays are bolded and underlined below:
[1,0,1,0,1]
[1,0,1,0,1]
[1,0,1,0,1]
[1,0,1,0,1]

Example 2:

Input: nums = [0,0,0,0,0], goal = 0
Output: 15

__Constraints:__

1 <= nums.length <= 3 * 10^4
nums[i] is either 0 or 1.
0 <= goal <= nums.length

__题目描述__:
给你一个二元数组 nums ，和一个整数 goal ，请你统计并返回有多少个和为 goal 的 非空 子数组。

子数组 是数组的一段连续部分。

__示例 :__

示例 1：

输入：nums = [1,0,1,0,1], goal = 2
输出：4
解释：
有 4 个满足题目要求的子数组：[1,0,1]、[1,0,1,0]、[0,1,0,1]、[1,0,1]

示例 2：

输入：nums = [0,0,0,0,0], goal = 0
输出：15

__提示:__

1 <= nums.length <= 3 * 10^4
nums[i] 不是 0 就是 1
0 <= goal <= nums.length

__思路__:

前缀和 ➕ 哈希表
参考 [LeetCode #560 Subarray Sum Equals K 和为K的子数组](https://www.jianshu.com/p/a061228f047a)
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numSubarraysWithSum(vector<int>& nums, int goal) 
    {
        int n = nums.size(), result = 0;
        vector<int> pre(n + 1, 0);
        unordered_map<int, int> m;
        for (int i = 0; i < n; i++) 
        {
            ++m[pre[i]];
            result += m[(pre[i + 1] = pre[i] + nums[i]) - goal];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numSubarraysWithSum(int[] nums, int goal) {
        int n = nums.length, result = 0;
        int pre[] = new int[n + 1];
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < n; i++) {
            map.put(pre[i], map.getOrDefault(pre[i], 0) + 1);
            result += map.getOrDefault((pre[i + 1] = pre[i] + nums[i]) - goal, 0);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numSubarraysWithSum(self, nums: List[int], goal: int) -> int:
        pre, result, d = [0] * ((n := len(nums)) + 1), 0, defaultdict(int)
        for i in range(n):
            d[pre[i]] += 1
            pre[i + 1] = pre[i] + nums[i]
            result += d[pre[i + 1] - goal]
        return result
```
