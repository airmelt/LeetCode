__Description__:
Given a string S of lowercase letters, a duplicate removal consists of choosing two adjacent and equal letters, and removing them.

We repeatedly make duplicate removals on S until we no longer can.

Return the final string after all such duplicate removals have been made.  It is guaranteed the answer is unique.

__Example:__
Example 1:

Input: "abbaca"
Output: "ca"
Explanation: 
For example, in "abbaca" we could remove "bb" since the letters are adjacent and equal, and this is the only possible move.  The result of this move is that the string is "aaca", of which only "aa" is possible, so the final string is "ca".
 
__Note:__

1 <= S.length <= 20000
S consists only of English lowercase letters.

__题目描述__:
给出由小写字母组成的字符串 S，重复项删除操作会选择两个相邻且相同的字母，并删除它们。

在 S 上反复执行重复项删除操作，直到无法继续删除。

在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。

__示例 :__

输入："abbaca"
输出："ca"
解释：
例如，在 "abbaca" 中，我们可以删除 "bb" 由于两字母相邻且相同，这是此时唯一可以执行删除操作的重复项。之后我们得到字符串 "aaca"，其中又只有 "aa" 可以执行重复项删除操作，所以最后的字符串为 "ca"。
 
__提示：__

1 <= S.length <= 20000
S 仅由小写英文字母组成。

__思路__:
参考[LeetCode #20 Valid Parentheses 有效的括号](https://www.jianshu.com/p/6987cab23331)
将字符压入栈, 再比较, 相同的丢弃
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution {
public:
    string removeDuplicates(string S) {
        int top = 0;
        for (auto c : S)
        {
            if (!top || S[top - 1] != c) S[top++] = c;
            else top--;
        }
        S.resize(top);
        return S;
    }
};
```

__Java__:
```Java
class Solution {
    public String removeDuplicates(String S) {
        StringBuilder result = new StringBuilder();
        for (int i = 0; i < S.length(); ++i) {
            if (result.length() == 0) result.append(S.charAt(i));
            else if (result.charAt(result.length() - 1) == S.charAt(i)) result.setLength(result.length() - 1);
            else result.append(S.charAt(i));
        }
        return  result.toString();
    }
}
```

__Python__:
```Python
class Solution:
    def removeDuplicates(self, S: str) -> str:
        s = []
        for c in S:
            if s and s[-1] == c:
                s.pop()
            else:
                s.append(c)
        return ''.join(s)
```