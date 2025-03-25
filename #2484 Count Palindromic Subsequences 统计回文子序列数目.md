# 2484 Count Palindromic Subsequences 统计回文子序列数目

__Description:__

Given a string of digits `s`, return _the number of __palindromic subsequences__ of_ `s` _having length_ `5`. Since the answer may be very large, return it __modulo__ `10 ^ 9 + 7`.

Note:

- A string is __palindromic__ if it reads the same forward and backward.
- A __subsequence__ is a string that can be derived from another string by deleting some or no characters without changing the order of the remaining characters.

__Example:__

Example 1:

```text
Input: s = "103301"
Output: 2
Explanation: 
There are 6 possible subsequences of length 5: "10330","10331","10301","10301","13301","03301". 
Two of them (both equal to "10301") are palindromic.
```

Example 2:

```text
Input: s = "0000000"
Output: 21
Explanation: All 21 subsequences are "00000", which is palindromic.
```

Example 3:

```text
Input: s = "9999900000"
Output: 2
Explanation: The only two palindromic subsequences are "99999" and "00000".
```

__Constraints:__

- `1 <= s.length <= 10 ^ 4`
- `s` consists of digits.

__题目描述:__

给你数字字符串 `s` ，请你返回 `s` 中长度为 `5` 的 _回文子序列_ 数目。由于答案可能很大，请你将答案对 `10 ^ 9 + 7` __取余__ 后返回。

__提示：__

- 如果一个字符串从前往后和从后往前读相同，那么它是 __回文字符串__ 。
- 子序列是一个字符串中删除若干个字符后，不改变字符顺序，剩余字符构成的字符串。

__示例:__

示例 1：

```text
输入：s = "103301"
输出：2
解释：
总共有 6 长度为 5 的子序列："10330" ，"10331" ，"10301" ，"10301" ，"13301" ，"03301" 。
它们中有两个（都是 "10301"）是回文的。
```

示例 2：

```text
输入：s = "0000000"
输出：21
解释：所有 21 个长度为 5 的子序列都是 "00000" ，都是回文的。
```

示例 3：

```text
输入：s = "9999900000"
输出：2
解释：仅有的两个回文子序列是 "99999" 和 "00000" 。
```

__提示：__

- `1 <= s.length <= 10 ^ 4`
- `s` 只包含数字字符。

__思路:__

```text
前缀和 ➕ 后缀和
记录 suf[] 和 suf2[], pre[] 和 pre2[]
suf[i] 表示 i 之后的数字 d 的个数
pre[i] 表示 i 之前的数字 d 的个数
suf2[d][j] 表示 i 之后的数字 d * 10 + j 的个数
pre2[d][j] 表示 i 之前的数字 d * 10 + j 的个数
计算方式 suf2[d][j] += suf[j]
即 suf2[d * 10 + j] += suf[j]
d 表示当前数字, s[i] - '0' 或 int(s[i])
统计时, 以 s[i] 为中心
先减去 suf, 再减去 suf2
累加 suf2[j / 10][j % 10] * pre2[j / 10][j % 10]
或 sum(v1 * v2 for v1, v2 in zip(pre2, suf2))
最后加上 pre2, 再加上 pre
时间复杂度为 O(NC ^ 2), 空间复杂度为 O(C ^ 2), 其中 C 为 s 的字符集大小, 本题为 10
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countPalindromes(string s) 
    {
        int n = s.length(), suf[10]{0}, suf2[10][10]{0}, pre[10]{0}, pre2[10][10]{0}, result = 0, MOD = 1e9 + 7;
        for (int i = n - 1; i > -1; i--) 
        {
            for (int j = 0; j < 10; j++) suf2[s[i] - '0'][j] += suf[j];
            ++suf[s[i] - '0'];
        }
        for (int i = 0; i < n; i++) 
        {
            --suf[s[i] - '0'];
            for (int j = 0; j < 10; j++) suf2[s[i] - '0'][j] -= suf[j];
            for (int j = 0; j < 100; j++) result = (result + (long long)pre2[j / 10][j % 10] * suf2[j / 10][j % 10] % MOD) % MOD;
            for (int j = 0; j < 10; j++) pre2[s[i] - '0'][j] += pre[j];
            ++pre[s[i] - '0'];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countPalindromes(String s) {
        int n = s.length(), suf[] = new int[10], suf2[][] = new int[10][10], pre[] = new int[10], pre2[][] = new int[10][10], nums[] = new int[n], result = 0, MOD = 1_000_000_007;
        for (int i = 0; i < n; i++) nums[i] = s.charAt(i) - '0';
        for (int i = n - 1; i > -1; i--) {
            for (int j = 0; j < 10; j++) suf2[nums[i]][j] += suf[j];
            ++suf[nums[i]];
        }
        for (int i = 0; i < n; i++) {
            --suf[nums[i]];
            for (int j = 0; j < 10; j++) suf2[nums[i]][j] -= suf[j];
            for (int j = 0; j < 100; j++) result = (int)(result + (long)pre2[j / 10][j % 10] * suf2[j / 10][j % 10] % MOD) % MOD;
            for (int j = 0; j < 10; j++) pre2[nums[i]][j] += pre[j];
            ++pre[nums[i]];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countPalindromes(self, s: str) -> int:
        suf, suf2, pre, pre2, result, MOD = [0] * 10, [0] * 100, [0] * 10, [0] * 100, 0, 10 ** 9 + 7
        for d in map(int, reversed(s)):
            for i, v in enumerate(suf):
                suf2[d * 10 + i] += v
            suf[d] += 1
        for d in map(int, s):
            suf[d] -= 1
            for i, v in enumerate(suf):
                suf2[d * 10 + i] -= v
            result += sum(v1 * v2 for v1, v2 in zip(pre2, suf2))
            for i, v in enumerate(pre):
                pre2[d * 10 + i] += v
            pre[d] += 1
        return result % MOD
```
