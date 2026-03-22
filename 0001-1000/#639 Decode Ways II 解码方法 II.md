# 639 Decode Ways II 解码方法 II

__Description__:
A message containing letters from A-Z can be encoded into numbers using the following mapping:

```text
'A' -> "1"
'B' -> "2"
...
'Z' -> "26"
```

To decode an encoded message, all the digits must be grouped then mapped back into letters using the reverse of the mapping above (there may be multiple ways). For example, "11106" can be mapped into:

"AAJF" with the grouping (1 1 10 6)
"KJF" with the grouping (11 10 6)
Note that the grouping (1 11 06) is invalid because "06" cannot be mapped into 'F' since "6" is different from "06".

In addition to the mapping above, an encoded message may contain the '\*' character, which can represent any digit from '1' to '9' ('0' is excluded). For example, the encoded message "1*" may represent any of the encoded messages "11", "12", "13", "14", "15", "16", "17", "18", or "19". Decoding "1*" is equivalent to decoding any of the encoded messages it can represent.

Given a string s containing digits and the '*' character, return the number of ways to decode it.

Since the answer may be very large, return it modulo 10^9 + 7.

__Example:__

Example 1:

Input: s = "*"
Output: 9
Explanation: The encoded message can represent any of the encoded messages "1", "2", "3", "4", "5", "6", "7", "8", or "9".
Each of these can be decoded to the strings "A", "B", "C", "D", "E", "F", "G", "H", and "I" respectively.
Hence, there are a total of 9 ways to decode "*".

Example 2:

Input: s = "1*"
Output: 18
Explanation: The encoded message can represent any of the encoded messages "11", "12", "13", "14", "15", "16", "17", "18", or "19".
Each of these encoded messages have 2 ways to be decoded (e.g. "11" can be decoded to "AA" or "K").
Hence, there are a total of 9 \* 2 = 18 ways to decode "1*".

Example 3:

Input: s = "2*"
Output: 15
Explanation: The encoded message can represent any of the encoded messages "21", "22", "23", "24", "25", "26", "27", "28", or "29".
"21", "22", "23", "24", "25", and "26" have 2 ways of being decoded, but "27", "28", and "29" only have 1 way.
Hence, there are a total of (6 \* 2) + (3 \* 1) = 12 + 3 = 15 ways to decode "2*".

__Constraints:__

1 <= s.length <= 10^5
s[i] is a digit or '\*'.

__题目描述__:
一条包含字母 A-Z 的消息通过以下的方式进行了编码：

```text
'A' -> 1
'B' -> 2
...
'Z' -> 26
```

除了上述的条件以外，现在加密字符串可以包含字符 '\*'了，字符'\*'可以被当做1到9当中的任意一个数字。

给定一条包含数字和字符'*'的加密信息，请确定解码方法的总数。

同时，由于结果值可能会相当的大，所以你应当对10^9 + 7取模。（翻译者标注：此处取模主要是为了防止溢出）

__示例 :__

示例 1 :

输入: "*"
输出: 9
解释: 加密的信息可以被解密为: "A", "B", "C", "D", "E", "F", "G", "H", "I".

示例 2 :

输入: "1*"
输出: 9 + 9 = 18（翻译者标注：这里1*可以分解为1,\* 或者当做1\*来处理，所以结果是9+9=18）

__说明:__

输入的字符串长度范围是 [1, 10^5]。
输入的字符串只会包含字符 '*' 和 数字'0' - '9'。

__思路__:

动态规划
dp[i] 表示前 i 个字符能够形成的解码方法总数
dp[i] = dp[i - 1] \* a + dp[i - 2] \* b
其中 a = s[i] == '\*' ? 9 : (s[i] != '0'), 表示当前的字符如果为 *, 有 9 种取法(1 - 9), 如果为 0 则为 0, 否则为 1; b 则需要再往前看一位, 如果 s[i - 1] == '1', 则一定可以和当前位置组合, b = a + (a == 0), 如果 s[i - 1] == '2', 则要求当前位要么是 '0'-'7', 要么是 '*' 才能组合, 其余情况都为 0
可以看出只需要常数个变量即可
时间复杂度 O(n), 空间复杂度 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numDecodings(string s) 
    {
        long result = s[0] == '*' ? 9 : (s[0] != '0'), pre = 1;
        for (int i = 1, n = s.size(), a = 0, b = 0, mod = 1e9 + 7; i < n; i++)
        {
            a = s[i] == '*' ? 9 : (s[i] != '0');
            switch (s[i - 1])
            {
                case '1':
                    b = a + (a == 0);
                    break;
                case '2':
                    b = s[i] == '*' ? 6 : s[i] < '7' ? 1 : 0;
                    break;
                case '*':
                    b = a + (a == 0) + (s[i] == '*' ? 6 : s[i] < '7' ? 1 : 0);
                    break;
                default:
                    b = 0;
            }
            pre = (result * a + pre * b) % mod;
            swap(result, pre);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numDecodings(String s) {
        long result = s.charAt(0) == '*' ? 9 : (s.charAt(0) == '0' ? 0 : 1), pre = 1, cur = 0;
        int n = s.length(), mod = 1000000007;
        for (int i = 1, a = 0, b = 0; i < n; i++) {
            a = s.charAt(i) == '*' ? 9 : (s.charAt(i) == '0' ? 0 : 1);
            switch (s.charAt(i - 1)) {
                case '1':
                    b = a + (a == 0 ? 1 : 0);
                    break;
                case '2':
                    b = (s.charAt(i) == '*' ? 6 : (s.charAt(i) < '7' ? 1 : 0));
                    break;
                case '*':
                    b = a + (a == 0 ? 1 : 0) + (s.charAt(i) == '*' ? 6 : (s.charAt(i) < '7' ? 1 : 0));
                    break;
                default:
                    b = 0;
            }
            cur = (result * a + pre * b) % mod;
            pre = result;
            result = cur;
        }
        return (int)result;
    }
}
```

__Python__:

```Python
class Solution:
    def numDecodings(self, s: str) -> int:
        result, pre = 9 if s[0] == '*' else s[0] != '0', 1
        for i in range(1, len(s)):
            a = 9 if s[i] == '*' else s[i] != '0'
            b = (a + (not a) if s[i - 1] == '1' or s[i - 1] == '*' else 0) + (6 if s[i] == '*' else 1 if s[i] < '7' else 0) * (s[i - 1] == '2' or s[i - 1] == '*')
            result, pre = a * result + b * pre, result
        return result % (10 ** 9 + 7)
```
