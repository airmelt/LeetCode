# 330 Patching Array 按要求补齐数组

__Description__:
Given a sorted positive integer array nums and an integer n, add/patch elements to the array such that any number in range [1, n] inclusive can be formed by the sum of some elements in the array. Return the minimum number of patches required.

__Example:__

Example 1:

Input: nums = [1,3], n = 6
Output: 1
Explanation:
Combinations of nums are [1], [3], [1,3], which form possible sums of: 1, 3, 4.
Now if we add/patch 2 to nums, the combinations are: [1], [2], [3], [1,3], [2,3], [1,2,3].
Possible sums are 1, 2, 3, 4, 5, 6, which now covers the range [1, 6].
So we only need 1 patch.

Example 2:

Input: nums = [1,5,10], n = 20
Output: 2
Explanation: The two patches can be [2, 4].

Example 3:

Input: nums = [1,2,2], n = 5
Output: 0

__题目描述__:
给定一个已排序的正整数数组 nums，和一个正整数 n 。从 [1, n] 区间内选取任意个数字补充到 nums 中，使得 [1, n] 区间内的任何数字都可以用 nums 中某几个数字的和来表示。请输出满足上述要求的最少需要补充的数字个数。

__示例 :__

示例 1:

输入: nums = [1,3], n = 6
输出: 1
解释:
根据 nums 里现有的组合 [1], [3], [1,3]，可以得出 1, 3, 4。
现在如果我们将 2 添加到 nums 中， 组合变为: [1], [2], [3], [1,3], [2,3], [1,2,3]。
其和可以表示数字 1, 2, 3, 4, 5, 6，能够覆盖 [1, 6] 区间里所有的数。
所以我们最少需要添加一个数字。

示例 2:

输入: nums = [1,5,10], n = 20
输出: 2
解释: 我们需要添加 [2, 4]。

示例 3:

输入: nums = [1,2,2], n = 5
输出: 0

__思路__:

类似转换成 2进制, 保证每一位都有值
比如 n = 20, nums = [1, 2, 10]
1和 2可以覆盖 [1, 4)
加入 4, 可以覆盖 [1, 8)
加入 8, 可以覆盖 [1, 16)
加入 16, 可以覆盖 [1, 32) > [1, 20]
返回 3
时间复杂度O(m + lg(n)), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minPatches(vector<int>& nums, int n) 
    {
        int result = 0, i = 0, s = nums.size();
        long miss = 1;
        while (miss <= n) 
        {
            if (i < s and miss >= nums[i]) miss += nums[i++];
            else 
            {
                miss <<= 1;
                ++result;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minPatches(int[] nums, int n) {
        int result = 0, i = 0, l = nums.length;
        long miss = 1;
        while (miss <= n) {
            if (i < l && miss >= nums[i]) miss += nums[i++];
            else {
                miss <<= 1;
                ++result;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minPatches(self, nums: List[int], n: int) -> int:
        result, miss, i = 0, 1, 0
        while miss <= n:
            if i < len(nums) and nums[i] <= miss:
                miss += nums[i]
                i += 1
            else:
                result += 1
                miss += miss
        return result
```
