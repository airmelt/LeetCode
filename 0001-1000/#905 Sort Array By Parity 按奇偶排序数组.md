# 905 Sort Array By Parity 按奇偶排序数组

__Description__:
Given an array A of non-negative integers, return an array consisting of all the even elements of A, followed by all the odd elements of A.

You may return any answer array that satisfies this condition.

__Example:__

Example 1:

Input: [3,1,2,4]
Output: [2,4,3,1]
The outputs [4,2,3,1], [2,4,1,3], and [4,2,1,3] would also be accepted.

__Note:__

1 <= A.length <= 5000
0 <= A[i] <= 5000

__题目描述__:
给定一个非负整数数组 A，返回一个数组，在该数组中， A 的所有偶数元素之后跟着所有奇数元素。

你可以返回满足此条件的任何数组作为答案。

__示例 :__

输入：[3,1,2,4]
输出：[2,4,3,1]
输出 [4,2,3,1]，[2,4,1,3] 和 [4,2,1,3] 也会被接受。

__提示：__

1 <= A.length <= 5000
0 <= A[i] <= 5000

__思路__:

1. 双指针, i指向一个不为偶数的下标, j从后往前扫描到偶数就跟 i交换数组对应的值
2. 快速排序, 选择 A[0]作为 pivot, 偶数全部放 A[0]左边, 奇数全部放 A[0]右边, 只需要一次遍历
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> sortArrayByParity(vector<int>& A) 
    {
        for (int i = 0, j = A.size(); i < j;) 
            if (!(A[i] & 1)) i++;
            else if (!(A[--j] & 1)) swap(A[i], A[j]);
        return A;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] sortArrayByParity(int[] A) {
        int i = 0, j = A.length - 1;
        while (i < j) {
            if ((A[i] & 1) > (A[j] & 1)) swap(A, i, j);
            if ((A[i] & 1) == 0) i++;
            if ((A[j] & 1) == 1) j--;
        }
        return A;
    }
    
    private void swap(int[] A, int i, int j) {
        A[i] ^= A[j];
        A[j] ^= A[i];
        A[i] ^= A[j];
    }
}
```

__Python__:

```Python
class Solution:
    def sortArrayByParity(self, A: List[int]) -> List[int]:
        return [i for i in A if (i & 1) == 0]  + [i for i in A if (i & 1) == 1]
```
