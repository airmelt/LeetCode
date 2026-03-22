# 1957 Delete Characters to Make Fancy String 删除字符使字符串变好

__Description:__

A fancy string is a string where no three consecutive characters are equal.

Given a string `s`, delete the __minimum__ possible number of characters from `s` to make it __fancy__.

Return the final string after the deletion. It can be shown that the answer will always be unique.

__Example:__

Example 1:

```text
Input: s = "leeetcode"
Output: "leetcode"
Explanation:
Remove an 'e' from the first group of 'e's to create "leetcode".
No three consecutive characters are equal, so return "leetcode".
```

Example 2:

```text
Input: s = "aaabaaaa"
Output: "aabaa"
Explanation:
Remove an 'a' from the first group of 'a's to create "aabaaaa".
Remove two 'a's from the second group of 'a's to create "aabaa".
No three consecutive characters are equal, so return "aabaa".
```

Example 3:

```text
Input: s = "aab"
Output: "aab"
Explanation: No three consecutive characters are equal, so return "aab".
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s` consists only of lowercase English letters.

__题目描述:__

一个字符串如果没有 三个连续 相同字符，那么它就是一个 好字符串 。

给你一个字符串 `s` ，请你从 `s` 删除 __最少__ 的字符，使它变成一个 __好字符串__ 。

请你返回删除后的字符串。题目数据保证答案总是 唯一的 。

__示例:__

示例 1：

```text
输入：s = "leeetcode"
输出："leetcode"
解释：
从第一组 'e' 里面删除一个 'e' ，得到 "leetcode" 。
没有连续三个相同字符，所以返回 "leetcode" 。
```

示例 2：

```text
输入：s = "aaabaaaa"
输出："aabaa"
解释：
从第一组 'a' 里面删除一个 'a' ，得到 "aabaaaa" 。
从第二组 'a' 里面删除两个 'a' ，得到 "aabaa" 。
没有连续三个相同字符，所以返回 "aabaa" 。
```

示例 3：

```text
输入：s = "aab"
输出："aab"
解释：没有连续三个相同字符，所以返回 "aab" 。
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s` 只包含小写英文字母。

__思路:__

```text

时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string makeFancyString(string s) 
    {
        string result;
        for (const auto& c : s) if (result.size() < 2 or result[result.size() - 2] != c or result.back() != c) result.push_back(c);
        return result;
    }
};
```

__Java__:

```Java
class Solution 
{
    public String makeFancyString(String s) {
        StringBuilder result = new StringBuilder();
        for (char c : s.toCharArray()) if (result.length() < 2 || result.charAt(result.length() - 2) != c || result.charAt(result.length() - 1) != c) result.append(c);
        return result.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def makeFancyString(self, s: str) -> str:
        return s if not any(chr(i + ord('a')) * 3 in s for i in range(26)) else self.makeFancyString(s.replace("aaa", "aa").replace("bbb", "bb").replace("ccc", "cc").replace("ddd", "dd").replace("eee", "ee").replace("fff", "ff").replace("ggg", "gg").replace("hhh", "hh").replace("iii", "ii").replace("jjj", "jj").replace("kkk", "kk").replace("lll", "ll").replace("mmm", "mm").replace("nnn", "nn").replace("ooo", "oo").replace("ppp", "pp").replace("qqq", "qq").replace("rrr", "rr").replace("sss", "ss").replace("ttt", "tt").replace("uuu", "uu").replace("vvv", "vv").replace("www", "ww").replace("xxx", "xx").replace("yyy", "yy").replace("zzz", "zz"))
```
