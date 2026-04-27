# 2827 Number of Beautiful Integers in the Range 范围中美丽整数的数目

__Description:__

You are given positive integers `low`, `high`, and `k`.

A number is beautiful if it meets both of the following conditions:

- The count of even digits in the number is equal to the count of odd digits.
- The number is divisible by `k`.

Return _the number of beautiful integers in the range_ `[low, high]`.

__Example:__

Example 1:

```text
Input: low = 10, high = 20, k = 3
Output: 2
Explanation: There are 2 beautiful integers in the given range: [12,18]. 
```

- 12 is beautiful because it contains 1 odd digit and 1 even digit, and is divisible by k = 3.
- 18 is beautiful because it contains 1 odd digit and 1 even digit, and is divisible by k = 3.

Additionally we can see that:

- 16 is not beautiful because it is not divisible by k = 3.
- 15 is not beautiful because it does not contain equal counts even and odd digits.

It can be shown that there are only 2 beautiful integers in the given range.

Example 2:

```text
Input: low = 1, high = 10, k = 1
Output: 1
Explanation: There is 1 beautiful integer in the given range: [10].
```

- 10 is beautiful because it contains 1 odd digit and 1 even digit, and is divisible by k = 1.

It can be shown that there is only 1 beautiful integer in the given range.

Example 3:

```text
Input: low = 5, high = 5, k = 2
Output: 0
Explanation: There are 0 beautiful integers in the given range.
```

- 5 is not beautiful because it is not divisible by k = 2 and it does not contain equal even and odd digits.

__Constraints:__

- `0 < low <= high <= 10 ^ 9`
- `0 < k <= 20`

__题目描述:__

给你正整数 `low` ， `high` 和 `k` 。

如果一个数满足以下两个条件，那么它是 美丽的 ：

- 偶数数位的数目与奇数数位的数目相同。
- 这个整数可以被 `k` 整除。

请你返回范围 `[low, high]` 中美丽整数的数目。

__示例:__

示例 1：

```text
输入：low = 10, high = 20, k = 3
输出：2
解释：给定范围中有 2 个美丽数字：[12,18]
```

- 12 是美丽整数，因为它有 1 个奇数数位和 1 个偶数数位，而且可以被 k = 3 整除。
- 18 是美丽整数，因为它有 1 个奇数数位和 1 个偶数数位，而且可以被 k = 3 整除。

以下是一些不是美丽整数的例子：

- 16 不是美丽整数，因为它不能被 k = 3 整除。
- 15 不是美丽整数，因为它的奇数数位和偶数数位的数目不相等。

给定范围内总共有 2 个美丽整数。

示例 2：

```text
输入：low = 1, high = 10, k = 1
输出：1
解释：给定范围中有 1 个美丽数字：[10]
```

- 10 是美丽整数，因为它有 1 个奇数数位和 1 个偶数数位，而且可以被 k = 1 整除。

给定范围内总共有 1 个美丽整数。

示例 3：

```text
输入：low = 5, high = 5, k = 2
输出：0
解释：给定范围中有 0 个美丽数字。
```

- 5 不是美丽整数，因为它的奇数数位和偶数数位的数目不相等。

__提示：__

- `0 < low <= high <= 10 ^ 9`
- `0 < k <= 20`

__思路:__

