# 1553 Minimum Number of Days to Eat N Oranges N个橘子的最少天数吃掉N个橘子的最少天数

__Description:__

There are `n` oranges in the kitchen and you decided to eat some of these oranges every day as follows:

- Eat one orange.
- If the number of remaining oranges `n` is divisible by `2` then you can eat `n / 2` oranges.
- If the number of remaining oranges `n` is divisible by `3` then you can eat `2 * (n / 3)` oranges.

You can only choose one of the actions per day.

Given the integer `n`, return _the minimum number of days to eat_ `n` _oranges_.

__Example:__

Example 1:

```text
Input: n = 10
Output: 4
Explanation: You have 10 oranges.
Day 1: Eat 1 orange,  10 - 1 = 9.  
Day 2: Eat 6 oranges, 9 - 2*(9/3) = 9 - 6 = 3. (Since 9 is divisible by 3)
Day 3: Eat 2 oranges, 3 - 2*(3/3) = 3 - 2 = 1. 
Day 4: Eat the last orange  1 - 1  = 0.
You need at least 4 days to eat the 10 oranges.
```

Example 2:

```text
Input: n = 6
Output: 3
Explanation: You have 6 oranges.
Day 1: Eat 3 oranges, 6 - 6/2 = 6 - 3 = 3. (Since 6 is divisible by 2).
Day 2: Eat 2 oranges, 3 - 2*(3/3) = 3 - 2 = 1. (Since 3 is divisible by 3)
Day 3: Eat the last orange  1 - 1  = 0.
You need at least 3 days to eat the 6 oranges.
```

__Constraints:__

- `1 <= n <= 2 * 10 ^ 9`

__题目描述:__

厨房里总共有 `n` 个橘子，你决定每一天选择如下方式之一吃这些橘子：

- 吃掉一个橘子。
- 如果剩余橘子数 `n` 能被 2 整除，那么你可以吃掉 `n/2` 个橘子。
- 如果剩余橘子数 `n` 能被 3 整除，那么你可以吃掉 `2*(n/3)` 个橘子。

每天你只能从以上 3 种方案中选择一种方案。

请你返回吃掉所有 `n` 个橘子的最少天数。

__示例:__

示例 1：

```text
输入：n = 10
输出：4
解释：你总共有 10 个橘子。
第 1 天：吃 1 个橘子，剩余橘子数 10 - 1 = 9。
第 2 天：吃 6 个橘子，剩余橘子数 9 - 2*(9/3) = 9 - 6 = 3。（9 可以被 3 整除）
第 3 天：吃 2 个橘子，剩余橘子数 3 - 2*(3/3) = 3 - 2 = 1。
第 4 天：吃掉最后 1 个橘子，剩余橘子数 1 - 1 = 0。
你需要至少 4 天吃掉 10 个橘子。
```

示例 2：

```text
输入：n = 6
输出：3
解释：你总共有 6 个橘子。
第 1 天：吃 3 个橘子，剩余橘子数 6 - 6/2 = 6 - 3 = 3。（6 可以被 2 整除）
第 2 天：吃 2 个橘子，剩余橘子数 3 - 2*(3/3) = 3 - 2 = 1。（3 可以被 3 整除）
第 3 天：吃掉剩余 1 个橘子，剩余橘子数 1 - 1 = 0。
你至少需要 3 天吃掉 6 个橘子。
```

示例 3：

```text
输入：n = 1
输出：1
```

示例 4：

```text
输入：n = 56
输出：6
```

__提示：__

- `1 <= n <= 2*10 ^ 9`

__思路:__

```text
记忆化搜索
如果一天吃一半橘子那么 f(n) = f(n / 2) + 1 + (n % 2), 最后的部分表示 n 为奇数时需要先吃 n % 2 个橘子才能被 2 整除
如果一天吃 2 / 3 的橘子那么 f(n) = f(n / 3) + 1 + (n % 3), 与第一种情况相同
所以 f(n) = min(f(n / 2) + n % 2 + 1, f(n / 3) + n % 3 + 1)
边界条件为 n = 0, f(0) = 0; n = 1, f(1) = 1
可以用哈希表记录已经出现过的天数方便快速查找
时间复杂度为 O(log ^ 2N), 空间复杂度为 O(log ^ 2N)
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    unordered_map<int, int> m;
public:
    int minDays(int n) 
    {
        if (!n or n == 1) return n;
        return m.count(n) ? m[n] : (m[n] = min(minDays(n / 2) + (n & 1) + 1, minDays(n / 3) + n % 3 + 1));
    }
};
```

__Java__:

```Java
class Solution {
    private Map<Integer, Integer> map = new HashMap<>();
    
    public int minDays(int n) {
        if (n == 0 || n == 1) return n;
        if (map.containsKey(n)) return map.get(n);
        map.put(n, Math.min(minDays(n / 2) + (n & 1) + 1, minDays(n / 3) + n % 3 + 1));
        return map.get(n);
    }
}
```

__Python__:

```Python
class Solution:
    def minDays(self, n: int) -> int:
        @lru_cache(None)
        def dfs(n):
            return min(dfs(n >> 1) + n % 2, dfs(n // 3) + n % 3) + 1 if n > 1 else n
        return dfs(n)
```
