# 2400 Number of Ways to Reach a Position After Exactly k Steps 恰好移动 k 步到达某一位置的方法数目

__Description:__

You are given two __positive__ integers `startPos` and `endPos`. Initially, you are standing at position `startPos` on an __infinite__ number line. With one step, you can move either one position to the left, or one position to the right.

Given a positive integer `k`, return _the number of __different__ ways to reach the position_ `endPos` _starting from_ `startPos`_, such that you perform __exactly___ `k` _steps_. Since the answer may be very large, return it __modulo__ `10 ^ 9 + 7`.

Two ways are considered different if the order of the steps made is not exactly the same.

Note that the number line includes negative integers.

__Example:__

Example 1:

```text
Input: startPos = 1, endPos = 2, k = 3
Output: 3
Explanation: We can reach position 2 from 1 in exactly 3 steps in three ways:
```

- 1 -> 2 -> 3 -> 2.
- 1 -> 2 -> 1 -> 2.
- 1 -> 0 -> 1 -> 2.

It can be proven that no other way is possible, so we return 3.

Example 2:

```text
Input: startPos = 2, endPos = 5, k = 10
Output: 0
Explanation: It is impossible to reach position 5 from position 2 in exactly 10 steps.
```

__Constraints:__

- `1 <= startPos, endPos, k <= 1000`

__题目描述:__

给你两个 __正__ 整数 `startPos` 和 `endPos` 。最初，你站在 __无限__ 数轴上位置 `startPos` 处。在一步移动中，你可以向左或者向右移动一个位置。

给你一个正整数 `k` ，返回从 `startPos` 出发、__恰好__ 移动 `k` 步并到达 `endPos` 的 __不同__ 方法数目。由于答案可能会很大，返回对 `10 ^ 9 + 7` __取余__ 的结果。

如果所执行移动的顺序不完全相同，则认为两种方法不同。

注意：数轴包含负整数。

__示例:__

示例 1：

```text
输入：startPos = 1, endPos = 2, k = 3
输出：3
解释：存在 3 种从 1 到 2 且恰好移动 3 步的方法：
```

- 1 -> 2 -> 3 -> 2.
- 1 -> 2 -> 1 -> 2.
- 1 -> 0 -> 1 -> 2.

可以证明不存在其他方法，所以返回 3 。

示例 2：

```text
输入：startPos = 2, endPos = 5, k = 10
输出：0
解释：不存在从 2 到 5 且恰好移动 10 步的方法。
```

__提示：__

- `1 <= startPos, endPos, k <= 1000`

__思路:__

```text
组合数学
从 startPos 移动到 endPos 需要的步数为 k
假设 startPos < endPos
规定向右移动为正方向，向左移动为负方向
需要向右走 a 步，向左走 b 步，有 a - b = endPos - startPos
则有 a + b = k
而走的组合数为 C(k, a)
令 d = abs(startPos - endPos)
a = (d + k) / 2
如果 (d + k) 为奇数或者 d > k, 则返回 0, 无论如何都无法到达 endPos
求组合数可以用逆元求解
inv[i] = (MOD - MOD / i) * inv[MOD % i] % MOD
最后的结果为 result = result * (k - i + 1) % MOD * inv[i] % MOD, i 从 1 到 (d + k) / 2, 闭区间
时间复杂度为 O(N + K), 空间复杂度为 O(K), 其中 N = startPos - endPos
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfWays(int startPos, int endPos, int k) 
    {
        long long d = abs(startPos - endPos), result = 1LL, MOD = 1e9 + 7;
        if (((d + k) & 1LL) == 1LL or d > k) return 0;
        vector<long long> inv(k + 1);
        inv[1] = 1;
        for (int i = 2; i <= k; i++) inv[i] = (MOD - MOD / i) * inv[MOD % i] % MOD;
        for (int i = 1, total = (d + k) >> 1LL; i <= total; i++) result = result * (k - i + 1) % MOD * inv[i] % MOD;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfWays(int startPos, int endPos, int k) {
        long result = 1L, inv[] = new long[k + 1], MOD = 1_000_000_007L, d = Math.abs(startPos - endPos);
        if (((d + k) & 1L) == 1L || d > k) return 0;
        inv[1] = 1L;
        for (int i = 2; i <= k; i++) inv[i] = (MOD - MOD / i) * inv[(int)MOD % i] % MOD;
        for (int i = 1, total = (int)(d + k) >> 1; i <= total; i++) result = result * (k - i + 1) % MOD * inv[i] % MOD;
        return (int)((result + MOD) % MOD);
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfWays(self, startPos: int, endPos: int, k: int) -> int:
        return 0 if ((d := abs(startPos - endPos)) + k) & 1 or d > k else comb(k, (d + k) >> 1) % (10 ** 9 + 7)
```
