# 467 Unique Substrings in Wraparound String 环绕字符串中唯一的子字符串

__Description__:
Consider the string s to be the infinite wraparound string of "abcdefghijklmnopqrstuvwxyz", so s will look like this: "...zabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcd....".

Now we have another string p. Your job is to find out how many unique non-empty substrings of p are present in s. In particular, your input is the string p and you need to output the number of different non-empty substrings of p in the string s.

__Note:__
p consists of only lowercase English letters and the size of p might be over 10000.

__Example:__

Example 1:
Input: "a"
Output: 1

Explanation: Only the substring "a" of string "a" is in the string s.

Example 2:
Input: "cac"
Output: 2
Explanation: There are two substrings "a", "c" of string "cac" in the string s.

Example 3:
Input: "zab"
Output: 6
Explanation: There are six substrings "z", "a", "b", "za", "ab", "zab" of string "zab" in the string s.

__题目描述__:
把字符串 s 看作是“abcdefghijklmnopqrstuvwxyz”的无限环绕字符串，所以 s 看起来是这样的："...zabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcd...."

现在我们有了另一个字符串 p 。你需要的是找出 s 中有多少个唯一的 p 的非空子串，尤其是当你的输入是字符串 p ，你需要输出字符串 s 中 p 的不同的非空子串的数目。

__注意:__
p 仅由小写的英文字母组成，p 的大小可能超过 10000。

__示例 :__

示例 1:

输入: "a"
输出: 1
解释: 字符串 S 中只有一个"a"子字符。

示例 2:

输入: "cac"
输出: 2
解释: 字符串 S 中的字符串“cac”只有两个子串“a”、“c”。

示例 3:

输入: "zab"
输出: 6
解释: 在字符串 S 中有六个子串“z”、“a”、“b”、“za”、“ab”、“zab”。

__思路__:

动态规划
统计以各字母结尾的最长连续递增子字符串的长度, 再求和
比如 abcdbcd, a -> 1, b -> 2(ab), c -> 3(abc, abcdbc不行因为不是连续递增的)
只看 abc, 有 a, b, c, ab, bc, abc共 6个刚好是前面的和
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findSubstringInWraproundString(string p) 
    {
        int n = p.size(), cur = 1;
        vector<int> count(26, 0);
        for (int i = 0; i < n; i++)
        {
            if (i and (p[i] - p[i - 1] == 1 or p[i - 1] - p[i] == 25)) ++cur;
            else cur = 1;
            count[p[i] - 'a'] = max(cur, count[p[i] - 'a']);
        }
        return accumulate(count.begin(), count.end(), 0);
    }
};
```

__Java__:

```Java
class Solution {
    public int findSubstringInWraproundString(String p) {
        int result = 0, n = p.length(), count[] = new int[26], cur = 1;
        for (int i = 0; i < n; i++) {
            if (i > 0 && (p.charAt(i) - p.charAt(i - 1) == 1 || p.charAt(i - 1) - p.charAt(i) == 25)) ++cur;
            else cur = 1;
            count[p.charAt(i) - 'a'] = Math.max(count[p.charAt(i) - 'a'], cur);
        }
        for (int c : count) result += c;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findSubstringInWraproundString(self, p: str) -> int:
        count, cur, result = [0] * 128, 1, 0
        for i in range(len(p)):
            cur = cur + 1 if i and (ord(p[i]) - ord(p[i - 1])) % 26 == 1 else 1
            count[ord(p[i])] = max(count[ord(p[i])], cur)
        return sum(count)
```
