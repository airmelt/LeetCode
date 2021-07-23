# 767 Reorganize String 重构字符串

__Description__:
Given a string s, rearrange the characters of s so that any two adjacent characters are not the same.

Return any possible rearrangement of s or return "" if not possible.

__Example:__

Example 1:

Input: s = "aab"
Output: "aba"

Example 2:

Input: s = "aaab"
Output: ""

__Constraints:__

1 <= s.length <= 500
s consists of lowercase English letters.

__题目描述__:
给定一个字符串S，检查是否能重新排布其中的字母，使得两相邻的字符不同。

若可行，输出任意可行的结果。若不可行，返回空字符串。

__示例 :__

示例 1:

输入: S = "aab"
输出: "aba"

示例 2:

输入: S = "aaab"
输出: ""

__注意:__

S 只包含小写字母并且长度在[1, 500]区间内。

__思路__:

贪心
按照出现次数将出现次数多的放在偶数位置, 出现次数少的放在奇数位置
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string reorganizeString(string s) 
    {
        if (s.size() < 2) return s;
        int n = s.size(), even = 0, odd = 1, half = (n >> 1);
        vector<int> count(26, 0);
        string result(n, '\0');
        for (const auto& c : s) ++count[c - 'a'];
        if (*max_element(count.begin(), count.end()) > ((n + 1) >> 1)) return "";
        for (int i = 0; i < 26; i++) 
        {
            char c = (char) ('a' + i);
            while (count[i] > 0 and count[i] <= half and odd < n) 
            {
                result[odd] = c;
                --count[i];
                odd += 2;
            }
            while (count[i] > 0) 
            {
                result[even] = c;
                --count[i];
                even += 2;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String reorganizeString(String s) {
        if (s.length() < 2) return s;
        int n = s.length(), maxCount = 0, count[] = new int[26], even = 0, odd = 1, half = (n >> 1);
        for (char c : s.toCharArray()) {
            ++count[c - 'a'];
            maxCount = Math.max(maxCount, count[c - 'a']);
        }
        if (maxCount > ((n + 1) >> 1)) return "";
        char[] result = new char[n];
        for (int i = 0; i < 26; i++) {
            char c = (char) ('a' + i);
            while (count[i] > 0 && count[i] <= half && odd < n) {
                result[odd] = c;
                --count[i];
                odd += 2;
            }
            while (count[i] > 0) {
                result[even] = c;
                --count[i];
                even += 2;
            }
        }
        return new String(result);
    }
}
```

__Python__:

```Python
class Solution:
    def reorganizeString(self, s: str) -> str:
        return '' if (l := sorted(sorted(s), key=lambda x: s.count(x), reverse=True))[0] == l[(n := (len(s) + 1) >> 1)] else ''.join(j for i in zip(l, l[n:]) for j in i) + ['', l[n - 1]][len(s) & 1]
```
