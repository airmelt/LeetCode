# 2337 Move Pieces to Obtain a String 移动片段得到字符串

__Description:__

You are given two strings `start` and `target`, both of length `n`. Each string consists __only__ of the characters `'L'`, `'R'`, and `'_'` where:

- The characters `'L'` and `'R'` represent pieces, where a piece `'L'` can move to the __left__ only if there is a __blank__ space directly to its left, and a piece `'R'` can move to the __right__ only if there is a __blank__ space directly to its right.
- The character `'_'` represents a blank space that can be occupied by __any__ of the `'L'` or `'R'` pieces.

Return `true` _if it is possible to obtain the string_ `target` _by moving the pieces of the string_ `start` ___any__ number of times_. Otherwise, return `false`.

__Example:__

Example 1:

```text
Input: start = "_LRR_", target = "LRR"
Output: true
Explanation: We can obtain the string target from start by doing the following moves:
```

- Move the first piece one step to the left, start becomes equal to "L_RR_".
- Move the last piece one step to the right, start becomes equal to "L_R_R".
- Move the second piece three steps to the right, start becomes equal to "LRR".

Since it is possible to get the string target from start, we return true.

Example 2:

```text
Input: start = "R_L_", target = "LR"
Output: false
Explanation: The 'R' piece in the string start can move one step to the right to obtain "_RL_".
After that, no pieces can move anymore, so it is impossible to obtain the string target from start.
```

Example 3:

```text
Input: start = "_R", target = "R_"
Output: false
Explanation: The piece in the string start can move only to the right, so it is impossible to obtain the string target from start.
```

__Constraints:__

- `n == start.length == target.length`
- `1 <= n <= 10 ^ 5`
- `start` and `target` consist of the characters `'L'`, `'R'`, and `'_'`.

__题目描述:__

给你两个字符串 `start` 和 `target` ，长度均为 `n` 。每个字符串 __仅__ 由字符 `'L'`、 `'R'` 和 `'_'` 组成，其中:

- 字符 `'L'` 和 `'R'` 表示片段，其中片段 `'L'` 只有在其左侧直接存在一个 __空位__ 时才能向 __左__ 移动，而片段 `'R'` 只有在其右侧直接存在一个 __空位__ 时才能向 __右__ 移动。
- 字符 `'_'` 表示可以被 __任意__ `'L'` 或 `'R'` 片段占据的空位。

如果在移动字符串 `start` 中的片段任意次之后可以得到字符串 `target` ，返回 `true` ；否则，返回 `false` 。

__示例:__

示例 1：

```text
输入：start = "_LRR_", target = "LRR"
输出：true
解释：可以从字符串 start 获得 target ，需要进行下面的移动：
```

- 将第一个片段向左移动一步，字符串现在变为 "L_RR_" 。
- 将最后一个片段向右移动一步，字符串现在变为 "L_R_R" 。
- 将第二个片段向右移动三步，字符串现在变为 "LRR" 。

可以从字符串 start 得到 target ，所以返回 true 。

示例 2：

```text
输入：start = "R_L_", target = "LR"
输出：false
解释：字符串 start 中的 'R' 片段可以向右移动一步得到 "_RL_" 。
但是，在这一步之后，不存在可以移动的片段，所以无法从字符串 start 得到 target 。
```

示例 3：

```text
输入：start = "_R", target = "R_"
输出：false
解释：字符串 start 中的片段只能向右移动，所以无法从字符串 start 得到 target 。
```

__提示：__

- `n == start.length == target.length`
- `1 <= n <= 10 ^ 5`
- `start` 和 `target` 由字符 `'L'`、 `'R'` 和 `'_'` 组成

__思路:__

```text
双指针
i 指向 start 的当前位置, j 指向 target 的当前位置
i 和 j 先找到第一个不是 '_' 的位置
如果 i 或 j 只有一个超出字符串长度, 则返回 false, 因为说明有一个字符匹配不上
如果 i 和 j 都没有超出字符串长度, 则判断是否满足以下条件:
start[i] != target[j]
start[i] == 'R' and i > j
start[i] == 'L' and i < j
这是因为首先要保证两个字符相等
其次 'R' 只能向右移动, 'L' 只能向左移动
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool canChange(string start, string target) 
    {
        for (int i = 0, j = 0, n = start.size(); i < n or j < n; i++, j++) 
        {
            while (i < n and start[i] == '_') ++i;
            while (j < n and target[j] == '_') ++j;
            if (((i < n) ^ (j < n)) or (i < n and ((start[i] != target[j]) or (start[i] == 'R' and i > j) or (start[i] == 'L' and i < j)))) return false;
        }
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canChange(String start, String target) {
        for (int i = 0, j = 0, n = start.length(); i < n || j < n; i++, j++) {
            while (i < n && start.charAt(i) == '_') ++i;
            while (j < n && target.charAt(j) == '_') ++j;
            if (((i < n) ^ (j < n)) || (i < n && ((start.charAt(i) != target.charAt(j)) || (start.charAt(i) == 'R' && i > j) || (start.charAt(i) == 'L' && i < j)))) return false;
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def canChange(self, start: str, target: str) -> bool:
        return False if (s := start.replace('_', '')) != target.replace('_', '') else True if not (start := [i for i, c in enumerate(start) if c != '_']) or not (target := [i for i, c in enumerate(target) if c != '_']) else not any((c == 'R' and start[i] > target[i]) or (c == 'L' and start[i] < target[i]) for i, c in enumerate(s))
```
