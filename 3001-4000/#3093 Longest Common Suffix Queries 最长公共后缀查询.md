# 3093 Longest Common Suffix Queries 最长公共后缀查询

__Description:__

You are given two arrays of strings `wordsContainer` and `wordsQuery`.

For each `wordsQuery[i]`, you need to find a string from `wordsContainer` that has the __longest common suffix__ with `wordsQuery[i]`. If there are two or more strings in `wordsContainer` that share the longest common suffix, find the string that is the __smallest__ in length. If there are two or more such strings that have the __same__ smallest length, find the one that occurred __earlier__ in `wordsContainer`.

Return _an array of integers_ `ans`_, where_ `ans[i]` _is the index of the string in_ `wordsContainer` _that has the __longest common suffix__ with_ `wordsQuery[i]`_._

__Example:__

Example 1:

```text
Input: wordsContainer = ["abcd","bcd","xbcd"], wordsQuery = ["cd","bcd","xyz"]

Output: [1,1,1]

Explanation:
```

Let's look at each `wordsQuery[i]` separately:

- For `wordsQuery[0] = "cd"`, strings from `wordsContainer` that share the longest common suffix `"cd"` are at indices 0, 1, and 2. Among these, the answer is the string at index 1 because it has the shortest length of 3.
- For `wordsQuery[1] = "bcd"`, strings from `wordsContainer` that share the longest common suffix `"bcd"` are at indices 0, 1, and 2. Among these, the answer is the string at index 1 because it has the shortest length of 3.
- For `wordsQuery[2] = "xyz"`, there is no string from `wordsContainer` that shares a common suffix. Hence the longest common suffix is `""`, that is shared with strings at index 0, 1, and 2. Among these, the answer is the string at index 1 because it has the shortest length of 3.

Example 2:

```text
Input: wordsContainer = ["abcdefgh","poiuygh","ghghgh"], wordsQuery = ["gh","acbfgh","acbfegh"]

Output: [2,0,2]

Explanation:
```

Let's look at each `wordsQuery[i]` separately:

- For `wordsQuery[0] = "gh"`, strings from `wordsContainer` that share the longest common suffix `"gh"` are at indices 0, 1, and 2. Among these, the answer is the string at index 2 because it has the shortest length of 6.
- For `wordsQuery[1] = "acbfgh"`, only the string at index 0 shares the longest common suffix `"fgh"`. Hence it is the answer, even though the string at index 2 is shorter.
- For `wordsQuery[2] = "acbfegh"`, strings from `wordsContainer` that share the longest common suffix `"gh"` are at indices 0, 1, and 2. Among these, the answer is the string at index 2 because it has the shortest length of 6.

__Constraints:__

- `1 <= wordsContainer.length, wordsQuery.length <= 10 ^ 4`
- `1 <= wordsContainer[i].length <= 5 * 10 ^ 3`
- `1 <= wordsQuery[i].length <= 5 * 10 ^ 3`
- `wordsContainer[i]` consists only of lowercase English letters.
- `wordsQuery[i]` consists only of lowercase English letters.
- Sum of `wordsContainer[i].length` is at most `5 * 10 ^ 5`.
- Sum of `wordsQuery[i].length` is at most `5 * 10 ^ 5`.

__题目描述:__

给你两个字符串数组 `wordsContainer` 和 `wordsQuery` 。

对于每个 `wordsQuery[i]` ，你需要从 `wordsContainer` 中找到一个与 `wordsQuery[i]` 有 __最长公共后缀__ 的字符串。如果 `wordsContainer` 中有两个或者更多字符串有最长公共后缀，那么答案为长度 __最短__ 的。如果有超过两个字符串有 __相同__ 最短长度，那么答案为它们在 `wordsContainer` 中出现 __更早__ 的一个。

请你返回一个整数数组 `ans` ，其中 `ans[i]`是 `wordsContainer`中与 `wordsQuery[i]` 有 __最长公共后缀__ 字符串的下标。

__示例:__

示例 1：

```text
输入：wordsContainer = ["abcd","bcd","xbcd"], wordsQuery = ["cd","bcd","xyz"]

输出：[1,1,1]

解释：
```

我们分别来看每一个 `wordsQuery[i]` :

- 对于 `wordsQuery[0] = "cd"` ， `wordsContainer` 中有最长公共后缀 `"cd"` 的字符串下标分别为 0 ，1 和 2 。这些字符串中，答案是下标为 1 的字符串，因为它的长度为 3 ，是最短的字符串。
- 对于 `wordsQuery[1] = "bcd"` ， `wordsContainer` 中有最长公共后缀 `"bcd"` 的字符串下标分别为 0 ，1 和 2 。这些字符串中，答案是下标为 1 的字符串，因为它的长度为 3 ，是最短的字符串。
- 对于 `wordsQuery[2] = "xyz"` ， `wordsContainer` 中没有字符串跟它有公共后缀，所以最长公共后缀为 `""` ，下标为 0 ，1 和 2 的字符串都得到这一公共后缀。这些字符串中， 答案是下标为 1 的字符串，因为它的长度为 3 ，是最短的字符串。

示例 2：

```text
输入：wordsContainer = ["abcdefgh","poiuygh","ghghgh"], wordsQuery = ["gh","acbfgh","acbfegh"]

输出：[2,0,2]

解释：
```

我们分别来看每一个 `wordsQuery[i]` :

