# 1137 N-th Tribonacci Number 第 N 个泰波那契数

__Description__:
The Tribonacci sequence Tn is defined as follows:

T0 = 0, T1 = 1, T2 = 1, and Tn+3 = Tn + Tn+1 + Tn+2 for n >= 0.

Given n, return the value of Tn.

__Example:__

Example 1:

Input: n = 4
Output: 4
Explanation:
T_3 = 0 + 1 + 1 = 2
T_4 = 1 + 1 + 2 = 4

Example 2:

Input: n = 25
Output: 1389537

__Constraints:__

0 <= n <= 37
The answer is guaranteed to fit within a 32-bit integer, ie. answer <= 2^31 - 1.

__题目描述__:
泰波那契序列 Tn 定义如下：

T0 = 0, T1 = 1, T2 = 1, 且在 n >= 0 的条件下 Tn+3 = Tn + Tn+1 + Tn+2

给你整数 n，请返回第 n 个泰波那契数 Tn 的值。

__示例 :__

示例 1：

输入：n = 4
输出：4
解释：
T_3 = 0 + 1 + 1 = 2
T_4 = 1 + 1 + 2 = 4

示例 2：

输入：n = 25
输出：1389537

__提示：__

0 <= n <= 37
答案保证是一个 32 位整数，即 answer <= 2^31 - 1。

__思路__:

1. 类似斐波拉契数列的计算, 递归要用动态规划或者尾递归优化
时间复杂度O(n), 空间复杂度O(1)
2. 直接找出前 37个泰波那契数
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int tribonacci(int n) 
    {
        int a = 0, b = 0, c = 1, result = n;
        for (int i = 1; i < n; ++i)
        {
            result = a + b + c;
            a = b;
            b = c;
            c = result;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int tribonacci(int n) {
        int a = 0, b = 0, c = 1, result = n;
        for (int i = 1; i < n; ++i) {
            result = a + b + c;
            a = b;
            b = c;
            c = result;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def tribonacci(self, n: int) -> int:
        return [0, 1, 1, 2, 4, 7, 13, 24, 44, 81, 149, 274, 504, 927, 1705, 3136, 5768, 10609, 19513, 35890, 66012, 121415, 223317, 410744, 755476, 1389537, 2555757, 4700770, 8646064, 15902591, 29249425, 53798080, 98950096, 181997601, 334745777, 615693474, 1132436852, 2082876103][n]
```
