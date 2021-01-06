# 50 Pow(x, n) Pow(x, n)

__Description__:
Implement pow(x, n), which calculates x raised to the power n (xn).

__Example:__

Example 1:

Input: 2.00000, 10
Output: 1024.00000

Example 2:

Input: 2.10000, 3
Output: 9.26100

Example 3:

Input: 2.00000, -2
Output: 0.25000
Explanation: 2-2 = 1/22 = 1/4 = 0.25

__Note:__

-100.0 < x < 100.0
n is a 32-bit signed integer, within the range [−2^31, 2^31 − 1]

__题目描述__:
实现 pow(x, n) ，即计算 x 的 n 次幂函数。

__示例 :__

示例 1:

输入: 2.00000, 10
输出: 1024.00000

示例 2:

输入: 2.10000, 3
输出: 9.26100

示例 3:

输入: 2.00000, -2
输出: 0.25000
解释: 2-2 = 1/22 = 1/4 = 0.25

__说明:__

-100.0 < x < 100.0
n 是 32 位有符号整数，其数值范围是 [−2^31, 2^31 − 1] 。

__思路__:

快速幂算法
每次将 n减半, 如果是奇数, 乘 x, 否则 x自乘
时间复杂度O(lgn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    double myPow(double x, int n) 
    {
        double result = 1.0;
        for (int i = n; i != 0; i /= 2) 
        {
            if (i & 1) result *= x;
            x *= x;
        }
        return n < 0 ? 1 / result : result;
    }
};
```

__Java__:

```Java
class Solution {
    public double myPow(double x, int n) {
        double result = 1.0;
        for (int i = n; i != 0; i /= 2) {
            if ((i & 1) != 0) result *= x;
            x *= x;
        }
        return n < 0 ? 1 / result : result;
    }
}
```

__Python__:

```Python
class Solution:
    def myPow(self, x: float, n: int) -> float:
        return x ** n
```
