# 1031 Maximum Sum of Two Non-Overlapping Subarrays 两个非重叠子数组的最大和

__Description__:
Given an integer array nums and two integers firstLen and secondLen, return the maximum sum of elements in two non-overlapping subarrays with lengths firstLen and secondLen.

The array with length firstLen could occur before or after the array with length secondLen, but they have to be non-overlapping.

A subarray is a contiguous part of an array.

__Example:__

Example 1:

Input: nums = [0,6,5,2,2,5,1,9,4], firstLen = 1, secondLen = 2
Output: 20
Explanation: One choice of subarrays is [9] with length 1, and [6,5] with length 2.

Example 2:

Input: nums = [3,8,1,3,2,1,8,9,0], firstLen = 3, secondLen = 2
Output: 29
Explanation: One choice of subarrays is [3,8,1] with length 3, and [8,9] with length 2.

Example 3:

Input: nums = [2,1,5,6,0,9,5,0,3,8], firstLen = 4, secondLen = 3
Output: 31
Explanation: One choice of subarrays is [5,6,0,9] with length 4, and [3,8] with length 3.

__Constraints:__

1 <= firstLen, secondLen <= 1000
2 <= firstLen + secondLen <= 1000
firstLen + secondLen <= nums.length <= 1000
0 <= nums[i] <= 1000

__题目描述__:
给出非负整数数组 A ，返回两个非重叠（连续）子数组中元素的最大和，子数组的长度分别为 L 和 M。（这里需要澄清的是，长为 L 的子数组可以出现在长为 M 的子数组之前或之后。）

从形式上看，返回最大的 V，而 V = (A[i] + A[i+1] + ... + A[i+L-1]) + (A[j] + A[j+1] + ... + A[j+M-1]) 并满足下列条件之一：

0 <= i < i + L - 1 < j < j + M - 1 < A.length, 或
0 <= j < j + M - 1 < i < i + L - 1 < A.length.

__示例 :__

示例 1：

输入：A = [0,6,5,2,2,5,1,9,4], L = 1, M = 2
输出：20
解释：子数组的一种选择中，[9] 长度为 1，[6,5] 长度为 2。

示例 2：

输入：A = [3,8,1,3,2,1,8,9,0], L = 3, M = 2
输出：29
解释：子数组的一种选择中，[3,8,1] 长度为 3，[8,9] 长度为 2。

示例 3：

输入：A = [2,1,5,6,0,9,5,0,3,8], L = 4, M = 3
输出：31
解释：子数组的一种选择中，[5,6,0,9] 长度为 4，[0,3,8] 长度为 3。

__提示:__

L >= 1
M >= 1
L + M <= A.length <= 1000
0 <= A[i] <= 1000

__思路__:

前缀和 ➕ 动态规划
直接用 nums[i] 记录前缀和
数组 a[i], b[i] 记录从 i 开始的前 firstLen/secondLen 的累计和的最大值
if i < firstLen + secondLen, result = nums[i], 这时可以把所有的 nums 中元素放入
else result = max(nums[i] - nums[i - firstLen] + b[i - firstLen],  nums[i] - nums[i - secondLen] + a[i - secondLen], 否则选择移除 a[i] 或者 b[i] 中多出的元素
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int maxSumTwoNoOverlap(vector<int>& nums, int firstLen, int secondLen) 
    {
        int n = nums.size(), result = 0;
        vector<int> a(n), b(n);
        for (int i = 1; i < n; i++) nums[i] += nums[i - 1];
        for (int i = 0; i < n; i++) 
        {
            a[i] = i < firstLen ? nums[i] : max(a[i - 1], nums[i] - nums[i - firstLen]);
            b[i] = i < secondLen ? nums[i] : max(b[i - 1], nums[i] - nums[i - secondLen]);
        result = i < firstLen + secondLen ? nums[i] : max({result, nums[i] - nums[i - firstLen] + b[i - firstLen], nums[i] - nums[i - secondLen] + a[i - secondLen]});
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxSumTwoNoOverlap(int[] nums, int firstLen, int secondLen) {
        int n = nums.length, a[] = new int[n], b[] = new int[n], result = 0;
        for (int i = 1; i < n; i++) nums[i] += nums[i - 1];
        for (int i = 0; i < n; i++) {
            a[i] = i < firstLen ? nums[i] : Math.max(a[i - 1], nums[i] - nums[i - firstLen]);
            b[i] = i < secondLen ? nums[i] : Math.max(b[i - 1], nums[i] - nums[i - secondLen]);
            result = i < firstLen + secondLen ? nums[i] : Math.max(result, Math.max(nums[i] - nums[i - firstLen] + b[i - firstLen], nums[i] - nums[i - secondLen] + a[i - secondLen]));
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxSumTwoNoOverlap(self, nums: List[int], firstLen: int, secondLen: int) -> int:
        a, b, result = [0] * (n := len(nums)), [0] * n, 0
        for i in range(1, n):
            nums[i] += nums[i - 1]
        for i in range(n):
            a[i], b[i] = nums[i] if i < firstLen else max(a[i - 1], nums[i] - nums[i - firstLen]), nums[i] if i < secondLen else max(b[i - 1], nums[i] - nums[i - secondLen])
            result = nums[i] if i < firstLen + secondLen else max(result, nums[i] - nums[i - firstLen] + b[i - firstLen], nums[i] - nums[i - secondLen] + a[i - secondLen])
        return result
```
