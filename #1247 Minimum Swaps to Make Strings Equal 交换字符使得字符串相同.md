# 1247 Minimum Swaps to Make Strings Equal 交换字符使得字符串相同

__Description:__

You are given two strings s1 and s2 of equal length consisting of letters "x" and "y" only. Your task is to make these two strings equal to each other. You can swap any two characters that belong to different strings, which means: swap s1[i] and s2[j].

Return the minimum number of swaps required to make s1 and s2 equal, or return -1 if it is impossible to do so.

__Example:__

Example 1:

Input: s1 = "xx", s2 = "yy"
Output: 1
Explanation: Swap s1[0] and s2[1], s1 = "yx", s2 = "yx".

Example 2:

Input: s1 = "xy", s2 = "yx"
Output: 2
Explanation: Swap s1[0] and s2[0], s1 = "yy", s2 = "xx".
Swap s1[0] and s2[1], s1 = "xy", s2 = "xy".
Note that you cannot swap s1[0] and s1[1] to make s1 equal to "yx", cause we can only swap chars in different strings.

Example 3:

Input: s1 = "xx", s2 = "xy"
Output: -1

__Constraints:__

1 <= s1.length, s2.length <= 1000
s1, s2 only contain 'x' or 'y'.

__题目描述：__

有两个长度相同的字符串 s1 和 s2，且它们其中 只含有 字符 "x" 和 "y"，你需要通过「交换字符」的方式使这两个字符串相同。

每次「交换字符」的时候，你都可以在两个字符串中各选一个字符进行交换。

交换只能发生在两个不同的字符串之间，绝对不能发生在同一个字符串内部。也就是说，我们可以交换 s1[i] 和 s2[j]，但不能交换 s1[i] 和 s1[j]。

最后，请你返回使 s1 和 s2 相同的最小交换次数，如果没有方法能够使得这两个字符串相同，则返回 -1 。

__示例：__

示例 1：

输入：s1 = "xx", s2 = "yy"
输出：1
解释：
交换 s1[0] 和 s2[1]，得到 s1 = "yx"，s2 = "yx"。

示例 2：

输入：s1 = "xy", s2 = "yx"
输出：2
解释：
交换 s1[0] 和 s2[0]，得到 s1 = "yy"，s2 = "xx" 。
交换 s1[0] 和 s2[1]，得到 s1 = "xy"，s2 = "xy" 。
注意，你不能交换 s1[0] 和 s1[1] 使得 s1 变成 "yx"，因为我们只能交换属于两个不同字符串的字符。

示例 3：

输入：s1 = "xx", s2 = "xy"
输出：-1

示例 4：

输入：s1 = "xxyyxyxyxx", s2 = "xyyxyxxxyx"
输出：4

__提示：__

1 <= s1.length, s2.length <= 1000
s1, s2 只包含 'x' 或 'y'。

__思路：__

模拟
当 s1 = 'xx', s2 = 'yy', 需要进行一次交换
当 s1 = 'xy', s2 = 'yx', 需要进行两次交换
统计当 s1 和 s2 对应字符不相等时, s1 为 'x' 的次数为 x, s1 为 'y' 的次数为 y
如果 x + y 为奇数, 交换之后必然剩下一个奇数的字符, 返回 -1
否则返回 (x + 1) / 2 + (y + 1) / 2
时间复杂度为 O(2 ^ mn), 空间复杂度为 O(mn)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int minimumSwap(string s1, string s2) 
    {
        int x = 0, y = 0, n = s1.size(), m = s2.size();
        if (n != m) return -1;
        for (int i = 0; i < n; i++) 
        {
            if (s1[i] != s2[i]) 
            {
                x += s1[i] == 'x';
                y += s1[i] == 'y';
            }
        }
        return ((x + y) & 1) ? -1 : ((x + 1) >> 1) + ((y + 1) >> 1);
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumSwap(String s1, String s2) {
        int x = 0, y = 0, n = s1.length(), m = s2.length();
        if (n != m) return -1;
        for (int i = 0; i < n; i++) {
            if (s1.charAt(i) != s2.charAt(i)) {
                x += (s1.charAt(i) == 'x' ? 1 : 0);
                y += (s1.charAt(i) == 'y' ? 1 : 0);
            }
        }
        return ((x + y) & 1) != 0 ? -1 : ((x + 1) >> 1) + ((y + 1) >> 1);
    }
}
```

__Python__:

```Python
class Solution:
    def minimumSwap(self, s1: str, s2: str) -> int:
        return -1 if ((x := len(['x' for i in range(len(s1)) if s1[i] != s2[i] and s1[i] == 'x'])) + (y := len(['y' for i in range(len(s1)) if s1[i] != s2[i] and s1[i] == 'y'])) & 1) or len(s1) != len(s2) else ((x + 1) >> 1) + ((y + 1) >> 1)
```