```text
数位 DP
使用 dfs(start, pre, diff, is_limit, is_num) 计算是否满足题意
其中 start 表示当前遍历位
pre 表示之前对于 k 取模的结果
diff 表示奇偶位数差, 这里用奇数减去偶数
is_limit 表示到达上限
is_num 表示一个合理的数
当 start 到达最后一个数位时, 检查 is_num 和 pre, diff 两个数均需要为 0
否则记录当前结果, 如果 is_num 为 False 可以直接从下一个数位开始计算
遍历的下限为 0, 如果 is_num 为 False 那就应该为 1, 因为不能有前导 0
上限为 9, 如果 is_limit 为 True 那么说明到达上限那只能取 s[start]
更新的时候 pre = (10 * pre + d) % k, diff = (diff + (1 if d 为奇数 else -1)) 再遍历下一位
也可以用数组缓存
这里 memo[i][j][p] 表示位数为 i 时, pre % k 为 j 时, diff + n 为 p
这样可以避免 diff 为负数的情况
最后返回时 diff 相应要去和 n 进行比较
时间复杂度为 O(KN ^ 2), 空间复杂度为 O(KN ^ 2)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfBeautifulIntegers(int low, int high, int k) 
    {
        string l = to_string(low), h = to_string(high);
        int n = h.size();
        l = string(n - l.size(), '0') + l;
        vector<vector<vector<int>>> memo(n, vector<vector<int>>(k, vector<int>((n << 1) + 1, -1)));
        auto dfs = [&](auto&& dfs, int start, int pre, int diff, bool limit_low, bool limit_high, bool is_num) -> int
        {
            if (start == n) return is_num and !pre and diff == n;
            if (!limit_low and !limit_high and is_num and memo[start][pre][diff] != -1) return memo[start][pre][diff];
            int result = !is_num and l[start] == '0' ? dfs(dfs, start + 1, pre, diff, true, false, false) : 0;
            for (int down = limit_low ? l[start] - '0' : 0, up = limit_high ? h[start] - '0' : 9, d = max(down, is_num ^ 1); d <= up; d++) result += dfs(dfs, start + 1, (10 * pre + d) % k, diff + ((d & 1) << 1) - 1, limit_low and d == down, limit_high and d == up, true); 
            return !limit_low and !limit_high and is_num ? memo[start][pre][diff] = result : result;
        };
        return dfs(dfs, 0, 0, n, true, true, false);
    }
};
```

__Java__:

```Java
class Solution {
    private int n, k;
    private char[] c1, c2;
    private int[][][] memo;

    public int numberOfBeautifulIntegers(int low, int high, int k) {
        String l = String.valueOf(low), h = String.valueOf(high);
        n = h.length();
        c1 = ("0".repeat(n - l.length()) + l).toCharArray();
        c2 = h.toCharArray();
        memo = new int[n][k][(n << 1) + 1];
        for (var row : memo) for (var m : row) Arrays.fill(m, -1);
        return dfs(0, 0, n, true, true, false, k);
    }

    private int dfs(int start, int pre, int diff, boolean limitLow, boolean limitHigh, boolean isNum, int k) {
        if (start == n) return isNum && pre == 0 && diff == n ? 1 : 0;
        if (!limitLow && !limitHigh && isNum && memo[start][pre][diff] != -1) return memo[start][pre][diff];
        int result = !isNum && c1[start] == '0' ? dfs(start + 1, pre, diff, true, false, false, k) : 0, down = limitLow ? c1[start] - '0' : 0, up = limitHigh ? c2[start] - '0' : 9;
        for (int d = Math.max(down, isNum ? 0 : 1); d <= up; d++) result += dfs(start + 1, (10 * pre + d) % k, diff + ((d & 1) << 1) - 1, limitLow && d == down, limitHigh && d == up, true, k);
        return !limitLow && !limitHigh && isNum ? (memo[start][pre][diff] = result) : result;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfBeautifulIntegers(self, low: int, high: int, k: int) -> int:
        def helper(s: str) -> int:
            @lru_cache(None)
            def dfs(start: int, pre: int, diff: int, is_limit: bool, is_num: bool) -> int:
                if start == len(s):
                    return int(is_num and not pre and not diff)
                result, down, up = 0 if is_num else dfs(start + 1, pre, diff, False, False), 0 if is_num else 1, int(s[start]) if is_limit else 9
                for d in range(down, up + 1):
                    result += dfs(start + 1, (10 * pre + d) % k, diff + ((d & 1) << 1) - 1, is_limit and d == up, True)
                return result
            return dfs(0, 0, 0, True, False)
        return helper(str(high)) - helper(str(low - 1))
```
