# 2062 Count Vowel Substrings of a String 统计字符串中的元音子字符串

__Description:__

A substring is a contiguous (non-empty) sequence of characters within a string.

A __vowel substring__ is a substring that __only__ consists of vowels ( `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`) and has __all five__ vowels present in it.

Given a string `word`, return _the number of __vowel substrings__ in_ `word`.

__Example:__

Example 1:

```text
Input: word = "aeiouu"
Output: 2
Explanation: The vowel substrings of word are as follows (underlined):

- "aeiouu"
- "aeiouu"
```

Example 2:

```text
Input: word = "unicornarihan"
Output: 0
Explanation: Not all 5 vowels are present, so there are no vowel substrings.
```

Example 3:

```text
Input: word = "cuaieuouac"
Output: 7
Explanation: The vowel substrings of word are as follows (underlined):
- "cuaieuouac"
- "cuaieuouac"
- "cuaieuouac"
- "cuaieuouac"
- "cuaieuouac"
- "cuaieuouac"
- "cuaieuouac"
```

__Constraints:__

- `1 <= word.length <= 100`
- `word` consists of lowercase English letters only.

__题目描述:__

子字符串 是字符串中的一个连续（非空）的字符序列。

__元音子字符串__ 是 __仅__ 由元音（ `'a'`、 `'e'`、 `'i'`、 `'o'` 和 `'u'`）组成的一个子字符串，且必须包含 __全部五种__ 元音。

给你一个字符串 `word` ，统计并返回 `word` 中 __元音子字符串的数目__ 。

__示例:__

示例 1：

```text
输入：word = "aeiouu"
输出：2
解释：下面列出 word 中的元音子字符串（斜体加粗部分）：
- "aeiouu"
- "aeiouu"
```

示例 2：

```text
输入：word = "unicornarihan"
输出：0
解释：word 中不含 5 种元音，所以也不会存在元音子字符串。
```

示例 3：

```text
输入：word = "cuaieuouac"
输出：7
解释：下面列出 word 中的元音子字符串（斜体加粗部分）：
- "cuaieuouac"
- "cuaieuouac"
- "cuaieuouac"
- "cuaieuouac"
- "cuaieuouac"
- "cuaieuouac"
- "cuaieuouac"
```

示例 4：

```text
输入：word = "bbaeixoubb"
输出：0
解释：所有包含全部五种元音的子字符串都含有辅音，所以不存在元音子字符串。
```

__提示：__

- `1 <= word.length <= 100`
- `word` 仅由小写英文字母组成

__思路:__

```text
哈希表
用哈希表计数
元音需要都存在
不需要按顺序
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countVowelSubstrings(string word) 
    {
        int n = word.size(), result = 0;
        unordered_set<char> target = {'a', 'e', 'i', 'o', 'u'};
        for (int i = 0; i < n; i++) 
        {
            unordered_set<char> cur;
            for (int j = i; j < n; j++)
            {
                cur.insert(word[j]);
                result += cur == target;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countVowelSubstrings(String word) {
        int n = word.length(), result = 0;
        Set<Character> target = new HashSet<>();
        target.add('a');
        target.add('e');
        target.add('i');
        target.add('o');
        target.add('u');
        for (int i = 0; i < n; i++) {
            Set<Character> cur = new HashSet<>();
            for (int j = i; j < n; j++) {
                cur.add(word.charAt(j));
                if (cur.equals(target)) ++result;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countVowelSubstrings(self, word: str) -> int:
        return sum(1 for i in range(len(word)) for j in range(i + 4, len(word)) if set(word[i:j + 1]) == set('aeiou'))
```
