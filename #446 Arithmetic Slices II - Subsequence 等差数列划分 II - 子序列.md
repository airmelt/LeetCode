# 446 Arithmetic Slices II - Subsequence 等差数列划分 II - 子序列

__Description__:
A sequence of numbers is called arithmetic if it consists of at least three elements and if the difference between any two consecutive elements is the same.

For example, these are arithmetic sequences:

1, 3, 5, 7, 9
7, 7, 7, 7
3, -1, -5, -9
The following sequence is not arithmetic.

1, 1, 2, 5, 7

A zero-indexed array A consisting of N numbers is given. A subsequence slice of that array is any sequence of integers (P0, P1, ..., Pk) such that 0 ≤ P0 < P1 < ... < Pk < N.

A subsequence slice (P0, P1, ..., Pk) of array A is called arithmetic if the sequence A[P0], A[P1], ..., A[Pk-1], A[Pk] is arithmetic. In particular, this means that k ≥ 2.

The function should return the number of arithmetic subsequence slices in the array A.

The input contains N integers. Every integer is in the range of -2^31 and 2^31-1 and 0 ≤ N ≤ 1000. The output is guaranteed to be less than 2^31-1.

__Example:__

Input: [2, 4, 6, 8, 10]

Output: 7

Explanation:
All arithmetic subsequence slices are:
[2,4,6]
[4,6,8]
[6,8,10]
[2,4,6,8]
[4,6,8,10]
[2,4,6,8,10]
[2,6,10]

__题目描述__:
如果一个数列至少有三个元素，并且任意两个相邻元素之差相同，则称该数列为等差数列。

例如，以下数列为等差数列:

1, 3, 5, 7, 9
7, 7, 7, 7
3, -1, -5, -9
以下数列不是等差数列。

1, 1, 2, 5, 7

数组 A 包含 N 个数，且索引从 0 开始。该数组子序列将划分为整数序列 (P0, P1, ..., Pk)，满足 0 ≤ P0 < P1 < ... < Pk < N。

如果序列 A[P0]，A[P1]，...，A[Pk-1]，A[Pk] 是等差的，那么数组 A 的子序列 (P0，P1，…，PK) 称为等差序列。值得注意的是，这意味着 k ≥ 2。

函数要返回数组 A 中所有等差子序列的个数。

输入包含 N 个整数。每个整数都在 -2^31 和 2^31-1 之间，另外 0 ≤ N ≤ 1000。保证输出小于 2^31-1。

__示例 :__

输入：[2, 4, 6, 8, 10]

输出：7

解释：
所有的等差子序列为：
[2,4,6]
[4,6,8]
[6,8,10]
[2,4,6,8]
[4,6,8,10]
[2,4,6,8,10]
[2,6,10]

__思路__:

动态规划
用一个 map记录数组对应元素对其他元素的差
k = A[j] - A[i]
dp[i][k] = dp[i][k] + dp[j][k]
时间复杂度O(n ^ 2), 空间复杂度O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numberOfArithmeticSlices(vector<int>& A) 
    {
        int result = 0, n = A.size();
        if (n == 0) return result;
        map<long, long> dp[n];
        for (int i = 1; i < n; i++)
        {
            for (int j = 0; j < i; j++)
            {
                long gap = (long)A[i] - (long)A[j];
                ++dp[i][gap];
                if (dp[j].count(gap))
                {
                    dp[i][gap] += dp[j][gap];
                    result += (int)dp[j][gap];
                }
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfArithmeticSlices(int[] A) {
        int result = 0, n = A.length;
        Map<Integer, Integer>[] map = new HashMap[n];
        for (int i = 0; i < n; i++) {
            Map<Integer, Integer> gap = new HashMap<>();
            map[i] = gap;
            long num = A[i];
            for (int j = 0; j < i; j++) {
                if (num - A[j] > Integer.MAX_VALUE || num-A[j]<Integer.MIN_VALUE) continue;
                int diff = (int)(num - A[j]);
                int count = map[j].getOrDefault(diff, 0);
                map[i].put(diff, map[i].getOrDefault(diff, 0) + count + 1);
                result += count;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfArithmeticSlices(self, A: List[int]) -> int:
        result, n, dp = 0, len(A), [collections.defaultdict(int) for _ in range(len(A))]
        for i in range(1, n):
            for j in range(i):
                dp[i][(gap := A[i] - A[j])] += 1
                if gap in dp[j]:
                    dp[i][gap] += dp[j][gap]
                    result += dp[j][gap]
        return result
```
