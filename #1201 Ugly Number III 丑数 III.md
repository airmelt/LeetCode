# 1201 Ugly Number III 丑数 III

__Description__:
An ugly number is a positive integer that is divisible by a, b, or c.

Given four integers n, a, b, and c, return the nth ugly number.

__Example:__

Example 1:

Input: n = 3, a = 2, b = 3, c = 5
Output: 4
Explanation: The ugly numbers are 2, 3, 4, 5, 6, 8, 9, 10... The 3rd is 4.

Example 2:

Input: n = 4, a = 2, b = 3, c = 4
Output: 6
Explanation: The ugly numbers are 2, 3, 4, 6, 8, 9, 10, 12... The 4th is 6.

Example 3:

Input: n = 5, a = 2, b = 11, c = 13
Output: 10
Explanation: The ugly numbers are 2, 4, 6, 8, 10, 11, 12, 13... The 5th is 10.

__Constraints:__

1 <= n, a, b, c <= 109
1 <= a \* b \* c <= 10^18
It is guaranteed that the result will be in range [1, 2 \* 10^9].

__题目描述__:
给你四个整数：n 、a 、b 、c ，请你设计一个算法来找出第 n 个丑数。

丑数是可以被 a 或 b 或 c 整除的 正整数 。

__示例 :__

示例 1：

输入：n = 3, a = 2, b = 3, c = 5
输出：4
解释：丑数序列为 2, 3, 4, 5, 6, 8, 9, 10... 其中第 3 个是 4。

示例 2：

输入：n = 4, a = 2, b = 3, c = 4
输出：6
解释：丑数序列为 2, 3, 4, 6, 8, 9, 10, 12... 其中第 4 个是 6。

示例 3：

输入：n = 5, a = 2, b = 11, c = 13
输出：10
解释：丑数序列为 2, 4, 6, 8, 10, 11, 12, 13... 其中第 5 个是 10。

示例 4：

输入：n = 1000000000, a = 2, b = 217983653, c = 336916467
输出：1999999984

__提示:__

1 <= n, a, b, c <= 10^9
1 <= a \* b \* c <= 10^18
本题结果在 [1, 2 \* 10^9] 的范围内

__思路__:

二分法 ➕ 容斥原理
第 count 个数可以用 mid / a + mid / b + mid / c - mid / lcm(a, b) - mid / lcm(a, c) - mid / lcm(b, c) + mid / lcm(a, b, c) 求得
再用二分法快速缩小范围
lcm(a, b) 表示 a 和 b 的最小公倍数, 可由 a \* b / gcd(a, b) 求得, 其中 gcd(a, b) 为 a 和 b 的最大公约数, 使用辗转相除法可以很容易得 gcd(a, b)
时间复杂度为 O(lgabc), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int nthUglyNumber(int n, int a, int b, int c) 
    {
        long left = 1, right = 2e9, ab = lcm<long>(a, b), bc = lcm<long>(b, c), ac = lcm<long>(a, c), abc = lcm<long>(ab, c);
        while (left < right) 
        {
            long mid = left + ((right - left) >> 1);
            long count = mid / a + mid / b + mid / c - mid / ab - mid / ac - mid / bc + mid / abc;
            if (count >= n) right = mid;
            else left = mid + 1;
        }
        return left;
    }
};
```

__Java__:

```Java
class Solution {
    public int nthUglyNumber(int n, int a, int b, int c) {
        long left = 1, right = 2_000_000_000, ab = lcm(a, b), bc = lcm(b, c), ac = lcm(a, c), abc = lcm(ab, c);
        while (left < right) {
            long mid = left + ((right - left) >>> 1);
            long count = mid / a + mid / b + mid / c - mid / ab - mid / ac - mid / bc + mid / abc;
            if (count >= n) right = mid;
            else left = mid + 1;
        }
        return (int)left;
    }
    
    private long lcm(long a, long b) {
        return a * b / gcd(a, b);
    }
    
    private long gcd(long a, long b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

__Python__:

```Python
class Solution:
    def nthUglyNumber(self, n: int, a: int, b: int, c: int) -> int:
        left, right = 1, 2 * 10 ** 9
        while left < right:
            mid = left + ((right - left) >> 1)
            if (count := mid // a + mid // b + mid // c - mid // lcm(a, b) - mid // lcm(a, c) - mid // lcm(b, c) + mid // lcm(a, b, c)) >= n:
                right = mid
            else:
                left = mid + 1
        return left
```
