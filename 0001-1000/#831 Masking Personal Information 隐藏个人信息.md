# 831 Masking Personal Information 隐藏个人信息

__Description__:
You are given a personal information string s, representing either an email address or a phone number. Return the masked personal information using the below rules.

Email address:

An email address is:

A name consisting of uppercase and lowercase English letters, followed by
The '@' symbol, followed by
The domain consisting of uppercase and lowercase English letters with a dot '.' somewhere in the middle (not the first or last character).
To mask an email:

The uppercase letters in the name and domain must be converted to lowercase letters.
The middle letters of the name (i.e., all but the first and last letters) must be replaced by 5 asterisks "*****".
Phone number:

A phone number is formatted as follows:

The phone number contains 10-13 digits.
The last 10 digits make up the local number.
The remaining 0-3 digits, in the beginning, make up the country code.
Separation characters from the set {'+', '-', '(', ')', ' '} separate the above digits in some way.
To mask a phone number:

Remove all separation characters.
The masked phone number should have the form:
"***-***-XXXX" if the country code has 0 digits.
"+*-***-***-XXXX" if the country code has 1 digit.
"+**-***-***-XXXX" if the country code has 2 digits.
"+***-***-***-XXXX" if the country code has 3 digits.
"XXXX" is the last 4 digits of the local number.

__Example:__

Example 1:

Input: s = "LeetCode@LeetCode.com"
Output: "l*****e@leetcode.com"
Explanation: s is an email address.
The name and domain are converted to lowercase, and the middle of the name is replaced by 5 asterisks.

Example 2:

Input: s = "AB@qq.com"
Output: "a*****b@qq.com"
Explanation: s is an email address.
The name and domain are converted to lowercase, and the middle of the name is replaced by 5 asterisks.
Note that even though "ab" is 2 characters, it still must have 5 asterisks in the middle.

Example 3:

Input: s = "1(234)567-890"
Output: "***-***-7890"
Explanation: s is a phone number.
There are 10 digits, so the local number is 10 digits and the country code is 0 digits.
Thus, the resulting masked number is "***-***-7890".

Example 4:

Input: s = "86-(10)12345678"
Output: "+**-***-***-5678"
Explanation: s is a phone number.
There are 12 digits, so the local number is 10 digits and the country code is 2 digits.
Thus, the resulting masked number is "+**-***-***-7890".

__Constraints:__

s is either a valid email or a phone number.
If s is an email:
8 <= s.length <= 40
s consists of uppercase and lowercase English letters and exactly one '@' symbol and '.' symbol.
If s is a phone number:
10 <= s.length <= 20
s consists of digits, spaces, and the symbols '(', ')', '-', and '+'.

__题目描述__:
给你一条个人信息字符串 S，它可能是一个 邮箱地址 ，也可能是一串 电话号码 。

我们将隐藏它的隐私信息，通过如下规则:

电子邮箱:

定义名称 name 是长度大于等于 2 （length ≥ 2），并且只包含小写字母 a-z 和大写字母 A-Z 的字符串。

电子邮箱地址由名称 name 开头，紧接着是符号 '@'，后面接着一个名称 name，再接着一个点号 '.'，然后是一个名称 name。

电子邮箱地址确定为有效的，并且格式是 "name1@name2.name3"。

为了隐藏电子邮箱，所有的名称 name 必须被转换成小写的，并且第一个名称 name 的第一个字母和最后一个字母的中间的所有字母由 5 个 '*' 代替。

电话号码:

电话号码是一串包括数字 0-9，以及 {'+', '-', '(', ')', ' '} 这几个字符的字符串。你可以假设电话号码包含 10 到 13 个数字。

电话号码的最后 10 个数字组成本地号码，在这之前的数字组成国际号码。注意，国际号码是可选的。我们只暴露最后 4 个数字并隐藏所有其他数字。

本地号码是有格式的，并且如 "***-***-1111" 这样显示，这里的 1 表示暴露的数字。

为了隐藏有国际号码的电话号码，像 "+111 111 111 1111"，我们以 "+***-***-***-1111" 的格式来显示。在本地号码前面的 '+' 号和第一个 '-' 号仅当电话号码中包含国际号码时存在。例如，一个 12 位的电话号码应当以 "+**-" 开头进行显示。

注意：像 "("，")"，" " 这样的不相干的字符以及不符合上述格式的额外的减号或者加号都应当被删除。

最后，将提供的信息正确隐藏后返回。

__示例 :__

示例 1：

输入: "LeetCode@LeetCode.com"
输出: "l*****e@leetcode.com"
解释：
所有的名称转换成小写, 第一个名称的第一个字符和最后一个字符中间由 5 个星号代替。
因此，"leetcode" -> "l*****e"。

示例 2：

输入: "AB@qq.com"
输出: "a*****b@qq.com"
解释:
第一个名称"ab"的第一个字符和最后一个字符的中间必须有 5 个星号
因此，"ab" -> "a*****b"。

示例 3：

输入: "1(234)567-890"
输出: "***-***-7890"
解释:
10 个数字的电话号码，那意味着所有的数字都是本地号码。

示例 4：

输入: "86-(10)12345678"
输出: "+**-***-***-5678"
解释:
12 位数字，2 个数字是国际号码另外 10 个数字是本地号码 。

__注意:__

S.length <= 40。
邮箱的长度至少是 8。
电话号码的长度至少是 10。

__思路__:

模拟
按照 '@' 是否在 s 中可以分成邮箱或电话号码两类
如果 '@' 在 s 中, 则 s 是邮箱, 将 s 在 '@' 之前的子串中的第一个字符和最后一个字符取出, 和加密字符拼接在一起, 再加上 '@' 及之后的子串, 并将结果小写化
否则 s 是电话号码, 找到所有在 s 中的数字, 如果数字总长 n 超过 10 则是国际电话需要在最前面加上 '+' + '\*' \* (n - 10), 然后拼接上加密字符和 s 最后 4 位数字
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string maskPII(string s) 
    {
        string result = "";
        int i = s.find('@'), n = s.size();
        if (i != -1)
        {
            result += tolower(s[0]);
            result += "*****";
            for (int j = i - 1; j < n; j++) result += tolower(s[j]);
        }
        else 
        {
            string num = "";
            for (int j = 0; j < n; j++) if (isdigit(s[j])) num += s[j];
            if ((n = num.size()) > 10) result += '+' + string(n - 10, '*') + '-';
            result += "***-***-" + num.substr(n - 4);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String maskPII(String s) {
        int i = s.indexOf('@');
        if (i != -1) return (s.substring(0, 1) + "*****" + s.substring(i - 1)).toLowerCase();
        else {
            String digits = s.replaceAll("\\D+", ""), result = "+";
            if (digits.length() == 10) return "***-***-" + digits.substring(digits.length() - 4);
            for (int j = 0; j < digits.length() - 10; j++) result += "*";
            return result + "-***-***-" + digits.substring(digits.length() - 4);
        }
    }
}
```

__Python__:

```Python
class Solution:
    def maskPII(self, s: str) -> str:
        return (s[0] + '*' * 5 + s[i - 1:]).lower() if (i := s.find('@')) != -1 else '***-' * 2 + "".join(s[-4:]) if (n := len(s := [c for c in s if c.isdigit()])) == 10 else '+' + '*' * (n - 10) + "-" + '***-' * 2 + "".join(s[-4:])
```
