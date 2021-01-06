# 209 Minimum Size Subarray Sum 长度最小的子数组

__Description__:
Given an array of n positive integers and a positive integer s, find the minimal length of a contiguous subarray of which the sum ≥ s. If there isn't one, return 0 instead.

__Example:__

Input: s = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: the subarray [4,3] has the minimal length under the problem constraint.

__Follow up:__
If you have figured out the O(n) solution, try coding another solution of which the time complexity is O(n log n).

__题目描述__:
给定一个含有 n 个正整数的数组和一个正整数 s ，找出该数组中满足其和 ≥ s 的长度最小的 连续 子数组，并返回其长度。如果不存在符合条件的子数组，返回 0。

__示例 :__

输入：s = 7, nums = [2,3,1,2,4,3]
输出：2
解释：子数组 [4,3] 是该条件下的长度最小的子数组。

__进阶：__

如果你已经完成了 O(n) 时间复杂度的解法, 请尝试 O(n log n) 时间复杂度的解法。(怎么会有人提出这么奇怪的要求, Python)

__思路__:

1. 暴力法(Java)
可以以每一个下标出发, 找到最短能满足题意的长度
时间复杂度O(n ^ 2), 空间复杂度O(1)
2. 前缀和(Python)
一般求连续子数组, 考虑前缀和或者滑动窗口
用一个数组记录 sums记录前缀和, sums[i + 1]表示∑nums[i]
每次使用二分查找找到 s + sums[i - 1]在 sums数组中的位置, 即为下标 i对应的连续子数组的右边界 j
连续子数组长度为 j - i + 1
时间复杂度O(nlgn), 空间复杂度O(n), 前缀和时间复杂度O(n), 空间复杂度O(n), 二分查找时间复杂O(lgn)
3. 双指针法(C++)
设置两个指针left, right, 初始指针都指向nums[0]
如果两个指针指向的子数组之和小于 s, 右指针右移直到移到最后一个元素
如果两个指针指向的子数组之和不小于 s, 左指针右移直到和小于 s
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minSubArrayLen(int s, vector<int>& nums) 
    {
        if (accumulate(nums.begin(), nums.end(), 0) < s) return 0;
        int left = 0, right = 0, sum = 0, n = nums.size(), result = INT_MAX;
        while (right < n)
        {
            sum += nums[right];
            while (sum >= s)
            {
                result = min(result, right - left + 1);
                sum -= nums[left++];
            }
            ++right;
        }
        return result == INT_MAX ? 0 : result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        if (nums == null || nums.length == 0) return 0;
        int result = Integer.MAX_VALUE, sum = 0, n = nums.length;
        for (int i = 0; i < n; i++) {
            sum = 0;
            for (int j = i; j < n; j++) {
                sum += nums[j];
                if (sum >= s) {
                    result = Math.min(result, j - i + 1);
                    break;
                }
            }
        }
        return result == Integer.MAX_VALUE ? 0 : result;
    }
}
```

__Python__:

```Python
class Solution:
    def minSubArrayLen(self, s: int, nums: List[int]) -> int:
        if not nums:
            return 0
        n, result, sums = len(nums), len(nums) + 1, [sum(nums[:i]) for i in range(len(nums) + 1)]
        for i in range(1, n + 1):
            j = bisect.bisect_left(sums, s + sums[i - 1])
            result = min(result, j - i + 1) if j != n + 1 else result
        return 0 if result == n + 1 else result
```
