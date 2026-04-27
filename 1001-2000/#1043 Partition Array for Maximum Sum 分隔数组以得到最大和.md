# 1043 Partition Array for Maximum Sum 分隔数组以得到最大和

__Description__:
Given an integer array arr, partition the array into (contiguous) subarrays of length at most k. After partitioning, each subarray has their values changed to become the maximum value of that subarray.

Return the largest sum of the given array after partitioning. Test cases are generated so that the answer fits in a 32-bit integer.

__Example:__

Example 1:

Input: arr = [1,15,7,9,2,5,10], k = 3
Output: 84
Explanation: arr becomes [15,15,15,9,10,10,10]

Example 2:

Input: arr = [1,4,1,5,7,3,6,1,9,9,3], k = 4
Output: 83

Example 3:

Input: arr = [1], k = 1
Output: 1

__Constraints:__

1 <= arr.length <= 500
0 <= arr[i] <= 10^9
1 <= k <= arr.length

__题目描述__:
给你一个整数数组 arr，请你将该数组分隔为长度最多为 k 的一些（连续）子数组。分隔完成后，每个子数组的中的所有值都会变为该子数组中的最大值。

返回将数组分隔变换后能够得到的元素最大和。

注意，原数组和分隔后的数组对应顺序应当一致，也就是说，你只能选择分隔数组的位置而不能调整数组中的顺序。

__示例 :__

示例 1：

输入：arr = [1,15,7,9,2,5,10], k = 3
输出：84
解释：
因为 k=3 可以分隔成 [1,15,7] [9] [2,5,10]，结果为 [15,15,15,9,10,10,10]，和为 84，是该数组所有分隔变换后元素总和最大的。
若是分隔成 [1] [15,7,9] [2,5,10]，结果就是 [1, 15, 15, 15, 10, 10, 10] 但这种分隔方式的元素总和（76）小于上一种。

示例 2：

输入：arr = [1,4,1,5,7,3,6,1,9,9,3], k = 4
输出：83

示例 3：

输入：arr = [1], k = 1
输出：1

__提示:__

1 <= arr.length <= 500
0 <= arr[i] <= 10^9
1 <= k <= arr.length

__思路__:

动态规划
设 dp[i] 表示 arr[:i] 能分割成长度最多为 k 的子数组的最大得分
dp[i] = max(dp[j] + max(arr[j:i] * (j - i)), 其中 j >= max(i - k, 0)
也就是说搜索 [max(i - k, 0), i] 这个区间内的最大值乘上区间长度加上端点值就是这个区间的最大得分
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution
{
public:
    int maxSumAfterPartitioning(vector<int>& arr, int k) 
    {
        int n = arr.size();
        vector<int> dp(n + 1);
        for (int i = 1; i <= n; i++) 
        {
            int cur = -1;
            for (int j = i - 1; j >= max(i - k, 0); j--) 
            {
                cur = max(cur, arr[j]);
                dp[i] = max(dp[i], dp[j] + cur * (i - j));
            }
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int maxSumAfterPartitioning(int[] arr, int k) {
        int n = arr.length, dp[] = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            int cur = -1;
            for (int j = i - 1; j >= Math.max(i - k, 0); j--) {
                cur = Math.max(cur, arr[j]);
                dp[i] = Math.max(dp[i], dp[j] + cur * (i - j));
            }
        }
        return dp[n];
    }
}
```

__Python__:

```Python
class Solution:
    def maxSumAfterPartitioning(self, arr: List[int], k: int) -> int:
        dp = [0] * ((n := len(arr)) + 1)
        for i in range(1, n + 1):
            cur = -1
            for j in range(i - 1, max(i - k - 1, -1), -1):
                dp[i] = max(dp[i], dp[j] + (cur := max(cur, arr[j])) * (i - j))
        return dp[-1]
```
