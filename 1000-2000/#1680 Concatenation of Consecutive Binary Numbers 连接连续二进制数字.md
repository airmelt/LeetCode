# 1680 Concatenation of Consecutive Binary Numbers 连接连续二进制数字

__Description:__

Given an integer `n`, return _the __decimal value__ of the binary string formed by concatenating the binary representations of_ `1` _to_ `n` _in order, __modulo___ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: n = 1
Output: 1
Explanation: "1" in binary corresponds to the decimal value 1.
```

Example 2:

```text
Input: n = 3
Output: 27
Explanation: In binary, 1, 2, and 3 corresponds to "1", "10", and "11".
After concatenating them, we have "11011", which corresponds to the decimal value 27.
```

Example 3:

```text
Input: n = 12
Output: 505379714
Explanation: The concatenation results in "1101110010111011110001001101010111100".
The decimal value of that is 118505380540.
After modulo 10^9 + 7, the result is 505379714.
```

__Constraints:__

- `1 <= n <= 10 ^ 5`

__题目描述:__

给你一个整数 `n` ，请你将 `1` 到 `n` 的二进制表示连接起来，并返回连接结果对应的 __十进制__ 数字对 `10 ^ 9 + 7` 取余的结果。

__示例:__

示例 1：

```text
输入：n = 1
输出：1
解释：二进制的 "1" 对应着十进制的 1 。
```

示例 2：

```text
输入：n = 3
输出：27
解释：二进制下，1，2 和 3 分别对应 "1" ，"10" 和 "11" 。
将它们依次连接，我们得到 "11011" ，对应着十进制的 27 。
```

示例 3：

```text
输入：n = 12
输出：505379714
解释：连接结果为 "1101110010111011110001001101010111100" 。
对应的十进制数字为 118505380540 。
对 10^9 + 7 取余后，结果为 505379714 。
```

__提示：__

- `1 <= n <= 10 ^ 5`

__思路:__

```text
位运算
前导 0 可利用 C++ 内置函数 __builtin_clz(i) 计算
遍历 [1, n], 每次结果需要向左移动 32 - __builtin_clz(i) 位, 再加上 i, 并对 10 ^ 9 + 7 取余
也可以每次到 2 的幂次方时, bit + 1, 用于计算左移位数
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int concatenatedBinary(int n) 
    {
        long long result = 0, MOD = 1e9 + 7;
        for (int i = 1; i <= n; i++) result = ((result << (32 - __builtin_clz(i))) % MOD + i) % MOD;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int concatenatedBinary(int n) {
        long result = 0, MOD = 1_000_000_007, bit = 0;
        for (int i = 1; i <= n; i++) {
            if ((i & (i - 1)) == 0) ++bit;
            result = ((result << bit) + i) % MOD;
        }
        return (int)result;
    }
}
```

__Python__:

```Python
class Solution:
    def concatenatedBinary(self, n: int) -> int:
        return int('0b' + ("".join([bin(i)[2:] for i in range(1, n + 1)])), 2) % int(10 ** 9 + 7)
```
