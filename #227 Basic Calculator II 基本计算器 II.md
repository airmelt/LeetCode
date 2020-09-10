__Description__:
Implement a basic calculator to evaluate a simple expression string.

The expression string contains only non-negative integers, +, -, *, / operators and empty spaces . The integer division should truncate toward zero.

__Example:__
Example 1:

Input: "3+2*2"
Output: 7

Example 2:

Input: " 3/2 "
Output: 1

Example 3:

Input: " 3+5 / 2 "
Output: 5

__Note:__

You may assume that the given expression is always valid.
Do not use the eval built-in library function.

__题目描述__:
实现一个基本的计算器来计算一个简单的字符串表达式的值。

字符串表达式仅包含非负整数，+， - ，*，/ 四种运算符和空格  。 整数除法仅保留整数部分。

__示例 :__
示例 1:

输入: "3+2*2"
输出: 7

示例 2:

输入: " 3/2 "
输出: 1

示例 3:

输入: " 3+5 / 2 "
输出: 5

__说明：__

你可以假设所给定的表达式都是有效的。
请不要使用内置的库函数 eval。

__思路__:
用栈记录运算结果
可以给运算式最开始添加一个 '+'号
遇到 '+'就将数加入到栈, 遇到 ‘-’就加入相反数
遇到 '*'或 '/'就将栈顶拿出来进行运算
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int calculate(string s) 
    {
        int cur = 0, sign = (int)'+';
        vector<int> nums;
        for (int i = 0; i < s.length(); i++)
        {
            if (s[i] >= '0') cur = 10 * cur - '0' + s[i];
            if ((s[i] < '0' && s[i] != ' ') || i == s.size() - 1)
            {
                if (sign == '+') nums.push_back(cur);
                else if (sign == '-') nums.push_back(-cur);
                else if (sign == '/' || sign == '*') nums[nums.size() - 1] = sign == '*' ? nums[nums.size() - 1] * cur : nums[nums.size() - 1] / cur;
                sign = s[i];
                cur = 0;
            }
        }
        return accumulate(nums.begin(), nums.end(), 0);
    }
};
```

__Java__:
```Java
class Solution {
    public int calculate(String s) {
        int result = 0, cur = 0, sign = '+';
        List<Integer> nums = new ArrayList<>();
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) >= '0') cur = 10 * cur - '0' + s.charAt(i);
            if ((s.charAt(i) < '0' && s.charAt(i) != ' ') || i == s.length() - 1) {
                if (sign == '+') nums.add(cur);
                else if (sign == '-') nums.add(-cur);
                else if (sign == '/' || sign == '*') nums.set(nums.size() - 1, sign == '*' ? nums.get(nums.size() - 1) * cur : nums.get(nums.size() - 1) / cur);
                sign = s.charAt(i);
                cur = 0;
            }
        }
        for (int num : nums) result += num;
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def calculate(self, s: str) -> int:
        return eval(s.replace('/', '//'))
```