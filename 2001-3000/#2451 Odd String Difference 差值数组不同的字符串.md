# 2451 Odd String Difference 差值数组不同的字符串

__Description:__

You are given an array of equal-length strings `words`. Assume that the length of each string is `n`.

Each string `words[i]` can be converted into a __difference integer array__ `difference[i]` of length `n - 1` where `difference[i][j] = words[i][j+1] - words[i][j]` where `0 <= j <= n - 2`. Note that the difference between two letters is the difference between their __positions__ in the alphabet i.e. the position of `'a'` is `0`, `'b'` is `1`, and `'z'` is `25`.

- For example, for the string `"acb"`, the difference integer array is `[2 - 0, 1 - 2] = [2, -1]`.

All the strings in words have the same difference integer array, except one. You should find that string.

Return _the string in_ `words` _that has different __difference integer array__._

__Example:__

Example 1:

```text
Input: words = ["adc","wzy","abc"]
Output: "abc"
Explanation: 
```

- The difference integer array of "adc" is [3 - 0, 2 - 3] = [3, -1].
- The difference integer array of "wzy" is [25 - 22, 24 - 25]= [3, -1].
- The difference integer array of "abc" is [1 - 0, 2 - 1] = [1, 1].

The odd array out is [1, 1], so we return the corresponding string, "abc".

Example 2:

```text
Input: words = ["aaa","bob","ccc","ddd"]
Output: "bob"
Explanation: All the integer arrays are [0, 0] except for "bob", which corresponds to [13, -13].
```

__Constraints:__

- `3 <= words.length <= 100`
- `n == words[i].length`
- `2 <= n <= 20`
- `words[i]` consists of lowercase English letters.

__题目描述:__

给你一个字符串数组 `words` ，每一个字符串长度都相同，令所有字符串的长度都为 `n` 。

每个字符串 `words[i]` 可以被转化为一个长度为 `n - 1` 的 __差值整数数组__ `difference[i]` ，其中对于 `0 <= j <= n - 2` 有 `difference[i][j] = words[i][j+1] - words[i][j]` 。注意两个字母的差值定义为它们在字母表中 __位置__ 之差，也就是说 `'a'` 的位置是 `0` ， `'b'` 的位置是 `1` ， `'z'` 的位置是 `25` 。

- 比方说，字符串 `"acb"` 的差值整数数组是 `[2 - 0, 1 - 2] = [2, -1]` 。

`words` 中所有字符串 __除了一个字符串以外__ ，其他字符串的差值整数数组都相同。你需要找到那个不同的字符串。

请你返回 `words`中 __差值整数数组__ 不同的字符串。

__示例:__

示例 1：

```text
输入：words = ["adc","wzy","abc"]
输出："abc"
解释：
```

- "adc" 的差值整数数组是 [3 - 0, 2 - 3] = [3, -1] 。
- "wzy" 的差值整数数组是 [25 - 22, 24 - 25]= [3, -1] 。
- "abc" 的差值整数数组是 [1 - 0, 2 - 1] = [1, 1] 。

不同的数组是 [1, 1]，所以返回对应的字符串，"abc"。

示例 2：

```text
输入：words = ["aaa","bob","ccc","ddd"]
输出："bob"
解释：除了 "bob" 的差值整数数组是 [13, -13] 以外，其他字符串的差值整数数组都是 [0, 0] 。
```

__提示：__

- `3 <= words.length <= 100`
- `n == words[i].length`
- `2 <= n <= 20`
- `words[i]` 只含有小写英文字母。

__思路:__

```text
哈希表
用一个哈希表记录每个字符串的差值数组
将对应字符串插入对应的差值数组中
遍历哈希表，返回差值数组对应的字符串数组
如果差值数组对应的字符串数组的长度为 1，则返回该字符串
时间复杂度为 O(MN), 空间复杂度为 O(M), 其中 M 为字符串的长度，N 为字符串的个数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string oddString(vector<string>& words) 
    {
        map<vector<int>, vector<string>> m;
        for (const auto& word : words) 
        {
            vector<int> cur;
            for (int i = 1, n = word.size(); i < n; i++) cur.push_back(word[i] - word[i - 1]);
            m[cur].emplace_back(word);
        }
        for (auto it = m.begin(); it != m.end(); it++) if (it -> second.size() == 1) return it -> second[0];
        return "";
    }
};
```

__Java__:

```Java
class Solution {
    public String oddString(String[] words) {
        var map = new HashMap<List<Integer>, List<String>>();
        for (String word : words) {
            List<Integer> list = new ArrayList<>();
            for (int i = 1, n = word.length(); i < n; i++) list.add(word.charAt(i) - word.charAt(i - 1));
            map.computeIfAbsent(list, k -> new ArrayList<>()).add(word);
        }
        for (var v : map.values()) if (v.size() == 1) return v.get(0);
        return "";
    }
}
```

__Python__:

```Python
class Solution:
    def oddString(self, words: List[str]) -> str:
        return words[diff[-1][1]] if (diff := sorted([([(ord(w[j + 1]) - ord(w[j])) for j in range(len(w) - 1)], i) for i, w in enumerate(words)]))[0][0] == diff[1][0] else words[diff[0][1]]
```
