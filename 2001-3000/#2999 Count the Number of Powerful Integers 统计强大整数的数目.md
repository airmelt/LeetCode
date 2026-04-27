# 2999 Count the Number of Powerful Integers 统计强大整数的数目

__Description:__

You are given three integers `start`, `finish`, and `limit`. You are also given a __0-indexed__ string `s` representing a __positive__ integer.

A __positive__ integer `x` is called __powerful__ if it ends with `s` (in other words, `s` is a __suffix__ of `x`) and each digit in `x` is at most `limit`.

Return _the __total__ number of powerful integers in the range_ `[start..finish]`.

A string `x` is a suffix of a string `y` if and only if `x` is a substring of `y` that starts from some index (__including__ `0`) in `y` and extends to the index `y.length - 1`. For example, `25` is a suffix of `5125` whereas `512` is not.

__Example:__

Example 1:

```text
Input: start = 1, finish = 6000, limit = 4, s = "124"
Output: 5
Explanation: The powerful integers in the range [1..6000] are 124, 1124, 2124, 3124, and, 4124. All these integers have each digit <= 4, and "124" as a suffix. Note that 5124 is not a powerful integer because the first digit is 5 which is greater than 4.
It can be shown that there are only 5 powerful integers in this range.
```

Example 2:

```text
Input: start = 15, finish = 215, limit = 6, s = "10"
Output: 2
Explanation: The powerful integers in the range [15..215] are 110 and 210. All these integers have each digit <= 6, and "10" as a suffix.
It can be shown that there are only 2 powerful integers in this range.
```

Example 3:

```text
Input: start = 1000, finish = 2000, limit = 4, s = "3000"
Output: 0
Explanation: All integers in the range [1000..2000] are smaller than 3000, hence "3000" cannot be a suffix of any integer in this range.
```

__Constraints:__

- `1 <= start <= finish <= 10 ^ 15`
- `1 <= limit <= 9`
- `1 <= s.length <= floor(log10(finish)) + 1`
- `s` only consists of numeric digits which are at most `limit`.
- `s` does not have leading zeros.

__题目描述:__

给你三个整数 `start` ， `finish` 和 `limit` 。同时给你一个下标从 __0__ 开始的字符串 `s` ，表示一个 __正__ 整数。

如果一个 __正__ 整数 `x` 末尾部分是 `s` （换句话说， `s` 是 `x` 的 __后缀__），且 `x` 中的每个数位至多是 `limit` ，那么我们称 `x` 是 __强大的__ 。

请你返回区间 `[start..finish]` 内强大整数的 __总数目__ 。

如果一个字符串 `x` 是 `y` 中某个下标开始（__包括__ `0` ），到下标为 `y.length - 1` 结束的子字符串，那么我们称 `x` 是 `y` 的一个后缀。比方说， `25` 是 `5125` 的一个后缀，但不是 `512` 的后缀。

__示例:__

示例 1：

```text
输入：start = 1, finish = 6000, limit = 4, s = "124"
输出：5
解释：区间 [1..6000] 内的强大数字为 124 ，1124 ，2124 ，3124 和 4124 。这些整数的各个数位都 <= 4 且 "124" 是它们的后缀。注意 5124 不是强大整数，因为第一个数位 5 大于 4 。
这个区间内总共只有这 5 个强大整数。
```

示例 2：

```text
输入：start = 15, finish = 215, limit = 6, s = "10"
输出：2
解释：区间 [15..215] 内的强大整数为 110 和 210 。这些整数的各个数位都 <= 6 且 "10" 是它们的后缀。
这个区间总共只有这 2 个强大整数。
```

示例 3：

```text
输入：start = 1000, finish = 2000, limit = 4, s = "3000"
输出：0
解释：区间 [1000..2000] 内的整数都小于 3000 ，所以 "3000" 不可能是这个区间内任何整数的后缀。
```

__提示：__

- `1 <= start <= finish <= 10 ^ 15`
- `1 <= limit <= 9`
- `1 <= s.length <= floor(log10(finish)) + 1`
- `s` 数位中每个数字都小于等于 `limit` 。
- `s` 不包含任何前导 0 。

