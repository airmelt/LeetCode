# 1704 Determine if String Halves Are Alike 判断字符串的两半是否相似

__Description:__

You are given a string `s` of even length. Split this string into two halves of equal lengths, and let `a` be the first half and `b` be the second half.

Two strings are __alike__ if they have the same number of vowels ( `'a'`, `'e'`, `'i'`, `'o'`, `'u'`, `'A'`, `'E'`, `'I'`, `'O'`, `'U'`). Notice that `s` contains uppercase and lowercase letters.

Return `true` _if_ `a` _and_ `b` _are __alike___. Otherwise, return `false`.

__Example:__

Example 1:

```text
Input: s = "book"
Output: true
Explanation: a = "bo" and b = "ok". a has 1 vowel and b has 1 vowel. Therefore, they are alike.
```

Example 2:

```text
Input: s = "textbook"
Output: false
Explanation: a = "text" and b = "book". a has 1 vowel whereas b has 2. Therefore, they are not alike.
Notice that the vowel o is counted twice.
```

__Constraints:__

- `2 <= s.length <= 1000`
- `s.length` is even.
- `s` consists of __uppercase and lowercase__ letters.

__题目描述:__

给你一个偶数长度的字符串 `s` 。将其拆分成长度相同的两半，前一半为 `a` ，后一半为 `b` 。

两个字符串 __相似__ 的前提是它们都含有相同数目的元音（ `'a'`， `'e'`， `'i'`， `'o'`， `'u'`， `'A'`， `'E'`， `'I'`， `'O'`， `'U'`）。注意， `s` 可能同时含有大写和小写字母。

如果 `a` 和 `b` 相似，返回 `true` ；否则，返回 `false` 。

__示例:__

示例 1：

```text
输入：s = "book"
输出：true
解释：a = "bo" 且 b = "ok" 。a 中有 1 个元音，b 也有 1 个元音。所以，a 和 b 相似。
```

示例 2：

```text
输入：s = "textbook"
输出：false
解释：a = "text" 且 b = "book" 。a 中有 1 个元音，b 中有 2 个元音。因此，a 和 b 不相似。
注意，元音 o 在 b 中出现两次，记为 2 个。
```

__提示：__

- `2 <= s.length <= 1000`
- `s.length` 是偶数
- `s` 由 __大写和小写__ 字母组成

__思路:__

```text
位运算
或者用哈希表记录元音字母
前半段和后半段分别统计元音字母的个数
前半段相加, 后半段相减
最后判断是否为 0
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool halvesAreAlike(string s) 
    {
        int n = s.size() >> 1, result = 0;
        unordered_set<char> st = {'a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'};
        for (int i = 0; i < n; i++) result += st.count(s[i]) - st.count(s[i + n]); 
        return !result;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean halvesAreAlike(String s) {
        int mask = 1065233, n = s.length() >> 1, count = 0;
        for (int i = 0; i < n; i++) count += ((((1 << ((int)s.charAt(i) | 32) - 97)) | mask) == mask ? 1 : 0) - ((((1 << ((int)s.charAt(i + n) | 32) - 97)) | mask) == mask ? 1 : 0);
        return count == 0;
    }
}
```

__Python__:

```Python
class Solution:
    def halvesAreAlike(self, s: str) -> bool:
        return len(re.compile(r'[a|e|i|o|u|A|E|I|O|U].*?').findall(s[:len(s) >> 1])) == len(re.compile(r'[a|e|i|o|u|A|E|I|O|U].*?').findall(s[len(s) >> 1:]))
```
