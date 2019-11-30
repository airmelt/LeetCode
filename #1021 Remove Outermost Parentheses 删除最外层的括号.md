__Description__:
A valid parentheses string is either empty (""), "(" + A + ")", or A + B, where A and B are valid parentheses strings, and + represents string concatenation.  For example, "", "()", "(())()", and "(()(()))" are all valid parentheses strings.

A valid parentheses string S is primitive if it is nonempty, and there does not exist a way to split it into S = A+B, with A and B nonempty valid parentheses strings.

Given a valid parentheses string S, consider its primitive decomposition: S = P_1 + P_2 + ... + P_k, where P_i are primitive valid parentheses strings.

Return S after removing the outermost parentheses of every primitive string in the primitive decomposition of S.

__Example:__
Example 1:

Input: "(()())(())"
Output: "()()()"
Explanation: 
The input string is "(()())(())", with primitive decomposition "(()())" + "(())".
After removing outer parentheses of each part, this is "()()" + "()" = "()()()".

Example 2:

Input: "(()())(())(()(()))"
Output: "()()()()(())"
Explanation: 
The input string is "(()())(())(()(()))", with primitive decomposition "(()())" + "(())" + "(()(()))".
After removing outer parentheses of each part, this is "()()" + "()" + "()(())" = "()()()()(())".

Example 3:

Input: "()()"
Output: ""
Explanation: 
The input string is "()()", with primitive decomposition "()" + "()".
After removing outer parentheses of each part, this is "" + "" = "".
 
__Note:__

S.length <= 10000
S[i] is "(" or ")"
S is a valid parentheses string

__题目描述__:
有效括号字符串为空 ("")、"(" + A + ")" 或 A + B，其中 A 和 B 都是有效的括号字符串，+ 代表字符串的连接。例如，""，"()"，"(())()" 和 "(()(()))" 都是有效的括号字符串。

如果有效字符串 S 非空，且不存在将其拆分为 S = A+B 的方法，我们称其为原语（primitive），其中 A 和 B 都是非空有效括号字符串。

给出一个非空有效字符串 S，考虑将其进行原语化分解，使得：S = P_1 + P_2 + ... + P_k，其中 P_i 是有效括号字符串原语。

对 S 进行原语化分解，删除分解中每个原语字符串的最外层括号，返回 S 。

__示例 :__
示例 1：

输入："(()())(())"
输出："()()()"
解释：
输入字符串为 "(()())(())"，原语化分解得到 "(()())" + "(())"，
删除每个部分中的最外层括号后得到 "()()" + "()" = "()()()"。

示例 2：

输入："(()())(())(()(()))"
输出："()()()()(())"
解释：
输入字符串为 "(()())(())(()(()))"，原语化分解得到 "(()())" + "(())" + "(()(()))"，
删除每隔部分中的最外层括号后得到 "()()" + "()" + "()(())" = "()()()()(())"。

示例 3：

输入："()()"
输出：""
解释：
输入字符串为 "()()"，原语化分解得到 "()" + "()"，
删除每个部分中的最外层括号后得到 "" + "" = ""。
 
__提示：__

S.length <= 10000
S[i] 为 "(" 或 ")"
S 是一个有效括号字符串

__思路__:
由于 S一定是一个递归匹配的括号字符串, 如果给左括号赋值 +1, 右括号赋值 -1, 对每一个匹配的原语最后的和一定是 0
那么最外层的括号一定满足: 左括号, count == 0; 右括号, count == 1
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    string removeOuterParentheses(string S) 
    {
        string result = "";
        int count = 0;
        for(auto c : S)
        {
            if (c == ')') --count;
            if (count > 0) result += c;
            if (c == '(') ++count;
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public String removeOuterParentheses(String S) {
        StringBuilder result = new StringBuilder();
        int count = 0;
        for(char c : S.toCharArray()) {
            if (c == ')') --count;
            if (count > 0) result.append(c);
            if (c == '(') ++count;
        }
        return result.toString();
    }
}
```

__Python__:
```Python
class Solution:
    def removeOuterParentheses(self, S: str) -> str:
        count, result = 0, ""
        for s in S:
            if s == '(':
                count += 1
                if count == 1:
                    continue
            else:
                count -= 1
                if not count:
                    continue
            result += s
        return result
```