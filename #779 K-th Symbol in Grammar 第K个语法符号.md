# 779 K-th Symbol in Grammar 第K个语法符号

__Description__:
We build a table of n rows (1-indexed). We start by writing 0 in the 1st row. Now in every subsequent row, we look at the previous row and replace each occurrence of 0 with 01, and each occurrence of 1 with 10.

For example, for n = 3, the 1st row is 0, the 2nd row is 01, and the 3rd row is 0110.
Given two integer n and k, return the kth (1-indexed) symbol in the nth row of a table of n rows.

__Example:__

Example 1:

Input: n = 1, k = 1
Output: 0
Explanation: row 1: 0

Example 2:

Input: n = 2, k = 1
Output: 0
Explanation:
row 1: 0
row 2: 01

Example 3:

Input: n = 2, k = 2
Output: 1
Explanation:
row 1: 0
row 2: 01

Example 4:

Input: n = 3, k = 1
Output: 0

Explanation:
row 1: 0
row 2: 01
row 3: 0110

__Constraints:__

1 <= n <= 30
1 <= k <= 2^(n - 1)

__题目描述__:
在第一行我们写上一个 0。接下来的每一行，将前一行中的0替换为01，1替换为10。

给定行数 N 和序数 K，返回第 N 行中第 K个字符。（K从1开始）

__示例 :__

示例 1:
输入: N = 1, K = 1
输出: 0

示例 2:
输入: N = 2, K = 1
输出: 0

示例 3:
输入: N = 2, K = 2
输出: 1

示例 4:
输入: N = 4, K = 5
输出: 1

解释:
第一行: 0
第二行: 01
第三行: 0110
第四行: 01101001

__注意:__

N 的范围 [1, 30].
K 的范围 [1, 2^(N-1)].

__思路__:

数学
找规律可以发现, 其实就是给 k 做了一个奇偶校验
返回 k - 1 对应的二进制的 1 的个数再和 1 做与运算
时间复杂度为 O(1), 空间复杂度为 O(1), k 最长为 32 位

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int kthGrammar(int n, int k) 
    {
        return __builtin_popcount(k - 1) & 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int kthGrammar(int n, int k) {
        int result = 0;
        --k;
        while (k != 0) {
            k = (k & (k - 1));
            ++result;
        }
        return result & 1;
    }
}
```

__Python__:

```Python
class Solution:
    def kthGrammar(self, n: int, k: int) -> int:
        return bin(k - 1).count('1') & 1
```
