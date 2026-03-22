# 1343 Number of Sub-arrays of Size K and Average Greater than or Equal to Threshold 大小为 K 且平均值大于等于阈值的子数组数目

__Description:__

Given an array of integers arr and two integers k and threshold, return the number of sub-arrays of size k and average greater than or equal to threshold.

__Example:__

Example 1:

Input: arr = [2,2,2,2,5,5,5,8], k = 3, threshold = 4
Output: 3
Explanation: Sub-arrays [2,5,5],[5,5,5] and [5,5,8] have averages 4, 5 and 6 respectively. All other sub-arrays of size 3 have averages less than 4 (the threshold).

Example 2:

Input: arr = [11,13,17,23,29,31,7,5,2,3], k = 3, threshold = 5
Output: 6
Explanation: The first 6 sub-arrays of size 3 have averages greater than 5. Note that averages are not integers.

__Constraints:__

1 <= arr.length <= 10^5
1 <= arr[i] <= 10^4
1 <= k <= arr.length
0 <= threshold <= 10^4

__题目描述：__

给你一个整数数组 arr 和两个整数 k 和 threshold 。

请你返回长度为 k 且平均值大于等于 threshold 的子数组数目。

__示例：__

示例 1：

输入：arr = [2,2,2,2,5,5,5,8], k = 3, threshold = 4
输出：3
解释：子数组 [2,5,5],[5,5,5] 和 [5,5,8] 的平均值分别为 4，5 和 6 。其他长度为 3 的子数组的平均值都小于 4 （threshold 的值)。

示例 2：

输入：arr = [11,13,17,23,29,31,7,5,2,3], k = 3, threshold = 5
输出：6
解释：前 6 个长度为 3 的子数组平均值都大于 5 。注意平均值不是整数。

__提示：__

1 <= arr.length <= 10^5
1 <= arr[i] <= 10^4
1 <= k <= arr.length
0 <= threshold <= 10^4

__思路：__

模拟
先记录 arr[:k] 的值 cur
比较 cur 和 k * threshold 的大小
遍历 arr[k:] 并更新结果
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int numOfSubarrays(vector<int>& arr, int k, int threshold) 
    {
        int result = 0, cur = accumulate(arr.begin(), arr.begin() + k, 0), n = arr.size();
        for (int i = k; i < n; i++) 
        {
            result += cur >= k * threshold;
            cur += arr[i] - arr[i - k];
        }
        return result + (cur >= k * threshold);
    }
};
```

__Java__:

```Java
class Solution {
    public int numOfSubarrays(int[] arr, int k, int threshold) {
        int result = 0, cur = 0, n = arr.length;
        for (int i = 0; i < k; i++) cur += arr[i];
        for (int i = k; i < n; i++) {
            result += (cur >= k * threshold ? 1 : 0);
            cur += arr[i] - arr[i - k];
        }
        return result + (cur >= k * threshold ? 1 : 0);
    }
}
```

__Python__:

```Python
class Solution:
    def numOfSubarrays(self, arr: List[int], k: int, threshold: int) -> int:
        result, cur = 0, sum(arr[:k])
        for i in range(k, len(arr)):
            result += cur >= k * threshold
            cur += arr[i] - arr[i - k]
        return result + (cur >= k * threshold)
```
