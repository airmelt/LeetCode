# 2452 Words Within Two Edits of Dictionary 距离字典两次编辑以内的单词

__Description:__

You are given two string arrays, `queries` and `dictionary`. All words in each array comprise of lowercase English letters and have the same length.

In one __edit__ you can take a word from `queries`, and change any letter in it to any other letter. Find all words from `queries` that, after a __maximum__ of two edits, equal some word from `dictionary`.

Return _a list of all words from_ `queries`_,_ _that match with some word from_ `dictionary` _after a maximum of __two edits___. Return the words in the __same order__ they appear in `queries`.

__Example:__

Example 1:

```text
Input: queries = ["word","note","ants","wood"], dictionary = ["wood","joke","moat"]
Output: ["word","note","wood"]
Explanation:
```

- Changing the 'r' in "word" to 'o' allows it to equal the dictionary word "wood".
- Changing the 'n' to 'j' and the 't' to 'k' in "note" changes it to "joke".
- It would take more than 2 edits for "ants" to equal a dictionary word.
- "wood" can remain unchanged (0 edits) and match the corresponding dictionary word.

Thus, we return ["word","note","wood"].

Example 2:

```text
Input: queries = ["yes"], dictionary = ["not"]
Output: []
Explanation:
Applying any two edits to "yes" cannot make it equal to "not". Thus, we return an empty array.
```

__Constraints:__

- `1 <= queries.length, dictionary.length <= 100`
- `n == queries[i].length == dictionary[j].length`
- `1 <= n <= 100`
- All `queries[i]` and `dictionary[j]` are composed of lowercase English letters.

__题目描述:__

给你两个字符串数组 `queries` 和 `dictionary` 。数组中所有单词都只包含小写英文字母，且长度都相同。

一次 __编辑__ 中，你可以从 `queries` 中选择一个单词，将任意一个字母修改成任何其他字母。从 `queries` 中找到所有满足以下条件的字符串:__不超过__ 两次编辑内，字符串与 `dictionary` 中某个字符串相同。

请你返回 `queries` 中的单词列表，这些单词距离 `dictionary` 中的单词 __编辑次数__ 不超过 __两次__ 。单词返回的顺序需要与 `queries` 中原本顺序相同。

__示例:__

示例 1：

```text
输入：queries = ["word","note","ants","wood"], dictionary = ["wood","joke","moat"]
输出：["word","note","wood"]
解释：
```

- 将 "word" 中的 'r' 换成 'o' ，得到 dictionary 中的单词 "wood" 。
- 将 "note" 中的 'n' 换成 'j' 且将 't' 换成 'k' ，得到 "joke" 。
- "ants" 需要超过 2 次编辑才能得到 dictionary 中的单词。
- "wood" 不需要修改（0 次编辑），就得到 dictionary 中相同的单词。

所以我们返回 ["word","note","wood"] 。

示例 2：

```text
输入：queries = ["yes"], dictionary = ["not"]
输出：[]
解释：
"yes" 需要超过 2 次编辑才能得到 "not" 。
所以我们返回空数组。
```

__提示：__

- `1 <= queries.length, dictionary.length <= 100`
- `n == queries[i].length == dictionary[j].length`
- `1 <= n <= 100`
- 所有 `queries[i]` 和 `dictionary[j]` 都只包含小写英文字母。

__思路:__

```text
模拟
对查询中的每一个单词
遍历字典中的每一个词
若其字符差在 2 以内就加入结果
时间复杂度为 O(MQN), 空间复杂度为 O(1), 其中 Q 为查询的单词个数，N 为字典的单词个数，M 为单词的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<string> twoEditWords(vector<string>& queries, vector<string>& dictionary) 
    {
        vector<string> result;
        for (const auto& word : queries) if (helper(word, dictionary)) result.emplace_back(word);
        return result;
    }
private:
    bool helper(string word, vector<string>& dictionary) 
    {
        for (const auto& s : dictionary) 
        {
            int cur = 0, n = word.size();
            for (int i = 0; i < n and cur <= 2; i++) cur += s[i] != word[i];
            if (cur <= 2) return true;
        }
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> twoEditWords(String[] queries, String[] dictionary) {
        List<String> result = new ArrayList<>();
        for (String word : queries) if (helper(word, dictionary)) result.add(word);
        return result;
    }

    private boolean helper(String word, String[] dictionary) {
        for (String s : dictionary) {
            int cur = 0, n = word.length();
            for (int i = 0; i < n && cur <= 2; i++) if (s.charAt(i) != word.charAt(i)) ++cur;
            if (cur <= 2) return true;
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def twoEditWords(self, queries: List[str], dictionary: List[str]) -> List[str]:
        return [word for word in queries if f(word, dictionary) <= 2] if (f := lambda word, dictionary: min(sum(a != b for a, b in zip(word, s)) for s in dictionary)) else []
```