__思路:__

```text
数位 DP
记 finish 为 finish 的字符串形式
记 start 为 start 以及补上 finish 和 start 位数个前缀 0 的字符串
设 dfs(i, low_limit, high_limit) 表示当前位为 i, 达到下限 low_limit 和上限 high_limit
当 i 达到边界时返回 1
如果前面已经访问过 i 记录下 !low_limit 和 !high_limit 的状态 memo[i], 访问过的可以直接返回
否则下边界 low = start[i] if low_limit else 0, 达到边界的时候只能用 start[i] 没达到边界可以取最小值 0
上边界 finish[i] if high_limit else 9, 达到边界的时候只能用 finish[i], 没达到边界的时候可以取 9, 这里还有 limit 的限制
所以上边界为 min(high, limit)
数位可以取的范围为 [low, min(high, limit)] 两边闭区间
dfs 的入口为 dfs(0, true, true)
表示从下标 0 开始, 一开始需要控制边界
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long numberOfPowerfulInt(long long start, long long finish, int limit, string s) 
    {
        string high = to_string(finish);
        string low = to_string(start);
        int n = high.size(), diff = n - s.size();
        low = string(n - low.size(), '0') + low;
        vector<long long> memo(n, -1);
        auto dfs = [&](this auto&& dfs, int i, bool low_limit, bool high_limit) -> long long
        {
            if (i == n) return 1LL;
            if (!low_limit and !high_limit and memo[i] != -1) return memo[i];
            int l = low_limit ? low[i] - '0' : 0, h = high_limit ? high[i] - '0' : 9, d = 0;
            long long result = 0LL;
            if (i < diff) for (int d = l; d <= min(h, limit); d++) result += dfs(i + 1, low_limit and d == l, high_limit and d == h);
            else if (l <= (d = s[i - diff] - '0') and d <= h) result = dfs(i + 1, low_limit and d == l, high_limit and d == h);
            return !low_limit and !high_limit ? (memo[i] = result) : result;
        };
        return dfs(0, 1, 1);
    }
};
```

__Java__:

```Java
class Solution {
    private char[] start, finish, s;
    private int n, diff, limit;
    private long[] memo;

    public long numberOfPowerfulInt(long start, long finish, int limit, String s) {
        this.limit = limit;
        this.s = s.toCharArray();
        this.finish = String.valueOf(finish).toCharArray();
        n = this.finish.length;
        diff = n - s.length();
        this.start = ("0".repeat(n - String.valueOf(start).length()) + String.valueOf(start)).toCharArray();
        memo = new long[n];
        Arrays.fill(memo, -1L);
        return dfs(0, true, true);
    }

    private long dfs(int i, boolean limitLow, boolean limitHigh) {
        if (i == n) return 1L;
        if (!limitLow && !limitHigh && memo[i] != -1) return memo[i];
        int low = limitLow ? start[i] - '0' : 0, high = limitHigh ? finish[i] - '0' : 9, d = 0;
        long result = 0L;
        if (i < diff) for (d = low; d <= Math.min(high, limit); d++) result += dfs(i + 1, limitLow && d == low, limitHigh && d == high);
        else if (low <= (d = s[i - diff] - '0') && d <= high) result = dfs(i + 1, limitLow && d == low, limitHigh && d == high);
        return !limitLow && !limitHigh ? memo[i] = result : result;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfPowerfulInt(self, start: int, finish: int, limit: int, s: str) -> int:
        start, diff = list(map(int, str(start).zfill(n := len(finish := list(map(int, str(finish))))))), n - len(s)

        @lru_cache(None)
        def dfs(i: int, low_limit: bool, high_limit: bool) -> int:
            if i == n:
                return 1
            low, high, result = start[i] if low_limit else 0, finish[i] if high_limit else 9, 0
            if i < diff:
                for d in range(low, min(high, limit) + 1):
                    result += dfs(i + 1, low_limit and d == low, high_limit and d == high)
            elif low <= (d := int(s[i - diff])) <= high:
                    return dfs(i + 1, low_limit and d == low, high_limit and d == high)
            return result
        return dfs(0, 1, 1)
```
