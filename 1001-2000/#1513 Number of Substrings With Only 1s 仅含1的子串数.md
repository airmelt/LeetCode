# 1513 Number of Substrings With Only 1s 仅含1的子串数

__Description:__

Given a binary string `s`, return _the number of substrings with all characters_ `1`_'s_. Since the answer may be too large, return it modulo `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: s = "0110111"
Output: 9
Explanation: There are 9 substring in total with only 1's characters.
"1" -> 5 times.
"11" -> 3 times.
"111" -> 1 time.
```

Example 2:

```text
Input: s = "101"
Output: 2
Explanation: Substring "1" is shown 2 times in s.
```

Example 3:

```text
Input: s = "111111"
Output: 21
Explanation: Each substring contains only 1's characters.
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s[i]` is either `'0'` or `'1'`.

__题目描述:__

给你一个二进制字符串 `s`（仅由 '0' 和 '1' 组成的字符串）。

返回所有字符都为 1 的子字符串的数目。

由于答案可能很大，请你将它对 10 ^ 9 + 7 取模后返回。

__示例:__

示例 1：

```text
输入：s = "0110111"
输出：9
解释：共有 9 个子字符串仅由 '1' 组成
"1" -> 5 次
"11" -> 3 次
"111" -> 1 次
```

示例 2：

```text
输入：s = "101"
输出：2
解释：子字符串 "1" 在 s 中共出现 2 次
```

示例 3：

```text
输入：s = "111111"
输出：21
解释：每个子字符串都仅由 '1' 组成
```

示例 4：

```text
输入：s = "000"
输出：0
```

__提示：__

- `s[i] == '0'` 或 `s[i] == '1'`
- `1 <= s.length <= 10 ^ 5`

__思路:__

```text
计数
用一个变量记录重复出现的 1 的个数
遇到 0 重置为 0
遇到 1 同时更新 result 和 cur, result += ++cur
主意需要取余防止越界
时间复杂度为 O(N), 空间复杂度为 O(1)
也可以用哈希表记录连续的 1 的个数, 不过空间复杂度会上升为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numSub(string s) 
    {
        int result = 0, MOD = 1e9 + 7, cur = 0;
        for (const auto& c: s) 
        {
            if (c == '0') cur = 0;
            else result = (++cur + result) % MOD;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numSub(String s) {
        int result = 0, MOD = 1_000_000_007, cur = 0;
        for (char c: s.toCharArray()) {
            if (c == '0') cur = 0;
            else result = (++cur + result) % MOD;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numSub(self, s: str) -> int:
        return sum(len(i) * (len(i) + 1) >> 1 for i in s.split('0')) % (10 ** 9 + 7)
```
