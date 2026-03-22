# 2900 Longest Unequal Adjacent Groups Subsequence I 最长相邻不相等子序列 I

__Description:__

You are given a string array `words` and a __binary__ array `groups` both of length `n`.

A subsequence of `words` is __alternating__ if for any two _consecutive_ strings in the sequence, their corresponding elements at the _same_ indices in `groups` are __different__ (that is, there _cannot_ be consecutive 0 or 1).

Your task is to select the __longest alternating__ subsequence from `words`.

Return the selected subsequence. If there are multiple answers, return any of them.

__Note:__ The elements in `words` are distinct.

__Example:__

Example 1:

```text
Input: words = ["e","a","b"], groups = [0,0,1]

Output: ["e","b"]
```

__Explanation:__ A subsequence that can be selected is `["e","b"]` because `groups[0] != groups[2]`. Another subsequence that can be selected is `["a","b"]` because `groups[1] != groups[2]`. It can be demonstrated that the length of the longest subsequence of indices that satisfies the condition is `2`.

Example 2:

```text
Input: words = ["a","b","c","d"], groups = [1,0,1,1]

Output: ["a","b","c"]
```

__Explanation:__ A subsequence that can be selected is `["a","b","c"]` because `groups[0] != groups[1]` and `groups[1] != groups[2]`. Another subsequence that can be selected is `["a","b","d"]` because `groups[0] != groups[1]` and `groups[1] != groups[3]`. It can be shown that the length of the longest subsequence of indices that satisfies the condition is `3`.

__Constraints:__

- `1 <= n == words.length == groups.length <= 100`
- `1 <= words[i].length <= 10`
- `groups[i]` is either `0` or `1.`
- `words` consists of __distinct__ strings.
- `words[i]` consists of lowercase English letters.

__题目描述:__

给定一个字符串数组 `words` ，和一个 __二进制__ 数组 `groups` ，两个数组长度都是 `n` 。

如果 `words` 的一个 __子序列__ 是交替的，那么对于序列中的任意两个连续字符串，它们在 `groups` 中相同索引的对应元素是 __不同__ 的（也就是说，不能有连续的 0 或 1），

你需要从 `words` 中选出 __最长交替子序列__。

返回选出的子序列。如果有多个答案，返回 任意 一个。

__注意:__
`words` 中的元素是不同的 。

__示例:__

示例 1：

```text
输入：words = ["e","a","b"], groups = [0,0,1]
输出：["e","b"]
解释：一个可行的子序列是 [0,2] ，因为 groups[0] != groups[2] 。
所以一个可行的答案是 [words[0],words[2]] = ["e","b"] 。
另一个可行的子序列是 [1,2] ，因为 groups[1] != groups[2] 。
得到答案为 [words[1],words[2]] = ["a","b"] 。
这也是一个可行的答案。
符合题意的最长子序列的长度为 2 。
```

示例 2：

```text
输入：words = ["a","b","c","d"], groups = [1,0,1,1]
输出：["a","b","c"]
解释：一个可行的子序列为 [0,1,2] 因为 groups[0] != groups[1] 且 groups[1] != groups[2] 。
所以一个可行的答案是 [words[0],words[1],words[2]] = ["a","b","c"] 。
另一个可行的子序列为 [0,1,3] 因为 groups[0] != groups[1] 且 groups[1] != groups[3] 。
得到答案为 [words[0],words[1],words[3]] = ["a","b","d"] 。
这也是一个可行的答案。
符合题意的最长子序列的长度为 3 。
```

__提示：__

- `1 <= n == words.length == groups.length <= 100`
- `1 <= words[i].length <= 10`
- `groups[i]` 是 `0` 或 `1`。
- `words` 中的字符串 __互不相同__ 。
- `words[i]` 只包含小写英文字母。

__思路:__

```text
模拟
遍历字符串
如果对应的 groups 在相邻处不相等就加入结果中
最后加入最后一个字符串
这是因为对于一个二进制字符串 0001100
可以按照值分组 000 11 00
每组只能选择一个来组成子序列
最后一组的最后一个一定可以选
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<string> getLongestSubsequence(vector<string>& words, vector<int>& groups) 
    {
        vector<string> result;
        for (int n = words.size(), i = 1; i < n; i++) if (groups[i] != groups[i - 1]) result.emplace_back(words[i - 1]);
        result.emplace_back(words.back());
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> getLongestSubsequence(String[] words, int[] groups) {
        var result = new ArrayList<String>();
        for (int n = words.length, i = 1; i < n; i++) if (groups[i] != groups[i - 1]) result.add(words[i - 1]);
        result.add(words[words.length - 1]);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def getLongestSubsequence(self, words: List[str], groups: List[int]) -> List[str]:
        return [w for (x, y), w in zip(pairwise(groups), words) if x != y] + [words[-1]]
```
