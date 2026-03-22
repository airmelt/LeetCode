# 1249 Minimum Remove to Make Valid Parentheses 移除无效的括号

__Description:__

Given a string s of '(' , ')' and lowercase English characters.

Your task is to remove the minimum number of parentheses ( '(' or ')', in any positions ) so that the resulting parentheses string is valid and return any valid string.

Formally, a parentheses string is valid if and only if:

It is the empty string, contains only lowercase characters, or
It can be written as AB (A concatenated with B), where A and B are valid strings, or
It can be written as (A), where A is a valid string.

__Example:__

Example 1:

Input: s = "lee(t(c)o)de)"
Output: "lee(t(c)o)de"
Explanation: "lee(t(co)de)" , "lee(t(c)ode)" would also be accepted.

Example 2:

Input: s = "a)b(c)d"
Output: "ab(c)d"

Example 3:

Input: s = "))(("
Output: ""
Explanation: An empty string is also valid.

__Constraints:__

1 <= s.length <= 10^5
s[i] is either'(' , ')', or lowercase English letter.

__题目描述：__

给你一个由 '('、')' 和小写字母组成的字符串 s。

你需要从字符串中删除最少数目的 '(' 或者 ')' （可以删除任意位置的括号)，使得剩下的「括号字符串」有效。

请返回任意一个合法字符串。

有效「括号字符串」应当符合以下 任意一条 要求：

空字符串或只包含小写字母的字符串
可以被写作 AB（A 连接 B）的字符串，其中 A 和 B 都是有效「括号字符串」
可以被写作 (A) 的字符串，其中 A 是一个有效的「括号字符串」

__示例：__

示例 1：

输入：s = "lee(t(c)o)de)"
输出："lee(t(c)o)de"
解释："lee(t(co)de)" , "lee(t(c)ode)" 也是一个可行答案。

示例 2：

输入：s = "a)b(c)d"
输出："ab(c)d"

示例 3：

输入：s = "))(("
输出：""
解释：空字符串也是有效的

__提示：__

1 <= s.length <= 10^5
s[i] 可能是 '('、')' 或英文小写字母

__思路：__

栈
用一个栈 left 记录出现的左括号的位置
每次遇到右括号, 如果栈非空就弹出
否则加入到哈希表中
最后栈中的元素都加入哈希表中, 哈希表中都是需要删除的不匹配括号的下标
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    string minRemoveToMakeValid(string s) 
    {
        int n = s.size();
        unordered_set<int> right;
        stack<int> left;
        for (int i = 0; i < n; i++) 
        {
            if (s[i] == '(') left.push(i);
            else if (s[i] == ')') 
            {
                if (!left.empty()) left.pop();
                else right.insert(i);
            }
        }
        while (!left.empty())
        {
            right.insert(left.top());
            left.pop();
        }
        string result = "";
        for (int i = 0; i < n; i++) if (!right.count(i)) result += s[i];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String minRemoveToMakeValid(String s) {
        int n = s.length();
        Set<Integer> right = new HashSet<>();
        Stack<Integer> left = new Stack<>();
        for (int i = 0; i < n; i++) {
            if (s.charAt(i) == '(') left.push(i);
            else if (s.charAt(i) == ')') {
                if (!left.isEmpty()) left.pop();
                else right.add(i);
            }
        }
        while (!left.isEmpty()) right.add(left.pop());
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < n; i++) if (!right.contains(i)) sb.append(s.charAt(i));
        return sb.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def minRemoveToMakeValid(self, s: str) -> str:
        right, left = set(), []
        for i, c in enumerate(s):
            if c == "(":
                left.append(i)
            elif c == ")":
                if left:
                    left.pop()
                else:
                    right.add(i)
        right |= set(left)
        return "".join([c for i, c in enumerate(s) if i not in right])
```