- 对于 `wordsQuery[0] = "gh"` ， `wordsContainer` 中有最长公共后缀 `"gh"` 的字符串下标分别为 0 ，1 和 2 。这些字符串中，答案是下标为 2 的字符串，因为它的长度为 6 ，是最短的字符串。
- 对于 `wordsQuery[1] = "acbfgh"` ，只有下标为 0 的字符串有最长公共后缀 `"fgh"` 。所以尽管下标为 2 的字符串是最短的字符串，但答案是 0 。
- 对于 `wordsQuery[2] = "acbfegh"` ， `wordsContainer` 中有最长公共后缀 `"gh"` 的字符串下标分别为 0 ，1 和 2 。这些字符串中，答案是下标为 2 的字符串，因为它的长度为 6 ，是最短的字符串。

__提示：__

- `1 <= wordsContainer.length, wordsQuery.length <= 10 ^ 4`
- `1 <= wordsContainer[i].length <= 5 * 10 ^ 3`
- `1 <= wordsQuery[i].length <= 5 * 10 ^ 3`
- `wordsContainer[i]` 只包含小写英文字母。
- `wordsQuery[i]` 只包含小写英文字母。
- `wordsContainer[i].length` 的和至多为 `5 * 10 ^ 5` 。
- `wordsQuery[i].length` 的和至多为 `5 * 10 ^ 5` 。

__思路:__

```text
字典树
使用字典树记录反转之后的字符串
就把记录最长后缀转化为记录最长前缀
在 root 层记录所有 wordsContainer 的最短长度及位置
在每一个字符串遍历的位置记录最短长度及位置
匹配 wordsQuery 中的每一个字符串的反转
找到最长的前缀并输出位置
时间复杂度为 O(CN + M), 空间复杂度为 O(N), 其中 N 为 wordsContainer 中字符的个数, M 为 wordsQuery 中字符的个数, C 为字符的种类, 本题为 26
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> stringIndices(vector<string>& wordsContainer, vector<string>& wordsQuery) 
    {
        Trie root{};
        for (int i = 0, m = wordsContainer.size(), s_len = 0; i < m; i++) 
        {
            if ((s_len = wordsContainer[i].size()) < root.min_len) 
            {
                root.min_len = s_len;
                root.best_index = i;
            }
            Trie* cur = &root;
            for (int j = s_len - 1, c = 0; j > -1; j--) 
            {
                if (!cur -> children[c = wordsContainer[i][j] - 'a']) cur -> children[c] = new Trie();
                cur = cur -> children[c];
                if (s_len < cur -> min_len) 
                {
                    cur -> min_len = s_len;
                    cur -> best_index = i;
                }
            }
        }
        vector<int> result;
        for (const auto& s : wordsQuery) 
        {
            Trie* cur = &root;
            for (int j = s.size() - 1, c = 0; j > -1 and cur -> children[c = s[j] - 'a']; j--) cur = cur -> children[c];
            result.emplace_back(cur -> best_index);
        }
        return result;
    }
private:
    struct Trie 
    {
        Trie* children[26]{ };
        int min_len = INT_MAX;
        int best_index;

        ~Trie() 
        {
            for (Trie* child : children) delete child;
        }
    };
};
```

__Java__:

```Java
class Solution 
{
public:
    vector<int> stringIndices(vector<string>& wordsContainer, vector<string>& wordsQuery) 
    {
        Trie root{};
        for (int i = 0, m = wordsContainer.size(), s_len = 0; i < m; i++) 
        {
            if ((s_len = wordsContainer[i].size()) < root.min_len) 
            {
                root.min_len = s_len;
                root.best_index = i;
            }
            Trie* cur = &root;
            for (int j = s_len - 1, c = 0; j > -1; j--) 
            {
                if (!cur -> children[c = wordsContainer[i][j] - 'a']) cur -> children[c] = new Trie();
                cur = cur -> children[c];
                if (s_len < cur -> min_len) 
                {
                    cur -> min_len = s_len;
                    cur -> best_index = i;
                }
            }
        }
        vector<int> result;
        for (const auto& s : wordsQuery) 
        {
            Trie* cur = &root;
            for (int j = s.size() - 1, c = 0; j > -1 and cur -> children[c = s[j] - 'a']; j--) cur = cur -> children[c];
            result.emplace_back(cur -> best_index);
        }
        return result;
    }
private:
    struct Trie 
    {
        Trie* children[26]{ };
        int min_len = INT_MAX;
        int best_index;

        ~Trie() 
        {
            for (Trie* child : children) delete child;
        }
    };
};
```

__Python__:

```Python
class Solution 
{
public:
    vector<int> stringIndices(vector<string>& wordsContainer, vector<string>& wordsQuery) 
    {
        Trie root{};
        for (int i = 0, m = wordsContainer.size(), s_len = 0; i < m; i++) 
        {
            if ((s_len = wordsContainer[i].size()) < root.min_len) 
            {
                root.min_len = s_len;
                root.best_index = i;
            }
            Trie* cur = &root;
            for (int j = s_len - 1, c = 0; j > -1; j--) 
            {
                if (!cur -> children[c = wordsContainer[i][j] - 'a']) cur -> children[c] = new Trie();
                cur = cur -> children[c];
                if (s_len < cur -> min_len) 
                {
                    cur -> min_len = s_len;
                    cur -> best_index = i;
                }
            }
        }
        vector<int> result;
        for (const auto& s : wordsQuery) 
        {
            Trie* cur = &root;
            for (int j = s.size() - 1, c = 0; j > -1 and cur -> children[c = s[j] - 'a']; j--) cur = cur -> children[c];
            result.emplace_back(cur -> best_index);
        }
        return result;
    }
private:
    struct Trie 
    {
        Trie* children[26]{ };
        int min_len = INT_MAX;
        int best_index;

        ~Trie() 
        {
            for (Trie* child : children) delete child;
        }
    };
};
```
