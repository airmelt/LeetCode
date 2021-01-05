# 961 N-Repeated Element in Size 2N Array 重复 N 次的元素

__Description__:
In a array A of size 2N, there are N+1 unique elements, and exactly one of these elements is repeated N times.

Return the element repeated N times.

__Example:__

Example 1:

Input: [1,2,3,3]
Output: 3

Example 2:

Input: [2,1,2,5,3,2]
Output: 2

Example 3:

Input: [5,1,5,2,5,3,5,4]
Output: 5

__Note:__

4 <= A.length <= 10000
0 <= A[i] < 10000
A.length is even

__题目描述__:
在大小为 2N 的数组 A 中有 N+1 个不同的元素，其中有一个元素重复了 N 次。

返回重复了 N 次的那个元素。

__示例 :__

示例 1：

输入：[1,2,3,3]
输出：3

示例 2：

输入：[2,1,2,5,3,2]
输出：2

示例 3：

输入：[5,1,5,2,5,3,5,4]
输出：5

__提示：__

4 <= A.length <= 10000
0 <= A[i] < 10000
A.length 为偶数

__思路__:

要么连续两个数中有重复值, 要么重复值间隔出现, 要么只有 4个元素的时候, 出现在数组的最后一个
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int repeatedNTimes(vector<int>& A) 
    {
        for (int i = 0; i < A.size() - 2; i++) if (A[i] == A[i + 1] or A[i] == A[i + 2]) return A[i];
        return A[A.size() - 1];
    }
};
```

__Java__:

```Java
class Solution {
    public int repeatedNTimes(int[] A) {
        for (int i = 0; i < A.length - 2; i++) if (A[i] == A[i + 1] || A[i] == A[i + 2]) return A[i];
        return A[A.length - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def repeatedNTimes(self, A: List[int]) -> int:
        return sorted(A)[len(A) // 2] if sorted(A)[0] != sorted(A)[1] else sorted(A)[0]
```
