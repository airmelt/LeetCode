# 801 Minimum Swaps To Make Sequences Increasing 使序列递增的最小交换次数

__Description__:
You are given two integer arrays of the same length nums1 and nums2. In one operation, you are allowed to swap nums1[i] with nums2[i].

For example, if nums1 = [1,2,3,8], and nums2 = [5,6,7,4], you can swap the element at i = 3 to obtain nums1 = [1,2,3,4] and nums2 = [5,6,7,8].
Return the minimum number of needed operations to make nums1 and nums2 strictly increasing. The test cases are generated so that the given input always makes it possible.

An array arr is strictly increasing if and only if arr[0] < arr[1] < arr[2] < ... < arr[arr.length - 1].

__Example:__

Example 1:

Input: nums1 = [1,3,5,4], nums2 = [1,2,3,7]
Output: 1
Explanation:
Swap nums1[3] and nums2[3]. Then the sequences are:
nums1 = [1, 3, 5, 7] and nums2 = [1, 2, 3, 4]
which are both strictly increasing.

Example 2:

Input: nums1 = [0,3,5,8,9], nums2 = [2,1,4,6,9]
Output: 1

__Constraints:__

2 <= nums1.length <= 10^5
nums2.length == nums1.length
0 <= nums1[i], nums2[i] <= 2 * 10^5

__题目描述__:
我们有两个长度相等且不为空的整型数组 A 和 B 。

我们可以交换 A[i] 和 B[i] 的元素。注意这两个元素在各自的序列中应该处于相同的位置。

在交换过一些元素之后，数组 A 和 B 都应该是严格递增的（数组严格递增的条件仅为A[0] < A[1] < A[2] < ... < A[A.length - 1]）。

给定数组 A 和 B ，请返回使得两个数组均保持严格递增状态的最小交换次数。假设给定的输入总是有效的。

__示例 :__

输入: A = [1,3,5,4], B = [1,2,3,7]
输出: 1
解释:
交换 A[3] 和 B[3] 后，两个数组如下:
A = [1, 3, 5, 7] ， B = [1, 2, 3, 4]
两个数组均为严格递增的。

__注意:__

A, B 两个数组的长度总是相等的，且长度的范围为 [1, 1000]。
A[i], B[i] 均为 [0, 2000]区间内的整数。

__思路__:

动态规划
设 dp[0][i] 表示不交换 A[i] 和 B[i] 在下标 i 的交换次数
设 dp[1][i] 表示交换 A[i] 和 B[i] 在下标 i 的交换次数
只用考察前一个数就可以了, 再前一个一定是递增的
首先要满足两个数组最终都递增所有位置必须都满足 A[i] > A[i - 1] or A[i] > B[i - 1]
所以分成以下两种情况讨论
如果 A[i] > A[i - 1] and B[i] > B[i - 1], 数组本身在 i 之前均递增, 这时 dp[0][i] = min(dp[0][i], dp[0][i - 1]), 不需要交换不用增加交换次数, dp[1][i] = min(dp[1][i], dp[1][i - 1] + 1), 如果要交换前一个也必须交换才能满足递增的条件
如果 B[i] > A[i - 1] and A[i] > B[i - 1], 这时必须交换才能满足, dp[0][i] = min(dp[0][i], dp[1][i - 1]) 表示 i - 1 位置发生交换, dp[1][i] = min(dp[1][i], dp[0][i - 1] + 1), 表示在 i - 1 不换的基础上, i 发生了交换
可以看到交换与否只取决与前一个状态, 可以将空间复杂度压缩到 O(1)
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minSwap(vector<int>& nums1, vector<int>& nums2) 
    {
        int n = nums1.size(), pre = 0, cur = 1, t = 0;
        for (int i = 1; i < n; i++) {
            if (nums1[i] > nums1[i - 1] and nums2[i] > nums2[i - 1]) 
            {
                if (nums1[i] > nums2[i - 1] and nums2[i] > nums1[i - 1])
                {
                    pre = min(cur, pre);
                    cur = min(cur, pre) + 1;
                }
                else ++cur;
            } 
            else
            {
                t = pre;
                pre = cur;
                cur = t + 1;
            }
        }
        return min(cur, pre);
    }
};
```

__Java__:

```Java
class Solution {
    public int minSwap(int[] nums1, int[] nums2) {
        int n = nums1.length;
        int[][] dp = new int[2][n];
        for (int row[] : dp) Arrays.fill(row, Integer.MAX_VALUE);
        dp[0][0] = 0;
        dp[1][0] = 1;
        for (int i = 1; i < n; i++) {
            if (nums1[i] > nums1[i - 1] && nums2[i] > nums2[i - 1]) {
                dp[0][i] = Math.min(dp[0][i], dp[0][i - 1]);
                dp[1][i] = Math.min(dp[1][i], dp[1][i - 1] + 1);
            }
            if(nums1[i] > nums2[i - 1] && nums2[i] > nums1[i - 1]) {
                dp[0][i] = Math.min(dp[0][i], dp[1][i - 1]);
                dp[1][i] = Math.min(dp[1][i], dp[0][i - 1] + 1);
            }
        }
        return Math.min(dp[0][n - 1], dp[1][n - 1]);
    }
}
```

__Python__:

```Python
class Solution:
    def minSwap(self, nums1: List[int], nums2: List[int]) -> int:
        n, pre_not_change, pre_change = len(nums1), 0, 1
        for i in range(1, n):
            cur_not_change = cur_change = float('inf')
            if nums1[i] > nums1[i - 1] and nums2[i] > nums2[i - 1]:
                cur_not_change, cur_change = pre_not_change, pre_change + 1
            if nums1[i] > nums2[i - 1] and nums2[i] > nums1[i - 1]:
                cur_not_change, cur_change = min(pre_change, cur_not_change), min(cur_change, pre_not_change + 1)
            pre_not_change, pre_change = cur_not_change, cur_change
        return min(pre_not_change, pre_change)
```
