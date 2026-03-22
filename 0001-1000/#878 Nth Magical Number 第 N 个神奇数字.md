# 878 Nth Magical Number 第 N 个神奇数字

__Description__:
A positive integer is magical if it is divisible by either a or b.

Given the three integers n, a, and b, return the nth magical number. Since the answer may be very large, return it modulo 10^9 + 7.

__Example:__

Example 1:

Input: n = 1, a = 2, b = 3
Output: 2

Example 2:

Input: n = 4, a = 2, b = 3
Output: 6

Example 3:

Input: n = 5, a = 2, b = 4
Output: 10

Example 4:

Input: n = 3, a = 6, b = 4
Output: 8

__Constraints:__

1 <= n <= 10^9
2 <= a, b <= 4 * 10^4

__题目描述__:
如果正整数可以被 A 或 B 整除，那么它是神奇的。

返回第 N 个神奇数字。由于答案可能非常大，返回它模 10^9 + 7 的结果。

__示例 :__

示例 1：

输入：N = 1, A = 2, B = 3
输出：2

示例 2：

输入：N = 4, A = 2, B = 3
输出：6

示例 3：

输入：N = 5, A = 2, B = 4
输出：10

示例 4：

输入：N = 3, A = 6, B = 4
输出：8

__提示:__

1 <= N <= 10^9
2 <= A <= 40000
2 <= B <= 40000

__思路__:

数学 ➕ 二分查找
先求出 a, b 的最小公倍数 lcm(a, b) = a \* b / gcd(a, b), 其中 gcd 表示最大公约数
由韦恩图可知 f(x) = x / a + x / b - x / lcm(a, b)
即找出 x / a + x / b - x / lcm(a, b) >= n 的最小 x
那么可以尝试采用二分查找, 找到 n 个满足能整除 a 或 b 的数
下界取 0, 上界取 n \* max(a, b) = 10 ^ 15
时间复杂度为 O(lg(n \* max(a, b))), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int nthMagicalNumber(int n, int a, int b) 
    {
        int MOD = 1e9 + 7, lcm = a * b / gcd(a, b);
        long left = 0, right = (long)1e15;
        while (left < right) 
        {
            long mid = left + ((right - left) >> 1);
            if (mid / a + mid / b - mid / lcm < n) left = mid + 1;
            else right = mid;
        }
        return (int)(left % MOD);
    }
};
```

__Java__:

```Java
class Solution {
    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
    
    public int nthMagicalNumber(int n, int a, int b) {
        int MOD = 1_000_000_007, lcm = a * b / gcd(a, b);
        long left = 0, right = (long)1e15;
        while (left < right) {
            long mid = left + ((right - left) >>> 1);
            if (mid / a + mid / b - mid / lcm < n) left = mid + 1;
            else right = mid;
        }
        return (int)(left % MOD);
    }
}
```

__Python__:

```Python
class Solution:
    def nthMagicalNumber(self, n: int, a: int, b: int) -> int:
        MOD, lcm, left, right = 10 ** 9 + 7, a * b // gcd(a, b), 0, 10 ** 15
        while left < right:
            if (mid := left + ((right - left) >> 1)) // a + mid // b - mid // lcm < n:
                left = mid + 1
            else:
                right = mid
        return left % MOD
```
