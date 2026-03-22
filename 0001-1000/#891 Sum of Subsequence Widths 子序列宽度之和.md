# 891 Sum of Subsequence Widths 子序列宽度之和

__Description__:
The width of a sequence is the difference between the maximum and minimum elements in the sequence.

Given an array of integers nums, return the sum of the widths of all the non-empty subsequences of nums. Since the answer may be very large, return it modulo 109 + 7.

A subsequence is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, [3,6,2,7] is a subsequence of the array [0,3,1,6,2,2,7].

__Example:__

Example 1:

Input: nums = [2,1,3]
Output: 6
Explanation: The subsequences are [1], [2], [3], [2,1], [2,3], [1,3], [2,1,3].
The corresponding widths are 0, 0, 0, 1, 1, 2, 2.
The sum of these widths is 6.

Example 2:

Input: nums = [2]
Output: 0

__Constraints:__

1 <= nums.length <= 10^5
1 <= nums[i] <= 10^5

__题目描述__:
给定一个整数数组 A ，考虑 A 的所有非空子序列。

对于任意序列 S ，设 S 的宽度是 S 的最大元素和最小元素的差。

返回 A 的所有子序列的宽度之和。

由于答案可能非常大，请返回答案模 10^9+7。

__示例 :__

输入：[2,1,3]
输出：6
解释：
子序列为 [1]，[2]，[3]，[2,1]，[2,3]，[1,3]，[2,1,3] 。
相应的宽度是 0，0，0，1，1，2，2 。
这些宽度之和是 6 。

__提示:__

1 <= A.length <= 20000
1 <= A[i] <= 20000

__思路__:

数学
需要最大值和最小值, 与顺序无关
先排序
设 j > i, A[j] >= A[i], 最大值为 A[j], 最小值为 A[i]
则含有这两个值的子序列一共有 2 ^ (j - i - 1) 个, 即选择子数组 A[i] - A[j] 的所有子集
所以子序列宽度为所有 j > i, A[j] - A[i] \* 2 ^ (j - i - 1) 的和
这样可以得到一个 O(n ^ 2) 的算法
对 A[j] - A[i] \* 2 ^ (j - i - 1) 展开并化简
可得 2 ^ i - 2 ^ (n - i - 1) \* A[i] 求和
这样就可以得到一个 O(n) 的算法
主要瓶颈就转化为排序
时间复杂度为 O(nlgn), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int sumSubseqWidths(vector<int>& nums) 
    {
        sort(nums.begin(), nums.end());
        long result = 0, e = 1, mod = (long)(1e9 + 7);
        for (int i = 0, n = nums.size(); i < n; i++, e = (e << 1) % mod) result =  (result + (nums[i] - nums[n - i - 1]) * e) % mod;
        return (result + mod) % mod;
    }
};
```

__Java__:

```Java
class Solution {
    public int sumSubseqWidths(int[] nums) {
        Arrays.sort(nums);
        long result = 0, e = 1, mod = (long)(1e9 + 7);
        for (int i = 0, n = nums.length; i < n; i++, e = (e << 1) % mod) result =  (result + (nums[i] - nums[n - i - 1]) * e) % mod;
        return (int)((result + mod) % mod);
    }
}
```

__Python__:

```Python
class Solution:
    def sumSubseqWidths(self, nums: List[int]) -> int:
        nums.sort()
        sums, n, mod = 0, len(nums), 10 ** 9 + 7
        for i in range(n):
            sums += (pow(2, i, mod) - pow(2, n - 1 - i, mod)) * nums[i]
        return sums % mod
```
