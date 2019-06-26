__Description__:
Given a string which consists of lowercase or uppercase letters, find the length of the longest palindromes that can be built with those letters.

This is case sensitive, for example "Aa" is not considered a palindrome here.

__Note:__
Assume the length of given string will not exceed 1,010.

__Example:__

Input:
"abccccdd"

Output:
7

__Explanation:__
One longest palindrome that can be built is "dccaccd", whose length is 7.

__题目描述__:
给定一个包含大写字母和小写字母的字符串，找到通过这些字母构造成的最长的回文串。

在构造过程中，请注意区分大小写。比如 "Aa" 不能当做一个回文字符串。

__注意:__
假设字符串的长度不会超过 1010。

__示例：__

输入:
"abccccdd"

输出:
7

__解释:__
我们可以构造的最长的回文串是"dccaccd", 它的长度是 7。

__思路__:
初始化一个数组用来存放 ASCII码的数量
字符为偶数的直接加入结果, 奇数的找到最近的偶数, 可以用除 2再乘上 2来找, 也可以直接减 1
最后结果没达到字符串的长度, 则可以加 1, 因为总可以找到一个单独的字符放在回文串的正中间
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    int longestPalindrome(string s) {
        int temp[128] = {0};
        for (char c: s) temp[c]++;
        int result = 0;
        for (int i = 65; i < 123; i++) result += temp[i] > 1 ? (temp[i] >> 1) << 1 : 0;
        return result + (result == s.size() ? 0 : 1);
    }
};
```

__Java__:
```
class Solution {
    public int longestPalindrome(String s) {
        int temp[] = new int[128];
        for (char c: s.toCharArray()) temp[c]++;
        int result = 0;
        for (int i = 65; i < 123; i++) result += temp[i] > 1 ? (temp[i] >> 1) << 1 : 0;
        return result + (result == s.length() ? 0 : 1);
    }
}
```

__Python__:
```
class Solution:
    def longestPalindrome(self, s: str) -> int:
        d = collections.Counter(s)
        odd = False
        result = 0
        for key, n in d.items():
            if n & 1:
                odd = True
                result += n - 1
            else:
                result += n
        if odd:
            result += 1
        return result
```
