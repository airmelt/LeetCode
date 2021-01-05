# 400 Nth Digit 第N个数字

__Description__:
Find the nth digit of the infinite integer sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ...

__Note:__
n is positive and will fit within the range of a 32-bit signed integer (n < 231).

**Example :**

Example 1:
Input:
3
Output:
3

Example 2:
Input:
11
Output:
0

__Explanation:__
The 11th digit of the sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ... is a 0, which is part of the number 10.

__题目描述__:
在无限的整数序列 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ...中找到第 n 个数字。

__注意:__
n 是正数且在32为整形范围内 ( n < 231)。

**示例 :**

示例 1:
输入:
3
输出:
3

示例 2:
输入:
11
输出:
0

__说明:__
第11个数字在序列 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ... 里是0，它是10的一部分。

__思路__:

题意是1234567891011...依次排列的正整数序列中找到第 n位的数字

1. 先保存下来 i位数总长, 1位数长 9位, 2位数180, 再计算
2. 找规律直接用迭代计算
时间复杂度O(logn), 空间复杂度O(1)

__代码__:
__C++_:

```C++
class Solution 
{
public:
    int findNthDigit(int n) 
    {
        n--;
        int num = 1, i = 1;
        while (n / 9 / num / i >= 1) 
        {
            n -= 9 * num * i;
            i++;
            num *= 10;
        }
        return to_string(num + n / i)[n % i] - '0';
    }
};
```

__Java__:

```Java
class Solution {
    public int findNthDigit(int n) {
        int[] temp = {0, 9, 180, 2700, 36000, 450000, 5400000, 63000000, 720000000, 2147483647};
        for (int i = 1; i < 10; i++) {
            if (n <= temp[i]) {
                int num = (int)Math.pow(10, i - 1) + (n - 1) / i;
                int j = (n - 1) % i;
                return Integer.toString(num).charAt(j) - '0';
            } else {
                n -= temp[i];
            }
        }
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def findNthDigit(self, n: int) -> int:
        num, i = 0, 1
        while(n > num + i * 9 * 10 ** (i - 1)):
            num += i * 9 * 10 ** (i - 1)
            i += 1
        a, b = 10 ** (i - 1) - 1 + (n - num) // i, (n - num) % i
        if not b:
            return a % 10
        else:
            a += 1
            return (a // 10 ** (i - b)) % 10
```
