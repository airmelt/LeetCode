# 479 Largest Palindrome Product 最大回文数乘积

__Description__:
Find the largest palindrome made from the product of two n-digit numbers.

Since the result could be very large, you should return the largest palindrome mod 1337.

__Example:__

Input: 2

Output: 987

Explanation: 99 x 91 = 9009, 9009 % 1337 = 987

__Note:__

The range of n is [1,8].

__题目描述__:
你需要找到由两个 n 位数的乘积组成的最大回文数。

由于结果会很大，你只需返回最大回文数 mod 1337得到的结果。

__示例 :__

输入: 2

输出: 987

解释: 99 x 91 = 9009, 9009 % 1337 = 987

__说明:__

n 的取值范围为 [1,8]。

__思路__:

1. 总共就 10个数据, 可以用打表解决
时间复杂度O(1), 空间复杂度O(1)
2. 从最大的 n位数 - 1开始构造回文数
比如 2位数, 最大为 99, 99 \* 99 = 9801 < 9889, 9889不是两个 2位数的乘积,
平方都达不到的一定不是
时间复杂度O(10 ^ n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int largestPalindrome(int n) 
    {
        int upper = pow(10, n) - 1;
        for (int i = upper - 1; i > upper / 10; i--)
        {
            string s = to_string(i);
            long target = stol(s + string(s.rbegin(), s.rend()));
            for (long j = upper; j * j >= target; j--) if (!(target % j)) return (int)(target % 1337);
        }
        return 9;
    }
};
```

__Java__:

```Java
class Solution {
    public int largestPalindrome(int n) {
        return new int[]{0, 9, 987, 123, 597, 677, 1218, 877, 475}[n];
    }
}
```

__Python__:

```Python
class Solution:
    def largestPalindrome(self, n: int) -> int:
        return [0, 9, 987, 123, 597, 677, 1218, 877, 475][n]
```
