# 3163 String Compression III 压缩字符串 III

__Description:__

Given a string `word`, compress it using the following algorithm:

- Begin with an empty string `comp`. While `word` is __not__ empty, use the following operation:
  - Remove a maximum length prefix of `word` made of a _single character_ `c` repeating __at most__ 9 times.
  - Append the length of the prefix followed by `c` to `comp`.

Return the string `comp`.

__Example:__

Example 1:

```text
Input: word = "abcde"

Output: "1a1b1c1d1e"

Explanation:

Initially, comp = "". Apply the operation 5 times, choosing "a", "b", "c", "d", and "e" as the prefix in each operation.

For each prefix, append "1" followed by the character to comp.
```

Example 2:

```text
Input: word = "aaaaaaaaaaaaaabb"

Output: "9a5a2b"

Explanation:

Initially, comp = "". Apply the operation 3 times, choosing "aaaaaaaaa", "aaaaa", and "bb" as the prefix in each operation.

For prefix "aaaaaaaaa", append "9" followed by "a" to comp.
For prefix "aaaaa", append "5" followed by "a" to comp.
For prefix "bb", append "2" followed by "b" to comp.
```

__Constraints:__

- `1 <= word.length <= 2 * 10 ^ 5`
- `word` consists only of lowercase English letters.

__题目描述:__

给你一个字符串 `word`，请你使用以下算法进行压缩：

- 从空字符串 `comp` 开始。当 `word` __不为空__ 时，执行以下操作：
  - 移除 `word` 的最长单字符前缀，该前缀由单一字符 `c` 重复多次组成，且该前缀长度 __最多__ 为 9 。
  - 将前缀的长度和字符 `c` 追加到 `comp` 。

返回字符串 `comp` 。

__示例:__

示例 1：

```text
输入：word = "abcde"

输出："1a1b1c1d1e"

解释：

初始时，comp = "" 。进行 5 次操作，每次操作分别选择 "a"、"b"、"c"、"d" 和 "e" 作为前缀。

对每个前缀，将 "1" 和对应的字符追加到 comp。
```

示例 2：

```text
输入：word = "aaaaaaaaaaaaaabb"

输出："9a5a2b"

解释：

初始时，comp = ""。进行 3 次操作，每次操作分别选择 "aaaaaaaaa"、"aaaaa" 和 "bb" 作为前缀。

对于前缀 "aaaaaaaaa"，将 "9" 和 "a" 追加到 comp。
对于前缀 "aaaaa"，将 "5" 和 "a" 追加到 comp。
对于前缀 "bb"，将 "2" 和 "b" 追加到 comp。
```

__提示：__

- `1 <= word.length <= 2 * 10 ^ 5`
- `word` 仅由小写英文字母组成。

__思路:__

```text
模拟
遍历字符串
如果遍历到结尾或者下一个元素与当前元素不相等
就需要分出一个压缩字符串
由于字符串最长为 9
对 9 求商 times 和余数 remain
需要加 times 组 9 和字符
剩下的加到 remain 和字符构成新的一组
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string compressedString(string word) 
    {
        string result;
        for (int last = -1, i = 0, n = word.size(); i < n; i++) 
        {
            if (i == n - 1 or word[i] != word[i + 1])
            {
                for (int j = 0, times = (i - last) / 9; j < times; j++) result += "9" + string(1, word[i]);
                if ((i - last) % 9) result += string(1, '0' + ((i - last) % 9)) + string(1, word[i]);
                last = i;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String compressedString(String word) {
        StringBuilder result = new StringBuilder();
        for (int last = -1, i = 0, n = word.length(); i < n; i++) {
            if (i == n - 1 || word.charAt(i) != word.charAt(i + 1)) {
                for (int j = 0, times = (i - last) / 9; j < times; j++) result.append('9').append(word.charAt(i));
                if ((i - last) % 9 > 0) result.append((char) ('0' + ((i - last) % 9))).append(word.charAt(i));
                last = i;
            }
        }
        return result.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def compressedString(self, word: str) -> str:
        result, last, n = [], -1, len(word)
        for i, c in enumerate(word):
            if i == n - 1 or c != word[i + 1]:
                times, remain = divmod(i - last, 9)
                result.append(("9" + c) * times)
                if remain:
                    result.append(str(remain) + c)
                last = i
        return ''.join(result)
```
