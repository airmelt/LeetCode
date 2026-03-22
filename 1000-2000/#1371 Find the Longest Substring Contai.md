# 1371 Find the Longest Substring Containing Vowels in Even Counts 每个元音包含偶数次的最长子字符串

__Description:__

Given the string s, return the size of the longest substring containing each vowel an even number of times. That is, 'a', 'e', 'i', 'o', and 'u' must appear an even number of times.

__Example:__

Example 1:

Input: s = "eleetminicoworoep"
Output: 13
Explanation: The longest substring is "leetminicowor" which contains two each of the vowels: e, i and o and zero of the vowels: a and u.

Example 2:

Input: s = "leetcodeisgreat"
Output: 5
Explanation: The longest substring is "leetc" which contains two e's.

Example 3:

Input: s = "bcbcbc"
Output: 6
Explanation: In this case, the given string "bcbcbc" is the longest because all vowels: a, e, i, o and u appear zero times.

__Constraints:__

1 <= s.length <= 5 x 10^5
s contains only lowercase English letters.

__题目描述：__
给你一个字符串 s ，请你返回满足以下条件的最长子字符串的长度：每个元音字母，即 'a'，'e'，'i'，'o'，'u' ，在子字符串中都恰好出现了偶数次。

__示例：__

示例 1：

输入：s = "eleetminicoworoep"
输出：13
解释：最长子字符串是 "leetminicowor" ，它包含 e，i，o 各 2 个，以及 0 个 a，u 。

示例 2：

输入：s = "leetcodeisgreat"
输出：5
解释：最长子字符串是 "leetc" ，其中包含 2 个 e 。

示例 3：

输入：s = "bcbcbc"
输出：6
解释：这个示例中，字符串 "bcbcbc" 本身就是最长的，因为所有的元音 a，e，i，o，u 都出现了 0 次。

__提示：__

1 <= s.length <= 5 x 10^5
s 只包含小写英文字母。

__思路：__

状态压缩
分别用 0x00000 的各位记录下 'aeiou' 的出现次数
使用异或运算保证出现偶数次就为 0
找到 state 出现过的位置更新最大值
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int findTheLongestSubstring(string s) 
    {
        int state = 0, result = 0, n = s.size();
        unordered_map<int, int> m;
        m[state] = -1;
        for (int i = 0; i < n; i++) 
        {
            state ^= (s[i] == 'a') ^ ((s[i] == 'e') << 1) ^ ((s[i] == 'i') << 2) ^ ((s[i] == 'o') << 3) ^ ((s[i] == 'u') << 4);
            if (m.count(state)) result = max(result, i - m[state]);
            else m[state] = i;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findTheLongestSubstring(String s) {
        int state = 0, result = 0, n = s.length();
        Map<Integer, Integer> map = new HashMap<>();
        map.put(state, -1);
        for (int i = 0; i < n; i++) {
            state ^= (s.charAt(i) == 'a' ? 1 : 0) ^ (s.charAt(i) == 'e' ? 2 : 0) ^ (s.charAt(i) == 'i' ? 4 : 0) ^ (s.charAt(i) == 'o' ? 8 : 0) ^ (s.charAt(i) == 'u' ? 16 : 0);
            if (map.containsKey(state)) result = Math.max(result, i - map.get(state));
            else map.put(state, i);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findTheLongestSubstring(self, s: str) -> int:
        result, d, n = 0, {(state := 0): -1}, len(s)
        for i in range(n):
            state ^= (s[i] == 'a') ^ ((s[i] == 'e') << 1) ^ ((s[i] == 'i') << 2) ^ ((s[i] == 'o') << 3) ^ ((s[i] == 'u') << 4)
            if state in d:
                result = max(result, i - d[state])
            else:
                d[state] = i
        return result
```
