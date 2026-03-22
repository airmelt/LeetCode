# 2131 Longest Palindrome by Concatenating Two Letter Words 连接两字母单词得到的最长回文串

__Description:__

You are given an array of strings `words`. Each element of `words` consists of __two__ lowercase English letters.

Create the __longest possible palindrome__ by selecting some elements from `words` and concatenating them in __any order__. Each element can be selected __at most once__.

Return _the __length__ of the longest palindrome that you can create_. If it is impossible to create any palindrome, return `0`.

A palindrome is a string that reads the same forward and backward.

__Example:__

Example 1:

```text
Input: words = ["lc","cl","gg"]
Output: 6
Explanation: One longest palindrome is "lc" + "gg" + "cl" = "lcggcl", of length 6.
Note that "clgglc" is another longest palindrome that can be created.
```

Example 2:

```text
Input: words = ["ab","ty","yt","lc","cl","ab"]
Output: 8
Explanation: One longest palindrome is "ty" + "lc" + "cl" + "yt" = "tylcclyt", of length 8.
Note that "lcyttycl" is another longest palindrome that can be created.
```

Example 3:

```text
Input: words = ["cc","ll","xx"]
Output: 2
Explanation: One longest palindrome is "cc", of length 2.
Note that "ll" is another longest palindrome that can be created, and so is "xx".
```

__Constraints:__

- `1 <= words.length <= 10 ^ 5`
- `words[i].length == 2`
- `words[i]` consists of lowercase English letters.

__题目描述:__

给你一个字符串数组 `words` 。 `words` 中每个元素都是一个包含 __两个__ 小写英文字母的单词。

请你从 `words` 中选择一些元素并按 _任意顺序_ 连接它们，并得到一个 __尽可能长的回文串__ 。每个元素 __至多__ 只能使用一次。

请你返回你能得到的最长回文串的 __长度__ 。如果没办法得到任何一个回文串，请你返回 `0` 。

回文串 指的是从前往后和从后往前读一样的字符串。

__示例:__

示例 1：

```text
输入：words = ["lc","cl","gg"]
输出：6
解释：一个最长的回文串为 "lc" + "gg" + "cl" = "lcggcl" ，长度为 6 。
"clgglc" 是另一个可以得到的最长回文串。
```

示例 2：

```text
输入：words = ["ab","ty","yt","lc","cl","ab"]
输出：8
解释：最长回文串是 "ty" + "lc" + "cl" + "yt" = "tylcclyt" ，长度为 8 。
"lcyttycl" 是另一个可以得到的最长回文串。
```

示例 3：

```text
输入：words = ["cc","ll","xx"]
输出：2
解释：最长回文串是 "cc" ，长度为 2 。
"ll" 是另一个可以得到的最长回文串。"xx" 也是。
```

__提示：__

- `1 <= words.length <= 10 ^ 5`
- `words[i].length == 2`
- `words[i]` 仅包含小写英文字母。

__思路:__

```text
贪心
记录字符出现的次数, 也可以用哈希表记录每个字符串出现的次数
遍历字符串, 记录其反转的字符串 rev
如果 rev == word, 那么 word 可以放在中间, 不影响回文
且这时的 word 的个数为奇数, 那么可以放在中间, 记录 mid = true
将 word 的个数的偶数部分加入结果
否则, 只记录 word > rev 的部分, 防止重复计算
最后加上 mid << 1 的部分
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int longestPalindrome(vector<string>& words) 
    {
        unordered_map<string, int> freq;
        for (const auto& word: words) ++freq[word];
        int result = 0, mid = false;
        for (const auto& [word, v]: freq) 
        {
            string rev = string(1, word[1]) + word[0];
            if (word == rev) 
            {
                if (v & 1) mid = true;
                result += (v >> 1) << 2;
            }
            else if (word > rev) result += 4 * min(freq[word], freq[rev]);
        }
        return result + (mid << 1);
    }
};
```

__Java__:

```Java
class Solution {
    public int longestPalindrome(String[] words) {
        int add = 0, result = 0, n = words.length, count[][] = new int[26][26];
        for (int i = 0; i < n; i++) ++count[words[i].charAt(0) - 'a'][words[i].charAt(1) - 'a'];
        for (int i = 0; i < 26; i++) {
            for (int j = i; j < 26; j++) {
                if (i == j) {
                    result += (count[i][i] >> 1) << 2;
                    if ((count[i][i] & 1) == 1) add = 2;
                } else result += Math.min(count[i][j], count[j][i]) << 2;
            }
        }
        return result + add;
    }
}
```

__Python__:

```Python
class Solution:
    def longestPalindrome(self, words: List[str]) -> int:
        freq, result, mid = Counter(words), 0, False
        for word, v in freq.items():
            rev = word[::-1]
            if word == rev:
                if v & 1:
                    mid = True
                result += v >> 1 << 2
            elif word > rev:
                result += min(freq[word], freq[rev]) << 2
        return result + (mid << 1)
```
