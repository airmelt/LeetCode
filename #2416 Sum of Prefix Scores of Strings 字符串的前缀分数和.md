# 2416 Sum of Prefix Scores of Strings 字符串的前缀分数和

__Description:__

You are given an array `words` of size `n` consisting of __non-empty__ strings.

We define the __score__ of a string `term` as the __number__ of strings `words[i]` such that `term` is a __prefix__ of `words[i]`.

- For example, if `words = ["a", "ab", "abc", "cab"]`, then the score of `"ab"` is `2`, since `"ab"` is a prefix of both `"ab"` and `"abc"`.

Return _an array_ `answer` _of size_ `n` _where_ `answer[i]` _is the __sum__ of scores of every __non-empty__ prefix of_ `words[i]`.

Note that a string is considered as a prefix of itself.

__Example:__

Example 1:

```text
Input: words = ["abc","ab","bc","b"]
Output: [5,4,3,2]
Explanation: The answer for each string is the following:
```

- "abc" has 3 prefixes: "a", "ab", and "abc".
- There are 2 strings with the prefix "a", 2 strings with the prefix "ab", and 1 string with the prefix "abc".
The total is answer[0] = 2 + 2 + 1 = 5.
- "ab" has 2 prefixes: "a" and "ab".
- There are 2 strings with the prefix "a", and 2 strings with the prefix "ab".
The total is answer[1] = 2 + 2 = 4.
- "bc" has 2 prefixes: "b" and "bc".
- There are 2 strings with the prefix "b", and 1 string with the prefix "bc".
The total is answer[2] = 2 + 1 = 3.
- "b" has 1 prefix: "b".
- There are 2 strings with the prefix "b".
The total is answer[3] = 2.

Example 2:

```text
Input: words = ["abcd"]
Output: [4]
Explanation:
"abcd" has 4 prefixes: "a", "ab", "abc", and "abcd".
Each prefix has a score of one, so the total is answer[0] = 1 + 1 + 1 + 1 = 4.
```

__Constraints:__

- `1 <= words.length <= 1000`
- `1 <= words[i].length <= 1000`
- `words[i]` consists of lowercase English letters.

__题目描述:__

给你一个长度为 `n` 的数组 `words` ，该数组由 __非空__ 字符串组成。

定义字符串 `term` 的 __分数__ 等于以 `term` 作为 __前缀__ 的 `words[i]` 的数目。

- 例如，如果 `words = ["a", "ab", "abc", "cab"]` ，那么 `"ab"` 的分数是 `2` ，因为 `"ab"` 是 `"ab"` 和 `"abc"` 的一个前缀。

返回一个长度为 `n` 的数组 `answer` ，其中 `answer[i]` 是 `words[i]` 的每个非空前缀的分数 __总和__ _。_

注意：字符串视作它自身的一个前缀。

__示例:__

示例 1：

```text
输入：words = ["abc","ab","bc","b"]
输出：[5,4,3,2]
解释：对应每个字符串的答案如下：
```

- "abc" 有 3 个前缀："a"、"ab" 和 "abc" 。
- 2 个字符串的前缀为 "a" ，2 个字符串的前缀为 "ab" ，1 个字符串的前缀为 "abc" 。
总计 answer[0] = 2 + 2 + 1 = 5 。
- "ab" 有 2 个前缀："a" 和 "ab" 。
- 2 个字符串的前缀为 "a" ，2 个字符串的前缀为 "ab" 。
总计 answer[1] = 2 + 2 = 4 。
- "bc" 有 2 个前缀："b" 和 "bc" 。
- 2 个字符串的前缀为 "b" ，1 个字符串的前缀为 "bc" 。
总计 answer[2] = 2 + 1 = 3 。
- "b" 有 1 个前缀："b"。
- 2 个字符串的前缀为 "b" 。
总计 answer[3] = 2 。

示例 2：

```text
输入：words = ["abcd"]
输出：[4]
解释：
"abcd" 有 4 个前缀 "a"、"ab"、"abc" 和 "abcd"。
每个前缀的分数都是 1 ，总计 answer[0] = 1 + 1 + 1 + 1 = 4 。
```

__提示：__

- `1 <= words.length <= 1000`
- `1 <= words[i].length <= 1000`
- `words[i]` 由小写英文字母组成

__思路:__

```text
前缀树
遍历所有字符串, 构建前缀树
在构建前缀树的时候顺便记录每个前缀的分数
再次遍历所有字符串, 根据前缀树累计每个字符串的分数
时间复杂度为 O(MN), 空间复杂度为 O(MN), 其中 M 为字符串的平均长度, N 为字符串的个数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> sumPrefixScores(vector<string>& words) 
    {
        struct Trie 
        {
            Trie* children[26]{};
            int score = 0;
        };
        Trie* root = new Trie();
        for (const auto& word : words) 
        {
            auto cur = root;
            for (char c : word) 
            {
                if (!cur -> children[c - 'a']) cur -> children[c - 'a'] = new Trie();
                cur = cur -> children[c - 'a'];
                ++cur -> score;
            }
        }
        vector<int> result(words.size());
        for (int i = 0, n = words.size(); i < n; i++) 
        {
            auto cur = root;
            for (char c : words[i]) 
            {
                cur = cur -> children[c - 'a'];
                result[i] += cur->score; 
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Trie {
    Trie[] children = new Trie[26];
    int score;
}

class Solution {
    public int[] sumPrefixScores(String[] words) {
        var root = new Trie();
        for (var word : words) {
            var cur = root;
            for (var c : word.toCharArray()) {
                if (cur.children[c - 'a'] == null) cur.children[c - 'a'] = new Trie();
                cur = cur.children[c - 'a'];
                ++cur.score;
            }
        }
        int n = words.length, result[] = new int[n];
        for (int i = 0; i < n; i++) {
            var cur = root;
            for (var c : words[i].toCharArray()) {
                cur = cur.children[c - 'a'];
                result[i] += cur.score;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def sumPrefixScores(self, words: List[str]) -> List[int]:
        d = defaultdict(int)
        for w in words:
            for i in range(len(w)):
                d[w[:i + 1]] += 1
        return [sum(d[w[:j + 1]] for j in range(len(w))) for i, w in enumerate(words)]
```
