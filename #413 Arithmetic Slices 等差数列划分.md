# 413 Arithmetic Slices 等差数列划分

__Description__:
A sequence of numbers is called arithmetic if it consists of at least three elements and if the difference between any two consecutive elements is the same.

For example, these are arithmetic sequences:

```text
1, 3, 5, 7, 9
7, 7, 7, 7
3, -1, -5, -9
```

The following sequence is not arithmetic.

```text
1, 1, 2, 5, 7
```

A zero-indexed array A consisting of N numbers is given. A slice of that array is any pair of integers (P, Q) such that 0 <= P < Q < N.

A slice (P, Q) of the array A is called arithmetic if the sequence:
A[P], A[P + 1], ..., A[Q - 1], A[Q] is arithmetic. In particular, this means that P + 1 < Q.

The function should return the number of arithmetic slices in the array A.

__Example:__

A = [1, 2, 3, 4]

return: 3, for 3 arithmetic slices in A: [1, 2, 3], [2, 3, 4] and [1, 2, 3, 4] itself.

__题目描述__:
如果一个数列至少有三个元素，并且任意两个相邻元素之差相同，则称该数列为等差数列。

例如，以下数列为等差数列:

```text
1, 3, 5, 7, 9
7, 7, 7, 7
3, -1, -5, -9
```

以下数列不是等差数列。

```text
1, 1, 2, 5, 7
```

数组 A 包含 N 个数，且索引从0开始。数组 A 的一个子数组划分为数组 (P, Q)，P 与 Q 是整数且满足 0<=P<Q<N 。

如果满足以下条件，则称子数组(P, Q)为等差数组：

元素 A[P], A[p + 1], ..., A[Q - 1], A[Q] 是等差的。并且 P + 1 < Q 。

函数要返回数组 A 中所有为等差数组的子数组个数。

__示例 :__

A = [1, 2, 3, 4]

返回: 3, A 中有三个子等差数组: [1, 2, 3], [2, 3, 4] 以及自身 [1, 2, 3, 4]。

__思路__:

动态规划
dp[i] 表示以 A[i]结尾的等差数列的长度
则 dp[i] = dp[i - 1] + 1, 如果 A[i - 2], A[i - 1], A[i]为等差数列, 否则 dp[i] = 0
result = sum(dp)
可以看出来只需要记录一个 dp[i - 1]变量, 压缩空间复杂度到 O(1)
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numberOfArithmeticSlices(vector<int>& A) 
    {
        int result = 0, cur = 0;
        for (int i = 2; i < A.size(); i++) 
        {
            if (A[i - 2] - A[i - 1] == A[i - 1] - A[i]) result += ++cur;
            else cur = 0;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfArithmeticSlices(int[] A) {
        int result = 0, cur = 0;
        for (int i = 2; i < A.length; i++) {
            if (A[i - 2] - A[i - 1] == A[i - 1] - A[i]) result += ++cur;
            else cur = 0;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfArithmeticSlices(self, A: List[int]) -> int:
        result, cur = 0, 0
        for i in range(2, len(A)):
            if A[i - 2] - A[i - 1] == A[i - 1] - A[i]:
                cur += 1
                result += cur
            else:
                cur = 0
        return result
```
