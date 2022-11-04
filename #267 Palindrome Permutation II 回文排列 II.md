# 267 Palindrome Permutation II 回文排列 II

__Description:__

Given a string s, return all the palindromic permutations (without duplicates) of it.

You may return the answer in any order. If s has no palindromic permutation, return an empty list.

__Example:__

Example 1:

Input: s = "aabb"
Output: ["abba","baab"]

Example 2:

Input: s = "abc"
Output: []

__Constraints:__

1 <= s.length <= 16
s consists of only lowercase English letters.

__题目描述：__

给定一个字符串 s ，返回 其重新排列组合后可能构成的所有回文字符串，并去除重复的组合 。

你可以按 任意顺序 返回答案。如果 s 不能形成任何回文排列时，则返回一个空列表。

__示例：__

示例 1：

输入: s = "aabb"
输出: ["abba", "baab"]

示例 2：

输入: s = "abc"
输出: []

__提示：__

1 <= s.length <= 16
s 仅由小写英文字母组成

__思路：__

回溯
先统计 s 的字符及数目
如果 s 中字符单个字符数超过 1 就返回空列表
只需要全排列 s 中的一半字符, 剩下的一半用倒序生成即可
时间复杂度为 O(n!), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    vector<string> generatePalindromes(string s) 
    {
        unordered_map<char, int> count;
        vector<string> result;
        string even_s = "", odd_s = "";
        for (const auto& c : s) ++count[c];
        for (const auto& [c, v] : count) 
        {
            even_s += string(v >> 1, c);
            odd_s += string(v & 1, c);
        }
        if (odd_s.size() > 1) return result;
        string pre = even_s, rev_s;
        do 
        {
            rev_s = even_s;
            reverse(rev_s.begin(), rev_s.end());
            result.push_back(even_s + odd_s + rev_s);
            next_permutation(even_s.begin(), even_s.end());
        } 
        while (even_s != pre);
        return result;
    }
};
```

__Java__:

```Java
public class Solution {
    private Set<String> set = new HashSet<>();
    
    public List<String> generatePalindromes(String s) {
        int n = s.length(), k = 0, map[] = new int[128];
        char ch = 0, st[] = new char[n >> 1];
        if (!canPermutePalindrome(s, map)) return new ArrayList<>();
        for (int i = 0; i < 128; i++) if ((map[i] & 1) == 1) ch = (char)i;
        for (int i = 0; i < 128; i++) for (int j = 0; j < (map[i] >> 1); j++) st[k++] = (char)i;
        permute(st, 0, ch);
        return new ArrayList<String>(set);
    }
    
    private boolean canPermutePalindrome(String s, int[] map) {
        int count = 0, n = s.length();
        for (int i = 0; i < n; i++) {
            ++map[s.charAt(i)];
            if ((map[s.charAt(i)] & 1) == 0) --count;
            else ++count;
        }
        return count <= 1;
    }
    
    private void swap(char[] s, int i, int j) {
        char temp = s[i];
        s[i] = s[j];
        s[j] = temp;
    }
    
    private void permute(char[] s, int l, char ch) {
        if (l == s.length) {
            set.add(new String(s) + (ch == 0 ? "" : ch) + new StringBuilder(new String(s)).reverse());
        } else {
            for (int i = l; i < s.length; i++) {
                if (s[l] != s[i] || l == i) {
                    swap(s, l, i);
                    permute(s, l + 1, ch);
                    swap(s, l, i);
                }
            }
        }
    }
}
```

__Python__:

```Python
class Solution:
    def generatePalindromes(self, s: str) -> List[str]:
        count, mid, pre_tail = Counter(s), '', ''
        for val, freq in count.items():
            pre_tail += val * (freq >> 1)
            mid += val * (freq & 1)
        return [''.join(half) + mid + ''.join(half)[::-1] for half in set(permutations(pre_tail))] if len(mid) <= 1 else []
```
