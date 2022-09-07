# 1297 Maximum Number of Occurrences of a Substring 子串的最大出现次数

__Description:__

Given a string s, return the maximum number of ocurrences of any substring under the following rules:

The number of unique characters in the substring must be less than or equal to maxLetters.
The substring size must be between minSize and maxSize inclusive.

__Example:__

Example 1:

Input: s = "aababcaab", maxLetters = 2, minSize = 3, maxSize = 4
Output: 2
Explanation: Substring "aab" has 2 ocurrences in the original string.
It satisfies the conditions, 2 unique letters and size 3 (between minSize and maxSize).

Example 2:

Input: s = "aaaa", maxLetters = 1, minSize = 3, maxSize = 3
Output: 2
Explanation: Substring "aaa" occur 2 times in the string. It can overlap.

__Constraints:__

1 <= s.length <= 10^5
1 <= maxLetters <= 26
1 <= minSize <= maxSize <= min(26, s.length)
s consists of only lowercase English letters.

__题目描述：__

给你一个字符串 s ，请你返回满足以下条件且出现次数最大的 任意 子串的出现次数：

子串中不同字母的数目必须小于等于 maxLetters 。
子串的长度必须大于等于 minSize 且小于等于 maxSize 。

__示例：__

示例 1：

输入：s = "aababcaab", maxLetters = 2, minSize = 3, maxSize = 4
输出：2
解释：子串 "aab" 在原字符串中出现了 2 次。
它满足所有的要求：2 个不同的字母，长度为 3 （在 minSize 和 maxSize 范围内）。

示例 2：

输入：s = "aaaa", maxLetters = 1, minSize = 3, maxSize = 3
输出：2
解释：子串 "aaa" 在原字符串中出现了 2 次，且它们有重叠部分。

示例 3：

输入：s = "aabcabcab", maxLetters = 2, minSize = 2, maxSize = 3
输出：3

示例 4：

输入：s = "abcde", maxLetters = 2, minSize = 3, maxSize = 3
输出：0

__提示：__

1 <= s.length <= 10^5
1 <= maxLetters <= 26
1 <= minSize <= maxSize <= min(26, s.length)
s 只包含小写英文字母。

__思路：__

滑动窗口
首先较短的子串一定是较长子串的一部分
也就是说较短的子串一定会被计算更多次, 比如 ‘aaba’ 和 ‘aabc’ 都包含 ‘aab’ 这个子串
所以只需要考虑最短的子串, 其长度为 minSize
初始化一个滑动窗口, 包含 s[:minSize] 中所有出现过的字符
然后进行滑动窗口, 窗口的大小固定为 minSize, 如果窗口中的字符数量小于 maxLetters
将该字符存入哈希表中
最后返回哈希表中出现最多的字符串的次数
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int maxFreq(string s, int maxLetters, int minSize, int maxSize) 
    {
        unordered_map<string, int> m;
        int result = 0, n = s.size();
        for (int i = 0; i + minSize <= n; i++)
        {
            string cur = s.substr(i, minSize);
            unordered_set<char> char_set(cur.begin(), cur.end());
            if (char_set.size() <= maxLetters) result = max(result, ++m[cur]);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxFreq(String s, int maxLetters, int minSize, int maxSize) {
        int left = 0, right = minSize - 1, n = s.length(), result = 0, exist = 0, count[] = new int[26];
        Map<String, Integer> map = new HashMap<>();
        for (int i = 0; i < minSize; i++) exist += count[s.charAt(i) - 'a']++ == 0 ? 1 : 0;
        while (right < n) {
            if (exist <= maxLetters) {
                String cur = s.substring(left, right + 1);
                map.put(cur, map.getOrDefault(cur, 0) + 1);
                result = Math.max(map.get(cur), result);
            }
            if (++right == n) break;
            exist += (count[s.charAt(right) - 'a']++ == 0 ? 1 : 0) - (count[s.charAt(left++) - 'a']-- == 1 ? 1 : 0);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxFreq(self, s: str, maxLetters: int, minSize: int, maxSize: int) -> int:
        count, n = defaultdict(int), len(s)
        for i in range(minSize, n + 1):
            if len(set(s[i - minSize:i])) <= maxLetters:
                count[s[i - minSize:i]] += 1
        return max(count.values(), default=0)
```
