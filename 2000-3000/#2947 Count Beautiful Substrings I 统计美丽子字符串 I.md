# 2947 Count Beautiful Substrings I 统计美丽子字符串 I

__Description:__

You are given a string `s` and a positive integer `k`.

Let `vowels` and `consonants` be the number of vowels and consonants in a string.

A string is beautiful if:

- `vowels == consonants`.
- `(vowels * consonants) % k == 0`, in other terms the multiplication of `vowels` and `consonants` is divisible by `k`.

Return _the number of __non-empty beautiful substrings__ in the given string_ `s`.

A substring is a contiguous sequence of characters in a string.

__Vowel letters__ in English are `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`.

Consonant letters in English are every letter except vowels.

__Example:__

Example 1:

```text
Input: s = "baeyh", k = 2
Output: 2
Explanation: There are 2 beautiful substrings in the given string.
```

- Substring "baeyh", vowels = 2 (["a",e"]), consonants = 2 (["y","h"]).
You can see that string "aeyh" is beautiful as vowels == consonants and vowels * consonants % k == 0.
- Substring "baeyh", vowels = 2 (["a",e"]), consonants = 2 (["b","y"]).

You can see that string "baey" is beautiful as vowels == consonants and vowels * consonants % k == 0.

It can be shown that there are only 2 beautiful substrings in the given string.

Example 2:

```text
Input: s = "abba", k = 1
Output: 3
Explanation: There are 3 beautiful substrings in the given string.
```

- Substring "abba", vowels = 1 (["a"]), consonants = 1 (["b"]).
- Substring "abba", vowels = 1 (["a"]), consonants = 1 (["b"]).
- Substring "abba", vowels = 2 (["a","a"]), consonants = 2 (["b","b"]).

It can be shown that there are only 3 beautiful substrings in the given string.

Example 3:

```text
Input: s = "bcdf", k = 1
Output: 0
Explanation: There are no beautiful substrings in the given string.
```

__Constraints:__

- `1 <= s.length <= 1000`
- `1 <= k <= 1000`
- `s` consists of only English lowercase letters.

__题目描述:__

给你一个字符串 `s` 和一个正整数 `k` 。

用 `vowels` 和 `consonants` 分别表示字符串中元音字母和辅音字母的数量。

如果某个字符串满足以下条件，则称其为 美丽字符串 ：

- `vowels == consonants`，即元音字母和辅音字母的数量相等。
- `(vowels * consonants) % k == 0`，即元音字母和辅音字母的数量的乘积能被 `k` 整除。

返回字符串 `s` 中 __非空美丽子字符串__ 的数量。

子字符串是字符串中的一个连续字符序列。

英语中的 __元音字母__ 为 `'a'`、 `'e'`、 `'i'`、 `'o'` 和 `'u'` 。

英语中的 辅音字母 为除了元音字母之外的所有字母。

__示例:__

示例 1：

```text
输入：s = "baeyh", k = 2
输出：2
解释：字符串 s 中有 2 个美丽子字符串。
```

- 子字符串 "baeyh"，vowels = 2（["a","e"]），consonants = 2（["y","h"]）。
可以看出字符串 "aeyh" 是美丽字符串，因为 vowels == consonants 且 vowels * consonants % k == 0 。
- 子字符串 "baeyh"，vowels = 2（["a","e"]），consonants = 2（["b","y"]）。

可以看出字符串 "baey" 是美丽字符串，因为 vowels == consonants 且 vowels * consonants % k == 0 。

可以证明字符串 s 中只有 2 个美丽子字符串。

示例 2：

```text
输入：s = "abba", k = 1
输出：3
解释：字符串 s 中有 3 个美丽子字符串。
```

- 子字符串 "abba"，vowels = 1（["a"]），consonants = 1（["b"]）。
- 子字符串 "abba"，vowels = 1（["a"]），consonants = 1（["b"]）。
- 子字符串 "abba"，vowels = 2（["a","a"]），consonants = 2（["b","b"]）。

可以证明字符串 s 中只有 3 个美丽子字符串。

示例 3：

```text
输入：s = "bcdf", k = 1
输出：0
解释：字符串 s 中没有美丽子字符串。
```

__提示：__

- `1 <= s.length <= 1000`
- `1 <= k <= 1000`
- `s` 仅由小写英文字母组成。

__思路:__

```text
模拟
由于字符串长度只有 1000
可以直接尝试模拟
遍历所有子字符串
如果满足元音辅音的数量相等且数量乘积能整除 k
结果就累加
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int beautifulSubstrings(string s, int k) 
    {
        int n = s.size(), result = 0, i = 0;
        for (string vowels = "aeiou"; i < n; i++) for (int v = vowels.find(s[i]) != string::npos, j = i + 1, cur = 0; j < n; j++) if (((v += vowels.find(s[j]) != string::npos) << 1) == (cur = j - i + 1) and !((v * (cur - v)) % k)) ++result;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int beautifulSubstrings(String s, int k) {
        int n = s.length(), result = 0, i = 0;
        for (String vowels = "aeiou"; i < n; i++) for (int v = vowels.indexOf(s.charAt(i)) != -1 ? 1 : 0, j = i + 1, cur = 0; j < n; j++) if (((v += vowels.indexOf(s.charAt(j)) != -1 ? 1 : 0) << 1) == (cur = j - i + 1) && (v * (cur - v)) % k == 0) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def beautifulSubstrings(self, s: str, k: int) -> int:
        vowels, n, result = {'a', 'e', 'i', 'o', 'u'}, len(s), 0
        for i in range(n):
            v = int(s[i] in vowels)
            for j in range(i + 1, n):
                v += s[j] in vowels
                result += v == ((cur := j - i + 1) - v) and (not v * (cur - v) % k)
        return result
```
