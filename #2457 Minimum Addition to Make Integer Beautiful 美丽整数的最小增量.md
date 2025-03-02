# 2457 Minimum Addition to Make Integer Beautiful 美丽整数的最小增量

__Description:__

You are given two positive integers `n` and `target`.

An integer is considered __beautiful__ if the sum of its digits is less than or equal to `target`.

Return the _minimum __non-negative__ integer_ `x` _such that_ `n + x` _is beautiful_. The input will be generated such that it is always possible to make `n` beautiful.

__Example:__

Example 1:

```text
Input: n = 16, target = 6
Output: 4
Explanation: Initially n is 16 and its digit sum is 1 + 6 = 7. After adding 4, n becomes 20 and digit sum becomes 2 + 0 = 2. It can be shown that we can not make n beautiful with adding non-negative integer less than 4.
```

Example 2:

```text
Input: n = 467, target = 6
Output: 33
Explanation: Initially n is 467 and its digit sum is 4 + 6 + 7 = 17. After adding 33, n becomes 500 and digit sum becomes 5 + 0 + 0 = 5. It can be shown that we can not make n beautiful with adding non-negative integer less than 33.
```

Example 3:

```text
Input: n = 1, target = 1
Output: 0
Explanation: Initially n is 1 and its digit sum is 1, which is already smaller than or equal to target.
```

__Constraints:__

- `1 <= n <= 10 ^ 12`
- `1 <= target <= 150`
- The input will be generated such that it is always possible to make `n` beautiful.

__题目描述:__

给你两个正整数 `n` 和 `target` 。

如果某个整数每一位上的数字相加小于或等于 `target` ，则认为这个整数是一个 __美丽整数__ 。

找出并返回满足 `n + x` 是 __美丽整数__ 的最小非负整数 `x` 。生成的输入保证总可以使 `n` 变成一个美丽整数。

__示例:__

示例 1：

```text
输入：n = 16, target = 6
输出：4
解释：最初，n 是 16 ，且其每一位数字的和是 1 + 6 = 7 。在加 4 之后，n 变为 20 且每一位数字的和变成 2 + 0 = 2 。可以证明无法加上一个小于 4 的非负整数使 n 变成一个美丽整数。
```

示例 2：

```text
输入：n = 467, target = 6
输出：33
解释：最初，n 是 467 ，且其每一位数字的和是 4 + 6 + 7 = 17 。在加 33 之后，n 变为 500 且每一位数字的和变成 5 + 0 + 0 = 5 。可以证明无法加上一个小于 33 的非负整数使 n 变成一个美丽整数。
```

示例 3：

```text
输入：n = 1, target = 1
输出：0
解释：最初，n 是 1 ，且其每一位数字的和是 1 ，已经小于等于 target 。
```

__提示：__

- `1 <= n <= 10 ^ 12`
- `1 <= target <= 150`
- 生成的输入保证总可以使 `n` 变成一个美丽整数。

__思路:__

```text
贪心
将一个数字从 a + remain 变成 a + 10 ^ cur, 由于 remain 各位数字和至少大于等于 1, 所以 a + remain <= a + 10 ^ cur, 其中 a 是 10 ^ cur 的倍数, remain <= 10 ^ cur
因此总可以找到一个最小的 need 使得 a + need 的每一位数字和小于等于 target
比如 467, target = 1, 467(17) -> 470(11) -> 500(5) -> 1000(1)
need = (cur - n % cur) % cur
注意 cur = 1 时, need = 0, 不需要单独讨论 n 的每一位数字和小于等于 target 的情况
因此我们可以从 1 开始枚举 cur, 直到找到一个 need 使得 a + need 的每一位数字和小于等于 target
计算每一位的数字和可以使用取模运算 x % 10 和整除运算 x // 10
时间复杂度为 O(log ^ 2n), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution
{
public:
    long long makeIntegerBeautiful(long long n, int target) 
    {
        long long cur = 1LL, need = 0LL, x = 0LL, s = 0LL;
        while (1) 
        {
            x = n + (need = (cur - n % cur) % cur);
            s = 0LL;
            while (x) 
            {
                s += x % 10;
                x /= 10;
            }
            if (s <= target) break;
            cur *= 10L;
        }
        return need;
    }
};
```

__Java__:

```Java
class Solution {
    public long makeIntegerBeautiful(long n, int target) {
        long cur = 1L, remain = 0L, x = 0L, s = 0L;
        while (true) {
            x = n + (need = (cur - n % cur) % cur);
            s = 0L;
            while (x > 0L) 
            {
                s += x % 10L;
                x /= 10L;
            }
            if (s <= target) break;
            cur *= 10L;
        }
        return need;
    }
}
```

__Python__:

```Python
class Solution:
    def makeIntegerBeautiful(self, n: int, target: int, cur: int=1) -> int:
        return (cur - n % cur) % cur if sum(int(i) for i in str(n + (cur - n % cur) % cur)) <= target else self.makeIntegerBeautiful(n, target, cur * 10)
```
