# 2839 Check if Strings Can be Made Equal With Operations I 判断通过操作能否让字符串相等 I

__Description:__

You are given two strings `s1` and `s2`, both of length `4`, consisting of __lowercase__ English letters.

You can apply the following operation on any of the two strings any number of times:

- Choose any two indices `i` and `j` such that `j - i = 2`, then __swap__ the two characters at those indices in the string.

Return `true` _if you can make the strings_ `s1` _and_ `s2` _equal, and_ `false` _otherwise_.

__Example:__

Example 1:

```text
Input: s1 = "abcd", s2 = "cdab"
Output: true
Explanation: We can do the following operations on s1:
```

- Choose the indices i = 0, j = 2. The resulting string is s1 = "cbad".
- Choose the indices i = 1, j = 3. The resulting string is s1 = "cdab" = s2.

Example 2:

```text
Input: s1 = "abcd", s2 = "dacb"
Output: false
Explanation: It is not possible to make the two strings equal.
```

__Constraints:__

- `s1.length == s2.length == 4`
- `s1` and `s2` consist only of lowercase English letters.

__题目描述:__

给你两个字符串 `s1` 和 `s2` ，两个字符串的长度都为 `4` ，且只包含 __小写__ 英文字母。

你可以对两个字符串中的 任意一个 执行以下操作 任意 次：

- 选择两个下标 `i` 和 `j` 且满足 `j - i = 2` ，然后 __交换__ 这个字符串中两个下标对应的字符。

如果你可以让字符串 `s1` 和 `s2` 相等，那么返回 `true` ，否则返回 `false` 。

__示例:__

示例 1：

```text
输入：s1 = "abcd", s2 = "cdab"
输出：true
解释： 我们可以对 s1 执行以下操作：
```

- 选择下标 i = 0 ，j = 2 ，得到字符串 s1 = "cbad" 。
- 选择下标 i = 1 ，j = 3 ，得到字符串 s1 = "cdab" = s2 。

示例 2：

```text
输入：s1 = "abcd", s2 = "dacb"
输出：false
解释：无法让两个字符串相等。
```

__提示：__

- `s1.length == s2.length == 4`
- `s1` 和 `s2` 只包含小写英文字母。

__思路:__

```text
模拟
分别判断 0/1 和 2/3 上对应相等或者成对相等
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool canBeEqual(string s1, string s2) 
    {
        return ((s1[0] == s2[0] and s1[2] == s2[2]) or (s1[0] == s2[2] and s1[2] == s2[0])) and ((s1[1] == s2[1] and s1[3] == s2[3]) or (s1[1] == s2[3] and s1[3] == s2[1]));
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canBeEqual(String s1, String s2) {
        return ((s1.charAt(0) == s2.charAt(0) && s1.charAt(2) == s2.charAt(2)) || (s1.charAt(0) == s2.charAt(2) && s1.charAt(2) == s2.charAt(0))) && ((s1.charAt(1) == s2.charAt(1) && s1.charAt(3) == s2.charAt(3)) || (s1.charAt(1) == s2.charAt(3) && s1.charAt(3) == s2.charAt(1)));
    }
}
```

__Python__:

```Python
class Solution:
    def canBeEqual(self, s1: str, s2: str) -> bool:
        return ((s1[0] == s2[0] and s1[2] == s2[2]) or (s1[0] == s2[2] and s1[2] == s2[0])) and ((s1[1] == s2[1] and s1[3] == s2[3]) or (s1[1] == s2[3] and s1[3] == s2[1]))
```
