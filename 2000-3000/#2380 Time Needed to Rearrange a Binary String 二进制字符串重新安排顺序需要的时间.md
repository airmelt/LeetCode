# 2380 Time Needed to Rearrange a Binary String 二进制字符串重新安排顺序需要的时间

__Description:__

You are given a binary string `s`. In one second, __all__ occurrences of `"01"` are __simultaneously__ replaced with `"10"`. This process __repeats__ until no occurrences of `"01"` exist.

Return the number of seconds needed to complete this process.

__Example:__

Example 1:

```text
Input: s = "0110101"
Output: 4
Explanation: 
After one second, s becomes "1011010".
After another second, s becomes "1101100".
After the third second, s becomes "1110100".
After the fourth second, s becomes "1111000".
No occurrence of "01" exists any longer, and the process needed 4 seconds to complete,
so we return 4.
```

Example 2:

```text
Input: s = "11100"
Output: 0
Explanation:
No occurrence of "01" exists in s, and the processes needed 0 seconds to complete,
so we return 0.
```

__Constraints:__

- `1 <= s.length <= 1000`
- `s[i]` is either `'0'` or `'1'`.

Follow up:

Can you solve this problem in O(n) time complexity?

__题目描述:__

给你一个二进制字符串 `s` 。在一秒之中，__所有__ 子字符串 `"01"` __同时__ 被替换成 `"10"` 。这个过程持续进行到没有 `"01"` 存在。

请你返回完成这个过程所需要的秒数。

__示例:__

示例 1：

```text
输入：s = "0110101"
输出：4
解释：
一秒后，s 变成 "1011010" 。
再过 1 秒后，s 变成 "1101100" 。
第三秒过后，s 变成 "1110100" 。
第四秒后，s 变成 "1111000" 。
此时没有 "01" 存在，整个过程花费 4 秒。
所以我们返回 4 。
```

示例 2：

```text
输入：s = "11100"
输出：0
解释：
s 中没有 "01" 存在，整个过程花费 0 秒。
所以我们返回 0 。
```

__提示：__

- `1 <= s.length <= 1000`
- `s[i]` 要么是 `'0'` ，要么是 `'1'` 。

进阶：

你能以 O(n) 的时间复杂度解决这个问题吗？

__思路:__

```text
1. 暴力法
每次将 "01" 替换为 "10", 直到没有 "01" 存在
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N), 最极端的情况是 "0000....01", 需要替换 N 次
2. 动态规划
设 dp[i] 表示字符串 s[0, i] 中 "01" 替换为 "10" 的最大次数
对于当前的字符 c, 如果 c == '0', dp[i] = dp[i - 1], 不需要移动
如果 c == '1', dp[i] = max(dp[i - 1] + 1, pre), pre 表示前一个 '0' 的个数, dp[i - 1] + 1 是由于前面可能出现 '1' 导致出现 "11" 字符串会一起向左移动
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int secondsToRemoveOccurrences(string s) 
    {
        int result = 0, pre = 0;
        for (const auto& c : s) 
        {
            if (c == '0') ++pre;
            else if (pre) result = max(result + 1, pre);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int secondsToRemoveOccurrences(String s) {
        int result = 0, pre = 0;
        for (char c : s.toCharArray()) {
            if (c == '0') ++pre;
            else if (pre != 0) result = Math.max(result + 1, pre);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def secondsToRemoveOccurrences(self, s: str) -> int:
        return 0 if not "01" in s else 1 + self.secondsToRemoveOccurrences(s.replace("01", "10"))
```
