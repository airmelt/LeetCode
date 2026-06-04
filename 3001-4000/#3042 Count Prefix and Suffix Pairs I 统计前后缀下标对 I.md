# 3042 Count Prefix and Suffix Pairs I 统计前后缀下标对 I

__Description:__

You are given a __0-indexed__ string array `words`.

Let's define a __boolean__ function `isPrefixAndSuffix` that takes two strings, `str1` and `str2`:

- `isPrefixAndSuffix(str1, str2)` returns `true` if `str1` is __both__ a prefix and a suffix of `str2`, and `false` otherwise.

For example, `isPrefixAndSuffix("aba", "ababa")` is `true` because `"aba"` is a prefix of `"ababa"` and also a suffix, but `isPrefixAndSuffix("abc", "abcd")` is `false`.

Return _an integer denoting the __number__ of index pairs_ `(i, j)` _such that_ `i < j`_, and_ `isPrefixAndSuffix(words[i], words[j])` _is_ `true`_._

__Example:__

Example 1:

```text
Input: words = ["a","aba","ababa","aa"]
Output: 4
Explanation: In this example, the counted index pairs are:
i = 0 and j = 1 because isPrefixAndSuffix("a", "aba") is true.
i = 0 and j = 2 because isPrefixAndSuffix("a", "ababa") is true.
i = 0 and j = 3 because isPrefixAndSuffix("a", "aa") is true.
i = 1 and j = 2 because isPrefixAndSuffix("aba", "ababa") is true.
Therefore, the answer is 4.
```

Example 2:

```text
Input: words = ["pa","papa","ma","mama"]
Output: 2
Explanation: In this example, the counted index pairs are:
i = 0 and j = 1 because isPrefixAndSuffix("pa", "papa") is true.
i = 2 and j = 3 because isPrefixAndSuffix("ma", "mama") is true.
Therefore, the answer is 2.
```

Example 3:

```text
Input: words = ["abab","ab"]
Output: 0
Explanation: In this example, the only valid index pair is i = 0 and j = 1, and isPrefixAndSuffix("abab", "ab") is false.
Therefore, the answer is 0.
```

__Constraints:__

- `1 <= words.length <= 50`
- `1 <= words[i].length <= 10`
- `words[i]` consists only of lowercase English letters.

__题目描述:__

给你一个下标从 __0__ 开始的字符串数组 `words` 。

定义一个 __布尔__ 函数 `isPrefixAndSuffix` ，它接受两个字符串参数 `str1` 和 `str2` :

- 当 `str1` 同时是 `str2` 的前缀（prefix）和后缀（suffix）时， `isPrefixAndSuffix(str1, str2)` 返回 `true`，否则返回 `false`。

例如， `isPrefixAndSuffix("aba", "ababa")` 返回 `true`，因为 `"aba"` 既是 `"ababa"` 的前缀，也是 `"ababa"` 的后缀，但是 `isPrefixAndSuffix("abc", "abcd")` 返回 `false`。

以整数形式，返回满足 `i < j` 且 `isPrefixAndSuffix(words[i], words[j])` 为 `true` 的下标对 `(i, j)` 的 __数量__ 。

__示例:__

示例 1：

输入：words = ["a","aba","ababa","aa"]
输出：4
解释：在本示例中，计数的下标对包括：
i = 0 且 j = 1 ，因为 isPrefixAndSuffix("a", "aba") 为 true 。
i = 0 且 j = 2 ，因为 isPrefixAndSuffix("a", "ababa") 为 true 。
i = 0 且 j = 3 ，因为 isPrefixAndSuffix("a", "aa") 为 true 。
i = 1 且 j = 2 ，因为 isPrefixAndSuffix("aba", "ababa") 为 true 。
因此，答案是 4 。

示例 2：

输入：words = ["pa","papa","ma","mama"]
输出：2
解释：在本示例中，计数的下标对包括：
i = 0 且 j = 1 ，因为 isPrefixAndSuffix("pa", "papa") 为 true 。
i = 2 且 j = 3 ，因为 isPrefixAndSuffix("ma", "mama") 为 true 。
因此，答案是 2 。

示例 3：

输入：words = ["abab","ab"]
输出：0
解释：在本示例中，唯一有效的下标对是 i = 0 且 j = 1 ，但是 isPrefixAndSuffix("abab", "ab") 为 false 。
因此，答案是 0 。

__提示：__

- `1 <= words.length <= 50`
- `1 <= words[i].length <= 10`
- `words[i]` 仅由小写英文字母组成。

__思路:__

```text
暴力法
由于 words 数组很小
可以直接使用暴力法
比较每一对字符串的前缀和后缀
时间复杂度为 O(MN ^ 2), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countPrefixSuffixPairs(vector<string>& words) 
    {
        int result = 0, n = words.size();
        for (int i = 0; i < n; i++) for (int j = i + 1; j < n; j++) result += !words[j].find(words[i]) and words[j].rfind(words[i]) == words[j].size() - words[i].size();
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countPrefixSuffixPairs(String[] words) {
        int result = 0, n = words.length;
        for (int i = 0; i < n; i++) for (int j = i + 1; j < n; j++) if (words[j].startsWith(words[i]) && words[j].endsWith(words[i])) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countPrefixSuffixPairs(self, words: List[str]) -> int:
        return sum(b.startswith(a) and b.endswith(a) for i, a in enumerate(words) for _, b in enumerate(words[i + 1:]))
```
