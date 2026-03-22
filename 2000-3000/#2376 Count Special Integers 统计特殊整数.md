# 2376 Count Special Integers 统计特殊整数

__Description:__

We call a positive integer special if all of its digits are distinct.

Given a __positive__ integer `n`, return _the number of special integers that belong to the interval_ `[1, n]`.

__Example:__

Example 1:

```text
Input: n = 20
Output: 19
Explanation: All the integers from 1 to 20, except 11, are special. Thus, there are 19 special integers.
```

Example 2:

```text
Input: n = 5
Output: 5
Explanation: All the integers from 1 to 5 are special.
```

Example 3:

```text
Input: n = 135
Output: 110
Explanation: There are 110 integers from 1 to 135 that are special.
Some of the integers that are not special are: 22, 114, and 131.
```

__Constraints:__

- `1 <= n <= 2 * 10 ^ 9`

__题目描述:__

如果一个正整数每一个数位都是 互不相同 的，我们称它是 特殊整数 。

给你一个 __正__ 整数 `n` ，请你返回区间 `[1, n]` 之间特殊整数的数目。

__示例:__

示例 1：

```text
输入：n = 20
输出：19
解释：1 到 20 之间所有整数除了 11 以外都是特殊整数。所以总共有 19 个特殊整数。
```

示例 2：

```text
输入：n = 5
输出：5
解释：1 到 5 所有整数都是特殊整数。
```

示例 3：

```text
输入：n = 135
输出：110
解释：从 1 到 135 总共有 110 个整数是特殊整数。
不特殊的部分数字为：22 ，114 和 131 。
```

__提示：__

- `1 <= n <= 2 * 10 ^ 9`

__思路:__

```text
数位 DP
设 s 为 n 的字符串表示
设 dp[i][mask] 表示当前处理到第 i 位，mask 表示当前数字中已经出现的数字，is_limit 表示当前数字是否受限制，is_num 表示当前数字是否已经填入
初始化 dp 数组，dp[i][mask] = -1
设当前数位为 d
mask 表示使用过的数位, mask >> d & 1 表示数字 d 是否使用过, mask | (1 << d) 表示使用数字 d
is_limit 表示前 i - 1 位已经到达 n 的上界, 比如 n = 135, 如果第一位填入 1 那么第二位就会受到限制只能填入不大于 3 的数字或者第二位填入 3, 那么第三位就受限制只能填入不大于 5 的数字, 所以当 is_limit 为 true 时, 第 i 位的上界为 s[i] - '0', 否则为 9, is_limit and d == up 表示当前数字是否受限制
is_num 表示前 i - 1 位是否跳过, 比如 n = 135, 如果跳过第一位, 那么第二位就可以填入 1, 2, 3, 4, 5, 6, 7, 8, 9, 不能填 0, 否则将使得第二位(实际上是一个数字的第一位)有前导 0, 只要填入任意数字, 之后都将 is_num 置为 true
当 i == s.size() 时, 返回 is_num, 如果 is_num 为 true, 说明有一个合法的数字
当且仅当 !is_limit and is_num 时, 说明当前数字不受限制且已经填入, 更新及使用记忆化数组 dp[i][mask] = result
初始化 result = is_num ? 0 : dfs(i + 1, mask, false, false), low = !is_num, up = is_limit ? s[i] - '0' : 9
result 是因为如果已经填入数字就可以开始计算, 否则可以递归的检查后面的位数填入的数字, 这时 is_limit 为 false, is_num 为 false, 因为还没有填入数字
遍历 low 到 up, 如果当前数字 d 未使用过, result += dfs(i + 1, mask | (1 << d), is_limit and d == up, true)
最后返回 result
dfs 的入口为 dfs(0, 0, true, false)
表示从 s[0] 开始, mask 为 0, is_limit 为 true, is_num 为 false, is_limit 为 true 表示第一位受限制, 只能填入 1 到 s[0] 的数字
时间复杂度为 O(ND * 2 ^ D), 空间复杂度为 O(N * 2 ^ D), 其中 N 为 n 的位数, 即 log(n), D = 10, 因为有 10 个数字可以选填
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countSpecialNumbers(int n) 
    {
        s = to_string(n);
        dp = vector<vector<int>>(s.size(), vector<int>(1 << 10, -1));
        return dfs(0, 0, true, false);    
    }
private:
    string s;
    vector<vector<int>> dp;

    int dfs(int i, int mask, bool is_limit, bool is_num)
    {
        if (i == s.size()) return is_num;
        if (!is_limit and is_num and dp[i][mask] != -1) return dp[i][mask];
        int result = is_num ? 0 : dfs(i + 1, mask, false, false), low = !is_num, up = is_limit ? s[i] - '0' : 9;
        for (int d = low; d <= up; d++) if (!(mask >> d & 1)) result += dfs(i + 1, mask | (1 << d), is_limit and d == up, true);
        if (!is_limit and is_num) dp[i][mask] = result;
        return result;
    }
};
```

__Java__:

```Java
class Solution:
    def countSpecialNumbers(self, n: int) -> int:
        m = len(s := str(n))

        @lru_cache
        def dfs(i: int, mask: int, is_limit: bool, is_num: bool) -> int:
            if i == m:
                return int(is_num)
            result, low, up = 0 if is_num else dfs(i + 1, mask, False, False), int(not is_num), int(s[i]) if is_limit else 9
            for d in range(low, up + 1):
                if not (mask >> d & 1):
                    result += dfs(i + 1, mask | (1 << d), is_limit and d == up, True)
            return result
        return dfs(0, 0, True, False)
```

__Python__:

```Python
class Solution:
    def countSpecialNumbers(self, n: int) -> int:
        m = len(s := str(n))

        @lru_cache
        def dfs(i: int, mask: int, is_limit: bool, is_num: bool) -> int:
            if i == m:
                return int(is_num)
            result, low, up = 0 if is_num else dfs(i + 1, mask, False, False), int(not is_num), int(s[i]) if is_limit else 9
            for d in range(low, up + 1):
                if not (mask >> d & 1):
                    result += dfs(i + 1, mask | (1 << d), is_limit and d == up, True)
            return result
        return dfs(0, 0, True, False)
```
