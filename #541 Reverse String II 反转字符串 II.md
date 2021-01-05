# 541 Reverse String II 反转字符串 II

__Description__:
Given a string and an integer k, you need to reverse the first k characters for every 2k characters counting from the start of the string. If there are less than k characters left, reverse all of them. If there are less than 2k but greater than or equal to k characters, then reverse the first k characters and left the other as original.

__Example:__

Input: s = "abcdefg", k = 2
Output: "bacdfeg"

__Restrictions:__
The string consists of lower English letters only.
Length of the given string and k will in the range [1, 10000]

__题目描述__:
给定一个字符串和一个整数 k，你需要对从字符串开头算起的每个 2k 个字符的前k个字符进行反转。如果剩余少于 k 个字符，则将剩余的所有全部反转。如果有小于 2k 但大于或等于 k 个字符，则反转前 k 个字符，并将剩余的字符保持原样。

__示例 :__

输入: s = "abcdefg", k = 2
输出: "bacdfeg"

__要求:__
该字符串只包含小写的英文字母。
给定字符串的长度和 k 在[1, 10000]范围内。

__思路__:

每次取间隔 2k的长度, 判断是否超过字符串长度, 将前 k位反转即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string reverseStr(string s, int k) 
    {
        for (int i = 0; i < s.size(); i += k * 2) 
        {
            if (i + k < s.size()) reverse(s, i, i + k - 1);
            else reverse(s, i, s.size() - 1);
        }
        return s;
    }
private:
    void reverse(string &s, int i, int j) 
    {
        while (i < j) {
            s[i] ^= s[j];
            s[j] ^= s[i];
            s[i++] ^= s[j--];
        }
    }
};
```

__Java__:

```Java
class Solution {
    public String reverseStr(String s, int k) {
        char c[] = s.toCharArray();
        for (int i = 0; i < c.length; i += k * 2) {
            if (i + k < c.length) reverse(c, i, i + k);
            else reverse(c, i, c.length);
        }
        return new String(c);
    }

    private void reverse(char[] s, int from, int end) {
        int i = from, j = end - 1;
        while (i < j) {
            s[i] ^= s[j];
            s[j] ^= s[i];
            s[i++] ^= s[j--];
        }
    }

}
```

__Python__:

```Python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        return ''.join([s[i:i + k][::-1] + s[i + k: i + 2 * k] for i in range(0, len(s), 2 * k)])
```
