# 941 Valid Mountain Array 有效的山脉数组

__Description__:
Given an array A of integers, return true if and only if it is a valid mountain array.

Recall that A is a mountain array if and only if:

A.length >= 3

There exists some i with 0 < i < A.length - 1 such that:

```text
A[0] < A[1] < ... A[i-1] < A[I]
A[i] > A[i+1] > ... > A[A.length - 1]
```

![Mountain Array](https://assets.leetcode.com/uploads/2019/10/20/hint_valid_mountain_array.png)

__Example:__

Example 1:

Input: [2,1]
Output: false

Example 2:

Input: [3,5,5]
Output: false

Example 3:

Input: [0,3,2,1]
Output: true

__Note:__

0 <= A.length <= 10000
0 <= A[i] <= 10000

__题目描述__:
给定一个整数数组 A，如果它是有效的山脉数组就返回 true，否则返回 false。

让我们回顾一下，如果 A 满足下述条件，那么它是一个山脉数组：

A.length >= 3
在 0 < i < A.length - 1 条件下，存在 I 使得：

```text
A[0] < A[1] < ... A[i-1] < A[I]
A[i] > A[i+1] > ... > A[B.length - 1]
```

__示例 :__

示例 1：

输入：[2,1]
输出：false

示例 2：

输入：[3,5,5]
输出：false

示例 3：

输入：[0,3,2,1]
输出：true

__提示：__

0 <= A.length <= 10000
0 <= A[i] <= 10000

__思路__:

双指针法
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool validMountainArray(vector<int>& A) 
    {
        int i = 0, j = A.size() - 1;
        while (i < j and A[i] < A[i + 1]) ++i;
        while (i < j and A[j] < A[j - 1]) --j;
        return i == j and i and j != (A.size() - 1);
    }
};
```

__Java__:

```Java
class Solution {
    public boolean validMountainArray(int[] A) {
        int i = 0, j = A.length - 1;
        while (i < j && A[i] < A[i + 1]) i++;
        while (i < j && A[j] < A[j - 1]) j--;
        return i == j && i != 0 && j != (A.length - 1);
    }
}
```

__Python__:

```Python
class Solution:
    def validMountainArray(self, A: List[int]) -> bool:
        i, j = 0, len(A) - 1
        while i < j and A[i] < A[i + 1]:
            i += 1
        while i < j and A[j] < A[j - 1]:
            j -= 1
        return i == j and i and j != len(A) - 1
```
