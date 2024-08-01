# 2193 Minimum Number of Moves to Make Palindrome 得到回文串的最少操作次数

__Description:__

You are given a string `s` consisting only of lowercase English letters.

In one __move__, you can select any two __adjacent__ characters of `s` and swap them.

Return _the __minimum number of moves__ needed to make_ `s` _a palindrome_.

__Note__ that the input will be generated such that `s` can always be converted to a palindrome.

__Example:__

Example 1:

```text
Input: s = "aabb"
Output: 2
Explanation:
We can obtain two palindromes from s, "abba" and "baab". 
```

- We can obtain "abba" from s in 2 moves: "aabb" -> "abab" -> "abba".
- We can obtain "baab" from s in 2 moves: "aabb" -> "abab" -> "baab".

Thus, the minimum number of moves needed to make s a palindrome is 2.

Example 2:

```text
Input: s = "letelt"
Output: 2
Explanation:
One of the palindromes we can obtain from s in 2 moves is "lettel".
One of the ways we can obtain it is "letelt" -> "letetl" -> "lettel".
Other palindromes such as "tleelt" can also be obtained in 2 moves.
It can be shown that it is not possible to obtain a palindrome in less than 2 moves.
```

__Constraints:__

- `1 <= s.length <= 2000`
- `s` consists only of lowercase English letters.
- `s` can be converted to a palindrome using a finite number of moves.

__题目描述:__

给你一个只包含小写英文字母的字符串 `s` 。

每一次 __操作__ ，你可以选择 `s` 中两个 __相邻__ 的字符，并将它们交换。

请你返回将 `s` 变成回文串的 __最少操作次数__ 。

__注意__ ，输入数据会确保 `s` 一定能变成一个回文串。

__示例:__

示例 1：

```text
输入：s = "aabb"
输出：2
解释：
我们可以将 s 变成 2 个回文串，"abba" 和 "baab" 。
```

- 我们可以通过 2 次操作得到 "abba" ："aabb" -> "abab" -> "abba" 。
- 我们可以通过 2 次操作得到 "baab" ："aabb" -> "abab" -> "baab" 。

因此，得到回文串的最少总操作次数为 2 。

示例 2：

```text
输入：s = "letelt"
输出：2
解释：
通过 2 次操作从 s 能得到回文串 "lettel" 。
其中一种方法是："letelt" -> "letetl" -> "lettel" 。
其他回文串比方说 "tleelt" 也可以通过 2 次操作得到。
可以证明少于 2 次操作，无法得到回文串。
```

__提示：__

- `1 <= s.length <= 2000`
- `s` 只包含小写英文字母。
- `s` 可以通过有限次操作得到一个回文串。

__思路:__

```text
贪心
如果 s 的长度小于 3, 则返回 0, 此时 s 本身就是回文串
每次我们考虑 s 的第一个字符 c 和最后一个与其相同的字符
如果最后一次出现字符的位置不为 0
为了将 s 变成回文串, 我们需要将 s.size() - 1 - idx 个字符移动到最后一个字符前面
此时 s 将变成 c?????c
这样我们就可以去掉首尾的字符, 递归处理中间的子串
并不需要真的移动字符直接处理 s[1:idx] + s[idx + 1:] 即可
如果 idx 为 0, 则说明 s 中只有一个字符, 将 s 反转在最后一次的时候再处理 c
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minMovesToMakePalindrome(string s) 
    {
        if (s.size() < 3) return 0;
        int idx = s.rfind(s.front());
        if (idx) return s.size() - 1 - idx + minMovesToMakePalindrome(s.substr(1, idx - 1) + s.substr(idx + 1));
        reverse(s.begin(), s.end());
        return minMovesToMakePalindrome(s);
    }
};
```

__Java__:

```Java
class Solution {
    public int minMovesToMakePalindrome(String s) {
        if (s.length() < 3) return 0;
        int idx = s.lastIndexOf(s.charAt(0));
        if (idx != 0) return s.length() - 1 - idx + minMovesToMakePalindrome(s.substring(1, idx) + s.substring(idx + 1));
        StringBuilder sb = new StringBuilder(s);
        sb.reverse();
        return minMovesToMakePalindrome(sb.toString());
    }
}
```

__Python__:

```Python
class Solution:
    def minMovesToMakePalindrome(self, s: str) -> int:
        return 0 if len(s) < 3 else len(s) - 1 - idx + self.minMovesToMakePalindrome(s[1:idx] + s[idx + 1:]) if (idx := s.rfind(s[0])) else self.minMovesToMakePalindrome(s[::-1])
```
