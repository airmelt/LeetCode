# 1044 Longest Duplicate Substring 最长重复子串

__Description__:
Given a string s, consider all duplicated substrings: (contiguous) substrings of s that occur 2 or more times. The occurrences may overlap.

Return any duplicated substring that has the longest possible length. If s does not have a duplicated substring, the answer is "".

__Example:__

Example 1:

Input: s = "banana"
Output: "ana"

Example 2:

Input: s = "abcd"
Output: ""

__Constraints:__

2 <= s.length <= 3 * 10^4
s consists of lowercase English letters.

__题目描述__:
给你一个字符串 s ，考虑其所有 重复子串 ：即 s 的（连续）子串，在 s 中出现 2 次或更多次。这些出现之间可能存在重叠。

返回 任意一个 可能具有最长长度的重复子串。如果 s 不含重复子串，那么答案为 "" 。

__示例 :__

示例 1：

输入：s = "banana"
输出："ana"

示例 2：

输入：s = "abcd"
输出：""

__提示:__

2 <= s.length <= 3 * 10^4
s 由小写英文字母组成

__思路__:

1. 暴力法
对每一个字串在后续不重合的区域找自身
保留最长的字串
时间复杂度O(n ^ 3), 空间复杂度O(1)
2. 二分
对查询字串, 可以用二分查找长度加快查找速度
3. 二分 ➕ 哈希
记录已经查找过的字串
哈希可以采用乘上一个质数来区分
时间复杂度O(nlgn), 空间复杂度O(n)
4. 后缀数组
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string longestDupSubstring(string s) 
    {
        int n = s.size(), l = 0, r = n;
        h.resize(n + 1); 
        p.resize(n + 1);
        p.front() = 1;
        for (int i = 0; i < n; i++) 
        {
            p[i + 1] = p[i] * P;
            h[i + 1] = h[i] * P + s[i];
        }
        string result = "";
        while (l < r) 
        {
            int mid = (l + r + 1) >> 1;
            string t = check(s, mid);
            if (t.size() != 0) l = mid;
            else r = mid - 1;
            result = t.size() > result.size() ? t : result;
        }
        return result;
    }
private:
    vector<unsigned long long> h, p;
    const int P = 37;
    
    string check(string& s, int len) 
    {
        set<unsigned long long> char_set;
        for (int i = 1, n = s.size(); i + len - 1 <= n; i++) 
        {
            int j = i + len - 1;
            unsigned long long cur = (h[j] - h[i - 1] * p[j - i + 1]);
            if (char_set.count(cur)) return s.substr(i - 1, j - i + 1);
            char_set.insert(cur);
        }
        return "";
    }
};
```

__Java__:

```Java
class Solution {
    private long[] h, p;
    private static int P = 37;
    
    public String longestDupSubstring(String s) {
        int n = s.length(), l = 0, r = n;
        h = new long[n + 1]; p = new long[n + 1];
        p[0] = 1;
        for (int i = 0; i < n; i++) {
            p[i + 1] = p[i] * P;
            h[i + 1] = h[i] * P + s.charAt(i);
        }
        String result = "";
        while (l < r) {
            int mid = l + r + 1 >> 1;
            String t = check(s, mid);
            if (t.length() != 0) l = mid;
            else r = mid - 1;
            result = t.length() > result.length() ? t : result;
        }
        return result;
    }
    
    private String check(String s, int len) {
        Set<Long> set = new HashSet<>();
        for (int i = 1, n = s.length(); i + len - 1 <= n; i++) {
            int j = i + len - 1;
            long cur = h[j] - h[i - 1] * p[j - i + 1];
            if (set.contains(cur)) return s.substring(i - 1, j);
            set.add(cur);
        }
        return "";
    }
}
```

__Python__:

```Python
class Solution:
    def longestDupSubstring(self, s: str) -> str:
        MOD, P, l, r, result = int(1e18 + 7), 37, 0, len(s), ''
        
        def check(l: int) -> tuple:
            hash_code, seen, x = 0, set(), P ** l % MOD
            for i, char in enumerate(s):
                if (hash_code := (hash_code * P + ord(s[i])) % MOD if i < l else (hash_code * P + ord(s[i]) - ord(s[i - l]) * x) % MOD) in seen:
                    return s[i - l + 1: i + 1]
                if i >= l - 1:
                    seen.add(hash_code)
            return ''
        while l < r:
            mid = (l + r + 1) >> 1
            if t := check(mid):
                l = mid
                result = t
            else:
                r = mid - 1
        return result
```
