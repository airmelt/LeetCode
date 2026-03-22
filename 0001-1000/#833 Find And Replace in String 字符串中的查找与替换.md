# 833 Find And Replace in String 字符串中的查找与替换

__Description__:
You are given a 0-indexed string s that you must perform k replacement operations on. The replacement operations are given as three 0-indexed parallel arrays, indices, sources, and targets, all of length k.

To complete the ith replacement operation:

Check if the substring sources[i] occurs at index indices[i] in the original string s.
If it does not occur, do nothing.
Otherwise if it does occur, replace that substring with targets[i].
For example, if s = "abcd", indices[i] = 0, sources[i] = "ab", and targets[i] = "eee", then the result of this replacement will be "eeecd".

All replacement operations must occur simultaneously, meaning the replacement operations should not affect the indexing of each other. The testcases will be generated such that the replacements will not overlap.

For example, a testcase with s = "abc", indices = [0, 1], and sources = ["ab","bc"] will not be generated because the "ab" and "bc" replacements overlap.
Return the resulting string after performing all replacement operations on s.

A substring is a contiguous sequence of characters in a string.

__Example:__

Example 1:

![exchange 1](https://assets.leetcode.com/uploads/2021/06/12/833-ex1.png)

Input: s = "abcd", indices = [0, 2], sources = ["a", "cd"], targets = ["eee", "ffff"]
Output: "eeebffff"
Explanation:
"a" occurs at index 0 in s, so we replace it with "eee".
"cd" occurs at index 2 in s, so we replace it with "ffff".

Example 2:

![exchange 2](https://assets.leetcode.com/uploads/2021/06/12/833-ex2-1.png)

Input: s = "abcd", indices = [0, 2], sources = ["ab","ec"], targets = ["eee","ffff"]
Output: "eeecd"
Explanation:
"ab" occurs at index 0 in s, so we replace it with "eee".
"ec" does not occur at index 2 in s, so we do nothing.

__Constraints:__

1 <= s.length <= 1000
k == indices.length == sources.length == targets.length
1 <= k <= 100
0 <= indexes[i] < s.length
1 <= sources[i].length, targets[i].length <= 50
s consists of only lowercase English letters.
sources[i] and targets[i] consist of only lowercase English letters.

__题目描述__:
某个字符串 S 需要执行一些替换操作，用新的字母组替换原有的字母组（不一定大小相同）。

每个替换操作具有 3 个参数：起始索引 i，源字 x 和目标字 y。规则是：如果 x 从原始字符串 S 中的位置 i 开始，那么就用 y 替换出现的 x。如果没有，则什么都不做。

举个例子，如果 S = “abcd” 并且替换操作 i = 2，x = “cd”，y = “ffff”，那么因为 “cd” 从原始字符串 S 中的位置 2 开始，所以用 “ffff” 替换它。

再来看 S = “abcd” 上的另一个例子，如果一个替换操作 i = 0，x = “ab”，y = “eee”，以及另一个替换操作 i = 2，x = “ec”，y = “ffff”，那么第二个操作将不会执行，因为原始字符串中 S[2] = 'c'，与 x[0] = 'e' 不匹配。

所有这些操作同时发生。保证在替换时不会有任何重叠： S = "abc", indexes = [0, 1], sources = ["ab","bc"] 不是有效的测试用例。

__示例 :__

示例 1：

输入：S = "abcd", indexes = [0,2], sources = ["a","cd"], targets = ["eee","ffff"]
输出："eeebffff"
解释：
"a" 从 S 中的索引 0 开始，所以它被替换为 "eee"。
"cd" 从 S 中的索引 2 开始，所以它被替换为 "ffff"。

示例 2：

输入：S = "abcd", indexes = [0,2], sources = ["ab","ec"], targets = ["eee","ffff"]
输出："eeecd"
解释：
"ab" 从 S 中的索引 0 开始，所以它被替换为 "eee"。
"ec" 没有从原始的 S 中的索引 2 开始，所以它没有被替换。

__提示:__

0 <= S.length <= 1000
S 仅由小写英文字母组成
0 <= indexes.length <= 100
0 <= indexes[i] < S.length
sources.length == indexes.length
targets.length == indexes.length
1 <= sources[i].length, targets[i].length <= 50
sources[i] 和 targets[i] 仅由小写英文字母组成

__思路__:

模拟
将需要修改的字符串位置记录或者将 indices 数组排序之后从后往前遍历修改
用一个可变的字符串存储, 不需要修改的直接插入, 需要修改的插入之后更新下标
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string findReplaceString(string s, vector<int>& indices, vector<string>& sources, vector<string>& targets) 
    {
        stringstream ss;
        int n = s.size(), m = indices.size(), j = 0;
        vector<int> mark(n);
        for (int i = 0; i < m; i++) if (s.substr(indices[i], sources[i].size()) == sources[i]) mark[indices[i]] = i + 1;
        while (j < n)
        {
            if (!mark[j]) ss << s[j++];
            else
            {
                ss << targets[mark[j] - 1];
                j += sources[mark[j] - 1].size();
            }
        }
        return ss.str();
    }
};
```

__Java__:

```Java
class Solution {
    public String findReplaceString(String s, int[] indices, String[] sources, String[] targets) {
        StringBuilder sb = new StringBuilder();
        int n = s.length(), m = indices.length, j = 0, mark[] = new int[n];
        for (int i = 0; i < m; i++) if (sources[i].equals(s.substring(indices[i], sources[i].length() + indices[i]))) mark[indices[i]] = i + 1;
        while (j < n)
        {
            if (mark[j] == 0) sb.append(s.charAt(j++));
            else
            {
                sb.append(targets[mark[j] - 1]);
                j += sources[mark[j] - 1].length();
            }
        }
        return sb.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def findReplaceString(self, s: str, indices: List[int], sources: List[str], targets: List[str]) -> str:
        n, s = len(indices), list(s)
        for i, c, t in sorted([(indices[i], sources[i], targets[i]) for i in range(n)], reverse = True):
            if s[i:i + len(c)] == list(c) : s[i:i + len(c)] = list(t)
        return ''.join(s)
```
