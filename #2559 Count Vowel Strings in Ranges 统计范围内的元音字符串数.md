# 2559 Count Vowel Strings in Ranges 统计范围内的元音字符串数

__Description:__

You are given a __0-indexed__ array of strings `words` and a 2D array of integers `queries`.

Each query `queries[i] = [li, ri]` asks us to find the number of strings present at the indices ranging from `li` to `ri` (both __inclusive__) of `words` that start and end with a vowel.

Return _an array_ `ans` _of size_ `queries.length`_, where_ `ans[i]` _is the answer to the_ `i` ^ th _query_.

__Note__ that the vowel letters are `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`.

__Example:__

Example 1:

```text
Input: words = ["aba","bcb","ece","aa","e"], queries = [[0,2],[1,4],[1,1]]
Output: [2,3,0]
Explanation: The strings starting and ending with a vowel are "aba", "ece", "aa" and "e".
The answer to the query [0,2] is 2 (strings "aba" and "ece").
to query [1,4] is 3 (strings "ece", "aa", "e").
to query [1,1] is 0.
We return [2,3,0].
```

Example 2:

```text
Input: words = ["a","e","i"], queries = [[0,2],[0,1],[2,2]]
Output: [3,2,1]
Explanation: Every string satisfies the conditions, so we return [3,2,1].
```

__Constraints:__

- `1 <= words.length <= 10 ^ 5`
- `1 <= words[i].length <= 40`
- `words[i]` consists only of lowercase English letters.
- `sum(words[i].length) <= 3 * 10 ^ 5`
- `1 <= queries.length <= 10 ^ 5`
- `0 <= li <= ri < words.length`

__题目描述:__

给你一个下标从 __0__ 开始的字符串数组 `words` 以及一个二维整数数组 `queries` 。

每个查询 `queries[i] = [li, ri]` 会要求我们统计在 `words` 中下标在 `li` 到 `ri` 范围内（__包含__ 这两个值）并且以元音开头和结尾的字符串的数目。

返回一个整数数组，其中数组的第 `i` 个元素对应第 `i` 个查询的答案。

__注意:__元音字母是 `'a'`、 `'e'`、 `'i'`、 `'o'` 和 `'u'` 。

__示例:__

示例 1：

```text
输入：words = ["aba","bcb","ece","aa","e"], queries = [[0,2],[1,4],[1,1]]
输出：[2,3,0]
解释：以元音开头和结尾的字符串是 "aba"、"ece"、"aa" 和 "e" 。
查询 [0,2] 结果为 2（字符串 "aba" 和 "ece"）。
查询 [1,4] 结果为 3（字符串 "ece"、"aa"、"e"）。
查询 [1,1] 结果为 0 。
返回结果 [2,3,0] 。
```

示例 2：

```text
输入：words = ["a","e","i"], queries = [[0,2],[0,1],[2,2]]
输出：[3,2,1]
解释：每个字符串都满足这一条件，所以返回 [3,2,1] 。
```

__提示：__

- `1 <= words.length <= 10 ^ 5`
- `1 <= words[i].length <= 40`
- `words[i]` 仅由小写英文字母组成
- `sum(words[i].length) <= 3 * 10 ^ 5`
- `1 <= queries.length <= 10 ^ 5`
- `0 <= queries[j][0] <= queries[j][1] < words.length`

__思路:__

```text
前缀和
离线处理所有的 words
用前缀和计算 word 的首尾都是元音的字符串数
查询的时候直接使用前缀和数组即可
时间复杂度为 O(N + M), 空间复杂度为 O(N), 其中 N 为 words 的长度, M 为 queries 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> vowelStrings(vector<string>& words, vector<vector<int>>& queries) 
    {
        int m = words.size(), n = queries.size();
        vector<int> pre(m + 1), result(n);
        for (int i = 0; i < m; i++) pre[i + 1] = pre[i] + ((words[i].front() == 'a' or words[i].front() == 'e' or words[i].front() == 'i' or words[i].front() == 'o' or words[i].front() == 'u') and (words[i].back() == 'a' or words[i].back() == 'e' or words[i].back() == 'i' or words[i].back() == 'o' or words[i].back() == 'u'));
        for (int i = 0; i < n; i++) result[i] = pre[queries[i][1] + 1] - pre[queries[i][0]];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] vowelStrings(String[] words, int[][] queries) {
        int m = words.length, n = queries.length, pre[] = new int[m + 1], result[] = new int[n];
        for (int i = 0; i < m; i++) pre[i + 1] = pre[i] + check(words[i], words[i].length());
        for (int i = 0; i < n; i++) result[i] = pre[queries[i][1] + 1] - pre[queries[i][0]];
        return result;
    }

    private int check(String word, int n) {
        return (word.charAt(0) == 'a' || word.charAt(0) == 'e' || word.charAt(0) == 'i' || word.charAt(0) == 'o' || word.charAt(0) == 'u') && (word.charAt(n - 1) == 'a' || word.charAt(n - 1) == 'e' || word.charAt(n - 1) == 'i' || word.charAt(n - 1) == 'o' || word.charAt(n - 1) == 'u') ? 1 : 0;
    }
}
```

__Python__:

```Python
class Solution:
    def vowelStrings(self, words: List[str], queries: List[List[int]]) -> List[int]:
        return [] if not (pre := list(accumulate((w[0] in "aeiou" and w[-1] in "aeiou" for w in words), initial=0))) else [pre[q[1] + 1] - pre[q[0]] for q in queries]
```
