# 1545 Find Kth Bit in Nth Binary String 找出第N个二进制字符串中的第K位

__Description:__

Given two positive integers `n` and `k`, the binary string `Sn` is formed as follows:

- `S1 = "0"`
- `Si = Si - 1 + "1" + reverse(invert(Si - 1))` for `i > 1`

Where `+` denotes the concatenation operation, `reverse(x)` returns the reversed string `x`, and `invert(x)` inverts all the bits in `x` ( `0` changes to `1` and `1` changes to `0`).

For example, the first four strings in the above sequence are:

- `S1 = "0"`
- `S2 = "0__1__1"`
- `S3 = "011__1__001"`
- `S4 = "0111001__1__0110001"`

Return _the_ `k ^ th` _bit_ _in_ `Sn`. It is guaranteed that `k` is valid for the given `n`.

__Example:__

Example 1:

```text
Input: n = 3, k = 1
Output: "0"
Explanation: S3 is "0111001".
The 1st bit is "0".
```

Example 2:

```text
Input: n = 4, k = 11
Output: "1"
Explanation: S4 is "011100110110001".
The 11th bit is "1".
```

__Constraints:__

- `1 <= n <= 20`
- `1 <= k <= 2 ^ n - 1`

__题目描述:__

给你两个正整数 `n` 和 `k`，二进制字符串  `Sn` 的形成规则如下：

- `S1 = "0"`
- 当 `i > 1` 时， `Si = Si-1 + "1" + reverse(invert(Si-1))`

其中 `+` 表示串联操作， `reverse(x)` 返回反转 `x` 后得到的字符串，而 `invert(x)` 则会翻转 x 中的每一位（0 变为 1，而 1 变为 0）。

例如，符合上述描述的序列的前 4 个字符串依次是：

- `S1 = "0"`
- `S2 = "0__1__1"`
- `S3 = "011__1__001"`
- `S4 = "0111001__1__0110001"`

请你返回  `Sn` 的 __第 `k` 位字符__ ，题目数据保证 `k` 一定在 `Sn` 长度范围以内。

__示例:__

示例 1：

```text
输入：n = 3, k = 1
输出："0"
解释：S3 为 "0111001"，其第 1 位为 "0" 。
```

示例 2：

```text
输入：n = 4, k = 11
输出："1"
解释：S4 为 "011100110110001"，其第 11 位为 "1" 。
```

示例 3：

```text
输入：n = 1, k = 1
输出："0"
```

示例 4：

```text
输入：n = 2, k = 3
输出："1"
```

__提示：__

- `1 <= n <= 20`
- `1 <= k <= 2 ^ n - 1`

__思路:__

```text
递归模拟
当 k == 1 时, 第一位一定为 '0'
每次形成下一个字符时, 前一半字符是相同的, 正中间的字符一定是 1
每次形成的字符串的长度为 2 ^ n - 1
找到正中间的位置, 如果在前半部分去前一个字符中寻找第 k 个字符否则寻找 2 ^ n - k 位置对应的字符并进行翻转
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    char findKthBit(int n, int k) 
    {
        return k == 1 ? '0' : (k == (1 << (n - 1)) ? '1' : (k < (1 << (n - 1)) ? findKthBit(n - 1, k) : (findKthBit(n - 1, (1 << n) - k) == '1' ? '0' : '1')));
    }
};
```

__Java__:

```Java
class Solution {
    public char findKthBit(int n, int k) {
        return k == 1 ? '0' : (k == (1 << (n - 1)) ? '1' : (k < (1 << (n - 1)) ? findKthBit(n - 1, k) : (findKthBit(n - 1, (1 << n) - k) == '1' ? '0' : '1')));
    }
}
```

__Python__:

```Python
class Solution:
    def findKthBit(self, n: int, k: int) -> str:
        return '0' if k == 1 else '1' if k == (mid := 1 << (n - 1)) else self.findKthBit(n - 1, k) if k < mid else "0" if self.findKthBit(n - 1, (mid << 1) - k) == "1" else "1"
```
