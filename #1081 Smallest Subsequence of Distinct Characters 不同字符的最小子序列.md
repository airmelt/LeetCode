# 1081 Smallest Subsequence of Distinct Characters 不同字符的最小子序列

__Description__:
Given a string s, return the lexicographically smallest subsequence of s that contains all the distinct characters of s exactly once.

__Example:__

Example 1:

Input: s = "bcabc"
Output: "abc"

Example 2:

Input: s = "cbacdcbc"
Output: "acdb"

__Constraints:__

1 <= s.length <= 1000
s consists of lowercase English letters.

__Note:__
This question is the same as [316](https://leetcode.com/problems/remove-duplicate-letters/)

__题目描述__:
返回 s 字典序最小的子序列，该子序列包含 s 的所有不同字符，且只包含一次。

__注意：__
该题与[316](https://leetcode.com/problems/remove-duplicate-letters/)相同

__示例 :__

示例 1：

输入：s = "bcabc"
输出："abc"

示例 2：

输入：s = "cbacdcbc"
输出："acdb"

__提示:__

1 <= s.length <= 1000
s 由小写英文字母组成

__思路__:

单调栈
先统计字符串中的字符的个数
遍历字符串, 减去对应的字符
访问过的字符就跳过
如果栈中的元素比遍历的字符要大, 尝试弹出
如果之后没有该元素, 就不弹出, 否则全部弹出直到遍历元素为最小元素
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string smallestSubsequence(string s) 
    {
        vector<int> count(128);
        vector<bool> visited(128, false);
        for (auto &c : s) ++count[c];
        string result = "0";
        for (auto &c : s)
        {
            --count[c];
            if (visited[c]) continue;
            while (c < result.back() and count[result.back()])
            {
                visited[result.back()] = false;
                result.pop_back();
            }
            result += c;
            visited[c] = true;
        }
        return result.substr(1);
    }
};
```

__Java__:

```Java
class Solution {
    public String smallestSubsequence(String s) {
        Stack<Character> stack = new Stack<>();
        boolean visited[] = new boolean[128];
        int count[] = new int[128];
        for (char c : s.toCharArray()) ++count[c];
        for (char c : s.toCharArray()) {
            --count[c];
            if (visited[c]) continue;
            while (!stack.isEmpty() && stack.peek() > c) {
                if (count[stack.peek()] == 0) break;
                visited[stack.pop()] = false;
            }
            stack.push(c);
            visited[c] = true;
        }
        StringBuilder sb = new StringBuilder();
        while (!stack.isEmpty()) sb.append(stack.pop());
        return sb.reverse().toString();
    }
}
```

__Python__:

```Python
class Solution:
    def smallestSubsequence(self, s: str) -> str:
        for a in sorted(set(s)):
            temp = s[s.index(a):]
            if len(set(temp)) == len(set(s)):
                return a + self.smallestSubsequence(temp.replace(a, ""))
        return ""
```
