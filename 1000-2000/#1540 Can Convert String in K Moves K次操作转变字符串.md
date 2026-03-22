# 1540 Can Convert String in K Moves K次操作转变字符串

__Description:__

Given two strings `s` and `t`, your goal is to convert `s` into `t` in `k` moves or less.

During the `i ^ th` ( `1 <= i <= k`) move you can:

- Choose any index `j` (1-indexed) from `s`, such that `1 <= j <= s.length` and `j` has not been chosen in any previous move, and shift the character at that index `i` times.
- Do nothing.

Shifting a character means replacing it by the next letter in the alphabet (wrapping around so that `'z'` becomes `'a'`). Shifting a character by `i` means applying the shift operations `i` times.

Remember that any index `j` can be picked at most once.

Return `true` if it's possible to convert `s` into `t` in no more than `k` moves, otherwise return `false`.

__Example:__

Example 1:

```text
Input: s = "input", t = "ouput", k = 9
Output: true
Explanation: In the 6th move, we shift 'i' 6 times to get 'o'. And in the 7th move we shift 'n' to get 'u'.
```

Example 2:

```text
Input: s = "abc", t = "bcd", k = 10
Output: false
Explanation: We need to shift each character in s one time to convert it into t. We can shift 'a' to 'b' during the 1st move. However, there is no way to shift the other characters in the remaining moves to obtain t from s.
```

Example 3:

```text
Input: s = "aab", t = "bbb", k = 27
Output: true
Explanation: In the 1st move, we shift the first 'a' 1 time to get 'b'. In the 27th move, we shift the second 'a' 27 times to get 'b'.
```

__Constraints:__

- `1 <= s.length, t.length <= 10 ^ 5`
- `0 <= k <= 10 ^ 9`
- `s`, `t` contain only lowercase English letters.

__题目描述:__

给你两个字符串 `s` 和 `t` ，你的目标是在 `k` 次操作以内把字符串 `s` 转变成 `t` 。

在第 `i` 次操作时（ `1 <= i <= k`），你可以选择进行如下操作：

- 选择字符串 `s` 中满足 `1 <= j <= s.length` 且之前未被选过的任意下标 `j` （下标从 1 开始），并将此位置的字符切换 `i` 次。
- 不进行任何操作。

切换 1 个字符的意思是用字母表中该字母的下一个字母替换它（字母表环状接起来，所以 `'z'` 切换后会变成 `'a'`）。第 `i` 次操作意味着该字符应切换 `i` 次

请记住任意一个下标 `j` 最多只能被操作 1 次。

如果在不超过 `k` 次操作内可以把字符串 `s` 转变成 `t` ，那么请你返回 `true` ，否则请你返回 `false` 。

__示例:__

示例 1：

```text
输入：s = "input", t = "ouput", k = 9
输出：true
解释：第 6 次操作时，我们将 'i' 切换 6 次得到 'o' 。第 7 次操作时，我们将 'n' 切换 7 次得到 'u' 。
```

示例 2：

```text
输入：s = "abc", t = "bcd", k = 10
输出：false
解释：我们需要将每个字符切换 1 次才能得到 t 。我们可以在第 1 次操作时将 'a' 切换成 'b' ，但另外 2 个字母在剩余操作中无法再转变为 t 中对应字母。
```

示例 3：

```text
输入：s = "aab", t = "bbb", k = 27
输出：true
解释：第 1 次操作时，我们将第一个 'a' 切换 1 次得到 'b' 。在第 27 次操作时，我们将第二个字母 'a' 切换 27 次得到 'b' 。
```

__提示：__

- `1 <= s.length, t.length <= 10 ^ 5`
- `0 <= k <= 10 ^ 9`
- `s` 和 `t` 只包含小写英文字母。

__思路:__

```text

时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool canConvertString(string s, string t, int k) 
    {
        vector<int> count(26, k / 26);
        if (s.size() != t.size()) return false;
        for (int i = 0; i < k % 26; i++) ++count[i];
        for (int i = 0, n = s.size(); i < n; i++) if (s[i] != t[i]) if (!~(--count[(t[i] - s[i] + 26) % 26 - 1])) return false;
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canConvertString(String s, String t, int k) {
        int m = s.length(), n = t.length(), count[] = new int[26];
        if (m != n) return false;
        Arrays.fill(count, k / 26);
        for (int i = 0; i < k % 26; i++) ++count[i];
        for (int i = 0; i < n; i++) if (s.charAt(i) != t.charAt(i)) if (--count[(t.charAt(i) - s.charAt(i) + 26) % 26 - 1] < 0) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def canConvertString(self, s: str, t: str, k: int) -> bool:
        return len(s) == len(t) and (all(26 * (cnt - 1) + diff <= k for diff, cnt in c.items()) if (c := Counter(val if val > 0 else 26 + val for x, y in zip(s, t) if (val := ord(y) - ord(x)))) else True)
```
