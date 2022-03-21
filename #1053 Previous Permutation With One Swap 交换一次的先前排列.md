# 1053 Previous Permutation With One Swap 交换一次的先前排列

__Description__:
Given an array of positive integers arr (not necessarily distinct), return the lexicographically largest permutation that is smaller than arr, that can be made with exactly one swap (A swap exchanges the positions of two numbers arr[i] and arr[j]). If it cannot be done, then return the same array.

__Example:__

Example 1:

Input: arr = [3,2,1]
Output: [3,1,2]
Explanation: Swapping 2 and 1.

Example 2:

Input: arr = [1,1,5]
Output: [1,1,5]
Explanation: This is already the smallest permutation.

Example 3:

Input: arr = [1,9,4,6,7]
Output: [1,7,4,6,9]
Explanation: Swapping 9 and 7.

__Constraints:__

1 <= arr.length <= 10^4
1 <= arr[i] <= 10^4

__题目描述__:
给你一个正整数的数组 A（其中的元素不一定完全不同），请你返回可在 一次交换（交换两数字 A[i] 和 A[j] 的位置）后得到的、按字典序排列小于 A 的最大可能排列。

如果无法这么操作，就请返回原数组。

__示例 :__

示例 1：

输入：arr = [3,2,1]
输出：[3,1,2]
解释：交换 2 和 1

示例 2：

输入：arr = [1,1,5]
输出：[1,1,5]
解释：已经是最小排列

示例 3：

输入：arr = [1,9,4,6,7]
输出：[1,7,4,6,9]
解释：交换 9 和 7

示例 4：

输入：arr = [3,1,1,3]
输出：[1,3,1,3]
解释：交换 1 和 3

__提示:__

1 <= arr.length <= 10^4
1 <= arr[i] <= 10^4

__思路__:

模拟
从后往前找到第一个逆序的位置 i
从后往前找到最大的 j 使得 arr[j] < arr[i - 1] 且要是唯一值
时间复杂度O(n ^ 2), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution
{
public:
    vector<int> prevPermOpt1(vector<int>& arr) 
    {
        for (int n = arr.size(), i = n - 1; i > 0; i--) 
        {
            if (arr[i - 1] > arr[i]) 
            {
                for (int j = n - 1; j > i - 1; j--) 
                {
                    if (arr[j] < arr[i - 1] && arr[j] != arr[j - 1]) 
                    {
                        swap(arr[i - 1], arr[j]);
                        return arr;
                    }
                }
            }
        }
        return arr;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] prevPermOpt1(int[] arr) {
        for (int n = arr.length, i = n - 1; i > 0; i--) {
            if (arr[i - 1] > arr[i]) {
                for (int j = n - 1; j > i - 1; j--) {
                    if (arr[j] < arr[i - 1] && arr[j] != arr[j - 1]) {
                        arr[i - 1] ^= arr[j];
                        arr[j] ^= arr[i - 1];
                        arr[i - 1] ^= arr[j];
                        return arr;
                    }
                }
            }
        }
        return arr;
    }
}
```

__Python__:

```Python
class Solution:
    def prevPermOpt1(self, arr: List[int]) -> List[int]:
        for i in range(len(arr) - 1, 0, -1):
            if arr[i - 1] > arr[i]:
                for j in range(len(arr) - 1, i - 1, -1):
                    if arr[j] < arr[i - 1] and arr[j] != arr[j - 1]:
                        arr[i - 1], arr[j] = arr[j], arr[i - 1]
                        return arr
        return arr
```
