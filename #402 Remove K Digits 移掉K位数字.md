__Description__:
Given a non-negative integer num represented as a string, remove k digits from the number so that the new number is the smallest possible.

__Note:__
The length of num is less than 10002 and will be ≥ k.
The given num does not contain any leading zero.

__Example:__
Example 1:

Input: num = "1432219", k = 3
Output: "1219"
Explanation: Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest.

Example 2:

Input: num = "10200", k = 1
Output: "200"
Explanation: Remove the leading 1 and the number is 200. Note that the output must not contain leading zeroes.

Example 3:

Input: num = "10", k = 2
Output: "0"
Explanation: Remove all the digits from the number and it is left with nothing which is 0.

__题目描述__:
给定一个以字符串表示的非负整数 num，移除这个数中的 k 位数字，使得剩下的数字最小。

__注意:__

num 的长度小于 10002 且 ≥ k。
num 不会包含任何前导零。

__示例 :__
示例 1 :

输入: num = "1432219", k = 3
输出: "1219"
解释: 移除掉三个数字 4, 3, 和 2 形成一个新的最小的数字 1219。

示例 2 :

输入: num = "10200", k = 1
输出: "200"
解释: 移掉首位的 1 剩下的数字为 200. 注意输出不能有任何前导零。

示例 3 :

输入: num = "10", k = 2
输出: "0"
解释: 从原数字移除所有的数字，剩余为空就是0。

__思路__:
最小栈
按照贪心的原则, 遇到大于前面的数字的就移除
存入栈中的是反的, 结果要反着输出
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    string removeKdigits(string num, int k) 
    {
        if (num.empty() or k == num.size()) return "0";
        stack<int> stack;
        for (auto &c : num) 
        {
            while (!stack.empty() and k and stack.top() > c - '0') 
            {
                stack.pop();
                --k;
            }
            stack.push(c - '0');
        }
        while (k-- > 0) stack.pop();
        string result;
        while (!stack.empty()) 
        {   
            result.append(to_string(stack.top()));
            stack.pop();
        }
        while (result.size() > 0 and result.back() == '0') result.erase(result.end() - 1, result.end());
        reverse(result.begin(), result.end());
        return result.empty() ? "0" : result;
    }
};
```

__Java__:
```Java
class Solution {
    public String removeKdigits(String num, int k) {
        if (num == null || num.length() == 0 || k == num.length()) return "0";
        Stack<Character> stack = new Stack<>();
        for (char c : num.toCharArray()) {
            while (!stack.isEmpty() && k != 0 && stack.peek() > c) {
                stack.pop();
                --k;
            }
            stack.push(c);
        }
        while (k-- > 0) stack.pop();
        StringBuilder result = new StringBuilder();
        while (!stack.isEmpty()) result.append(stack.pop());
        while (result.length() > 0 && result.charAt(result.length() - 1) == '0') result.setLength(result.length() - 1);
        return result.length() == 0 ? "0" : result.reverse().toString();
    }
}
```

__Python__:
```Python
class Solution:
    def removeKdigits(self, num: str, k: int) -> str:
        stack, remain = [], len(num) - k
        for digit in num:
            while k and stack and stack[-1] > digit:
                stack.pop()
                k -= 1
            stack.append(digit)
        return ''.join(stack[:remain]).lstrip('0') or '0'
```