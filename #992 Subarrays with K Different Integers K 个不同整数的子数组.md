# 992 Subarrays with K Different Integers K 个不同整数的子数组

__Description__:
Given an integer array nums and an integer k, return the number of good subarrays of nums.

A good array is an array where the number of different integers in that array is exactly k.

For example, [1,2,3,1,2] has 3 different integers: 1, 2, and 3.
A subarray is a contiguous part of an array.

__Example:__

Example 1:

Input: nums = [1,2,1,2,3], k = 2
Output: 7
Explanation: Subarrays formed with exactly 2 different integers: [1,2], [2,1], [1,2], [2,3], [1,2,1], [2,1,2], [1,2,1,2]

Example 2:

Input: nums = [1,2,1,3,4], k = 3
Output: 3
Explanation: Subarrays formed with exactly 3 different integers: [1,2,1,3], [2,1,3], [1,3,4].

__Constraints:__

1 <= nums.length <= 2 * 10^4
1 <= nums[i], k <= nums.length

__题目描述__:
给定一个正整数数组 A，如果 A 的某个子数组中不同整数的个数恰好为 K，则称 A 的这个连续、不一定不同的子数组为好子数组。

（例如，[1,2,3,1,2] 中有 3 个不同的整数：1，2，以及 3。）

返回 A 中好子数组的数目。

__示例 :__

示例 1：

输入：A = [1,2,1,2,3], K = 2
输出：7
解释：恰好由 2 个不同整数组成的子数组：[1,2], [2,1], [1,2], [2,3], [1,2,1], [2,1,2], [1,2,1,2].

示例 2：

输入：A = [1,2,1,3,4], K = 3
输出：3
解释：恰好由 3 个不同整数组成的子数组：[1,2,1,3], [2,1,3], [1,3,4].

__提示:__

1 <= A.length <= 20000
1 <= A[i] <= A.length
1 <= K <= A.length

__思路__:

滑动窗口
注意到数组中的数组都不大于数组的长度
可以用一个 n + 1 的 count 数组记录出现元素的个数
用 left 表示子数组的左边界
用 size 记录出现的不同元素的个数, 每次记录到一个 count[nums[i]] == 1, size 自增
当 size == k 时, 说明目前刚好就是所需要的子数组, left 指针右移直到某个元素只出现 1 次为止, 结果加上这一段的长度
当 size > k 时, 需要移除某个元素, 移除元素之后按照 size == k 处理
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int subarraysWithKDistinct(vector<int>& nums, int k) 
    {
        int result = 0, len = 1, left = 0, size = 0, n = nums.size();
        vector<int> count(n + 1);
        for (int i = 0; i < n; i++) 
        {
            if (++count[nums[i]] == 1) ++size;
            if (size == k) 
            {
                while (count[nums[left]] > 1) 
                {
                    --count[nums[left++]];
                    ++len;
                }
                result += len;
            } 
            else if (size > k) 
            {
                len = 1;
                while (size > k) 
                {
                    if (count[nums[left]] == 1) size--;
                    --count[nums[left++]];
                }
                while (count[nums[left]] > 1) 
                {
                    --count[nums[left++]];
                    ++len;
                }
                result += len; 
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int subarraysWithKDistinct(int[] nums, int k) {
        int result = 0, len = 1, left = 0, size = 0, n = nums.length, count[] = new int[n + 1];
        for (int i = 0; i < n; i++) {
            if (++count[nums[i]] == 1) ++size;
            if (size == k) {
                while (count[nums[left]] > 1) {
                    --count[nums[left++]];
                    ++len;
                }
                result += len;
            } else if (size > k) {
                len = 1;
                while (size > k) {
                    if (count[nums[left]] == 1) size--;
                    --count[nums[left++]];
                }
                while (count[nums[left]] > 1) {
                    --count[nums[left++]];
                    ++len;
                }
                result += len; 
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def subarraysWithKDistinct(self, nums: List[int], k: int) -> int:
        result, l, left, s, count = 0, 1, 0, 0, [0] * ((n := len(nums)) + 1)
        for i in range(n):
            count[nums[i]] += 1
            if count[nums[i]] == 1:
                s += 1
            if s == k:
                while count[nums[left]] > 1:
                    count[nums[left]] -= 1
                    left += 1
                    l += 1
                result += l
            elif s > k:
                l = 1
                while s > k:
                    if count[nums[left]] == 1:
                        s -= 1
                    count[nums[left]] -= 1
                    left += 1
                while count[nums[left]] > 1:
                    count[nums[left]] -= 1
                    left += 1
                    l += 1
                result += l
        return result
```
