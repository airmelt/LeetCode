# 2840 Check if Strings Can be Made Equal With Operations II 判断通过操作能否让字符串相等 II

__Description:__

You are given two strings `s1` and `s2`, both of length `n`, consisting of __lowercase__ English letters.

You can apply the following operation on any of the two strings any number of times:

- Choose any two indices `i` and `j` such that `i < j` and the difference `j - i` is __even__, then __swap__ the two characters at those indices in the string.

Return `true` _if you can make the strings_ `s1` _and_ `s2` _equal, and_ `false` _otherwise_.

__Example:__

Example 1:

```text
Input: s1 = "abcdba", s2 = "cabdab"
Output: true
Explanation: We can apply the following operations on s1:
```

- Choose the indices i = 0, j = 2. The resulting string is s1 = "cbadba".
- Choose the indices i = 2, j = 4. The resulting string is s1 = "cbbdaa".
- Choose the indices i = 1, j = 5. The resulting string is s1 = "cabdab" = s2.

Example 2:

```text
Input: s1 = "abe", s2 = "bea"
Output: false
Explanation: It is not possible to make the two strings equal.
```

__Constraints:__

- `n == s1.length == s2.length`
- `1 <= n <= 10 ^ 5`
- `s1` and `s2` consist only of lowercase English letters.

__题目描述:__

给你两个字符串 `s1` 和 `s2` ，两个字符串长度都为 `n` ，且只包含 __小写__ 英文字母。

你可以对两个字符串中的 任意一个 执行以下操作 任意 次：

- 选择两个下标 `i` 和 `j` ，满足 `i < j` 且 `j - i` 是 __偶数__，然后 __交换__ 这个字符串中两个下标对应的字符。

如果你可以让字符串 `s1` 和 `s2` 相等，那么返回 `true` ，否则返回 `false` 。

__示例:__

示例 1：

```text
输入：s1 = "abcdba", s2 = "cabdab"
输出：true
解释：我们可以对 s1 执行以下操作：
```

- 选择下标 i = 0 ，j = 2 ，得到字符串 s1 = "cbadba" 。
- 选择下标 i = 2 ，j = 4 ，得到字符串 s1 = "cbbdaa" 。
- 选择下标 i = 1 ，j = 5 ，得到字符串 s1 = "cabdab" = s2 。

示例 2：

```text
输入：s1 = "abe", s2 = "bea"
输出：false
解释：无法让两个字符串相等。
```

__提示：__

- `n == s1.length == s2.length`
- `1 <= n <= 10 ^ 5`
- `s1` 和 `s2` 只包含小写英文字母。

__思路:__

```text
模拟
记录下奇数和偶数位置的字符总数
比较两个字符串对应位置的值是否相等
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool checkStrings(string s1, string s2) 
    {
        vector<vector<int>> c1(2, vector<int>(26)), c2(2, vector<int>(26));
        for (int i = 0, n = s1.length(); i < n; i++) 
        {
            ++c1[i & 1][s1[i] - 'a'];
            ++c2[i & 1][s2[i] - 'a'];
        }
        return c1 == c2;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean checkStrings(String s1, String s2) {
        int[][] c1 = new int[2][26], c2 = new int[2][26];
        for (int i = 0, n = s1.length(); i < n; i++) {
            ++c1[i & 1][s1.charAt(i) - 'a'];
            ++c2[i & 1][s2.charAt(i) - 'a'];
        }
        return Arrays.deepEquals(c1, c2);
    }
}
```

__Python__:

```Python
class Solution:
    def checkStrings(self, s1: str, s2: str) -> bool:
        return Counter(s1[::2]) == Counter(s2[::2]) and Counter(s1[1::2]) == Counter(s2[1::2])
```
