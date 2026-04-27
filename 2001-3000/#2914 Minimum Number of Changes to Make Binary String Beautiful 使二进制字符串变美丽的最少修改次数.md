# 2914 Minimum Number of Changes to Make Binary String Beautiful 使二进制字符串变美丽的最少修改次数

__Description:__

You are given a __0-indexed__ binary string `s` having an even length.

A string is beautiful if it's possible to partition it into one or more substrings such that:

- Each substring has an __even length__.
- Each substring contains __only__ `1`'s or __only__ `0`'s.

You can change any character in `s` to `0` or `1`.

Return _the __minimum__ number of changes required to make the string_ `s` _beautiful_.

__Example:__

Example 1:

```text
Input: s = "1001"
Output: 2
Explanation: We change s[1] to 1 and s[3] to 0 to get string "1100".
It can be seen that the string "1100" is beautiful because we can partition it into "11|00".
It can be proven that 2 is the minimum number of changes needed to make the string beautiful.
```

Example 2:

```text
Input: s = "10"
Output: 1
Explanation: We change s[1] to 1 to get string "11".
It can be seen that the string "11" is beautiful because we can partition it into "11".
It can be proven that 1 is the minimum number of changes needed to make the string beautiful.
```

Example 3:

```text
Input: s = "0000"
Output: 0
Explanation: We don't need to make any changes as the string "0000" is beautiful already.
```

__Constraints:__

- `2 <= s.length <= 10 ^ 5`
- `s` has an even length.
- `s[i]` is either `'0'` or `'1'`.

__题目描述:__

给你一个长度为偶数下标从 __0__ 开始的二进制字符串 `s` 。

如果可以将一个字符串分割成一个或者更多满足以下条件的子字符串，那么我们称这个字符串是 美丽的 ：

- 每个子字符串的长度都是 __偶数__ 。
- 每个子字符串都 __只__ 包含 `1` 或 __只__ 包含 `0` 。

你可以将 `s` 中任一字符改成 `0` 或者 `1` 。

请你返回让字符串 `s` 美丽的 __最少__ 字符修改次数。

__示例:__

示例 1：

```text
输入：s = "1001"
输出：2
解释：我们将 s[1] 改为 1 ，且将 s[3] 改为 0 ，得到字符串 "1100" 。
字符串 "1100" 是美丽的，因为我们可以将它分割成 "11|00" 。
将字符串变美丽最少需要 2 次修改。
```

示例 2：

```text
输入：s = "10"
输出：1
解释：我们将 s[1] 改为 1 ，得到字符串 "11" 。
字符串 "11" 是美丽的，因为它已经是美丽的。
将字符串变美丽最少需要 1 次修改。
```

示例 3：

```text
输入：s = "0000"
输出：0
解释：不需要进行任何修改，字符串 "0000" 已经是美丽字符串。
```

__提示：__

- `2 <= s.length <= 10 ^ 5`
- `s` 的长度为偶数。
- `s[i]` 要么是 `'0'` ，要么是 `'1'` 。

__思路:__

```text
模拟
由于 s 的长度为偶数
所以 s 必然可以两个两个的分成 n / 2 组
如果这 n / 2 组要么是 01 要么是 10
那就需要修改 1 次
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minChanges(string s) 
    {
        int result = 0;
        for (int i = 0, n = s.size(); i < n; i += 2) if (s[i] == '0' and s[i + 1] == '1' or s[i] == '1' and s[i + 1] == '0') ++result;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minChanges(String s) {
        int result = 0;
        for (int i = 0, n = s.length(); i < n; i += 2) if (s.charAt(i) == '0' && s.charAt(i + 1) == '1' || s.charAt(i) == '1' && s.charAt(i + 1) == '0') ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minChanges(self, s: str) -> int:
        return sum(s[i:i + 2] in ('01', '10') for i in range(0, len(s), 2))
```
