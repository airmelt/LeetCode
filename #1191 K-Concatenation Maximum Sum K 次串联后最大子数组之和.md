# 1191 K-Concatenation Maximum Sum K 次串联后最大子数组之和

__Description__:
Given an integer array arr and an integer k, modify the array by repeating it k times.

For example, if arr = [1, 2] and k = 3 then the modified array will be [1, 2, 1, 2, 1, 2].

Return the maximum sub-array sum in the modified array. Note that the length of the sub-array can be 0 and its sum in that case is 0.

As the answer can be very large, return the answer modulo 109 + 7.

__Example:__

Example 1:

Input: arr = [1,2], k = 3
Output: 9

Example 2:

Input: arr = [1,-2,1], k = 5
Output: 2

Example 3:

Input: arr = [-1,-2], k = 7
Output: 0

__Constraints:__

1 <= arr.length <= 10^5
1 <= k <= 10^5
-10^4 <= arr[i] <= 10^4

__题目描述__:
给定一个整数数组 arr 和一个整数 k ，通过重复 k 次来修改数组。

例如，如果 arr = [1, 2] ， k = 3 ，那么修改后的数组将是 [1, 2, 1, 2, 1, 2] 。

返回修改后的数组中的最大的子数组之和。注意，子数组长度可以是 0，在这种情况下它的总和也是 0。

由于 结果可能会很大，需要返回的 109 + 7 的 模 。

__示例 :__

示例 1：

输入：arr = [1,2], k = 3
输出：9

示例 2：

输入：arr = [1,-2,1], k = 5
输出：2

示例 3：

输入：arr = [-1,-2], k = 7
输出：0

__提示:__

1 <= arr.length <= 10^5
1 <= k <= 10^5
-10^4 <= arr[i] <= 10^4

__思路__:

模拟
当 k = 1 时, 转化为最大连续子数组和, 注意答案非负
当 k = 2 时, 为数组最大前缀和和最大后缀和的和
当 k > 2 时, 如果数组的和为正, 需要加上 sum * (k - 2)
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int kConcatenationMaxSum(vector<int>& arr, int k) 
    {
        int n = arr.size(), pre = 0, result = 0, mod = 1e9 + 7, sum = 0;
        for (int i = 0; i < (n << 1); i++) 
        {
            pre = max(arr[i % n], pre + arr[i % n]);
            result = max(result, pre);
            if (k == 1 and i == n - 1) return result;
            if (i < n) sum += arr[i];
        }
        return (int)((max(0, sum) * (long)(k - 2) + result) % mod);
    }
};
```

__Java__:

```Java
class Solution {
    public int kConcatenationMaxSum(int[] arr, int k) {
        int n = arr.length, pre = 0, result = 0, mod = 1_000_000_007, sum = 0;
        for (int i = 0; i < (n << 1); i++) {
            pre = Math.max(arr[i % n], pre + arr[i % n]);
            result = Math.max(result, pre);
            if (k == 1 && i == n - 1) return result;
            if (i < n) sum += arr[i];
        }
        return (int)((Math.max(0, sum) * (long)(k - 2) + result) % mod);
    }
}
```

__Python__:

```Python
class Solution:
    def kConcatenationMaxSum(self, arr: List[int], k: int) -> int:
        result, pre, s, mod = 0, 0, sum(arr), 10 ** 9 + 7
        arr *= min(k, 2)
        for i, v in enumerate(arr):
            result = max(result, (pre := max(v, v + pre)))
        return (max(result, s * (k - 2) + result) if (s > 0 and k > 2) else result) % mod
```
