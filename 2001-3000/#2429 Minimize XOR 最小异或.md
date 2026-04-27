# 2429 Minimize XOR 最小异或

__Description:__

Given two positive integers `num1` and `num2`, find the positive integer `x` such that:

- `x` has the same number of set bits as `num2`, and
- The value `x XOR num1` is __minimal__.

Note that `XOR` is the bitwise XOR operation.

Return _the integer_ `x`. The test cases are generated such that `x` is __uniquely determined__.

The number of __set bits__ of an integer is the number of `1`'s in its binary representation.

__Example:__

Example 1:

```text
Input:  num1 = 3, num2 = 5
Output:  3
Explanation: 
The binary representations of num1 and num2 are 0011 and 0101, respectively.
The integer 3 has the same number of set bits as num2, and the value `3 XOR 3 = 0` is minimal.
```

Example 2:

```text
Input:  num1 = 1, num2 = 12
Output:  3
Explanation: 
The binary representations of num1 and num2 are 0001 and 1100, respectively.
The integer 3 has the same number of set bits as num2, and the value `3 XOR 1 = 2` is minimal.
```

__Constraints:__

- `1 <= num1, num2 <= 10 ^ 9`

__题目描述:__

给你两个正整数 `num1` 和 `num2` ，找出满足下述条件的正整数 `x` :

- `x` 的置位数和 `num2` 相同，且
- `x XOR num1` 的值 __最小__

注意 `XOR` 是按位异或运算。

返回整数 `x` 。题目保证，对于生成的测试用例， `x` 是 __唯一确定__ 的。

整数的 __置位数__ 是其二进制表示中 `1` 的数目。

__示例:__

示例 1：

```text
输入: num1 = 3, num2 = 5
输出: 3
解释: 
num1 和 num2 的二进制表示分别是 0011 和 0101 。
整数 3 的置位数与 num2 相同，且 `3 XOR 3 = 0` 是最小的。
```

示例 2：

```text
输入: num1 = 1, num2 = 12
输出: 3
解释: 
num1 和 num2 的二进制表示分别是 0001 和 1100 。
整数 3 的置位数与 num2 相同，且 `3 XOR 1 = 2` 是最小的。
```

__提示：__

- `1 <= num1, num2 <= 10 ^ 9`

__思路:__

```text
位运算
计算 num1 和 num2 的置位数
如果 num1 的置位数大于 num2 的置位数, 将 num1 的置位数减小到 num2 的置位数, 并将所有低位的 1 置为 0
如果 num1 的置位数小于 num2 的置位数, 将 num1 的置位数增加到 num2 的置位数, 并将所有低位的 0 置为 1
置 1 可以用 num1 |= num1 + 1
置 0 可以用 num1 &= num1 - 1
时间复杂度为 O(logN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimizeXor(int num1, int num2) 
    {
        for (int c1 = popcount((unsigned) num1), c2 = popcount((unsigned) num2); c2 < c1; c2++) num1 &= num1 - 1;
        for (int c1 = popcount((unsigned) num1), c2 = popcount((unsigned) num2); c2 > c1; c2--) num1 |= num1 + 1;
        return num1;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimizeXor(int num1, int num2) {
        for (int c1 = Integer.bitCount(num1), c2 = Integer.bitCount(num2); c2 < c1; c2++) num1 &= num1 - 1;
        for (int c1 = Integer.bitCount(num1), c2 = Integer.bitCount(num2); c2 > c1; c2--) num1 |= num1 + 1;
        return num1;
    }
}
```

__Python__:

```Python
class Solution:
    def minimizeXor(self, num1: int, num2: int) -> int:
        for i in range(num1.bit_count() - num2.bit_count()):
            num1 &= num1 - 1
        for i in range(num2.bit_count() - num1.bit_count()):
            num1 |= num1 + 1
        return num1
```
