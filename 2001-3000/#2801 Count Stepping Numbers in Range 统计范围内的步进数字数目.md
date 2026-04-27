# 2801 Count Stepping Numbers in Range 统计范围内的步进数字数目

__Description:__

Given two positive integers `low` and `high` represented as strings, find the count of __stepping numbers__ in the inclusive range `[low, high]`.

A __stepping number__ is an integer such that all of its adjacent digits have an absolute difference of __exactly__ `1`.

Return _an integer denoting the count of stepping numbers in the inclusive range_ `[low, high]`_._

Since the answer may be very large, return it __modulo__ `10 ^ 9 + 7`.

Note: A stepping number should not have a leading zero.

__Example:__

Example 1:

```text
Input: low = "1", high = "11"
Output: 10
Explanation: The stepping numbers in the range [1,11] are 1, 2, 3, 4, 5, 6, 7, 8, 9 and 10. There are a total of 10 stepping numbers in the range. Hence, the output is 10.
```

Example 2:

```text
Input: low = "90", high = "101"
Output: 2
Explanation: The stepping numbers in the range [90,101] are 98 and 101. There are a total of 2 stepping numbers in the range. Hence, the output is 2.
```

__Constraints:__

- `1 <= int(low) <= int(high) < 10 ^ 100`
- `1 <= low.length, high.length <= 100`
- `low` and `high` consist of only digits.
- `low` and `high` don't have any leading zeros.

__题目描述:__

给你两个正整数 `low` 和 `high` ，都用字符串表示，请你统计闭区间 `[low, high]` 内的 __步进数字__ 数目。

如果一个整数相邻数位之间差的绝对值都 __恰好__ 是 `1` ，那么这个数字被称为 __步进数字__ 。

请你返回一个整数，表示闭区间 `[low, high]` 之间步进数字的数目。

由于答案可能很大，请你将它对 `10 ^ 9 + 7` __取余__ 后返回。

注意：步进数字不能有前导 0 。

__示例:__

示例 1：

```text
输入：low = "1", high = "11"
输出：10
解释：区间 [1,11] 内的步进数字为 1 ，2 ，3 ，4 ，5 ，6 ，7 ，8 ，9 和 10 。总共有 10 个步进数字。所以输出为 10 。
```

示例 2：

```text
输入：low = "90", high = "101"
输出：2
解释：区间 [90,101] 内的步进数字为 98 和 101 。总共有 2 个步进数字。所以输出为 2 。
```

__提示：__

- `1 <= int(low) <= int(high) < 10 ^ 100`
- `1 <= low.length, high.length <= 100`
- `low` 和 `high` 只包含数字。
- `low` 和 `high` 都不含前导 0 。

__思路:__

```text
数位 DP
第一种数位 DP
可用 helper(high) - helper(low - 1) 得到
helper(num) 计算 [0, num] 满足题意的数量
使用记忆化搜索的方式
dfs(start, pre, is_limit, is_num)
其中 start 表示从该位开始, pre 表示上一次选择的数位, is_limit 表示是否到达上限, 如果到达上限那么下次选择的数字最大只能是 num[start], 否则可以是 9, is_num 表示已经填过数字, 如果填过数字, 至少从 1 开始因为不能有前导 0, 最后返回的时候也只需要返回 is_num 判断该数字是否合法
入口为 dfs(0, 0, True, False), 从 0 位开始上一位为 0, 一开始设定到达上限, 否则永远不可能到达上限, 一开始没填入数字
如果没填入数字可以跳过下一次遍历直接到 dfs(start, pre, False, False)
否则根据 is_limit 和 is_num 判断遍历的范围
设改轮遍历结果是 d, 如果没填入数字或者上一次和这一次绝对值相差 1
可以进入下一轮 dfs(start + 1, d, is_limit and d == up, True), 后面一定都填过数字
第二种数位 DP
直接将 low 用前导 0 补齐到 high 的位数
limit_low 表示到达 low 的下限
limit_high 表示到达 high 的上限
其余和第一种几乎相同
时间复杂度为 O(N), 空间复杂度为 O(N), N 为 high 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countSteppingNumbers(string low, string high) 
    {
        int n = high.size(), MOD = 1e9 + 7;
        low = string(n - low.size(), '0') + low;
        vector<vector<long long>> memo(n, vector<long long>(10, -1LL));
        auto dfs = [&](auto&& dfs, int start, int pre, bool limit_low, bool limit_high, bool is_num) -> int
        {
            if (start == n) return is_num;
            if (!limit_low and !limit_high and is_num and memo[start][pre] != -1) return memo[start][pre];
            long long result = !is_num and low[start] == '0' ? dfs(dfs, start + 1, pre, true, false, false) % MOD : 0LL;
            for (int down = limit_low ? low[start] - '0' : 0, up = limit_high ? high[start] - '0' : 9, d = max(down, is_num ^ 1); d <= up; d++) if (!is_num or abs(d - pre) == 1) result = (result + dfs(dfs, start + 1, d, limit_low and d == down, limit_high and d == up, true)) % MOD; 
            return !limit_low and !limit_high and is_num ? memo[start][pre] = result : result;
        };
        return dfs(dfs, 0, 0, true, true, false);
    }
};
```

__Java__:

```Java
class Solution {
    private int n;
    private char[] c1, c2;
    private int[][] memo;
    private static final int MOD = 1_000_000_007;

    public int countSteppingNumbers(String low, String high) {
        n = high.length();
        c1 = ("0".repeat(n - low.length()) + low).toCharArray();
        c2 = high.toCharArray();
        memo = new int[n][10];
        for (var m : memo) Arrays.fill(m, -1);
        return dfs(0, 0, true, true, false);
    }

    private int dfs(int start, int pre, boolean limitLow, boolean limitHigh, boolean isNum) {
        if (start == n) return isNum ? 1 : 0;
        if (!limitLow && !limitHigh && isNum && memo[start][pre] != -1) return memo[start][pre];
        int result = !isNum && c1[start] == '0' ? dfs(start + 1, pre, true, false, false) % MOD : 0, down = limitLow ? c1[start] - '0' : 0, up = limitHigh ? c2[start] - '0' : 9;
        for (int d = Math.max(down, isNum ? 0 : 1); d <= up; d++) if (!isNum || Math.abs(d - pre) == 1) result = (result + dfs(start + 1, d, limitLow && d == down, limitHigh && d == up, true)) % MOD;
        return !limitLow && !limitHigh && isNum ? (memo[start][pre] = result) : result;
    }
}
```

__Python__:

```Python
class Solution:
    def countSteppingNumbers(self, low: str, high: str) -> int:
        def helper(s: str) -> int:
            @lru_cache(None)
            def dfs(start: int, pre: int, is_limit: bool, is_num: bool) -> int:
                if start == len(s):
                    return int(is_num)
                result, down, up = 0 if is_num else dfs(start + 1, pre, False, False), 0 if is_num else 1, int(s[start]) if is_limit else 9
                for d in range(down, up + 1):
                    if not is_num or abs(d - pre) == 1:
                        result += dfs(start + 1, d, is_limit and d == up, True)
                return result % (10 ** 9 + 7)
            return dfs(0, 0, True, False)
        return (helper(high) - helper(str(int(low) - 1))) % (10 ** 9 + 7)
```
