# 696 Count Binary Substrings 计数二进制子串

__Description__:
Give a string s, count the number of non-empty (contiguous) substrings that have the same number of 0's and 1's, and all the 0's and all the 1's in these substrings are grouped consecutively.

Substrings that occur multiple times are counted the number of times they occur.

__Example:__

Example 1:

Input: "00110011"
Output: 6
Explanation: There are 6 substrings that have equal number of consecutive 1's and 0's: "0011", "01", "1100", "10", "0011", and "01".

Notice that some of these substrings repeat and are counted the number of times they occur.

Also, "00110011" is not a valid substring because all the 0's (and 1's) are not grouped together.

Example 2:

Input: "10101"
Output: 4
Explanation: There are 4 substrings: "10", "01", "10", "01" that have equal number of consecutive 1's and 0's.

__Note:__

s.length will be between 1 and 50,000.
s will only consist of "0" or "1" characters.

__题目描述__:
给定一个字符串 s，计算具有相同数量0和1的非空(连续)子字符串的数量，并且这些子字符串中的所有0和所有1都是组合在一起的。

重复出现的子串要计算它们出现的次数。

__示例 :__

示例 1 :

输入: "00110011"
输出: 6
解释: 有6个子串具有相同数量的连续1和0：“0011”，“01”，“1100”，“10”，“0011” 和 “01”。

请注意，一些重复出现的子串要计算它们出现的次数。

另外，“00110011”不是有效的子串，因为所有的0（和1）没有组合在一起。

示例 2 :

输入: "10101"
输出: 4
解释: 有4个子串：“10”，“01”，“10”，“01”，它们具有相同数量的连续1和0。

__注意:__

s.length 在1到50,000之间。
s 只包含“0”或“1”字符。

__思路__:

1. 分别查找连续的 0和 1个数, 比如 0011100 -> 232, 每次拿出 2个数字的较小值累加即可 232 -> 2 + 2 = 4
2. 用 last指针表示上次记录的值, cur记录出现的次数, 记录 last超过 cur的次数即可
3. 正则表达式找到连续的 0和 1
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int countBinarySubstrings(string s) 
    {
        int last = 0, cur = 1, result = 0;
        for (int i = 1; i < s.size(); i++) 
        {
            if (s[i] == s[i - 1]) cur++;
            else 
            {
                last = cur;
                cur = 1;
            }
            if (last >= cur) result++;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countBinarySubstrings(String s) {
        int last = 0, cur = 1, result = 0;
        for (int i = 1; i < s.length(); i++) {
            if (s.charAt(i) == s.charAt(i - 1)) cur++;
            else {
                last = cur;
                cur = 1;
            }
            if (last >= cur) result++;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countBinarySubstrings(self, s: str) -> int:
        last, cur, result = 0, 1, 0
        for i in range(1, len(s)):
            if s[i] == s[i - 1]:
                cur += 1
            else:
                last, cur = cur, 1
            if last >= cur:
                result += 1
        return result
```
