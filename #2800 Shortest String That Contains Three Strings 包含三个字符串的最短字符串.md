# 2800 Shortest String That Contains Three Strings 包含三个字符串的最短字符串

__Description:__

If there are multiple such strings, return the lexicographically smallest one.

Return a string denoting the answer to the problem.

Notes

- A string `a` is __lexicographically smaller__ than a string `b` (of the same length) if in the first position where `a` and `b` differ, string `a` has a letter that appears __earlier__ in the alphabet than the corresponding letter in `b`.
- A __substring__ is a contiguous sequence of characters within a string.

__Example:__

Example 1:

```text
Input: a = "abc", b = "bca", c = "aaa"
Output: "aaabca"
Explanation:  We show that "aaabca" contains all the given strings: a = ans[2...4], b = ans[3..5], c = ans[0..2]. It can be shown that the length of the resulting string would be at least 6 and "aaabca" is the lexicographically smallest one.
```

Example 2:

```text
Input: a = "ab", b = "ba", c = "aba"
Output: "aba"
Explanation: We show that the string "aba" contains all the given strings: a = ans[0..1], b = ans[1..2], c = ans[0..2]. Since the length of c is 3, the length of the resulting string would be at least 3. It can be shown that "aba" is the lexicographically smallest one.
```

__Constraints:__

- `1 <= a.length, b.length, c.length <= 100`
- `a`, `b`, `c` consist only of lowercase English letters.

__题目描述:__

如果有多个这样的字符串，请你返回 字典序最小 的一个。

请你返回满足题目要求的字符串。

注意：

- 两个长度相同的字符串 `a` 和 `b` ，如果在第一个不相同的字符处， `a` 的字母在字母表中比 `b` 的字母 __靠前__ ，那么字符串 `a` 比字符串 `b` __字典序小__ 。
- __子字符串__ 是一个字符串中一段连续的字符序列。

__示例:__

示例 1：

```text
输入：a = "abc", b = "bca", c = "aaa"
输出："aaabca"
解释：字符串 "aaabca" 包含所有三个字符串：a = ans[2...4] ，b = ans[3..5] ，c = ans[0..2] 。结果字符串的长度至少为 6 ，且"aaabca" 是字典序最小的一个。
```

示例 2：

```text
输入：a = "ab", b = "ba", c = "aba"
输出："aba"
解释：字符串 "aba" 包含所有三个字符串：a = ans[0..1] ，b = ans[1..2] ，c = ans[0..2] 。由于 c 的长度为 3 ，结果字符串的长度至少为 3 。"aba" 是字典序最小的一个。
```

__提示：__

- `1 <= a.length, b.length, c.length <= 100`
- `a` ， `b` ， `c` 只包含小写英文字母。

__思路:__

```text
模拟
对 abc 进行全排列一共有 6 种排列方式
以 abc 为例
先将 ab 进行合并再将 bc 进行合并
合并先判断是否为字串, 如果为字串直接返回较长的字符串
否则遍历字符串比较前缀和后缀
从最大可能的长度 (min(len(s), len(t))) 开始减少到 0
如果 s 后缀与 t 前缀匹配, 就返回 s 和 t 从 i 开始的后缀, 这样就把两者相同的部分消除了
合并也可以用 kmp 加快查找速度
最后比较的时候先比较长度, 再比较字符串的大小
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N), 使用 KMP 可以将时间复杂度减少至 O(N), 其中 N 为字符串的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string minimumString(const string& a, const string& b, const string& c) 
    {
        auto kmp = [](const auto& s) -> vector<int>
        {
            int n = s.size();
            vector<int> result(n);
            for (int i = 1, j = 0; i < n; i++) 
            {
                while (j and s[j] != s[i]) j = result[j - 1];
                result[i] = s[j] == s[i] ? ++j : j;
            }
            return result;
        };
        auto merge = [&](const auto& s, const auto& t) -> string
        {
            vector<int> cur = kmp(t);
            int j = 0;
            for (int i = 0, m = s.size(), n = t.size(); i < m; i++) 
            {
                while (j and t[j] != s[i]) j = cur[j - 1];
                if (t[j] == s[i]) if (++j == n) return s;
            }
            return s + t.substr(j);
        };
        return min({merge(merge(a, b), c), merge(merge(a, c), b), merge(merge(b, a), c), merge(merge(b, c), a), merge(merge(c, a), b), merge(merge(c, b), a)}, [&](const string& a, const string& b) { return a.size() == b.size() ? a < b : a.size() < b.size(); });
    }
};
```

__Java__:

```Java
class Solution {
    public String minimumString(String a, String b, String c) {
        String result = "", candidators[] = { merge(merge(a, b), c), merge(merge(a, c), b), merge(merge(b, a), c), merge(merge(b, c), a), merge(merge(c, a), b), merge(merge(c, b), a) };
        for (var s : candidators) if (result.isEmpty() || result.length() > s.length() || result.length() == s.length() && s.compareTo(result) < 0) result = s;
        return result;
    }

    private int[] kmp(char[] s) {
        int n = s.length, result[] = new int[n];
        for (int i = 1, j = 0; i < n; i++) {
            while (j > 0 && s[j] != s[i]) j = result[j - 1];
            result[i] = s[j] == s[i] ? ++j : j;
        }
        return result;
    }

    private String merge(String s, String t) {
        int cur[] = kmp(t.toCharArray()), j = 0, m = s.length(), n = t.length();
        for (int i = 0; i < m; i++) {
            while (j > 0 && t.charAt(j) != s.charAt(i)) j = cur[j - 1];
            if (t.charAt(j) == s.charAt(i)) if (++j == n) return s;
        }
        return s + t.substring(j);
    }
}
```

__Python__:

```Python
class Solution:
    def minimumString(self, a: str, b: str, c: str) -> str:
        return "" if not (merge := lambda s, t: s if t in s else t if s in t else l[0] if (l := [s + t[i:] for i in range(min(len(s), len(t)), 0, -1) if s[-i:] == t[:i]]) else s + t) else sorted([merge(merge(a, b), c) for a, b, c in permutations((a, b, c))], key=lambda x: (len(x), x))[0]
```
