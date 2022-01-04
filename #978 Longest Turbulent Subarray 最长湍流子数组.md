# 978 Longest Turbulent Subarray 最长湍流子数组

__Description__:
Given an integer array arr, return the length of a maximum size turbulent subarray of arr.

A subarray is turbulent if the comparison sign flips between each adjacent pair of elements in the subarray.

More formally, a subarray [arr[i], arr[i + 1], ..., arr[j]] of arr is said to be turbulent if and only if:

For i <= k < j:
arr[k] > arr[k + 1] when k is odd, and
arr[k] < arr[k + 1] when k is even.
Or, for i <= k < j:
arr[k] > arr[k + 1] when k is even, and
arr[k] < arr[k + 1] when k is odd.

__Example:__

Example 1:

Input: arr = [9,4,2,10,7,8,8,1,9]
Output: 5
Explanation: arr[1] > arr[2] < arr[3] > arr[4] < arr[5]

Example 2:

Input: arr = [4,8,12,16]
Output: 2

Example 3:

Input: arr = [100]
Output: 1

__Constraints:__

1 <= arr.length <= 4 * 10^4
0 <= arr[i] <= 10^9

__题目描述__:
当 A 的子数组 A[i], A[i+1], ..., A[j] 满足下列条件时，我们称其为湍流子数组：

若 i <= k < j，当 k 为奇数时， A[k] > A[k+1]，且当 k 为偶数时，A[k] < A[k+1]；
或 若 i <= k < j，当 k 为偶数时，A[k] > A[k+1] ，且当 k 为奇数时， A[k] < A[k+1]。
也就是说，如果比较符号在子数组中的每个相邻元素对之间翻转，则该子数组是湍流子数组。

返回 A 的最大湍流子数组的长度。

__示例 :__

示例 1：

输入：[9,4,2,10,7,8,8,1,9]
输出：5
解释：(A[1] > A[2] < A[3] > A[4] < A[5])

示例 2：

输入：[4,8,12,16]
输出：2

示例 3：

输入：[100]
输出：1

__提示:__

1 <= A.length <= 40000
0 <= A[i] <= 10^9

__思路__:

模拟
用两个指针分别表示递增长度和递减长度
如果单调递增令 up = down + 1, 且 down 置 1
如果单调递减令 down = up + 1, 且 up 置 1
否则如果相等令 down = up = 1 表示当前的湍流子数组长度为 1
result 取 up, down, result 的最大值
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int maxTurbulenceSize(vector<int>& arr) 
    {
        int n = arr.size(), up = 1, down = 1, result = 1;
        for (int i = 1; i < n; i++) 
        {
            if (arr[i] > arr[i - 1]) 
            { 
                up = down + 1; 
                down = 1; 
            }
            else if (arr[i] < arr[i - 1]) 
            { 
                down = up + 1; 
                up = 1; 
            }
            else up = down = 1;
            result = max({ result, up, down });
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxTurbulenceSize(int[] arr) {
        int n = arr.length, up = 1, down = 1, result = 1;
        for (int i = 1; i < n; i++) {
            if (arr[i] > arr[i - 1]) { 
                up = down + 1; 
                down = 1; 
            }
            else if (arr[i] < arr[i - 1]) { 
                down = up + 1; 
                up = 1; 
            }
            else up = down = 1;
            result = Math.max( result, Math.max(up, down));
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxTurbulenceSize(self, arr: List[int]) -> int:
        n, up, down, result = len(arr), 1, 1, 1
        for i in range(1, n):
            if arr[i] > arr[i - 1]:
                up, down = down + 1, 1
            elif arr[i] < arr[i - 1]:
                down, up = up + 1, 1
            else:
                up = down = 1
            result = max(up, down, result)
        return result
```
