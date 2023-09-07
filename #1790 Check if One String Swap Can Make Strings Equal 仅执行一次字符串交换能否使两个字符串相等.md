# 1790 Check if One String Swap Can Make Strings Equal 仅执行一次字符串交换能否使两个字符串相等

__Description:__

You are given two strings `s1` and `s2` of equal length. A __string swap__ is an operation where you choose two indices in a string (not necessarily different) and swap the characters at these indices.

Return `true` _if it is possible to make both strings equal by performing __at most one string swap__ on __exactly one__ of the strings._ Otherwise, return `false`.

__Example:__

Example 1:

```text
Input: s1 = "bank", s2 = "kanb"
Output: true
Explanation: For example, swap the first character with the last character of s2 to make "bank".
```

Example 2:

```text
Input: s1 = "attack", s2 = "defend"
Output: false
Explanation: It is impossible to make them equal with one string swap.
```

Example 3:

```text
Input: s1 = "kelb", s2 = "kelb"
Output: true
Explanation: The two strings are already equal, so no string swap operation is required.
```

__Constraints:__

- `1 <= s1.length, s2.length <= 100`
- `s1.length == s2.length`
- `s1` and `s2` consist of only lowercase English letters.

__题目描述:__

给你长度相等的两个字符串 `s1` 和 `s2` 。一次 __字符串交换__ 操作的步骤如下:选出某个字符串中的两个下标（不必不同），并交换这两个下标所对应的字符。

如果对 __其中一个字符串__ 执行 __最多一次字符串交换__ 就可以使两个字符串相等，返回 `true` ；否则，返回 `false` 。

__示例:__

示例 1：

```text
输入：s1 = "bank", s2 = "kanb"
输出：true
解释：例如，交换 s2 中的第一个和最后一个字符可以得到 "bank"
```

示例 2：

```text
输入：s1 = "attack", s2 = "defend"
输出：false
解释：一次字符串交换无法使两个字符串相等
```

示例 3：

```text
输入：s1 = "kelb", s2 = "kelb"
输出：true
解释：两个字符串已经相等，所以不需要进行字符串交换
```

示例 4：

```text
输入：s1 = "abcd", s2 = "dcba"
输出：false
```

__提示：__

- `1 <= s1.length, s2.length <= 100`
- `s1.length == s2.length`
- `s1` 和 `s2` 仅由小写英文字母组成

__思路:__

```text
模拟
用两个变量记录两个字符串不同的位置
出现第三个不同的时候直接返回 false
最后比较不同的位置是否相同即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool areAlmostEqual(string s1, string s2) 
    {
        int n = s1.size(), a = -1, b = -1;
        for (int i = 0; i < n; i++) 
        {
            if (s1[i] != s2[i])
            {
                if (a == -1) a = i;
                else if (b == -1) b = i;
                else return false;
            }
        }
        return a == b || (a > -1 and b > -1 and s1[a] == s2[b] and s2[a] == s1[b]);
    }
};
```

__Java__:

```Java
class Solution {
    public boolean areAlmostEqual(String s1, String s2) {
        int n = s1.length(), a = -1, b = -1;
        for (int i = 0; i < n; i++) {
            if (s1.charAt(i) != s2.charAt(i)) {
                if (a == -1) a = i;
                else if (b == -1) b = i;
                else return false;
            }
        }
        return a == b || (a > -1 && b > -1 && s1.charAt(a) == s2.charAt(b) && s2.charAt(a) == s1.charAt(b));
    }
}
```

__Python__:

```Python
class Solution:
    def areAlmostEqual(self, s1: str, s2: str) -> bool:
        return sorted(s1) == sorted(s2) and sum(a != b for a, b in zip(s1, s2)) < 3
```
