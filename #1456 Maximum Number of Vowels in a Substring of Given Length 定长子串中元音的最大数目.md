# 1456 Maximum Number of Vowels in a Substring of Given Length 定长子串中元音的最大数目

__Description:__

Given a string  `s` and an integer  `k`, return _the maximum number of vowel letters in any substring of_  `s` _with length_  `k`.

__Vowel letters__ in English are  `'a'`,  `'e'`,  `'i'`,  `'o'`, and  `'u'`.

__Example:__

Example 1:

```text
Input: s = "abciiidef", k = 3
Output: 3
Explanation: The substring "iii" contains 3 vowel letters.
```

Example 2:

```text
Input: s = "aeiou", k = 2
Output: 2
Explanation: Any substring of length 2 contains 2 vowels.
```

Example 3:

```text
Input: s = "leetcode", k = 3
Output: 2
Explanation: "lee", "eet" and "ode" contain 2 vowels.
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s` consists of lowercase English letters.
- `1 <= k <= s.length`

__题目描述:__

给你字符串  `s` 和整数  `k` 。

请返回字符串  `s` 中长度为  `k` 的单个子字符串中可能包含的最大元音字母数。

英文中的 __元音字母__ 为（ `a`,  `e`,  `i`,  `o`,  `u`）。

__示例:__

示例 1：

```text
输入：s = "abciiidef", k = 3
输出：3
解释：子字符串 "iii" 包含 3 个元音字母。
```

示例 2：

```text
输入：s = "aeiou", k = 2
输出：2
解释：任意长度为 2 的子字符串都包含 2 个元音字母。
```

示例 3：

```text
输入：s = "leetcode", k = 3
输出：2
解释："lee"、"eet" 和 "ode" 都包含 2 个元音字母。
```

示例 4：

```text
输入：s = "rhythms", k = 4
输出：0
解释：字符串 s 中不含任何元音字母。
```

示例 5：

```text
输入：s = "tryhard", k = 4
输出：1
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s` 由小写英文字母组成
- `1 <= k <= s.length`

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
    int maxVowels(string s, int k) 
    {
        unordered_set<char> st;
        st.insert('a');
        st.insert('e');
        st.insert('i');
        st.insert('o');
        st.insert('u');
        int r = 0, cur = 0, n = s.size();
        for (r = 0; r < k; r++) 
        {
            if (r == n) return cur;
            if (st.count(s[r])) ++cur;
        }
        int l = 0, result = cur;
        while (r < n) 
        {
            if (st.count(s[l++])) --cur;
            if (st.count(s[r++])) ++cur;
            result = max(result, cur);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxVowels(String s, int k) {
        Set<Character> set = new HashSet();
        set.add('a');
        set.add('e');
        set.add('i');
        set.add('o');
        set.add('u');
        int r = 0, cur = 0, n = s.length();
        for (r = 0; r < k; r++) {
            if (r >= n) return cur;
            if (set.contains(s.charAt(r))) cur++;
        }
        int l = 0, result = cur;
        while (r < n) {
            if (set.contains(s.charAt(l++))) cur--;
            if (set.contains(s.charAt(r++))) cur++;
            result = Math.max(result, cur);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxVowels(self, s: str, k: int) -> int:
        st, l, r, cur, result, n = {'a', 'e', 'i', 'o', 'u'}, 0, 0, 0, 0, len(s)
        while r < k:
            if r >= n:
                return cur
            if s[r] in st:
                cur += 1
        result = cur
        while r < n:
            if s[l] in st:
                cur -= 1
            l += 1
            if s[r] in st:
                cur += 1
            r += 1
            result = max(result, cur)
        return result
```
