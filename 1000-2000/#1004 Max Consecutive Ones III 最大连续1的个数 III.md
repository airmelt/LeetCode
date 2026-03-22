# 1004 Max Consecutive Ones III 最大连续1的个数 III

__Description__:
Given a binary array nums and an integer k, return the maximum number of consecutive 1's in the array if you can flip at most k 0's.

__Example:__

Example 1:

Input: nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
Output: 6
Explanation: [1,1,1,0,0,1,1,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

Example 2:

Input: nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
Output: 10
Explanation: [0,0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

__Constraints:__

1 <= nums.length <= 10^5
nums[i] is either 0 or 1.
0 <= k <= nums.length

__题目描述__:
给定一个二进制数组 nums 和一个整数 k ，如果可以翻转最多k 个 0 ，则返回 数组中连续 1 的最大个数 。

__示例 :__

示例 1：

输入：nums = [1,1,1,0,0,0,1,1,1,1,0], K = 2
输出：6
解释：[1,1,1,0,0,1,1,1,1,1,1]
粗体数字从 0 翻转到 1，最长的子数组长度为 6。

示例 2：

输入：nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], K = 3
输出：10
解释：[0,0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,1]
粗体数字从 0 翻转到 1，最长的子数组长度为 10。

__提示:__

1 <= nums.length <= 10^5
nums[i] 不是 0 就是 1
0 <= k <= nums.length

__思路__:

滑动窗口
[left, right] 窗口中记录已经修改最多 k 次之后的全为 1 的窗口, 长度为 right - left + 1
right 每次循环自增
当 nums[right] == 0 时, 如果 k 不为 0, 可以将 1 个 0 替换成 1, k 自减
否则 left 移动到下一个为 0 的位置
更新 result 为当前最大窗口的长度
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int longestOnes(vector<int>& nums, int k) 
    {
        int left = 0, right = 0, n = nums.size();
        while (right < n) 
        {
            if (!nums[right++]) --k;
            if (k < 0 and !nums[left++]) ++k;
        }
        return right - left;
    }
};
```

__Java__:

```Java
class Solution {
    public int longestOnes(int[] nums, int k) {
        int result = 0, left = 0, right = 0, n = nums.length;
        while (right < n) {
            if (nums[right] == 0) {
                if (k == 0) while (nums[left++] == 1);
                else --k;
            }
            result = Math.max(result, ++right - left);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def longestOnes(self, nums: List[int], k: int) -> int:
        result, left, right, n = 0, 0, 0, len(nums)
        while right < n:
            if not nums[right]:
                if not k:
                    while nums[left]:
                        left += 1
                    left += 1
                else:
                    k -= 1
            result = max(result, right - left + 1)
            right += 1
        return result
```
