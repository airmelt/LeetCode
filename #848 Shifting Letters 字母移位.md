# 848 Shifting Letters 字母移位

__Description__:
You are given a string s of lowercase English letters and an integer array shifts of the same length.

Call the shift() of a letter, the next letter in the alphabet, (wrapping around so that 'z' becomes 'a').

For example, shift('a') = 'b', shift('t') = 'u', and shift('z') = 'a'.
Now for each shifts[i] = x, we want to shift the first i + 1 letters of s, x times.

Return the final string after all such shifts to s are applied.

__Example:__

Example 1:

Input: s = "abc", shifts = [3,5,9]
Output: "rpl"
Explanation: We start with "abc".
After shifting the first 1 letters of s by 3, we have "dbc".
After shifting the first 2 letters of s by 5, we have "igc".
After shifting the first 3 letters of s by 9, we have "rpl", the answer.

Example 2:

Input: s = "aaa", shifts = [1,2,3]
Output: "gfd"

__Constraints:__

1 <= s.length <= 10^5
s consists of lowercase English letters.
shifts.length == s.length
0 <= shifts[i] <= 10^9

__题目描述__:
有一个由小写字母组成的字符串 S，和一个整数数组 shifts。

我们将字母表中的下一个字母称为原字母的 移位（由于字母表是环绕的， 'z' 将会变成 'a'）。

例如·，shift('a') = 'b'， shift('t') = 'u',， 以及 shift('z') = 'a'。

对于每个 shifts[i] = x ， 我们会将 S 中的前 i+1 个字母移位 x 次。

返回将所有这些移位都应用到 S 后最终得到的字符串。

__示例 :__

输入：S = "abc", shifts = [3,5,9]
输出："rpl"
解释：
我们以 "abc" 开始。
将 S 中的第 1 个字母移位 3 次后，我们得到 "dbc"。
再将 S 中的前 2 个字母移位 5 次后，我们得到 "igc"。
最后将 S 中的这 3 个字母移位 9 次后，我们得到答案 "rpl"。

__提示:__

1 <= S.length = shifts.length <= 20000
0 <= shifts[i] <= 10 ^ 9

__思路__:

后缀和
从后往前遍历 shifts 数组, 并加上后面的和
然后移动每一个字符
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string shiftingLetters(string s, vector<int>& shifts) 
    {
        for (int i = s.size() - 2; i >= 0; i--) shifts[i] += shifts[i + 1] % 26;
        for (int i = 0; i < s.size(); i++) s[i] = (s[i] - 'a' + shifts[i]) % 26 + 'a';
        return s;
    }
};
```

__Java__:

```Java
class Solution {
    public String shiftingLetters(String s, int[] shifts) {
        StringBuilder sb = new StringBuilder();
        for (int i = s.length() - 2; i >= 0; i--) shifts[i] += shifts[i + 1] % 26;
        for (int i = 0; i < s.length(); i++) sb.append((char)((s.charAt(i) - 'a' + shifts[i]) % 26 + 'a'));
        return sb.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def shiftingLetters(self, s: str, shifts: List[int]) -> str:
        result, n = '', len(s)
        for i in range(n - 2, -1, -1):
            shifts[i] += shifts[i + 1] % 26
        for i in range(n):
            result += chr((ord(s[i]) - ord('a') + shifts[i]) % 26 + ord('a'))
        return result
```
