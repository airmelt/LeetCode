# 2220 Minimum Bit Flips to Convert Number 转换数字的最少位翻转次数

__Description:__

A __bit flip__ of a number `x` is choosing a bit in the binary representation of `x` and __flipping__ it from either `0` to `1` or `1` to `0`.

- For example, for `x = 7`, the binary representation is `111` and we may choose any bit (including any leading zeros not shown) and flip it. We can flip the first bit from the right to get `110`, flip the second bit from the right to get `101`, flip the fifth bit from the right (a leading zero) to get `10111`, etc.

Given two integers `start` and `goal`, return _the __minimum__ number of __bit flips__ to convert_ `start` _to_ `goal`.

__Example:__

Example 1:

```text
Input: start = 10, goal = 7
Output: 3
Explanation: The binary representation of 10 and 7 are 1010 and 0111 respectively. We can convert 10 to 7 in 3 steps:
```

- Flip the first bit from the right: 1010 -> 1011.
- Flip the third bit from the right: 1011 -> 1111.
- Flip the fourth bit from the right: 1111 -> 0111.

It can be shown we cannot convert 10 to 7 in less than 3 steps. Hence, we return 3.

Example 2:

```text
Input: start = 3, goal = 4
Output: 3
Explanation: The binary representation of 3 and 4 are 011 and 100 respectively. We can convert 3 to 4 in 3 steps:
```

- Flip the first bit from the right: 011 -> 010.
- Flip the second bit from the right: 010 -> 000.
- Flip the third bit from the right: 000 -> 100.

It can be shown we cannot convert 3 to 4 in less than 3 steps. Hence, we return 3.

__Constraints:__

- `0 <= start, goal <= 10 ^ 9`

__题目描述:__

一次 __位翻转__ 定义为将数字 `x` 二进制中的一个位进行 __翻转__ 操作，即将 `0` 变成 `1` ，或者将 `1` 变成 `0` 。

- 比方说， `x = 7` ，二进制表示为 `111` ，我们可以选择任意一个位（包含没有显示的前导 0 ）并进行翻转。比方说我们可以翻转最右边一位得到 `110` ，或者翻转右边起第二位得到 `101` ，或者翻转右边起第五位（这一位是前导 0 ）得到 `10111` 等等。

给你两个整数 `start` 和 `goal` ，请你返回将 `start` 转变成 `goal` 的 __最少位翻转__ 次数。

__示例:__

示例 1：

```text
输入：start = 10, goal = 7
输出：3
解释：10 和 7 的二进制表示分别为 1010 和 0111 。我们可以通过 3 步将 10 转变成 7 ：
```

- 翻转右边起第一位得到：1010 -> 1011 。
- 翻转右边起第三位：1011 -> 1111 。
- 翻转右边起第四位：1111 -> 0111 。

我们无法在 3 步内将 10 转变成 7 。所以我们返回 3 。

示例 2：

```text
输入：start = 3, goal = 4
输出：3
解释：3 和 4 的二进制表示分别为 011 和 100 。我们可以通过 3 步将 3 转变成 4 ：
```

- 翻转右边起第一位：011 -> 010 。
- 翻转右边起第二位：010 -> 000 。
- 翻转右边起第三位：000 -> 100 。

我们无法在 3 步内将 3 变成 4 。所以我们返回 3 。

__提示：__

- `0 <= start, goal <= 10 ^ 9`

__思路:__

```text
位运算
由于数字不会超过 10 ^ 9, 因此只需要遍历 32 位即可
遍历 32 位, 比较 start 和 goal 的每一位是否相同
如果不同, 则 result 自增
比较可以用移位运算和与运算 start >> i & 1 != goal >> i & 1
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minBitFlips(int start, int goal) 
    {
        int result = 0;
        for (int i = 0; i < 32; i++) result += ((start >> i) & 1) != ((goal >> i) & 1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minBitFlips(int start, int goal) {
        int result = 0;
        for (int i = 0; i < 32; i++) if (((start >> i) & 1) != ((goal >> i) & 1)) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minBitFlips(self, start: int, goal: int) -> int:
        return sum((start & (1 << i) != (goal & (1 << i))) for i in range(32))
```
