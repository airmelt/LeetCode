# 1209 Remove All Adjacent Duplicates in String II 删除字符串中的所有相邻重复项 II

__Description:__

You are given a string s and an integer k, a k duplicate removal consists of choosing k adjacent and equal letters from s and removing them, causing the left and the right side of the deleted substring to concatenate together.

We repeatedly make k duplicate removals on s until we no longer can.

Return the final string after all such duplicate removals have been made. It is guaranteed that the answer is unique.

__Example:__

Example 1:

Input: s = "abcd", k = 2
Output: "abcd"
Explanation: There's nothing to delete.

Example 2:

Input: s = "deeedbbcccbdaa", k = 3
Output: "aa"
Explanation:
First delete "eee" and "ccc", get "ddbbbdaa"
Then delete "bbb", get "dddaa"
Finally delete "ddd", get "aa"

Example 3:

Input: s = "pbbcggttciiippooaais", k = 2
Output: "ps"

__Constraints:__

1 <= s.length <= 10^5
2 <= k <= 10^4
s only contains lower case English letters.

__题目描述：__

给你一个字符串 s，「k 倍重复项删除操作」将会从 s 中选择 k 个相邻且相等的字母，并删除它们，使被删去的字符串的左侧和右侧连在一起。

你需要对 s 重复进行无限次这样的删除操作，直到无法继续为止。

在执行完所有删除操作后，返回最终得到的字符串。

本题答案保证唯一。

__示例：__

示例 1：

输入：s = "abcd", k = 2
输出："abcd"
解释：没有要删除的内容。

示例 2：

输入：s = "deeedbbcccbdaa", k = 3
输出："aa"
解释：
先删除 "eee" 和 "ccc"，得到 "ddbbbdaa"
再删除 "bbb"，得到 "dddaa"
最后删除 "ddd"，得到 "aa"

示例 3：

输入：s = "pbbcggttciiippooaais", k = 2
输出："ps"

__提示：__

1 <= s.length <= 10^5
2 <= k <= 10^4
s 中只含有小写英文字母。

__思路：__

栈
用栈记录重复出现的字符以及出现的次数
当栈为空或者栈顶和当前元素不相等, 加入当前元素 1 次
否则累计出现次数
当次数达到 k 就弹出栈顶
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    string removeDuplicates(string s, int k) 
    {
        int j = 0;
        stack<int> counts;
        for (int i = 0; i < s.size(); ++i, ++j) 
        {
            s[j] = s[i];
            if (j == 0 || s[j] != s[j - 1]) counts.push(1);
            else if (++counts.top() == k) 
            {
                counts.pop();
                j -= k;
            }
        }
        return s.substr(0, j);
    }
};
```

__Java__:

```Java
class Solution {
    public String removeDuplicates(String s, int k) {
        StringBuilder sb = new StringBuilder(s);
        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < sb.length(); ++i) {
            if (i == 0 || sb.charAt(i) != sb.charAt(i - 1)) stack.push(1);
            else {
                int cur = stack.pop() + 1;
                if (cur == k) {
                    sb.delete(i - k + 1, i + 1);
                    i = i - k;
                } else stack.push(cur);
            }
        }
        return sb.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def removeDuplicates(self, s: str, k: int) -> str:
        stack = []
        for c in s:
            if not stack or stack[-1][0] != c:
                stack.append([c, 1])
            else:
                stack[-1][1] += 1
            if k == stack[-1][1]:
                stack.pop()
        return ''.join(c * n for c, n in stack)
```
