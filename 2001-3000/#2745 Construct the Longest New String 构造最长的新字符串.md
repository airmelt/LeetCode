# 2745 Construct the Longest New String 构造最长的新字符串

__Description:__

You are given three integers `x`, `y`, and `z`.

You have `x` strings equal to `"AA"`, `y` strings equal to `"BB"`, and `z` strings equal to `"AB"`. You want to choose some (possibly all or none) of these strings and concatenate them in some order to form a new string. This new string must not contain `"AAA"` or `"BBB"` as a substring.

Return the maximum possible length of the new string.

A substring is a contiguous non-empty sequence of characters within a string.

__Example:__

Example 1:

```text
Input: x = 2, y = 5, z = 1
Output: 12
Explanation: We can concatenate the strings "BB", "AA", "BB", "AA", "BB", and "AB" in that order. Then, our new string is "BBAABBAABBAB". 
That string has length 12, and we can show that it is impossible to construct a string of longer length.
```

Example 2:

```text
Input: x = 3, y = 2, z = 2
Output: 14
Explanation: We can concatenate the strings "AB", "AB", "AA", "BB", "AA", "BB", and "AA" in that order. Then, our new string is "ABABAABBAABBAA". 
That string has length 14, and we can show that it is impossible to construct a string of longer length.
```

__Constraints:__

- `1 <= x, y, z <= 50`

__题目描述:__

给你三个整数 `x` ， `y` 和 `z` 。

这三个整数表示你有 `x` 个 `"AA"` 字符串， `y` 个 `"BB"` 字符串，和 `z` 个 `"AB"` 字符串。你需要选择这些字符串中的部分字符串（可以全部选择也可以一个都不选择），将它们按顺序连接得到一个新的字符串。新字符串不能包含子字符串 `"AAA"` 或者 `"BBB"` 。

请你返回 新字符串的最大可能长度。

子字符串 是一个字符串中一段连续 非空 的字符序列。

__示例:__

示例 1：

```text
输入：x = 2, y = 5, z = 1
输出：12
解释： 我们可以按顺序连接 "BB" ，"AA" ，"BB" ，"AA" ，"BB" 和 "AB" ，得到新字符串 "BBAABBAABBAB" 。
字符串长度为 12 ，无法得到一个更长的符合题目要求的字符串。
```

示例 2：

```text
输入：x = 3, y = 2, z = 2
输出：14
解释：我们可以按顺序连接 "AB" ，"AB" ，"AA" ，"BB" ，"AA" ，"BB" 和 "AA" ，得到新字符串 "ABABAABBAABBAA" 。
字符串长度为 14 ，无法得到一个更长的符合题目要求的字符串。
```

__提示：__

- `1 <= x, y, z <= 50`

__思路:__

```text
数学
只看 AA 和 BB
选择 min(x, y) 的两倍, 至少可以组成 AABBAABBAA... 的形式
如果 x != y 那最后还可以挂载一个多的
然后 AB 可以插入到 AA 和 BB 中间, 也可以插入到 BB 和 AA 中间, 也可以全部放到最后, 所以 AB 可以全部加入
所以结果为 ((min(x, y) << 1) + (x != y) + z) << 1
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int longestString(int x, int y, int z) 
    {
        return ((min(x, y) << 1) + (x != y) + z) << 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int longestString(int x, int y, int z) {
        return ((Math.min(x, y) << 1) + (x != y ? 1 : 0) + z) << 1;
    }
}
```

__Python__:

```Python
class Solution:
    def longestString(self, x: int, y: int, z: int) -> int:
        return ((min(x, y) << 1) + (x != y) + z) << 1
```
