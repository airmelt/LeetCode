# 1221 Split a String in Balanced Strings 分割平衡字符串

__Description__:
Balanced strings are those who have equal quantity of 'L' and 'R' characters.

Given a balanced string s split it in the maximum amount of balanced strings.

Return the maximum amount of splitted balanced strings.

__Example:__

Example 1:

Input: s = "RLRRLLRLRL"
Output: 4
Explanation: s can be split into "RL", "RRLL", "RL", "RL", each substring contains same number of 'L' and 'R'.

Example 2:

Input: s = "RLLLLRRRLR"
Output: 3
Explanation: s can be split into "RL", "LLLRRR", "LR", each substring contains same number of 'L' and 'R'.

Example 3:

Input: s = "LLLLRRRR"
Output: 1
Explanation: s can be split into "LLLLRRRR".

Example 4:

Input: s = "RLRRRLLRLL"
Output: 2
Explanation: s can be split into "RL", "RRRLLRLL", since each substring contains an equal number of 'L' and 'R'

__Constraints:__

1 <= s.length <= 1000
s[i] = 'L' or 'R'

__题目描述__:
在一个「平衡字符串」中，'L' 和 'R' 字符的数量是相同的。

给出一个平衡字符串 s，请你将它分割成尽可能多的平衡字符串。

返回可以通过分割得到的平衡字符串的最大数量。

__示例 :__

示例 1：

输入：s = "RLRRLLRLRL"
输出：4
解释：s 可以分割为 "RL", "RRLL", "RL", "RL", 每个子字符串中都包含相同数量的 'L' 和 'R'。

示例 2：

输入：s = "RLLLLRRRLR"
输出：3
解释：s 可以分割为 "RL", "LLLRRR", "LR", 每个子字符串中都包含相同数量的 'L' 和 'R'。

示例 3：

输入：s = "LLLLRRRR"
输出：1
解释：s 只能保持原样 "LLLLRRRR".

__提示：__

1 <= s.length <= 1000
s[i] = 'L' 或 'R'

__思路__:

相当于 R和 L从头开始匹配, 相同的压入栈, 不同的弹出栈, 只有栈中的完全匹配(栈空, 即L和 R的数量相等), 结果➕1
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int balancedStringSplit(string s) 
    {
        int result = 0, count = 0;
        for (auto c : s) 
        {
            if (c == 'L') ++count;
            else --count;
            if (!count) ++result;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int balancedStringSplit(String s) {
        int result = 0, count = 0;
        for (char c : s.toCharArray()) {
            if (c == 'L') ++count;
            else --count;
            if (count == 0) ++result;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def balancedStringSplit(self, s: str) -> int:
        result, count = 0, 0
        for i in s:
            count += 1 if i == 'L' else -1
            if not count:
                result += 1
        return result
```
