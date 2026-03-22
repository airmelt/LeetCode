# 2381 Shifting Letters II 字母移位 II

__Description:__

You are given a string `s` of lowercase English letters and a 2D integer array `shifts` where `shifts[i] = [start_i, end_i, direction_i]`. For every `i`, __shift__ the characters in `s` from the index `start_i` to the index `end_i` (__inclusive__) forward if `direction_i = 1`, or shift the characters backward if `direction_i = 0`.

Shifting a character __forward__ means replacing it with the __next__ letter in the alphabet (wrapping around so that `'z'` becomes `'a'`). Similarly, shifting a character __backward__ means replacing it with the __previous__ letter in the alphabet (wrapping around so that `'a'` becomes `'z'`).

Return _the final string after all such shifts to_ `s` _are applied_.

__Example:__

Example 1:

```text
Input: s = "abc", shifts = [[0,1,0],[1,2,1],[0,2,1]]
Output: "ace"
Explanation: Firstly, shift the characters from index 0 to index 1 backward. Now s = "zac".
Secondly, shift the characters from index 1 to index 2 forward. Now s = "zbd".
Finally, shift the characters from index 0 to index 2 forward. Now s = "ace".
```

Example 2:

```text
Input: s = "dztz", shifts = [[0,0,0],[1,1,1]]
Output: "catz"
Explanation: Firstly, shift the characters from index 0 to index 0 backward. Now s = "cztz".
Finally, shift the characters from index 1 to index 1 forward. Now s = "catz".
```

__Constraints:__

- `1 <= s.length, shifts.length <= 5 * 10 ^ 4`
- `shifts[i].length == 3`
- `0 <= start_i <= end_i < s.length`
- `0 <= direction_i <= 1`
- `s` consists of lowercase English letters.

__题目描述:__

给你一个小写英文字母组成的字符串 `s` 和一个二维整数数组 `shifts` ，其中 `shifts[i] = [start_i, end_i, direction_i]` 。对于每个 `i` ，将 `s` 中从下标 `start_i` 到下标 `end_i` （两者都包含）所有字符都进行移位运算，如果 `direction_i = 1` 将字符向后移位，如果 `direction_i = 0` 将字符向前移位。

将一个字符 __向后__ 移位的意思是将这个字符用字母表中 __下一个__ 字母替换（字母表视为环绕的，所以 `'z'` 变成 `'a'`）。类似的，将一个字符 __向前__ 移位的意思是将这个字符用字母表中 __前一个__ 字母替换（字母表是环绕的，所以 `'a'` 变成 `'z'` ）。

请你返回对 `s` 进行所有移位操作以后得到的最终字符串。

__示例:__

示例 1：

```text
输入：s = "abc", shifts = [[0,1,0],[1,2,1],[0,2,1]]
输出："ace"
解释：首先，将下标从 0 到 1 的字母向前移位，得到 s = "zac" 。
然后，将下标从 1 到 2 的字母向后移位，得到 s = "zbd" 。
最后，将下标从 0 到 2 的字符向后移位，得到 s = "ace" 。
```

示例 2:

```text
输入：s = "dztz", shifts = [[0,0,0],[1,1,1]]
输出："catz"
解释：首先，将下标从 0 到 0 的字母向前移位，得到 s = "cztz" 。
最后，将下标从 1 到 1 的字符向后移位，得到 s = "catz" 。
```

__提示：__

- `1 <= s.length, shifts.length <= 5 * 10 ^ 4`
- `shifts[i].length == 3`
- `0 <= start_i <= end_i < s.length`
- `0 <= direction_i <= 1`
- `s` 只包含小写英文字母。

__思路:__

```text
差分数组
遍历 shifts, 计算差分数组 diff, diff[i] 表示第 i 个字符的移位次数
diff[shift[0]] += (shift[2] << 1) - 1, diff[shift[1] + 1] -= (shift[2] << 1) - 1
这里可以把右移 1 位的操作 0 * 2 - 1, 左移 1 位的操作 1 * 2 - 1
这样就能把移位操作统一为一次加减操作
然后计算 diff 的前缀和
同时遍历 s 和 diff, 计算移位后的字符
注意移位可能有负数, 所以需要先加上 26 再取模
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string shiftingLetters(string s, vector<vector<int>>& shifts) 
    {
        int n = s.size();
        vector<int> diff(n + 1);
        for (const auto& shift : shifts) 
        {
            diff[shift[0]] += (shift[2] << 1) - 1;
            diff[shift[1] + 1] -= (shift[2] << 1) - 1;
        }
        string result;
        for (int i = 0, pre = 0; i < n; i++) result += ((char)(((s[i] - 'a' + (pre = pre + diff[i])) % 26 + 26) % 26 + 'a'));    
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String shiftingLetters(String s, int[][] shifts) {
        int n = s.length(), diff[] = new int[n + 1];
        for (int[] shift : shifts) {
            diff[shift[0]] += (shift[2] << 1) - 1;
            diff[shift[1] + 1] -= (shift[2] << 1) - 1;
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 0, pre = 0; i < n; i++) sb.append(((char)(((s.charAt(i) - 'a' + (pre = pre + diff[i])) % 26 + 26) % 26 + 'a')));    
        return sb.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def shiftingLetters(self, s: str, shifts: List[List[int]]) -> str:
        diff = [0] * (len(s) + 1)
        for start, end, direction in shifts:
            diff[start] += (direction << 1) - 1
            diff[end + 1] -= (direction << 1) - 1
        return ''.join(chr((ord(c) + shift - ord('a')) % 26 + ord('a')) for c, shift in zip(s, accumulate(diff)))
```
