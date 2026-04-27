# 2901 Longest Unequal Adjacent Groups Subsequence II 最长相邻不相等子序列 II

__Description:__

You are given a string array `words`, and an array `groups`, both arrays having length `n`.

The hamming distance between two strings of equal length is the number of positions at which the corresponding characters are different.

You need to select the __longest__ subsequence from an array of indices `[0, 1, ..., n - 1]`, such that for the subsequence denoted as `[i0, i1, ..., ik-1]` having length `k`, the following holds:

- For __adjacent__ indices in the subsequence, their corresponding groups are __unequal__, i.e., `groups[ij] != groups[ij+1]`, for each `j` where `0 < j + 1 < k`.
- `words[ij]` and `words[ij+1]` are __equal__ in length, and the __hamming distance__ between them is `1`, where `0 < j + 1 < k`, for all indices in the subsequence.

Return a string array containing the words corresponding to the indices (in order) in the selected subsequence. If there are multiple answers, return any of them.

__Note:__ strings in `words` may be __unequal__ in length.

__Example:__

Example 1:

```text
Input: words = ["bab","dab","cab"], groups = [1,2,2]

Output: ["bab","cab"]
```

__Explanation:__ A subsequence that can be selected is `[0,2]`.

- `groups[0] != groups[2]`
- `words[0].length == words[2].length`, and the hamming distance between them is 1.

So, a valid answer is `[words[0],words[2]] = ["bab","cab"]`.

Another subsequence that can be selected is `[0,1]`.

- `groups[0] != groups[1]`
- `words[0].length == words[1].length`, and the hamming distance between them is `1`.

So, another valid answer is `[words[0],words[1]] = ["bab","dab"]`.

It can be shown that the length of the longest subsequence of indices that satisfies the conditions is `2`.

Example 2:

```text
Input: words = ["a","b","c","d"], groups = [1,2,3,4]

Output: ["a","b","c","d"]
```

__Explanation:__ We can select the subsequence `[0,1,2,3]`.

It satisfies both conditions.

Hence, the answer is `[words[0],words[1],words[2],words[3]] = ["a","b","c","d"]`.

It has the longest length among all subsequences of indices that satisfy the conditions.

Hence, it is the only answer.

__Constraints:__

- `1 <= n == words.length == groups.length <= 1000`
- `1 <= words[i].length <= 10`
- `1 <= groups[i] <= n`
- `words` consists of __distinct__ strings.
- `words[i]` consists of lowercase English letters.

__题目描述:__

给定一个字符串数组 `words` ，和一个数组 `groups` ，两个数组长度都是 `n` 。

两个长度相等字符串的 汉明距离 定义为对应位置字符 不同 的数目。

你需要从下标 `[0, 1, ..., n - 1]` 中选出一个 __最长子序列__ ，将这个子序列记作长度为 `k` 的 `[i0, i1, ..., ik - 1]` ，它需要满足以下条件:

- __相邻__ 下标对应的 `groups` 值 __不同__。即，对于所有满足 `0 < j + 1 < k` 的 `j` 都有 `groups[ij] != groups[ij + 1]` 。
- 对于所有 `0 < j + 1 < k` 的下标 `j` ，都满足 `words[ij]` 和 `words[ij + 1]` 的长度 __相等__ ，且两个字符串之间的 __汉明距离__ 为 `1` 。

请你返回一个字符串数组，它是下标子序列 __依次__ 对应 `words` 数组中的字符串连接形成的字符串数组。如果有多个答案，返回任意一个。

子序列 指的是从原数组中删掉一些（也可能一个也不删掉）元素，剩余元素不改变相对位置得到的新的数组。

_注意:_ `words` 中的字符串长度可能 __不相等__ 。

__示例:__

示例 1：

```text
输入：words = ["bab","dab","cab"], groups = [1,2,2]
输出：["bab","cab"]
解释：一个可行的子序列是 [0,2] 。
```

- groups[0] != groups[2]
- words[0].length == words[2].length 且它们之间的汉明距离为 1 。
所以一个可行的答案是 [words[0],words[2]] = ["bab","cab"] 。
另一个可行的子序列是 [0,1] 。
- groups[0] != groups[1]
- words[0].length = words[1].length 且它们之间的汉明距离为 1 。

所以另一个可行的答案是 [words[0],words[1]] = ["bab","dab"] 。
符合题意的最长子序列的长度为 2 。

