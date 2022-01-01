# 974 Subarray Sums Divisible by K 和可被 K 整除的子数组

__Description__:
Given an integer array nums and an integer k, return the number of non-empty subarrays that have a sum divisible by k.

A subarray is a contiguous part of an array.

__Example:__

Example 1:

Input: nums = [4,5,0,-2,-3,1], k = 5
Output: 7
Explanation: There are 7 subarrays with a sum divisible by k = 5:
[4, 5, 0, -2, -3, 1], [5], [5, 0], [5, 0, -2, -3], [0], [0, -2, -3], [-2, -3]

Example 2:

Input: nums = [5], k = 9
Output: 0

__Constraints:__

1 <= nums.length <= 3 * 10^4
-104 <= nums[i] <= 10^4
2 <= k <= 10^4

__题目描述__:
给定一个整数数组 A，返回其中元素之和可被 K 整除的（连续、非空）子数组的数目。

__示例 :__

输入：A = [4,5,0,-2,-3,1], K = 5
输出：7
解释：
有 7 个子数组满足其元素之和可被 K = 5 整除：
[4, 5, 0, -2, -3, 1], [5], [5, 0], [5, 0, -2, -3], [0], [0, -2, -3], [-2, -3]

__提示:__

1 <= A.length <= 30000
-10000 <= A[i] <= 10000
2 <= K <= 10000

__思路__:

前缀和
记录前缀和
那么每一个子数组之和可以用 pre[i] - pre[j] 快速得到
如果子数组 mod k 结果相同说明两个区间中间的和能整除 k
将前缀和的每一个值对 k 取余记录出现的次数, 注意和为负的时候需要加上 k 保证下标为正
然后用 n * (n - 1) / 2 累计即可
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int subarraysDivByK(vector<int>& nums, int k) 
    {
        int n = nums.size(), result = 0;
        vector<int> pre(n + 1), count(k);
        for (int i = 0; i < n; i++) pre[i + 1] = nums[i] + pre[i];
        for (int s : pre) ++count[(s % k + k) % k];
        for (int item : count) result += item * (item - 1) >> 1;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int subarraysDivByK(int[] nums, int k) {
        int n = nums.length, pre[] = new int[n + 1], count[] = new int[k], result = 0;
        for (int i = 0; i < n; i++) pre[i + 1] = nums[i] + pre[i];
        for (int s : pre) ++count[(s % k + k) % k];
        for (int item : count) result += item * (item - 1) >>> 1;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def subarraysDivByK(self, nums: List[int], k: int) -> int:
        return sum(x * (x - 1) >> 1 for x in Counter([item % k for item in (list(accumulate([0] + nums)))]).values())
```
