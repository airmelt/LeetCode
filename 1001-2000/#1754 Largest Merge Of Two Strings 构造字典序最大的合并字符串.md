# 1754 Largest Merge Of Two Strings 构造字典序最大的合并字符串

__Description:__

You are given two strings `word1` and `word2`. You want to construct a string `merge` in the following way: while either `word1` or `word2` are non-empty, choose __one__ of the following options:

- If `word1` is non-empty, append the __first__ character in `word1` to `merge` and delete it from `word1`.
  - For example, if `word1 = "abc"` and `merge = "dv"`, then after choosing this operation, `word1 = "bc"` and `merge = "dva"`.
- If `word2` is non-empty, append the __first__ character in `word2` to `merge` and delete it from `word2`.
  - For example, if `word2 = "abc"` and `merge = ""`, then after choosing this operation, `word2 = "bc"` and `merge = "a"`.

Return _the lexicographically __largest___ `merge` _you can construct_.

A string `a` is lexicographically larger than a string `b` (of the same length) if in the first position where `a` and `b` differ, `a` has a character strictly larger than the corresponding character in `b`. For example, `"abcd"` is lexicographically larger than `"abcc"` because the first position they differ is at the fourth character, and `d` is greater than `c`.

__Example:__

Example 1:

```text
Input: word1 = "cabaa", word2 = "bcaaa"
Output: "cbcabaaaaa"
Explanation: One way to get the lexicographically largest merge is:
```

- Take from word1: merge = "c", word1 = "abaa", word2 = "bcaaa"
- Take from word2: merge = "cb", word1 = "abaa", word2 = "caaa"
- Take from word2: merge = "cbc", word1 = "abaa", word2 = "aaa"
- Take from word1: merge = "cbca", word1 = "baa", word2 = "aaa"
- Take from word1: merge = "cbcab", word1 = "aa", word2 = "aaa"
- Append the remaining 5 a's from word1 and word2 at the end of merge.

Example 2:

```text
Input: word1 = "abcabc", word2 = "abdcaba"
Output: "abdcabcabcaba"
```

__Constraints:__

- `1 <= word1.length, word2.length <= 3000`
- `word1` and `word2` consist only of lowercase English letters.

__题目描述:__

给你两个字符串 `word1` 和 `word2` 。你需要按下述方式构造一个新字符串 `merge` :如果 `word1` 或 `word2` 非空，选择 __下面选项之一__ 继续操作:

- 如果 `word1` 非空，将 `word1` 中的第一个字符附加到 `merge` 的末尾，并将其从 `word1` 中移除。
  - 例如， `word1 = "abc"` 且 `merge = "dv"` ，在执行此选项操作之后， `word1 = "bc"` ，同时 `merge = "dva"` 。
- 如果 `word2` 非空，将 `word2` 中的第一个字符附加到 `merge` 的末尾，并将其从 `word2` 中移除。
  - 例如， `word2 = "abc"` 且 `merge = ""` ，在执行此选项操作之后， `word2 = "bc"` ，同时 `merge = "a"` 。

返回你可以构造的字典序 __最大__ 的合并字符串 `merge` _。_

长度相同的两个字符串 `a` 和 `b` 比较字典序大小，如果在 `a` 和 `b` 出现不同的第一个位置， `a` 中字符在字母表中的出现顺序位于 `b` 中相应字符之后，就认为字符串 `a` 按字典序比字符串 `b` 更大。例如， `"abcd"` 按字典序比 `"abcc"` 更大，因为两个字符串出现不同的第一个位置是第四个字符，而 `d` 在字母表中的出现顺序位于 `c` 之后。

__示例:__

示例 1：

```text
输入：word1 = "cabaa", word2 = "bcaaa"
输出："cbcabaaaaa"
解释：构造字典序最大的合并字符串，可行的一种方法如下所示：
```

- 从 word1 中取第一个字符：merge = "c"，word1 = "abaa"，word2 = "bcaaa"
- 从 word2 中取第一个字符：merge = "cb"，word1 = "abaa"，word2 = "caaa"
- 从 word2 中取第一个字符：merge = "cbc"，word1 = "abaa"，word2 = "aaa"
- 从 word1 中取第一个字符：merge = "cbca"，word1 = "baa"，word2 = "aaa"
- 从 word1 中取第一个字符：merge = "cbcab"，word1 = "aa"，word2 = "aaa"
- 将 word1 和 word2 中剩下的 5 个 a 附加到 merge 的末尾。

示例 2：

```text
输入：word1 = "abcabc", word2 = "abdcaba"
输出："abdcabcabcaba"
```

__提示：__

- `1 <= word1.length, word2.length <= 3000`
- `word1` 和 `word2` 仅由小写英文组成

__思路:__

```text
模拟
不需要真正的移除字符串中的字符
只需要移动字符串的指针
然后比较两个字符串的字典序
将较大的字符添加到合并字符串中
时间复杂度为 O(N ^ 2), 空间复杂度为 O(M + N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string largestMerge(string word1, string word2) 
    {
        string merge;
        int n = word1.size(), m = word2.size(), i = 0, j = 0;
        while(i < n or j < m)
        {
            if(i < n and word1.substr(i) > word2.substr(j)) merge += word1[i++];
            else merge += word2[j++];
        }
        return merge;
    }
};
```

__Java__:

```Java
class Solution {
    public String largestMerge(String word1, String word2) {
        int n = word1.length(), m = word2.length(), i = 0, j = 0;
        StringBuilder merge = new StringBuilder();
        while (i < n || j < m) {
            if (i < n && word1.substring(i, n).compareTo(word2.substring(j, m)) > 0) merge.append(word1.charAt(i++));
            else merge.append(word2.charAt(j++));
        }
        return merge.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def largestMerge(self, word1: str, word2: str) -> str:
        i, j, n, m, merge = 0, 0, len(word1), len(word2), []
        while i < n or j < m:
            if i < n and word1[i:] > word2[j:]:
                merge.append(word1[i])
                i += 1
            else:
                merge.append(word2[j])
                j += 1
        return ''.join(merge)
```
