# 3045 Count Prefix and Suffix Pairs II 统计前后缀下标对 II

__Description:__

You are given a __0-indexed__ string array `words`.

Let's define a __boolean__ function `isPrefixAndSuffix` that takes two strings, `str1` and `str2`:

- `isPrefixAndSuffix(str1, str2)` returns `true` if `str1` is __both__ a prefix and a suffix of `str2`, and `false` otherwise.

For example, `isPrefixAndSuffix("aba", "ababa")` is `true` because `"aba"` is a prefix of `"ababa"` and also a suffix, but `isPrefixAndSuffix("abc", "abcd")` is `false`.

Return _an integer denoting the __number__ of index pairs_ `(i_,_ j)` _such that_ `i < j`_, and_ `isPrefixAndSuffix(words[i], words[j])` _is_ `true`_._

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

- `1 <= words.length <= 10 ^ 5`
- `1 <= words[i].length <= 10 ^ 5`
- `words[i]` consists only of lowercase English letters.
- The sum of the lengths of all `words[i]` does not exceed `5 * 10 ^ 5`.

__题目描述:__

给你一个下标从 __0__ 开始的字符串数组 `words` 。

定义一个 __布尔__ 函数 `isPrefixAndSuffix` ，它接受两个字符串参数 `str1` 和 `str2` :

- 当 `str1` 同时是 `str2` 的前缀（prefix）和后缀（suffix）时， `isPrefixAndSuffix(str1, str2)` 返回 `true`，否则返回 `false`。

例如， `isPrefixAndSuffix("aba", "ababa")` 返回 `true`，因为 `"aba"` 既是 `"ababa"` 的前缀，也是 `"ababa"` 的后缀，但是 `isPrefixAndSuffix("abc", "abcd")` 返回 `false`。

以整数形式，返回满足 `i < j` 且 `isPrefixAndSuffix(words[i], words[j])` 为 `true` 的下标对 `(i, j)` 的 __数量__ 。

__示例:__

示例 1：

```text
输入：words = ["a","aba","ababa","aa"]
输出：4
解释：在本示例中，计数的下标对包括：
i = 0 且 j = 1 ，因为 isPrefixAndSuffix("a", "aba") 为 true 。
i = 0 且 j = 2 ，因为 isPrefixAndSuffix("a", "ababa") 为 true 。
i = 0 且 j = 3 ，因为 isPrefixAndSuffix("a", "aa") 为 true 。
i = 1 且 j = 2 ，因为 isPrefixAndSuffix("aba", "ababa") 为 true 。
因此，答案是 4 。
```

示例 2：

```text
输入：words = ["pa","papa","ma","mama"]
输出：2
解释：在本示例中，计数的下标对包括：
i = 0 且 j = 1 ，因为 isPrefixAndSuffix("pa", "papa") 为 true 。
i = 2 且 j = 3 ，因为 isPrefixAndSuffix("ma", "mama") 为 true 。
因此，答案是 2 。
```

示例 3：

```text
输入：words = ["abab","ab"]
输出：0
解释：在本示例中，唯一有效的下标对是 i = 0 且 j = 1 ，但是 isPrefixAndSuffix("abab", "ab") 为 false 。
因此，答案是 0 。
```

__提示：__

- `1 <= words.length <= 10 ^ 5`
- `1 <= words[i].length <= 10 ^ 5`
- `words[i]` 仅由小写英文字母组成。
- 所有 `words[i]` 的长度之和不超过 `5 * 10 ^ 5` 。

__思路:__

```text
字典树
在字典树中记录 (word[i], word[n - i - 1])
如果另一个字符串 t 是这个字符串的前缀
那么就满足是其前后缀
时间复杂度为 O(N), 空间复杂度为 O(N), N 为所有字符数量的总和
```

__代码:__

__C++__:

```C++
struct Trie
{
    unordered_map<int, Trie*> children;
    int cnt = 0;
};
class Solution 
{
public:
    long long countPrefixSuffixPairs(vector<string>& words) 
    {
        long long result = 0LL;
        Trie* root = new Trie();
        for (const auto& word : words)
        {
            Trie* cur = root;
            for (int i = 0, n = word.size(), p = 0; i < n; i++) 
            {
                if (!cur -> children[p = ((word[i] - 'a') << 5) + word[n - i - 1] - 'a']) cur -> children[p] = new Trie();
                result += (cur = cur -> children[p]) -> cnt;
            }
            ++cur -> cnt;
        }
        return result;
    }
};
```

__Java__:

```Java
class Trie {
    Map<Integer, Trie> children = new HashMap<>();
    int cnt;
}

class Solution {
    public long countPrefixSuffixPairs(String[] words) {
        long result = 0L;
        Trie root = new Trie(), cur = null;
        for (String word : words) {
            cur = root;
            for (int i = 0, n = word.length(); i < n; i++) result += (cur = cur.children.computeIfAbsent((word.charAt(i) - 'a') << 5 | (word.charAt(n - i - 1) - 'a'), k -> new Trie())).cnt;
            ++cur.cnt;
        }        
        return result;
    }
}
```

__Python__:

```Python
class Trie:
    def __init__(self):
        self.children = defaultdict(lambda: Trie())
        self.cnt = 0

class Solution:
    def countPrefixSuffixPairs(self, words: List[str]) -> int:
        result, root = 0, Trie()
        for word in words:
            cur = root
            for c in zip(word, word[::-1]):
                result += (cur := cur.children[c]).cnt
            cur.cnt += 1
        return result
```
