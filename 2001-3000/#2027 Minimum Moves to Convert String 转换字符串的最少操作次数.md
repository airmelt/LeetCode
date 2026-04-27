# 2027 Minimum Moves to Convert String 转换字符串的最少操作次数

__Description:__

You are given a string `s` consisting of `n` characters which are either `'X'` or `'O'`.

A __move__ is defined as selecting __three__ __consecutive characters__ of `s` and converting them to `'O'`. Note that if a move is applied to the character `'O'`, it will stay the __same__.

Return _the __minimum__ number of moves required so that all the characters of_ `s` _are converted to_ `'O'`.

__Example:__

Example 1:

```text
Input: s = "XXX"
Output: 1
Explanation: XXX -> OOO
We select all the 3 characters and convert them in one move.
```

Example 2:

```text
Input:  s = "XXOX"
Output:  2
Explanation:  XXOX -> OOOX -> OOOO
We select the first 3 characters in the first move, and convert them to `'O'`.
Then we select the last 3 characters and convert them so that the final string contains all `'O'`s.
```

Example 3:

```text
Input:  s = "OOOO"
Output:  0
Explanation:  There are no `'X's` in `s` to convert.
```

__Constraints:__

- `3 <= s.length <= 1000`
- `s[i]` is either `'X'` or `'O'`.

__题目描述:__

给你一个字符串 `s` ，由 `n` 个字符组成，每个字符不是 `'X'` 就是 `'O'` 。

一次 __操作__ 定义为从 `s` 中选出 __三个连续字符__ 并将选中的每个字符都转换为 `'O'` 。注意，如果字符已经是 `'O'` ，只需要保持 __不变__ 。

返回将 `s` 中所有字符均转换为 `'O'` 需要执行的 __最少__ 操作次数。

__示例:__

示例 1：

```text
输入: s = "XXX"
输出: 1
解释:_XXX_ -> OOO
一次操作，选中全部 3 个字符，并将它们转换为 `'O' 。`
```

示例 2：

```text
输入: s = "XXOX"
输出: 2
解释:_XXO_X -> O_OOX_ -> OOOO
第一次操作，选择前 3 个字符，并将这些字符转换为 `'O'` 。
然后，选中后 3 个字符，并执行转换。最终得到的字符串全由字符 `'O'` 组成。
```

示例 3：

```text
输入: s = "OOOO"
输出: 0
解释: s 中不存在需要转换的 `'X' 。`
```

__提示：__

- `3 <= s.length <= 1000`
- `s[i]` 为 `'X'` 或 `'O'`

__思路:__

```text
1. 正则
找出所有的 "X.."
正则为 "X(.{0,2})"
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
2. 贪心
每次找到 "X" 时，将 "X" 后的两个字符转换为 "O"
记录操作次数
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumMoves(string s) 
    {
        int cur = -1, result = 0, n = s.size();
        for (int i = 0; i < n; i++) 
        {
            if (s[i] == 'X' and i > cur) 
            {
                ++result;
                cur = i + 2;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumMoves(String s) {
        int cur = -1, result = 0, n = s.length();
        for (int i = 0; i < n; i++) {
            if (s.charAt(i) == 'X' && i > cur) {
                ++result;
                cur = i + 2;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumMoves(self, s: str) -> int:
        return len(re.findall(r'X(.{0,2})', s))
```
