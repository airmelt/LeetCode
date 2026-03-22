# 2595 Number of Even and Odd Bits 奇偶位数

__Description:__

You are given a __positive__ integer `n`.

Let `even` denote the number of even indices in the binary representation of `n` with value 1.

Let `odd` denote the number of odd indices in the binary representation of `n` with value 1.

Note that bits are indexed from right to left in the binary representation of a number.

Return the array `[even, odd]`.

__Example:__

Example 1:

```text
Input: n = 50

Output: [1,2]

Explanation:

The binary representation of 50 is 110010.

It contains 1 on indices 1, 4, and 5.
```

Example 2:

```text
Input: n = 2

Output: [0,1]

Explanation:

The binary representation of 2 is 10.

It contains 1 only on index 1.
```

__Constraints:__

- `1 <= n <= 1000`

__题目描述:__

给你一个 __正__ 整数 `n` 。

用 `even` 表示在 `n` 的二进制形式（下标从 __0__ 开始）中值为 `1` 的偶数下标的个数。

用 `odd` 表示在 `n` 的二进制形式（下标从 __0__ 开始）中值为 `1` 的奇数下标的个数。

请注意，在数字的二进制表示中，位下标的顺序 从右到左。

返回整数数组 `answer` ，其中 `answer = [even, odd]` 。

__示例:__

示例 1：

```text
输入：n = 50

输出：[1,2]

解释：

50 的二进制表示是 110010。

在下标 1，4，5 对应的值为 1。
```

示例 2：

```text
输入：n = 2

输出：[0,1]

解释：

2 的二进制表示是 10。

只有下标 1 对应的值为 1。
```

__提示：__

- `1 <= n <= 1000`

__思路:__

```text
1. 位运算
从小到大遍历每一位
如果是偶数位, 则累加到 even 中
如果是奇数位, 则累加到 odd 中
时间复杂度为 O(logN), 空间复杂度为 O(1)
2. 掩码
MASK = 0x55555555
如果是偶数位, 计算 n & MASK 的 1 的个数
如果是奇数位, 计算 n & ~MASK 的 1 的个数, 即 MASK 的反码
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> evenOddBit(int n) 
    {
        return { popcount(n & 0x55555555u), popcount(n & ~0x55555555u) };
    }
};
```

__Java__:

```Java
class Solution {
    public int[] evenOddBit(int n) {
        return new int[]{Integer.bitCount(n & 0x55555555), Integer.bitCount(n & ~0x55555555)};
    }
}
```

__Python__:

```Python
class Solution:
    def evenOddBit(self, n: int) -> List[int]:
        return [a := sum(i == '1' for i in bin(n)[2::][::-1][::2]), n.bit_count() - a]
```
