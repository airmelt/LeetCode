# 1071 Greatest Common Divisor of Strings 字符串的最大公因子

__Description__:
For strings S and T, we say "T divides S" if and only if S = T + ... + T  (T concatenated with itself 1 or more times)

Return the largest string X such that X divides str1 and X divides str2.

__Example:__

Example 1:

Input: str1 = "ABCABC", str2 = "ABC"
Output: "ABC"

Example 2:

Input: str1 = "ABABAB", str2 = "ABAB"
Output: "AB"

Example 3:

Input: str1 = "LEET", str2 = "CODE"
Output: ""

__Note:__

1 <= str1.length <= 1000
1 <= str2.length <= 1000
str1[i] and str2[i] are English uppercase letters.

__题目描述__:
对于字符串 S 和 T，只有在 S = T + ... + T（T 与自身连接 1 次或多次）时，我们才认定 “T 能除尽 S”。

返回字符串 X，要求满足 X 能除尽 str1 且 X 能除尽 str2。

__示例 :__

示例 1：

输入：str1 = "ABCABC", str2 = "ABC"
输出："ABC"
示例 2：

输入：str1 = "ABABAB", str2 = "ABAB"
输出："AB"
示例 3：

输入：str1 = "LEET", str2 = "CODE"
输出：""

__提示：__

1 <= str1.length <= 1000
1 <= str2.length <= 1000
str1[i] 和 str2[i] 为大写英文字母

__思路__:

辗转相除法, 如果两个字符串有公因子, 那么 A + B一定等于 B + A
再利用辗转相除法找到两个字符串的长度的公因子即可
时间复杂度O(lgn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string gcdOfStrings(string str1, string str2) 
    {
        return str1 + str2 == str2 + str1 ? str1.substr(0, gcd(str1.size(), str2.size())) : "";
    }
private:
    inline int gcd(int a, int b) { return b ? gcd(b, a % b) : a; }
};
```

__Java__:

```Java
class Solution {
    public String gcdOfStrings(String str1, String str2) {
        return (str1 + str2).equals(str2 + str1) ? str1.substring(0, gcd(str1.length(), str2.length())) : "";
    }
    
    private int gcd(int a, int b) { 
        return b == 0 ? a : gcd(b, a % b); 
    }
}
```

__Python__:

```Python
class Solution:
    def gcdOfStrings(self, str1: str, str2: str) -> str:
        def gcd(a: int, b: int) -> int:
            return a if b == 0 else gcd(b, a % b)
        return str1[:gcd(len(str1), len(str2))] if str1 + str2 == str2 + str1 else ""
```
