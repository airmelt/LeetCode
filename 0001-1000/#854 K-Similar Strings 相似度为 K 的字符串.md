# 854 K-Similar Strings 相似度为 K 的字符串

__Description__:
Strings s1 and s2 are k-similar (for some non-negative integer k) if we can swap the positions of two letters in s1 exactly k times so that the resulting string equals s2.

Given two anagrams s1 and s2, return the smallest k for which s1 and s2 are k-similar.

__Example:__

Example 1:

Input: s1 = "ab", s2 = "ba"
Output: 1

Example 2:

Input: s1 = "abc", s2 = "bca"
Output: 2

Example 3:

Input: s1 = "abac", s2 = "baca"
Output: 2

Example 4:

Input: s1 = "aabc", s2 = "abca"
Output: 2

__Constraints:__

1 <= s1.length <= 20
s2.length == s1.length
s1 and s2 contain only lowercase letters from the set {'a', 'b', 'c', 'd', 'e', 'f'}.
s2 is an anagram of s1.

__题目描述__:
如果可以通过将 A 中的两个小写字母精确地交换位置 K 次得到与 B 相等的字符串，我们称字符串 A 和 B 的相似度为 K（K 为非负整数）。

给定两个字母异位词 A 和 B ，返回 A 和 B 的相似度 K 的最小值。

__示例 :__

示例 1：

输入：A = "ab", B = "ba"
输出：1

示例 2：

输入：A = "abc", B = "bca"
输出：2

示例 3：

输入：A = "abac", B = "baca"
输出：2

示例 4：

输入：A = "aabc", B = "abca"
输出：2

__提示:__

1 <= A.length == B.length <= 20
A 和 B 只包含集合 {'a', 'b', 'c', 'd', 'e', 'f'} 中的小写字母。

__思路__:

状态压缩 ➕ BFS
因为只有 6 个字符, 每个字符用 3 位二进制表示, 最多需要 60 位, 可以用一个 Long 存储状态
用 BFS 搜索出最小交换次数即可, 注意这里要将相同的字符的交换剪枝, 否则 TLE
时间复杂度为 O(nlgn), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int kSimilarity(string s1, string s2) 
    {
        int n = s1.size();
        queue<string> q;
        q.push(s1);
        unordered_set<long> visited;
        visited.insert(state(s1));
        for (int result = 0, size = q.size(); ; result++, size = q.size()) 
        {
            while (size-- > 0) 
            {
                string cur = q.front();
                q.pop();
                int i = 0;
                while (i < n and cur[i] == s2[i]) ++i;
                if (i == n) return result;
                for (int j = i + 1; j < n; j++) 
                {
                    if (cur[j] == s2[i] and cur[j] != s2[j]) 
                    {
                        string next(cur);
                        next[j] = next[i];
                        next[i] = s2[i];
                        if (!visited.count(state(next))) q.push(next);
                    }
                }
            }
        }
    }
private:
    long state(const string& s) 
    {
        long result = 0;
        for (const auto& c : s) 
        {
            result <<= 3;
            result |= c - 'a';
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int kSimilarity(String s1, String s2) {
        char[] source = s1.toCharArray(), target = s2.toCharArray();
        int n = s1.length();
        Queue<char[]> queue = new ArrayDeque<>();
        queue.offer(source);
        Set<Long> visited = new HashSet<>();
        visited.add(state(source));
        for (int result = 0, size = queue.size(); ; result++, size = queue.size()) {
            while (size-- > 0) {
                char[] cur = queue.poll();
                int i = 0;
                while (i < n && cur[i] == target[i]) ++i;
                if (i == n) return result;
                for (int j = i + 1; j < n; j++) {
                    if (cur[j] == target[i]) {
                        char[] next = cur.clone();
                        next[j] = next[i];
                        next[i] = target[i];
                        if (visited.add(state(next))) queue.offer(next);
                    }
                }
            }
        }
    }
    
    private long state(char[] source) {
        long result = 0;
        for (char c : source) {
            result <<= 3;
            result |= c - 'a';
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def kSimilarity(self, s1: str, s2: str) -> int:
        def getSwap(source: str) -> str:
            for i, c in enumerate(source):
                if c != s2[i]: 
                    break
            cur = list(source)
            for j in range(i + 1, n):
                if source[j] == s2[i]:
                    cur[i], cur[j] = cur[j], cur[i]
                    yield ''.join(cur)
                    cur[j], cur[i] = cur[i], cur[j]
        queue, count, n = [s1], { s1: 0 }, len(s1)
        while queue:
            source = queue.pop(0)
            if source == s2: 
                return count[source]
            for target in getSwap(source):
                if target not in count:
                    count[target] = count[source] + 1
                    queue.append(target)
```
