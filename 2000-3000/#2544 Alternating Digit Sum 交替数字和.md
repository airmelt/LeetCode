# 2544 Alternating Digit Sum 交替数字和

__Description:__

You are given a positive integer `n`. Each digit of `n` has a sign according to the following rules:

- The __most significant digit__ is assigned a __positive__ sign.
- Each other digit has an opposite sign to its adjacent digits.

Return the sum of all digits with their corresponding sign.

__Example:__

Example 1:

```text
Input: n = 521
Output: 4
Explanation: (+5) + (-2) + (+1) = 4.
```

Example 2:

```text
Input: n = 111
Output: 1
Explanation: (+1) + (-1) + (+1) = 1.
```

Example 3:

```text
Input: n = 886996
Output: 0
Explanation: (+8) + (-8) + (+6) + (-9) + (+9) + (-6) = 0.
```

__Constraints:__

- `1 <= n <= 10 ^ 9`

__题目描述:__

给你一个正整数 `n` 。 `n` 中的每一位数字都会按下述规则分配一个符号:

- __最高有效位__ 上的数字分配到 __正__ 号。
- 剩余每位上数字的符号都与其相邻数字相反。

返回所有数字及其对应符号的和。

__示例:__

示例 1：

```text
输入：n = 521
输出：4
解释：(+5) + (-2) + (+1) = 4
```

示例 2：

```text
输入：n = 111
输出：1
解释：(+1) + (-1) + (+1) = 1
```

示例 3：

```text
输入：n = 886996
输出：0
解释：(+8) + (-8) + (+6) + (-9) + (+9) + (-6) = 0
```

__提示：__

- `1 <= n <= 10 ^ 9`

__思路:__

```text
1. 字符串
将数字转换为字符串
逐个处理每一位
时间复杂度为 O(logN), 空间复杂度为 O(logN)
2. 数学
取每一位的方式是对 10 取模
然后将每一位的符号交替相加
并将数字除以 10
直到数字为 0
时间复杂度为 O(logN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int alternateDigitSum(int n) 
    {
        int result = 0, sign = 1;
        while (n) 
        {
            result += n % 10 * sign;
            sign = -sign;
            n /= 10;
        }
        return result * -sign;
    }
};
```

__Java__:

```Java
class Solution {
    public int alternateDigitSum(int n) {
        int result = 0, sign = 1;
        while (n > 0) {
            result += n % 10 * sign;
            sign = -sign;
            n /= 10;
        }
        return result * -sign;
    }
}
```

__Python__:

```Python
class Solution:
    def alternateDigitSum(self, n: int) -> int:
        return sum((-1) ** i * int(j) for i, j in enumerate(str(n)))
```
