# 926 Flip String to Monotone Increasing 将字符串翻转到单调递增

__Description__:
A binary string is monotone increasing if it consists of some number of 0's (possibly none), followed by some number of 1's (also possibly none).

You are given a binary string s. You can flip s[i] changing it from 0 to 1 or from 1 to 0.

Return the minimum number of flips to make s monotone increasing.

__Example:__

Example 1:

Input: s = "00110"
Output: 1
Explanation: We flip the last digit to get 00111.

Example 2:

Input: s = "010110"
Output: 2
Explanation: We flip to get 011111, or alternatively 000111.

Example 3:

Input: s = "00011000"
Output: 2
Explanation: We flip to get 00000000.

__Constraints:__

1 <= s.length <= 10^5
s[i] is either '0' or '1'.

__题目描述__:
如果一个由 '0' 和 '1' 组成的字符串，是以一些 '0'（可能没有 '0'）后面跟着一些 '1'（也可能没有 '1'）的形式组成的，那么该字符串是单调递增的。

我们给出一个由字符 '0' 和 '1' 组成的字符串 S，我们可以将任何 '0' 翻转为 '1' 或者将 '1' 翻转为 '0'。

返回使 S 单调递增的最小翻转次数。

__示例 :__

示例 1：

输入："00110"
输出：1
解释：我们翻转最后一位得到 00111.

示例 2：

输入："010110"
输出：2
解释：我们翻转得到 011111，或者是 000111。

示例 3：

输入："00011000"
输出：2
解释：我们翻转得到 00000000。

__提示:__

1 <= S.length <= 20000
S 中只包含字符 '0' 和 '1'

__思路__:

动态规划
设 dp[i] 为下标为 i 对应的翻转次数
如果当前字符为 '1', 这个字符不影响翻转次数 dp[i] = dp[i - 1]
如果当前字符为 '0', 要么 s[i + 1] = '1' 发生一次翻转 dp[i] = dp[i - 1] + 1, 要么将之前的 '1' 全部翻转为 '0', 取两者的最小值, 即 dp[i] = min(dp[i - 1] + 1, count of '1')
注意到 dp[i] 仅与 dp[i - 1] 有关, 可以将空间复杂度优化为 O(1)
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minFlipsMonoIncr(string s) 
    {
        int result = 0, one = 0, n = s.size();
        for (const auto& c : s) 
        {
            if (c == '1') ++one;
            else result = min(result + 1, one);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minFlipsMonoIncr(String s) {
        int result = 0, one = 0, n = s.length();
        for (char c : s.toCharArray()) {
            if (c == '1') ++one;
            else result = Math.min(result + 1, one);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minFlipsMonoIncr(self, s: str) -> int:
        result, one, n = 0, 0, len(s)
        for c in s:
            result = min((one := one + 1 if c == '1' else one), (result := result + 1 if c == '0' else result))
        return result
```
