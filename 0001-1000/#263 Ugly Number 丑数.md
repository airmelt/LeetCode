# 263 Ugly Number 丑数

__Description__:
Write a program to check whether a given number is an ugly number.

Ugly numbers are positive numbers whose prime factors only include 2, 3, 5.

**Example :**

Example 1:
Input: 6
Output: true
Explanation: 6 = 2 × 3

Example 2:
Input: 8
Output: true
Explanation: 8 = 2 × 2 × 2

Example 3:
Input: 14
Output: false
Explanation: 14 is not ugly since it includes another prime factor 7.

__Note:__
1 is typically treated as an ugly number.
Input is within the 32-bit signed integer range: [−2^31,  2^31 − 1].

__题目描述__:
编写一个程序判断给定的数是否为丑数。

丑数就是只包含质因数 2, 3, 5 的正整数。

**示例 :**

示例 1:
输入: 6
输出: true
解释: 6 = 2 × 3

示例 2:
输入: 8
输出: true
解释: 8 = 2 × 2 × 2

示例 3:
输入: 14
输出: false
解释: 14 不是丑数，因为它包含了另外一个质因数 7。

__说明：__
1 是丑数。
输入不会超过 32 位有符号整数的范围: [−2^31,  2^31 − 1]。

__思路__:

判断能否除尽 2/3/5即可
可以用迭代或者递归
时间复杂度O(lgn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool isUgly(int num) 
    {
        if (num < 1) return false;
        if (num == 1) return true;
        if (!(num & 1)) return isUgly(num >> 1);
        if (!(num % 3)) return isUgly(num / 3);
        if (!(num % 5)) return isUgly(num / 5);
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isUgly(int num) {
        if (num < 1) {
            return false;
        }
        while ((num & 1) == 0) num >>= 1;
        while (num % 3 == 0) num /= 3;
        while (num % 5 == 0) num /= 5;
        return num == 1;
    }
}
```

__Python__:

```Python
class Solution:
    def isUgly(self, num: int) -> bool:
        if num < 1:
            return False
        if num == 1:
            return True
        while not num & 1:
            num >>= 1
        while not num % 3:
            num //= 3
        while not num % 5:
            num //= 5
        return num == 1
```
