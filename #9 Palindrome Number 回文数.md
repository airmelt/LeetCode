# 9 Palindrome Number 回文数

__Description__:
Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.

__Example__:

Example 1:

Input: 121
Output: true

Example 2:

Input: -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.

Example 3:

Input: 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.

__Follow up__:
Coud you solve it without converting the integer to a string?

__题目描述__:
判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

__示例__:

示例 1:

输入: 121
输出: true

示例 2:

输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。

示例 3:

输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。

__进阶__:
你能不将整数转为字符串来解决这个问题吗？

__思路__:

1. 字符串反转
2. 反转一半数字, 直到反转数字大于原数字, 最后一位可以忽略
时间复杂度O(lgn), 空间复杂度O(1)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    // C++ bool型: true/false
    bool isPalindrome(int x) 
    {
        if (x < 0 or x % 10 == 0 and x != 0) return false;
        int reverse = 0;
        while (x > reverse) 
        {
            reverse = reverse * 10 + x % 10;
            x /= 10;
        }
        return x == reverse or x == reverse / 10;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isPalindrome(int x) {
        String xString = String.valueOf(x);
        String reverse = new StringBuffer(xString).reverse().toString();
        return xString.equals(reverse);
    }
}
```

__Python__:

```Python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        return str(x) == str(x)[::-1]
```
