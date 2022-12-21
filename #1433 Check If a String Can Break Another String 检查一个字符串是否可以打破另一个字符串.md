# 1433 Check If a String Can Break Another String 检查一个字符串是否可以打破另一个字符串

__Description:__

Given two strings: `s1` and `s2` with the same size, check if some permutation of string `s1` can break some permutation of string `s2` or vice-versa. In other words `s2` can break `s1` or vice-versa.

A string `x` can break string `y` (both of size `n`) if `x[i] >= y[i]` (in alphabetical order) for all `i` between `0` and `n-1`.

__Example:__

Example 1:

```text
Input: s1 = "abc", s2 = "xya"
Output: true
Explanation: "ayx" is a permutation of s2="xya" which can break to string "abc" which is a permutation of s1="abc".
```

Example 2:

```text
Input: s1 = "abe", s2 = "acd"
Output: false 
Explanation: All permutations for s1="abe" are: "abe", "aeb", "bae", "bea", "eab" and "eba" and all permutation for s2="acd" are: "acd", "adc", "cad", "cda", "dac" and "dca". However, there is not any permutation from s1 which can break some permutation from s2 and vice-versa.
```

Example 3:

```text
Input: s1 = "leetcodee", s2 = "interview"
Output: true
```

__Constraints:__

- `s1.length == n`
- `s2.length == n`
- `1 <= n <= 10 ^ 5`
- All strings consist of lowercase English letters.

__题目描述:__

给你两个字符串 `s1` 和 `s2` ，它们长度相等，请你检查是否存在一个 `s1`  的排列可以打破 `s2` 的一个排列，或者是否存在一个 `s2` 的排列可以打破 `s1` 的一个排列。

字符串 `x` 可以打破字符串 `y` （两者长度都为 `n` ）需满足对于所有 `i`（在 `0` 到 `n - 1` 之间）都有 `x[i] >= y[i]`（字典序意义下的顺序）。

__示例:__

示例 1：

```text
输入：s1 = "abc", s2 = "xya"
输出：true
解释："ayx" 是 s2="xya" 的一个排列，"abc" 是字符串 s1="abc" 的一个排列，且 "ayx" 可以打破 "abc" 。
```

示例 2：

```text
输入：s1 = "abe", s2 = "acd"
输出：false 
解释：s1="abe" 的所有排列包括："abe"，"aeb"，"bae"，"bea"，"eab" 和 "eba" ，s2="acd" 的所有排列包括："acd"，"adc"，"cad"，"cda"，"dac" 和 "dca"。然而没有任何 s1 的排列可以打破 s2 的排列。也没有 s2 的排列能打破 s1 的排列。
```

示例 3：

```text
输入：s1 = "leetcodee", s2 = "interview"
输出：true
```

__提示：__

- `s1.length == n`
- `s2.length == n`
- `1 <= n <= 10 ^ 5`
- 所有字符串都只包含小写英文字母。

__思路:__

```text
贪心
先对两个字符串进行排序
要么一个字符串完全大于另一个字符串
要么一个字符串完全小于另一个字符串
用与运算保持大小关系
时间复杂度为 O(NlgN), 空间复杂度为 O(lgN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool checkIfCanBreak(string s1, string s2) 
    {
        sort(s1.begin(), s1.end());
        sort(s2.begin(), s2.end());
        bool b1 = true, b2 = true;
        int n = s1.size();
        for (int i = 0; i < n; i++) 
        {
            b1 &= (s1[i] >= s2[i]);
            b2 &= (s1[i] <= s2[i]);
        }
        return b1 || b2;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean checkIfCanBreak(String s1, String s2) {
        char[] c1 = s1.toCharArray(), c2 = s2.toCharArray();
        Arrays.sort(c1);
        Arrays.sort(c2);
        boolean b1 = true, b2 = true;
        int n = c1.length;
        for (int i = 0; i < n; i++) {
            b1 &= (c1[i] >= c2[i]);
            b2 &= (c1[i] <= c2[i]);
        }
        return b1 || b2;
    }
}
```

__Python__:

```Python
class Solution:
    def checkIfCanBreak(self, s1: str, s2: str) -> bool:
        return any(map(all, zip(*((a >= b, a <= b) for a, b in zip(sorted(s1), sorted(s2))))))
```
