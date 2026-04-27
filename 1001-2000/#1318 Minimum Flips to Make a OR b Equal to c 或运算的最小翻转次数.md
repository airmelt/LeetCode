# 1318 Minimum Flips to Make a OR b Equal to c 或运算的最小翻转次数

__Description:__

Given 3 positives numbers a, b and c. Return the minimum flips required in some bits of a and b to make ( a OR b == c ). (bitwise OR operation).
Flip operation consists of change any single bit 1 to 0 or change the bit 0 to 1 in their binary representation.

__Example:__

Example 1:

![Flips](https://assets.leetcode.com/uploads/2020/01/06/sample_3_1676.png)

Input: a = 2, b = 6, c = 5
Output: 3
Explanation: After flips a = 1 , b = 4 , c = 5 such that (a OR b == c)

Example 2:

Input: a = 4, b = 2, c = 7
Output: 1

Example 3:

Input: a = 1, b = 2, c = 3
Output: 0

__Constraints:__

1 <= a <= 10^9
1 <= b <= 10^9
1 <= c <= 10^9

__题目描述：__

给你三个正整数 a、b 和 c。

你可以对 a 和 b 的二进制表示进行位翻转操作，返回能够使按位或运算   a OR b == c  成立的最小翻转次数。

「位翻转操作」是指将一个数的二进制表示任何单个位上的 1 变成 0 或者 0 变成 1 。

__示例：__

示例 1：

![翻转](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/11/sample_3_1676.png)

输入：a = 2, b = 6, c = 5
输出：3
解释：翻转后 a = 1 , b = 4 , c = 5 使得 a OR b == c

示例 2：

输入：a = 4, b = 2, c = 7
输出：1

示例 3：

输入：a = 1, b = 2, c = 3
输出：0

__提示：__

1 <= a <= 10^9
1 <= b <= 10^9
1 <= c <= 10^9

__思路：__

位运算
对任何一位 a | b ^ c == 1, 说明 a 和 b 至少有 1 位与 c 不相同, 比如 a == 1, b == 0, c == 0, 需要翻转一次
另一种情况是 a == 1, b == 1, c == 0, 这种情况, 还需要 a & b & (a | b ^ c) 判断
时间复杂度为 O(1), 空间复杂度为 O(1)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int minFlips(int a, int b, int c) 
    {
        return __builtin_popcount(c ^= (a | b)) + __builtin_popcount(a & b & c);
    }
};
```

__Java__:

```Java
class Solution {
    public int minFlips(int a, int b, int c) {
        return Integer.bitCount(c ^= (a | b)) + Integer.bitCount(a & b & c);
    }
}
```

__Python__:

```Python
class Solution:
    def minFlips(self, a: int, b: int, c: int) -> int:
        return bin(d := ((a | b) ^ c)).count('1') + bin(a & b & d).count('1')
```
