# 1316 Distinct Echo Substrings 不同的循环子字符串

__Description:__

Return the number of distinct non-empty substrings of text that can be written as the concatenation of some string with itself (i.e. it can be written as a + a where a is some string).

__Example:__

Example 1:

Input: text = "abcabcabc"
Output: 3
Explanation: The 3 substrings are "abcabc", "bcabca" and "cabcab".

Example 2:

Input: text = "leetcodeleetcode"
Output: 2
Explanation: The 2 substrings are "ee" and "leetcodeleetcode".

__Constraints:__

1 <= text.length <= 2000
text has only lowercase English letters.

__题目描述：__

给你一个字符串 text ，请你返回满足下述条件的 不同 非空子字符串的数目：

可以写成某个字符串与其自身相连接的形式（即，可以写为 a + a，其中 a 是某个字符串）。
例如，abcabc 就是 abc 和它自身连接形成的。

__示例：__

示例 1：

输入：text = "abcabcabc"
输出：3
解释：3 个子字符串分别为 "abcabc"，"bcabca" 和 "cabcab" 。

示例 2：

输入：text = "leetcodeleetcode"
输出：2
解释：2 个子字符串为 "ee" 和 "leetcodeleetcode" 。

__提示：__

1 <= text.length <= 2000
text 只包含小写英文字母。

__思路：__

暴力法
注意到数据范围只到 2000
可以考虑使用 O(n ^ 3) 的方法通过本题
穷举所有子字符串, 将满足条件的字符串加入集合
返回集合的长度即可
时间复杂度为 O(n ^ 3), 空间复杂度为 O(n ^ 2)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int distinctEchoSubstrings(string text) 
    {
        int n = text.size();
        unordered_set<string_view> seen;
        string_view text_v(text);
        int result = 0;
        for (int i = 0; i < n; i++) 
        {
            for (int j = i + 1; j < n; j++) 
            {
                int l = j - i;
                if ((j << 1) - i <= n && text_v.substr(i, l) == text_v.substr(j, l) && !seen.count(text_v.substr(i, l))) 
                {
                    ++result;
                    seen.insert(text_v.substr(i, l));
                }
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int distinctEchoSubstrings(String text) {
        Set<String> set = new HashSet<>();
        for (int i = 1, n = text.length(); i <= (n << 1); i++) for (int j = i; j <= n - i; j++) if (text.substring(j, j + i).equals(text.substring(j - i, j))) set.add(text.substring(j, j + i));
        return set.size();
    }
}
```

__Python__:

```Python
class Solution:
    def distinctEchoSubstrings(self, text: str) -> int:
        return len({text[l: r] for r in range(len(text) + 1) for l in range(r & 1, r, 2) if text[l: ((l + r) >> 1)] == text[((l + r) >> 1): r]})
```
