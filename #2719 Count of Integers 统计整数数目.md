# 2719 Count of Integers 统计整数数目

__Description:__

You are given two numeric strings `num1` and `num2` and two integers `max_sum` and `min_sum`. We denote an integer `x` to be _good_ if:

- `num1 <= x <= num2`
- `min_sum <= digit_sum(x) <= max_sum`.

Return _the number of good integers_. Since the answer may be large, return it modulo `10 ^ 9 + 7`.

Note that `digit_sum(x)` denotes the sum of the digits of `x`.

__Example:__

Example 1:

```text
Input:  num1 = "1", num2 = "12", `min_sum` = 1, max_sum = 8
Output:  11
Explanation:  There are 11 integers whose sum of digits lies between 1 and 8 are 1,2,3,4,5,6,7,8,10,11, and 12. Thus, we return 11.
```

Example 2:

```text
Input:  num1 = "1", num2 = "5", `min_sum` = 1, max_sum = 5
Output:  5
Explanation:  The 5 integers whose sum of digits lies between 1 and 5 are 1,2,3,4, and 5. Thus, we return 5.
```

__Constraints:__

- `1 <= num1 <= num2 <= 10 ^ 22`
- `1 <= min_sum <= max_sum <= 400`

__题目描述:__

给你两个数字字符串 `num1` 和 `num2` ，以及两个整数 `max_sum` 和 `min_sum` 。如果一个整数 `x` 满足以下条件，我们称它是一个好整数:

- `num1 <= x <= num2`
- `min_sum <= digit_sum(x) <= max_sum`.

请你返回好整数的数目。答案可能很大，请返回答案对 `10 ^ 9 + 7` 取余后的结果。

注意， `digit_sum(x)` 表示 `x` 各位数字之和。

__示例:__

示例 1：

```text
输入：num1 = "1", num2 = "12", min_sum = 1, max_sum = 8
输出：11
解释：总共有 11 个整数的数位和在 1 到 8 之间，分别是 1,2,3,4,5,6,7,8,10,11 和 12 。所以我们返回 11 。
```

示例 2：

```text
输入：num1 = "1", num2 = "5", min_sum = 1, max_sum = 5
输出：5
解释：数位和在 1 到 5 之间的 5 个整数分别为 1,2,3,4 和 5 。所以我们返回 5 。
```

__提示：__

- `1 <= num1 <= num2 <= 10 ^ 22`
- `1 <= min_sum <= max_sum <= 400`

__思路:__

```text
数位 DP
记 num2 的数字对应的数组为 high
由于这里的前导 0 不影响结果, 为了使 num1 和 num2 同时遍历可以将 num1 加上前导 0 使得两者长度相等
记 num1 的数字对应的数组为 low
设 dfs(start, cur, limit_low, limit_high) 表示在遍历到第 start 位时, 当前的数位和为 cur, 当前最低位是否可选全部为 limit_low, 当前最高位是否可选全部为 limit_high
则当 cur > max_sum 剪枝, 后面的已经比 max_sum 大, 一定不合题意, 直接返回 0
如果遍历到最后一位 start 到 num2 的长度 n, 则检查 cur 是否不小于 min_sum, 是的话就有一种路径可能
left 表示当前为可选的最小值, 如果之前选了最小值, 即 limit_low 为 true, 那么下一位还是要选最小值即 low[start], 否则可以选 0
right 表示当前为可选的最大值, 如果之前选了最大值, 即 limit_high 为 true, 那么下一位还是要选最小值即 high[start], 否则可以选 9
从 [left, right] 中选择当前位 d
将 dfs(start + 1, cur + d, limit_low and d == left, limit_high and d == right) 累加到结果中
初始化 dfs(0, 0, 1, 1) 表示从 0 开始, 当前累加和为 0, 受到大小限制
时间复杂度为 O(MND), 空间复杂度为 O(MN), 其中 N 为 num2 的长度, M 为cur 最多需要检查的状态数, 本题为 min(9n, max_sum), 即 min(99999..., max_sum), 最多累加到 n 个 9 的情况, D 为每次检查的状态数, 本体为 10, 即每次遍历的数位个数
```

__代码:__

__C++__:

```C++
class Solution {
public:
    int count(string num1, string num2, int min_sum, int max_sum) {
        int n = num2.size();
        num1 = string(n - num1.size(), '0') + num1;
        vector memo(n, vector<int>(min(9 * n, max_sum) + 1, -1));
        auto dfs = [&](this auto&& dfs, int start, int cur, bool limit_low, bool limit_high) -> int
        {
            if (cur > max_sum) return 0;
            if (start == n) return cur >= min_sum;
            if (!limit_low and !limit_high and memo[start][cur] != -1) return memo[start][cur];
            int result = 0, MOD = 1e9 + 7;
            for (int left = limit_low ? num1[start] - '0' : 0, right = limit_high ? num2[start] - '0' : 9, d = left; d <= right; d++) result = (result + dfs(start + 1, cur + d, limit_low and d == left, limit_high and d == right)) % MOD;
            return (!limit_low and !limit_high) ? (memo[start][cur] = result) : result;
        };
        return dfs(0, 0, true, true);
    }
};
```

__Java__:

```Java
class Solution {
    public int count(String num1, String num2, int min_sum, int max_sum) {
        int n = num2.length(), memo[][] = new int[n][Math.min(9 * n, max_sum) + 1];
        num1 = "0".repeat(n - num1.length()) + num1;
        for (int[] row : memo) Arrays.fill(row, -1);
        return dfs(0, 0, true, true, num1.toCharArray(), num2.toCharArray(), min_sum, max_sum, memo);
    }

    private int dfs(int start, int cur, boolean limitLow, boolean limitHigh, char[] num1, char[] num2, int min_sum, int max_sum, int[][] memo) {
        if (cur > max_sum) return 0;
        if (start == num2.length) return cur >= min_sum ? 1 : 0;
        if (!limitLow && !limitHigh && memo[start][cur] != -1) return memo[start][cur];
        int result = 0, MOD = 1_000_000_007;
        for (int left = limitLow ? num1[start] - '0' : 0, right = limitHigh ? num2[start] - '0' : 9, d = left; d <= right; d++) result = (result + dfs(start + 1, cur + d, limitLow && d == left, limitHigh && d == right, num1, num2, min_sum, max_sum, memo)) % MOD;
        return (!limitLow && !limitHigh) ? (memo[start][cur] = result) : result;
    }
}
```

__Python__:

```Python
class Solution:
    def count(self, num1: str, num2: str, min_sum: int, max_sum: int) -> int:
        low = list(map(int, num1.zfill(n := len(high := list(map(int, num2))))))
        
        @lru_cache(None)
        def dfs(start: int, cur: int, limit_low: bool, limit_high: bool) -> int:
            return 0 if cur > max_sum else int(cur >= min_sum) if start == n else sum(dfs(start + 1, cur + d, limit_low and d == (low[start] if limit_low else 0), limit_high and d == (high[start] if limit_high else 9)) for d in range(low[start] if limit_low else 0, (high[start] if limit_high else 9) + 1)) 
        return dfs(0, 0, 1, 1) % (10 ** 9 + 7)
```
