# 896 Monotonic Array 单调数列

__Description__:
An array is monotonic if it is either monotone increasing or monotone decreasing.

An array A is monotone increasing if for all i <= j, A[i] <= A[j].  An array A is monotone decreasing if for all i <= j, A[i] >= A[j].

Return true if and only if the given array A is monotonic.

__Example:__

Example 1:

Input: [1,2,2,3]
Output: true

Example 2:

Input: [6,5,4,4]
Output: true

Example 3:

Input: [1,3,2]
Output: false

Example 4:

Input: [1,2,4,5]
Output: true

Example 5:

Input: [1,1,1]
Output: true

__Note:__

1 <= A.length <= 50000
-100000 <= A[i] <= 100000

__题目描述__:
如果数组是单调递增或单调递减的，那么它是单调的。

如果对于所有 i <= j，A[i] <= A[j]，那么数组 A 是单调递增的。 如果对于所有 i <= j，A[i]> = A[j]，那么数组 A 是单调递减的。

当给定的数组 A 是单调数组时返回 true，否则返回 false。

__示例 :__

示例 1：

输入：[1,2,2,3]
输出：true

示例 2：

输入：[6,5,4,4]
输出：true

示例 3：

输入：[1,3,2]
输出：false

示例 4：

输入：[1,2,4,5]
输出：true

示例 5：

输入：[1,1,1]
输出：true

__提示：__

1 <= A.length <= 50000
-100000 <= A[i] <= 100000

__思路__:

1. 求一个数组是单调递增或者单调递减
设两个指针, 分别表示严格单调递增或者严格单调递减, 如果同时出现, 表示不是单调递增或者单调递减
时间复杂度O(n), 空间复杂度O(1)
2. 对这个数组正向及逆向排序, 如果其中有一个相等说明数组是单调的
时间复杂度O(nlgn), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool isMonotonic(vector<int>& A) 
    {
        bool increasing = false, decreasing = false;
        for (int i = 1; i < A.size(); i++) 
        {
            increasing |= A[i] > A[i - 1];
            decreasing |= A[i] < A[i - 1];
            if (increasing && decreasing) return false;
        }
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isMonotonic(int[] A) {
        boolean increasing = false, decreasing = false;
        for (int i = 1; i < A.length; i++) {
            increasing |= A[i] > A[i - 1];
            decreasing |= A[i] < A[i - 1];
            if (increasing && decreasing) return false;
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def isMonotonic(self, A: List[int]) -> bool:
        return A == sorted(A) or A[::-1] == sorted(A)
```
