# 2749 Minimum Operations to Make the Integer Zero 得到整数零需要执行的最少操作数

__Description:__

You are given two integers `num1` and `num2`.

In one operation, you can choose integer `i` in the range `[0, 60]` and subtract `2 ^ i + num2` from `num1`.

Return _the integer denoting the __minimum__ number of operations needed to make_ `num1` _equal to_ `0`.

If it is impossible to make `num1` equal to `0`, return `-1`.

__Example:__

Example 1:

```text
Input: num1 = 3, num2 = -2
Output: 3
Explanation: We can make 3 equal to 0 with the following operations:
```

- We choose i = 2 and subtract 22 + (-2) from 3, 3 - (4 + (-2)) = 1.
- We choose i = 2 and subtract 22 + (-2) from 1, 1 - (4 + (-2)) = -1.
- We choose i = 0 and subtract 20 + (-2) from -1, (-1) - (1 + (-2)) = 0.

It can be proven, that 3 is the minimum number of operations that we need to perform.

Example 2:

```text
Input: num1 = 5, num2 = 7
Output: -1
Explanation: It can be proven, that it is impossible to make 5 equal to 0 with the given operation.
```

__Constraints:__

- `1 <= num1 <= 10 ^ 9`
- `-10 ^ 9 <= num2 <= 10 ^ 9`

__题目描述:__

给你两个整数: `num1` 和 `num2` 。

在一步操作中，你需要从范围 `[0, 60]` 中选出一个整数 `i` ，并从 `num1` 减去 `2 ^ i + num2` 。

请你计算，要想使 `num1` 等于 `0` 需要执行的最少操作数，并以整数形式返回。

如果无法使 `num1` 等于 `0` ，返回 `-1` 。

__示例:__

示例 1：

```text
输入：num1 = 3, num2 = -2
输出：3
解释：可以执行下述步骤使 3 等于 0 ：
```

- 选择 i = 2 ，并从 3 减去 22 + (-2) ，num1 = 3 - (4 + (-2)) = 1 。
- 选择 i = 2 ，并从 1 减去 22 + (-2) ，num1 = 1 - (4 + (-2)) = -1 。
- 选择 i = 0 ，并从 -1 减去 20 + (-2) ，num1 = (-1) - (1 + (-2)) = 0 。

可以证明 3 是需要执行的最少操作数。

示例 2：

```text
输入：num1 = 5, num2 = 7
输出：-1
解释：可以证明，执行操作无法使 5 等于 0 。
```

__提示：__

- `1 <= num1 <= 10 ^ 9`
- `-10 ^ 9 <= num2 <= 10 ^ 9`

__思路:__

```text
数学
num1 需要减 i 次 num2, 结果为 2 的整次幂之和
这个整次幂之和的 1 的位数要不大于 i 时就可以成立
所以当 num1 - i * num2 的二进制表示的 1 不大于 i 时返回 i
但是如果 num1 - i * num2 比 i 还小时, 由于至少需要 2 ^ 0 = 1 个 i 相加, 即 i, 这时无论如何也不可能得到合法的 i, 返回 -1
如果遍历完 [1, 32] 里的 i 也不能得到, 也返回 -1
时间复杂度为 O(1), 空间复杂度为 O(1), 最多枚举 32 个整数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int makeTheIntegerZero(int num1, int num2) 
    {
        for (long long i = 1LL; i <= num1 - num2 * i; i++) if (i >= popcount((uint64_t)num1 - num2 * i)) return i;
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int makeTheIntegerZero(int num1, int num2) {
        for (long i = 1L; i <= num1 - num2 * i; i++) if (i >= Long.bitCount(num1 - num2 * i)) return (int)i;
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def makeTheIntegerZero(self, num1: int, num2: int) -> int:
        return [j for j in a if j != -2][0] if (a := [-1 if (cur := num1 - i * num2) < i else i if bin(cur).count('1') <= i else -2 for i in range(1, 33)]) else -1
```
