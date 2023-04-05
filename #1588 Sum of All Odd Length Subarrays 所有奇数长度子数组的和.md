# 1588 Sum of All Odd Length Subarrays 所有奇数长度子数组的和

__Description:__

Given an array of positive integers `arr`, return _the sum of all possible __odd-length subarrays__ of_ `arr`.

A subarray is a contiguous subsequence of the array.

__Example:__

Example 1:

```text
Input: arr = [1,4,2,5,3]
Output: 58
Explanation: The odd-length subarrays of arr and their sums are:
[1] = 1
[4] = 4
[2] = 2
[5] = 5
[3] = 3
[1,4,2] = 7
[4,2,5] = 11
[2,5,3] = 10
[1,4,2,5,3] = 15
If we add all these together we get 1 + 4 + 2 + 5 + 3 + 7 + 11 + 10 + 15 = 58
```

Example 2:

```text
Input: arr = [1,2]
Output: 3
Explanation: There are only 2 subarrays of odd length, [1] and [2]. Their sum is 3.
```

Example 3:

```text
Input: arr = [10,11,12]
Output: 66
```

__Constraints:__

- `1 <= arr.length <= 100`
- `1 <= arr[i] <= 1000`

Follow up:

Could you solve this problem in O(n) time complexity?

__题目描述:__

给你一个正整数数组 `arr` ，请你计算所有可能的奇数长度子数组的和。

子数组 定义为原数组中的一个连续子序列。

请你返回 `arr` 中 __所有奇数长度子数组的和__ 。

__示例:__

示例 1：

```text
输入：arr = [1,4,2,5,3]
输出：58
解释：所有奇数长度子数组和它们的和为：
[1] = 1
[4] = 4
[2] = 2
[5] = 5
[3] = 3
[1,4,2] = 7
[4,2,5] = 11
[2,5,3] = 10
[1,4,2,5,3] = 15
我们将所有值求和得到 1 + 4 + 2 + 5 + 3 + 7 + 11 + 10 + 15 = 58
```

示例 2：

```text
输入：arr = [1,2]
输出：3
解释：总共只有 2 个长度为奇数的子数组，[1] 和 [2]。它们的和为 3 。
```

示例 3：

```text
输入：arr = [10,11,12]
输出：66
```

__提示：__

- `1 <= arr.length <= 100`
- `1 <= arr[i] <= 1000`

进阶：

你可以设计一个 O(n) 时间复杂度的算法解决此问题吗？

__思路:__

```text
1. 暴力法
直接计算所有长度为奇数的子数组的和
可以用前缀和加快求和的速度
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
2. 数学
对于一个元素 arr[i], 其左边的元素有 i 个, 右边的元素有 n - i - 1
如果这个元素处于一个奇数长度的子数组中, 那么左右两边的元素个数的奇偶性相同
如果两边的元素都是奇数个, left_odd = (i / 2) + 1, 即左边有奇数个数的情况的总数为 left_odd, 因为左边的长度为 [0, i], 其中奇数的情况为 left_odd, right_odd = (n - i - 1) / 2 + 1, 根据乘法原理一共有 left_odd * right_odd 种情况
类似的可以得到偶数长度的情况, 根据加法原理, arr[i] 一共在 left_odd * right_odd + left_even * right_even 个奇数长度的子数组中出现过
所以 arr[i] 的总和为 arr[i] * (left_odd * right_odd + left_even * right_even)
遍历数组加上所有数字即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int sumOddLengthSubarrays(vector<int>& arr) 
    {
        int n = arr.size(), result = 0;
        for (int i = 0; i < n; i++) result += arr[i] * (((i >> 1) + 1) * (((n - i - 1) >> 1) + 1) + ((i + 1) >> 1) * ((n - i) >> 1));
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int sumOddLengthSubarrays(int[] arr) {
        int n = arr.length, result = 0;
        for (int i = 0; i < n; i++) result += arr[i] * (((i >> 1) + 1) * (((n - i - 1) >> 1) + 1) + ((i + 1) >> 1) * ((n - i) >> 1));
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def sumOddLengthSubarrays(self, arr: List[int]) -> int:
        return sum(sum(arr[i:j + 1]) for i in range(len(arr)) for j in range(len(arr)) if not ((i - j) & 1))
```
