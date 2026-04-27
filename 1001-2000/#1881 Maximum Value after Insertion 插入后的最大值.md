# 1881 Maximum Value after Insertion 插入后的最大值

__Description:__

You are given a very large integer `n`, represented as a string,​​​​​​ and an integer digit `x`. The digits in `n` and the digit `x` are in the __inclusive__ range `[1, 9]`, and `n` may represent a _negative_ number.

You want to __maximize__ `n`__'s numerical value__ by inserting `x` anywhere in the decimal representation of `n`​​​​​​. You __cannot__ insert `x` to the left of the negative sign.

- For example, if `n = 73` and `x = 6`, it would be best to insert it between `7` and `3`, making `n = 763`.
- If `n = -55` and `x = 2`, it would be best to insert it before the first `5`, making `n = -255`.

Return _a string representing the __maximum__ value of_ `n`_​​​​​​ after the insertion_.

__Example:__

Example 1:

```text
Input: n = "99", x = 9
Output: "999"
Explanation: The result is the same regardless of where you insert 9.
```

Example 2:

```text
Input: n = "-13", x = 2
Output: "-123"
Explanation: You can make n one of {-213, -123, -132}, and the largest of those three is -123.
```

__Constraints:__

- `1 <= n.length <= 10 ^ 5`
- `1 <= x <= 9`
- The digits in `n`​​​ are in the range `[1, 9]`.
- `n` is a valid representation of an integer.
- In the case of a negative `n`,​​​​​​ it will begin with `'-'`.

__题目描述:__

给你一个非常大的整数 `n` 和一个整数数字 `x` ，大整数 `n` 用一个字符串表示。 `n` 中每一位数字和数字 `x` 都处于闭区间 `[1, 9]` 中，且 `n` 可能表示一个 __负数__ 。

你打算通过在 `n` 的十进制表示的任意位置插入 `x` 来 __最大化__ `n` 的 __数值__ ​​​​​​。但 __不能__ 在负号的左边插入 `x` 。

- 例如，如果 `n = 73` 且 `x = 6` ，那么最佳方案是将 `6` 插入 `7` 和 `3` 之间，使 `n = 763` 。
- 如果 `n = -55` 且 `x = 2` ，那么最佳方案是将 `2` 插在第一个 `5` 之前，使 `n = -255` 。

返回插入操作后，用字符串表示的 `n` 的最大值。

__示例:__

示例 1：

```text
输入：n = "99", x = 9
输出："999"
解释：不管在哪里插入 9 ，结果都是相同的。
```

示例 2：

```text
输入：n = "-13", x = 2
输出："-123"
解释：向 n 中插入 x 可以得到 -213、-123 或者 -132 ，三者中最大的是 -123 。
```

__提示：__

- `1 <= n.length <= 10 ^ 5`
- `1 <= x <= 9`
- `n`​​​ 中每一位的数字都在闭区间 `[1, 9]` 中。
- `n` 代表一个有效的整数。
- 当 `n` 表示负数时，将会以字符 `'-'` 开始。

__思路:__

```text
贪心
按照 n 开头是否为 '-' 分正负讨论
如果为正, 从开头开始找到第一个小于 x 的位置插入
如果为正, 从开头开始找到第一个大于 x 的位置插入
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string maxValue(string n, int x) 
    {
        int i = n.front() == '-', m = n.size();
        if (i)
        {
            for (; i < m and n[i] <= x + '0'; i++);
            n.insert(i, 1, (char)(x + '0'));
        }
        else
        {
            for (; i < m && n[i] >= x + '0'; i++);
            n.insert(i, 1, (char)(x + '0'));
        }
        return n;
    }
};
```

__Java__:

```Java
class Solution {
    public String maxValue(String n, int x) {
        StringBuilder sb = new StringBuilder();
        if (n.charAt(0) == '-') {
            for (int i = 1; i < n.length(); i++) if (n.charAt(i) - '0' > x) return new String(sb.append(n.substring(0, i)).append(x).append(n.substring(i)));
        } else {
            for (int i = 0; i < n.length(); i++) if (n.charAt(i) - '0' < x) return new String(sb.append(n.substring(0, i)).append(x).append(n.substring(i)));
        }
        return n + x;
    }
}
```

__Python__:

```Python
class Solution:
    def maxValue(self, n: str, x: int) -> str:
        i, x, m = n[0] == '-', str(x), len(n)
        if i:
            while i < m and n[i] <= x:
                i += 1
        else:
            while i < m and n[i] >= x:
                i += 1
        return n[:i] + x + n[i:]
```
