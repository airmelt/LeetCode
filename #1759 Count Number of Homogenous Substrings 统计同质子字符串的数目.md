# 1759 Count Number of Homogenous Substrings 统计同质子字符串的数目

__Description:__

Given a string `s`, return _the number of __homogenous__ substrings of_ `s`_._ Since the answer may be too large, return it __modulo__ `10 ^ 9 + 7`.

A string is homogenous if all the characters of the string are the same.

A substring is a contiguous sequence of characters within a string.

__Example:__

Example 1:

```text
Input: s = "abbcccaa"
Output: 13
Explanation: The homogenous substrings are listed as below:
"a"   appears 3 times.
"aa"  appears 1 time.
"b"   appears 2 times.
"bb"  appears 1 time.
"c"   appears 3 times.
"cc"  appears 2 times.
"ccc" appears 1 time.
3 + 1 + 2 + 1 + 3 + 2 + 1 = 13.
```

Example 2:

```text
Input: s = "xy"
Output: 2
Explanation: The homogenous substrings are "x" and "y".
```

Example 3:

```text
Input: s = "zzzzz"
Output: 15
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s` consists of lowercase letters.

__题目描述:__

给你一个字符串 `s` ，返回 `s` 中 __同质子字符串__ 的数目。由于答案可能很大，只需返回对 `10 ^ 9 + 7` __取余__ 后的结果。

同质字符串 的定义为：如果一个字符串中的所有字符都相同，那么该字符串就是同质字符串。

子字符串 是字符串中的一个连续字符序列。

__示例:__

示例 1：

```text
输入：s = "abbcccaa"
输出：13
解释：同质子字符串如下所列：
"a"   出现 3 次。
"aa"  出现 1 次。
"b"   出现 2 次。
"bb"  出现 1 次。
"c"   出现 3 次。
"cc"  出现 2 次。
"ccc" 出现 1 次。
3 + 1 + 2 + 1 + 3 + 2 + 1 = 13
```

示例 2：

```text
输入：s = "xy"
输出：2
解释：同质子字符串是 "x" 和 "y" 。
```

示例 3：

```text
输入：s = "zzzzz"
输出：15
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s` 由小写字符串组成

__思路:__

```text
数学
统计每一段完全相同的字符长度
每次统计一次就加上字符的长度
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countHomogenous(string s) 
    {
        int n = s.size(), result = 1, cur = 1, MOD = 1e9 + 7;
        for (int i = 1; i < n; i++) result = (result + (cur = s[i] == s[i - 1] ? cur + 1 : 1)) % MOD;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countHomogenous(String s) {
        int n = s.length(), result = 1, cur = 1, MOD = 1_000_000_007;
        for (int i = 1; i < n; i++) result = (result + (cur = s.charAt(i) == s.charAt(i - 1) ? cur + 1 : 1)) % MOD;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countHomogenous(self, s: str) -> int:
        result, n, cur = 1, len(s), 1
        for i in range(1, n):
            result += (cur := 1 if s[i] != s[i - 1] else cur + 1)
        return result % (10 ** 9 + 7)
```
