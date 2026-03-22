# 977 Squares of a Sorted Array 有序数组的平方

__Description__:
Given an array of integers A sorted in non-decreasing order, return an array of the squares of each number, also in sorted non-decreasing order.

__Example:__

Example 1:

Input: [-4,-1,0,3,10]
Output: [0,1,9,16,100]

Example 2:

Input: [-7,-3,2,3,11]
Output: [4,9,9,49,121]

__Note:__

1 <= A.length <= 10000
-10000 <= A[i] <= 10000
A is sorted in non-decreasing order.

__题目描述__:
给定一个按非递减顺序排序的整数数组 A，返回每个数字的平方组成的新数组，要求也按非递减顺序排序。

__示例 :__

示例 1：

输入：[-4,-1,0,3,10]
输出：[0,1,9,16,100]

示例 2：

输入：[-7,-3,2,3,11]
输出：[4,9,9,49,121]

__提示：__

1 <= A.length <= 10000
-10000 <= A[i] <= 10000
A 已按非递减顺序排序。

__思路__:

1. 直接原地平方之后再排序
时间复杂度O(nlgn), 空间复杂度O(1)
2. 双指针法, 由于原数组已经有序, 依次从后往前添加平方值较大的元素即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> sortedSquares(vector<int>& A) 
    {
        int i = 0, j = A.size() - 1, k = A.size() - 1;
        vector<int> result(A.size());
        while (i <= j)
        {
            if (A[i] * A[i] < A[j] * A[j]) result[k--] = A[j] * A[j--];
            else result[k--] = A[i] * A[i++];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] sortedSquares(int[] A) {
        int i = 0, j = A.length - 1, k = A.length - 1, result[] = new int[A.length];
        while (i <= j) {
            if (A[i] * A[i] < A[j] * A[j]) result[k--] = A[j] * A[j--];
            else result[k--] = A[i] * A[i++];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def sortedSquares(self, A: List[int]) -> List[int]:
        return sorted((i ** 2 for i in A))
```
