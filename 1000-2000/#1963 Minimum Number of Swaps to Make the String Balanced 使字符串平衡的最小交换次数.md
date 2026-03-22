# 1963 Minimum Number of Swaps to Make the String Balanced 使字符串平衡的最小交换次数

__Description:__

You are given a __0-indexed__ string `s` of __even__ length `n`. The string consists of __exactly__ `n / 2` opening brackets `'['` and `n / 2` closing brackets `']'`.

A string is called balanced if and only if:

- It is the empty string, or
- It can be written as `AB`, where both `A` and `B` are __balanced__ strings, or
- It can be written as `[C]`, where `C` is a __balanced__ string.

You may swap the brackets at any two indices any number of times.

Return _the __minimum__ number of swaps to make_ `s` ___balanced___.

__Example:__

Example 1:

```text
Input: s = "][]["
Output: 1
Explanation: You can make the string balanced by swapping index 0 with index 3.
The resulting string is "[[]]".
```

Example 2:

```text
Input: s = "]]][[["
Output: 2
Explanation: You can do the following to make the string balanced:
- Swap index 0 with index 4. s = "[]][][".
- Swap index 1 with index 5. s = "[[][]]".
The resulting string is "[[][]]".
```

Example 3:

```text
Input: s = "[]"
Output: 0
Explanation: The string is already balanced.
```

__Constraints:__

- `n == s.length`
- `2 <= n <= 10 ^ 6`
- `n` is even.
- `s[i]` is either `'['` or `']'`.
- The number of opening brackets `'['` equals `n / 2`, and the number of closing brackets `']'` equals `n / 2`.

__题目描述:__

给你一个字符串 `s` ，__下标从 0 开始__ ，且长度为偶数 `n` 。字符串 __恰好__ 由 `n / 2` 个开括号 `'['` 和 `n / 2` 个闭括号 `']'` 组成。

只有能满足下述所有条件的字符串才能称为 平衡字符串 ：

- 字符串是一个空字符串，或者
- 字符串可以记作 `AB` ，其中 `A` 和 `B` 都是 __平衡字符串__ ，或者
- 字符串可以写成 `[C]` ，其中 `C` 是一个 __平衡字符串__ 。

你可以交换 任意 两个下标所对应的括号 任意 次数。

返回使 `s` 变成 __平衡字符串__ 所需要的 __最小__ 交换次数。

__示例:__

示例 1：

```text
输入：s = "][]["
输出：1
解释：交换下标 0 和下标 3 对应的括号，可以使字符串变成平衡字符串。
最终字符串变成 "[[]]" 。
```

示例 2：

```text
输入：s = "]]][[["
输出：2
解释：执行下述操作可以使字符串变成平衡字符串：
- 交换下标 0 和下标 4 对应的括号，s = "[]][][" 。
- 交换下标 1 和下标 5 对应的括号，s = "[[][]]" 。
最终字符串变成 "[[][]]" 。
```

示例 3：

```text
输入：s = "[]"
输出：0
解释：这个字符串已经是平衡字符串。
```

__提示：__

- `n == s.length`
- `2 <= n <= 10 ^ 6`
- `n` 为偶数
- `s[i]` 为 `'['` 或 `']'`
- 开括号 `'['` 的数目为 `n / 2` ，闭括号 `']'` 的数目也是 `n / 2`

__思路:__

```text
贪心
统计左括号的个数 left
如果当前为左括号, left 自增
否则如果能够匹配, left 自减
如果不能匹配, left 自增, result 自增, 此时需要交换, 且交换后的左括号数目增加 1
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minSwaps(string s) 
    {
        int left = 0, result = 0;
        for (const auto& c : s)
        {
            if (c == '[') ++left;
            else if (left > 0) --left;
            else 
            {
                ++left;
                ++result;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minSwaps(String s) {
        int left = 0, result = 0;
        for (char c : s.toCharArray()) {
            if (c == '[') ++left;
            else if (left > 0) --left;
            else {
                ++left;
                ++result;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minSwaps(self, s: str) -> int:
        left, result = 0, 0
        for c in s:
            if c == '[':
                left += 1
            elif left > 0:
                left -= 1
            else:
                left += 1
                result += 1
        return result
```
