# 738 Monotone Increasing Digits 单调递增的数字

__Description__:
An integer has monotone increasing digits if and only if each pair of adjacent digits x and y satisfy x <= y.

Given an integer n, return the largest number that is less than or equal to n with monotone increasing digits.

__Example:__

Example 1:

Input: n = 10
Output: 9

Example 2:

Input: n = 1234
Output: 1234

Example 3:

Input: n = 332
Output: 299

__Constraints:__

0 <= n <= 10^9

__题目描述__:
给定一个非负整数 N，找出小于或等于 N 的最大的整数，同时这个整数需要满足其各个位数上的数字是单调递增。

（当且仅当每个相邻位数上的数字 x 和 y 满足 x <= y 时，我们称这个整数是单调递增的。）

__示例 :__

示例 1:

输入: N = 10
输出: 9

示例 2:

输入: N = 1234
输出: 1234

示例 3:

输入: N = 332
输出: 299

__说明:__
N 是在 [0, 10^9] 范围内的一个整数。

__思路__:

贪心
从后往前遍历找到第一个严格单调递减的数位
把这个数位左边的减 1, 右边的全部填充 9
利用数学可以将空间复杂度降为 O(1)
i 表示当前的位数, cur = result / i % 100 表示从 i 开始取两位数字
cur / 10 和 cur % 10 分别为这两位数字
比较大小之后 result = result / i * i - 1 来填充 9
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int monotoneIncreasingDigits(int n) 
    {
        int i = 1, result = n;
        while (i <= result / 10) 
        {
            int cur = result / i % 100;
            i *= 10;
            if (cur / 10 > cur % 10) result = result / i * i - 1;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int monotoneIncreasingDigits(int n) {
        int i = 1, result = n;
        while (i <= result / 10) {
            int cur = result / i % 100;
            i *= 10;
            if (cur / 10 > cur % 10) result = result / i * i - 1;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def monotoneIncreasingDigits(self, n: int) -> int:
        i, result = 1, n
        while i <= result // 10:
            cur = result // i % 100
            i *= 10
            if cur // 10 > cur % 10:
                result = result // i * i - 1
        return result
```
