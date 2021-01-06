# 394 Decode String 字符串解码

__Description__:
Given an encoded string, return its decoded string.

The encoding rule is: k[encoded_string], where the encoded_string inside the square brackets is being repeated exactly k times. Note that k is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, k. For example, there won't be input like 3a or 2[4].

__Example:__

Example 1:

Input: s = "3[a]2[bc]"
Output: "aaabcbc"

Example 2:

Input: s = "3[a2[c]]"
Output: "accaccacc"

Example 3:

Input: s = "2[abc]3[cd]ef"
Output: "abcabccdcdcdef"

Example 4:

Input: s = "abc3[cd]xyz"
Output: "abccdcdcdxyz"

__Constraints:__

1 <= s.length <= 30
s consists of lowercase English letters, digits, and square brackets '[]'.
s is guaranteed to be a valid input.
All the integers in s are in the range [1, 300].

__题目描述__:
给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: k[encoded_string]，表示其中方括号内部的 encoded_string 正好重复 k 次。注意 k 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 k ，例如不会出现像 3a 或 2[4] 的输入。

__示例 :__

示例 1：

输入：s = "3[a]2[bc]"
输出："aaabcbc"

示例 2：

输入：s = "3[a2[c]]"
输出："accaccacc"

示例 3：

输入：s = "2[abc]3[cd]ef"
输出："abcabccdcdcdef"

示例 4：

输入：s = "abc3[cd]xyz"
输出："abccdcdcdxyz"

__思路__:

用两个栈记录数字和目前的字符串
每次碰到 '[', 就把当前的字符串加入栈, 当前数字也加入栈
每次碰到 ']', 将字符串与之前的相加并更新, 弹出两个栈的栈顶
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string decodeString(string s) 
    {
        string result = "";
        stack<int> nums;
        stack<string> strs;
        int num = 0;
        for (auto &c : s)
        {
            if (isdigit(c)) num = num * 10 + c - '0';
            else if (isalpha(c)) result += c;
            else if (c == '[')
            {
                nums.push(num);
                num = 0;
                strs.push(result); 
                result = "";
            }
            else if (c == ']')
            {
                for (int j = 0; j < nums.top(); j++) strs.top() += result;
                result = strs.top();
                nums.pop();
                strs.pop();
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String decodeString(String s) {
        StringBuilder result = new StringBuilder();
        Stack<Integer> nums = new Stack();
        Stack<StringBuilder> strs = new Stack();
        int num = 0;
        for (Character c : s.toCharArray()) {
            if (Character.isDigit(c)) num = num * 10 + c - '0';
            else if (Character.isLetter(c)) result.append(c);
            else if (c == '[') {
                nums.push(num);
                num = 0;
                strs.push(result);
                result = new StringBuilder();
            } else if (c == ']') {
                for (int j = 0, times = nums.pop(); j < times; j++) strs.peek().append(result);
                result = strs.pop();
            }
        }
        return result.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def decodeString(self, s: str) -> str:
        while '[' in s:
            s = re.sub(r'(\d+)\[([A-Za-z]*)\]', lambda m: int(m.group(1))* m.group(2), s)
        return s
```
