# 233 Number of Digit One 数字 1 的个数

__Description__:
Given an integer n, count the total number of digit 1 appearing in all non-negative integers less than or equal to n.

__Example:__

Input: 13
Output: 6
Explanation: Digit 1 occurred in the following numbers: 1, 10, 11, 12, 13.

__题目描述__:
给定一个整数 n，计算所有小于等于 n 的非负整数中数字 1 出现的个数。

__示例 :__

输入: 13
输出: 6
解释: 数字 1 出现在以下数字中: 1, 10, 11, 12, 13 。

__思路__:

对于某个数, 按 10进位, 设 a = n / i, b = n % i
以百位为例:

1. 如12156, 当 a的个位为 1时, 百位为 1的数出现了 a / 10 \* 100 + (b + 1)次, a / 10 \* 100是因为对高位来说有 100-199, ..., 12000-12099, 共 1200个, b + 1是因为 12100-12156, 一共 b + 1个
2. 如12056, 当 a的个位为 0, 只取决于高位, 百位为 1的数出现了 a / 10 * 100次
3. 如12256, 当 a的个位大于 1, 只取决于高位, 百位为 1的数出现了 (a / 10 + 1) * 100次
这里 a > 2的 (a / 10 + 1)和 a = 1时的 a / 10可以合并成 (a + 8) / 10
时间复杂度O(lgn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int countDigitOne(int n) 
    {
        int result = 0;
        for (long i = 1; i <= n; i *= 10) result += (n / i + 8) / 10 * i + (n / i % 10 == 1) * (n % i + 1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countDigitOne(int n) {
        int result = 0;
        for (long i = 1; i <= n; i *= 10) result += (n / i + 8) / 10 * i + (n / i % 10 == 1 ? 1 : 0) * (n % i + 1);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countDigitOne(self, n: int) -> int:
        # return sum((n // (10 * i)) * i + min(max(n % (10 * i) - i + 1, 0), i) for i in (int('1' + '0' * j) for j in range(len(str(n)))))
        # return (n + 9) // 10 + self.countDigitOne(n // 10 - 1) * 10 + str(n // 10).count('1') * (n % 10 + 1) if n > 0 else 0
        return sum((n // i + 8) // 10 * i + (n // i % 10 == 1) * (n % i + 1) for i in (int('1' + '0' * j) for j in range(len(str(n)))))
```