示例 2：

```text
输入：words = ["a","b","c","d"], groups = [1,2,3,4]
输出：["a","b","c","d"]
解释：我们选择子序列 [0,1,2,3] 。
它同时满足两个条件。
所以答案为 [words[0],words[1],words[2],words[3]] = ["a","b","c","d"] 。
它是所有下标子序列里最长且满足所有条件的。
所以它是唯一的答案。
```

__提示：__

- `1 <= n == words.length == groups.length <= 1000`
- `1 <= words[i].length <= 10`
- `1 <= groups[i] <= n`
- `words` 中的字符串 __互不相同__ 。
- `words[i]` 只包含小写英文字母。

__思路:__

```text
1. 动态规划
设 dp[i] 表示 words[:i] 里的最长相邻不等字串长度
则 dp[j] = max(dp[i]) + 1, words[i] 和 word[j] 满足汉明距离为 1, 并且 -1 < i < j < n
所以遍历应该从后往前遍历
设 f[i] 表示 i 由 f[i] 转移过来
遍历字符串数组, 如果 dp[j] > dp[i]
检查 groups 和汉明距离
如果是更新 dp[i] = dp[j], 且更新 f[i] = j
在外层自增表示可以加上自身
最后由 f 数组还原字符串结果
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
2. 字符串处理
由于字符串互不相同所以只要匹配上其他字符一定不会出现相同的
所以可以枚举 26 个字符进行匹配
分别枚举每个位置上的所有不同字符
更新最长以及次长子序列
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<string> getWordsInLongestSubsequence(vector<string>& words, vector<int>& groups) 
    {
        int n = words.size(), m = n - 1;
        vector<int> dp(n), f(n);
        auto check = [](const auto& a, const auto& b) -> bool 
        {
            if (a.size() != b.size()) return false;
            bool flag = false;
            for (int i = 0, n = a.size(); i < n; i++) 
            {
                if (a[i] != b[i]) 
                {
                    if (flag) return false;
                    flag = true;
                }
            }
            return flag;
        };
        for (int i = n - 1; i > -1; i--) 
        {
            for (int j = i + 1; j < n; j++) 
            {
                if (dp[j] > dp[i] && groups[j] != groups[i] && check(words[i], words[j])) 
                {
                    dp[i] = dp[j];
                    f[i] = j;
                }
            }
            ++dp[i];
            if (dp[i] > dp[m]) m = i;
        }
        vector<string> result;
        for (int i = m, j = 0; j < dp[m]; j++) 
        {
            result.emplace_back(words[i]);
            i = f[i];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> getWordsInLongestSubsequence(String[] words, int[] groups) {
        int n = words.length, dp[] = new int[n], f[] = new int[n], m = n - 1;
        for (int i = n - 1; i > -1; i--) {
            for (int j = i + 1; j < n; j++) {
                if (dp[j] > dp[i] && groups[j] != groups[i] && check(words[i], words[j])) {
                    dp[i] = dp[j];
                    f[i] = j;
                }
            }
            ++dp[i];
            if (dp[i] > dp[m]) m = i;
        }
        var result = new ArrayList<String>();
        for (int i = m, j = 0; j < dp[m]; j++) {
            result.add(words[i]);
            i = f[i];
        }
        return result;
    }

    private boolean check(String a, String b) {
        if (a.length() != b.length()) return false;
        boolean flag = false;
        for (int i = 0, n = a.length(); i < n; i++) {
            if (a.charAt(i) != b.charAt(i)) {
                if (flag) return false;
                flag = true;
            }
        }
        return flag;
    }
}
```

__Python__:

```Python
class Solution:
    def getWordsInLongestSubsequence(self, words: List[str], groups: List[int]) -> List[str]:
        def check(a: str, b: str) -> bool:
            return len(a) == len(b) and sum(x != y for x, y in zip(a, b)) == 1
        dp, f, m = [0] * (n := len(words)), [0] * n, n - 1
        for i in range(n - 1, -1, -1):
            for j in range(i + 1, n):
                if dp[j] > dp[i] and groups[i] != groups[j] and check(words[i], words[j]):
                    dp[i], f[i] = dp[j], j
                    
            dp[i] += 1
            m = i if dp[i] > dp[m] else m
        result = [''] * dp[i := m]
        for j in range(dp[m]):
            result[j], i = words[i], f[i]
        return result
```
