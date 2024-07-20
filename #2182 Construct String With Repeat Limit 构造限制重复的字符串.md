# 2182 Construct String With Repeat Limit 构造限制重复的字符串

__Description:__

You are given a string `s` and an integer `repeatLimit`. Construct a new string `repeatLimitedString` using the characters of `s` such that no letter appears __more than__ `repeatLimit` times __in a row__. You do __not__ have to use all characters from `s`.

Return _the __lexicographically largest___ `repeatLimitedString` _possible_.

A string `a` is __lexicographically larger__ than a string `b` if in the first position where `a` and `b` differ, string `a` has a letter that appears later in the alphabet than the corresponding letter in `b`. If the first `min(a.length, b.length)` characters do not differ, then the longer string is the lexicographically larger one.

__Example:__

Example 1:

```text
Input: s = "cczazcc", repeatLimit = 3
Output: "zzcccac"
Explanation: We use all of the characters from s to construct the repeatLimitedString "zzcccac".
The letter 'a' appears at most 1 time in a row.
The letter 'c' appears at most 3 times in a row.
The letter 'z' appears at most 2 times in a row.
Hence, no letter appears more than repeatLimit times in a row and the string is a valid repeatLimitedString.
The string is the lexicographically largest repeatLimitedString possible so we return "zzcccac".
Note that the string "zzcccca" is lexicographically larger but the letter 'c' appears more than 3 times in a row, so it is not a valid repeatLimitedString.
```

Example 2:

```text
Input: s = "aababab", repeatLimit = 2
Output: "bbabaa"
Explanation: We use only some of the characters from s to construct the repeatLimitedString "bbabaa". 
The letter 'a' appears at most 2 times in a row.
The letter 'b' appears at most 2 times in a row.
Hence, no letter appears more than repeatLimit times in a row and the string is a valid repeatLimitedString.
The string is the lexicographically largest repeatLimitedString possible so we return "bbabaa".
Note that the string "bbabaaa" is lexicographically larger but the letter 'a' appears more than 2 times in a row, so it is not a valid repeatLimitedString.
```

__Constraints:__

- `1 <= repeatLimit <= s.length <= 10 ^ 5`
- `s` consists of lowercase English letters.

__题目描述:__

给你一个字符串 `s` 和一个整数 `repeatLimit` ，用 `s` 中的字符构造一个新字符串 `repeatLimitedString` ，使任何字母 __连续__ 出现的次数都不超过 `repeatLimit` 次。你不必使用 `s` 中的全部字符。

返回 __字典序最大的__ `repeatLimitedString` 。

如果在字符串 `a` 和 `b` 不同的第一个位置，字符串 `a` 中的字母在字母表中出现时间比字符串 `b` 对应的字母晚，则认为字符串 `a` 比字符串 `b` __字典序更大__ 。如果字符串中前 `min(a.length, b.length)` 个字符都相同，那么较长的字符串字典序更大。

__示例:__

示例 1：

```text
输入：s = "cczazcc", repeatLimit = 3
输出："zzcccac"
解释：使用 s 中的所有字符来构造 repeatLimitedString "zzcccac"。
字母 'a' 连续出现至多 1 次。
字母 'c' 连续出现至多 3 次。
字母 'z' 连续出现至多 2 次。
因此，没有字母连续出现超过 repeatLimit 次，字符串是一个有效的 repeatLimitedString 。
该字符串是字典序最大的 repeatLimitedString ，所以返回 "zzcccac" 。
注意，尽管 "zzcccca" 字典序更大，但字母 'c' 连续出现超过 3 次，所以它不是一个有效的 repeatLimitedString 。
```

示例 2：

```text
输入：s = "aababab", repeatLimit = 2
输出："bbabaa"
解释：
使用 s 中的一些字符来构造 repeatLimitedString "bbabaa"。 
字母 'a' 连续出现至多 2 次。 
字母 'b' 连续出现至多 2 次。 
因此，没有字母连续出现超过 repeatLimit 次，字符串是一个有效的 repeatLimitedString 。 
该字符串是字典序最大的 repeatLimitedString ，所以返回 "bbabaa" 。 
注意，尽管 "bbabaaa" 字典序更大，但字母 'a' 连续出现超过 2 次，所以它不是一个有效的 repeatLimitedString 。
```

__提示：__

- `1 <= repeatLimit <= s.length <= 10 ^ 5`
- `s` 由小写英文字母组成

__思路:__

```text
贪心
维护最大的字符和第二大的字符
先将所有字符计数
如果最大字符用完了, 则指向最大字符的指针向前移动
如果最大的字符还有而且还未达到 repeatLimit, 则添加最大的字符
如果第二大的字符比最大的字符大或者第二大的字符用完了, 则指向第二大字符的指针向前移动
否则如果第二大的字符还有, 则添加第二大的字符, 并重置最大字符计数
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string repeatLimitedString(string s, int repeatLimit) 
    {
        int i = 25, j = 24, cur = 0;
        vector<int> c(26);
        string result;
        for (const auto& a : s) ++c[a - 'a'];
        while (i > -1 and j > -1) 
        {
            if (!c[i])
            {
                cur = 0;
                --i;
            } 
            else if (cur < repeatLimit) 
            {
                --c[i];
                result += ((char)(i + 'a'));
                ++cur;
            } 
            else if (j >= i or !c[j]) --j;
            else 
            {
                --c[j];
                result += ((char)(j + 'a'));
                cur = 0;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String repeatLimitedString(String s, int repeatLimit) {
        int count[] = new int[26], i = 25, j = 24, cur = 0;
        StringBuilder result = new StringBuilder();
        for (char c : s.toCharArray()) ++count[c - 'a'];
        while (i > -1 && j > -1) {
            if (count[i] == 0) {
                cur = 0;
                --i;
            } else if (cur < repeatLimit) {
                --count[i];
                result.append((char)(i + 'a'));
                ++cur;
            } else if (j >= i || count[j] == 0) --j;
            else {
                --count[j];
                result.append((char)(j + 'a'));
                cur = 0;
            }
        }
        return result.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def repeatLimitedString(self, s: str, repeatLimit: int) -> str:
        c, result, i, j, cur = Counter(s), [], 25, 24, 0
        while i > -1 and j > -1:
            if not c[chr(i + ord('a'))]:
                cur, i = 0, i - 1
            elif cur < repeatLimit:
                c[chr(i + ord('a'))] -= 1
                result.append(chr(ord('a') + i))
                cur += 1
            elif j >= i or not c[chr(ord('a') + j)]:
                j -= 1
            else:
                c[chr(ord('a') + j)] -= 1
                result.append(chr(ord('a') + j))
                cur = 0
        return ''.join(result)
```
