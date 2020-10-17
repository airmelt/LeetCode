__Description__:
Given a string s, remove duplicate letters so that every letter appears once and only once. You must make sure your result is the smallest in lexicographical order among all possible results.

__Note:__
This question is the same as 1081: https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/

__Example:__
Example 1:

Input: s = "bcabc"
Output: "abc"

Example 2:

Input: s = "cbacdcbc"
Output: "acdb"
 
__Constraints:__

1 <= s.length <= 104
s consists of lowercase English letters.

__题目描述__:
给你一个字符串 s ，请你去除字符串中重复的字母，使得每个字母只出现一次。需保证 返回结果的字典序最小（要求不能打乱其他字符的相对位置）。

__注意：__
该题与 1081 https://leetcode-cn.com/problems/smallest-subsequence-of-distinct-characters 相同

__示例 :__
示例 1：

输入：s = "bcabc"
输出："abc"

示例 2：

输入：s = "cbacdcbc"
输出："acdb"
 
__提示：__

1 <= s.length <= 104
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
    string removeDuplicateLetters(string s) 
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
    public String removeDuplicateLetters(String s) {
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
    def removeDuplicateLetters(self, s: str) -> str:
        for a in sorted(set(s)):
            temp = s[s.index(a):]
            if len(set(temp)) == len(set(s)):
                return a + self.removeDuplicateLetters(temp.replace(a, ""))
        return ""
```