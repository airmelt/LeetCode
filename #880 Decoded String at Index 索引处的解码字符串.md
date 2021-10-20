# 880 Decoded String at Index 索引处的解码字符串

__Description__:
You are given an encoded string s. To decode the string to a tape, the encoded string is read one character at a time and the following steps are taken:

If the character read is a letter, that letter is written onto the tape.
If the character read is a digit d, the entire current tape is repeatedly written d - 1 more times in total.
Given an integer k, return the kth letter (1-indexed) in the decoded string.

__Example:__

Example 1:

Input: s = "leet2code3", k = 10
Output: "o"
Explanation: The decoded string is "leetleetcodeleetleetcodeleetleetcode".
The 10th letter in the string is "o".

Example 2:

Input: s = "ha22", k = 5
Output: "h"
Explanation: The decoded string is "hahahaha".
The 5th letter is "h".

Example 3:

Input: s = "a2345678999999999999999", k = 1
Output: "a"
Explanation: The decoded string is "a" repeated 8301530446056247680 times.
The 1st letter is "a".

__Constraints:__

2 <= s.length <= 100
s consists of lowercase English letters and digits 2 through 9.
s starts with a letter.
1 <= k <= 10^9
It is guaranteed that k is less than or equal to the length of the decoded string.
The decoded string is guaranteed to have less than 2^63 letters.

__题目描述__:
给定一个编码字符串 S。请你找出 解码字符串 并将其写入磁带。解码时，从编码字符串中 每次读取一个字符 ，并采取以下步骤：

如果所读的字符是字母，则将该字母写在磁带上。
如果所读的字符是数字（例如 d），则整个当前磁带总共会被重复写 d-1 次。
现在，对于给定的编码字符串 S 和索引 K，查找并返回解码字符串中的第 K 个字母。

__示例 :__

示例 1：

输入：S = "leet2code3", K = 10
输出："o"
解释：
解码后的字符串为 "leetleetcodeleetleetcodeleetleetcode"。
字符串中的第 10 个字母是 "o"。

示例 2：

输入：S = "ha22", K = 5
输出："h"
解释：
解码后的字符串为 "hahahaha"。第 5 个字母是 "h"。

示例 3：

输入：S = "a2345678999999999999999", K = 1
输出："a"
解释：
解码后的字符串为 "a" 重复 8301530446056247680 次。第 1 个字母是 "a"。

__提示:__

2 <= S.length <= 100
S 只包含小写字母与数字 2 到 9 。
S 以字母开头。
1 <= K <= 10^9
题目保证 K 小于或等于解码字符串的长度。
解码后的字符串保证少于 2^63 个字母。

__思路__:

栈
只需要 k % index == 0 处的字符
用栈或者递归的方式记录当前所有的字符
遇到字符直接插入栈, 遇到数字则重复当前的字符
从后往前依次进行 k % length 操作, 这样就不用展开字符串
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string decodeAtIndex(string s, int k) 
    {
        long index = 0;
        for (int i = 0; i < s.size(); i++) index = isdigit(s[i]) ? index * (s[i] - '0') : index + 1;
        for (int i = s.size() - 1; i > -1; i--)
        {
            k %= index;
            if (!k and isalpha(s[i])) return string(1, s[i]);
            index = isdigit(s[i]) ? index / (s[i] - '0') : index - 1;
        }
        return "";
    }
};
```

__Java__:

```Java
class Solution {
    public String decodeAtIndex(String s, int k) {
        long index = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) >= 'a') {
                if (k == ++index) return s.charAt(i) + "";
            } 
            else if (k > index * (s.charAt(i) - '0')) index *= s.charAt(i) - '0';
            else {
                k = ((int)(k % index) == 0) ? (int)index : (int)(k % index);
                return decodeAtIndex(s, k);
            }
        }
        return "";
    }
}
```

__Python__:

```Python
class Solution:
    def decodeAtIndex(self, s: str, k: int) -> str:
        stack, index = [], 0
        for c in s:
            index = index * int(c) if c.isdigit() else index + 1
            stack.append((index, stack[-1][1] if c.isdigit() else c))
        while k % stack[-1][0]:
            k %= stack.pop()[0]
        return stack[-1][1]
```
