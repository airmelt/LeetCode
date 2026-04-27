# 1177 Can Make Palindrome from Substring 构建回文串检测

__Description__:
You are given a string s and array queries where queries[i] = [lefti, righti, ki]. We may rearrange the substring s[lefti...righti] for each query and then choose up to ki of them to replace with any lowercase English letter.

If the substring is possible to be a palindrome string after the operations above, the result of the query is true. Otherwise, the result is false.

Return a boolean array answer where answer[i] is the result of the ith query queries[i].

Note that each letter is counted individually for replacement, so if, for example s[lefti...righti] = "aaa", and ki = 2, we can only replace two of the letters. Also, note that no query modifies the initial string s.

__Example:__

Input: s = "abcda", queries = [[3,3,0],[1,2,0],[0,3,1],[0,3,2],[0,4,1]]
Output: [true,false,false,true,true]
Explanation:
queries[0]: substring = "d", is palidrome.
queries[1]: substring = "bc", is not palidrome.
queries[2]: substring = "abcd", is not palidrome after replacing only 1 character.
queries[3]: substring = "abcd", could be changed to "abba" which is palidrome. Also this can be changed to "baab" first rearrange it "bacd" then replace "cd" with "ab".
queries[4]: substring = "abcda", could be changed to "abcba" which is palidrome.
Example 2:

Input: s = "lyb", queries = [[0,1,0],[2,2,1]]
Output: [false,true]

__Constraints:__

1 <= s.length, queries.length <= 10^5
0 <= lefti <= righti < s.length
0 <= ki <= s.length
s consists of lowercase English letters.

__题目描述__:
给你一个字符串 s，请你对 s 的子串进行检测。

每次检测，待检子串都可以表示为 queries[i] = [left, right, k]。我们可以 重新排列 子串 s[left], ..., s[right]，并从中选择 最多 k 项替换成任何小写英文字母。

如果在上述检测过程中，子串可以变成回文形式的字符串，那么检测结果为 true，否则结果为 false。

返回答案数组 answer[]，其中 answer[i] 是第 i 个待检子串 queries[i] 的检测结果。

注意：在替换时，子串中的每个字母都必须作为 独立的 项进行计数，也就是说，如果 s[left..right] = "aaa" 且 k = 2，我们只能替换其中的两个字母。（另外，任何检测都不会修改原始字符串 s，可以认为每次检测都是独立的）

__示例 :__

输入：s = "abcda", queries = [[3,3,0],[1,2,0],[0,3,1],[0,3,2],[0,4,1]]
输出：[true,false,false,true,true]
解释：
queries[0] : 子串 = "d"，回文。
queries[1] : 子串 = "bc"，不是回文。
queries[2] : 子串 = "abcd"，只替换 1 个字符是变不成回文串的。
queries[3] : 子串 = "abcd"，可以变成回文的 "abba"。 也可以变成 "baab"，先重新排序变成 "bacd"，然后把 "cd" 替换为 "ab"。
queries[4] : 子串 = "abcda"，可以变成回文的 "abcba"。

__提示:__

1 <= s.length, queries.length <= 10^5
0 <= queries[i][0] <= queries[i][1] < s.length
0 <= queries[i][2] <= s.length
s 中只有小写英文字母

__思路__:

前缀和 ➕ 位运算
这里的回文串实际上指的是字母出现次数是偶数次还是奇数次
为了快速计算出子串中的字母数, 可以使用前缀和
前缀和中记录每个字母出现的次数, 用异或表示
统计二进制中 1 出现的次数, 出现奇数次的次数不能超过 2 * k
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<bool> canMakePaliQueries(string s, vector<vector<int>>& queries) 
    {
        int n = s.size();
        vector<int> status(n + 1);
        for (int i = 0; i < n; i++) status[i + 1] = (status[i] ^ (1 << (s[i] - 'a')));
        vector<bool> result;
        for (const auto& query: queries) 
        {
            int count = 0, cur = (status[query[1] + 1] ^ status[query[0]]);
            while (cur) 
            {
                cur &= cur - 1;
                ++count;
            }
            result.emplace_back((count >> 1) <= query[2]);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Boolean> canMakePaliQueries(String s, int[][] queries) {
        int n = s.length(), status[] = new int[n + 1];
        for (int i = 0; i < n; i++) status[i + 1] = (status[i] ^ (1 << (s.charAt(i) - 'a')));
        List<Boolean> result = new ArrayList<>();
        for (int query[]: queries) {
            int count = 0, cur = (status[query[1] + 1] ^ status[query[0]]);
            while (cur != 0) {
                cur &= cur - 1;
                ++count;
            }
            result.add((count >> 1) <= query[2]);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def canMakePaliQueries(self, s: str, queries: List[List[int]]) -> List[bool]:
        status, result = [0] * ((n := len(s)) + 1), []
        for i in range(n):
            status[i + 1] = (status[i] ^ (1 << (ord(s[i]) - ord('a'))))
        for left, right, k in queries:
            result.append((bin(status[right + 1] ^ status[left]).count('1') >> 1) <= k)
        return result
```
