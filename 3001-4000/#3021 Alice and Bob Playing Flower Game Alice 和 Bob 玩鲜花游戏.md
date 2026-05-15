# 3021 Alice and Bob Playing Flower Game Alice 和 Bob 玩鲜花游戏

__Description:__

Alice and Bob are playing a turn-based game on a field, with two lanes of flowers between them. There are `x` flowers in the first lane between Alice and Bob, and `y` flowers in the second lane between them.

![3021-1](https://assets.leetcode.com/uploads/2025/08/27/3021.png)

The game proceeds as follows:

Given two integers, `n` and `m`, the task is to compute the number of possible pairs `(x, y)` that satisfy the conditions:

- Alice must win the game according to the described rules.
- The number of flowers `x` in the first lane must be in the range `[1,n]`.
- The number of flowers `y` in the second lane must be in the range `[1,m]`.

Return _the number of possible pairs_ `(x, y)` _that satisfy the conditions mentioned in the statement_.

__Example:__

Example 1:

```text
Input: n = 3, m = 2
Output: 3
Explanation: The following pairs satisfy conditions described in the statement: (1,2), (3,2), (2,1).
```

Example 2:

```text
Input: n = 1, m = 1
Output: 0
Explanation: No pairs satisfy the conditions described in the statement.
```

__Constraints:__

- `1 <= n, m <= 10 ^ 5`

__题目描述:__

Alice 和 Bob 在一片田野上玩一个回合制游戏，他们之间有两排花。Alice 和 Bob 之间第一排有 `x` 朵花，第二排有 `y` 朵花。

![3021-2](https://assets.leetcode.com/uploads/2025/08/27/3021.png)

游戏过程如下：

给你两个整数 `n` 和 `m` ，你的任务是求出满足以下条件的所有 `(x, y)` 对:

- 按照上述规则，Alice 必须赢得游戏。
- 第一排的鲜花数目 `x` 必须在区间 `[1,n]` 之间。
- 第二排的鲜花数目 `y` 必须在区间 `[1,m]` 之间。

请你返回满足题目描述的数对 `(x, y)` 的数目。

__示例:__

示例 1：

```text
输入：n = 3, m = 2
输出：3
解释：以下数对满足题目要求：(1,2) ，(3,2) ，(2,1) 。
```

示例 2：

```text
输入：n = 1, m = 1
输出：0
解释：没有数对满足题目要求。
```

__提示：__

- `1 <= n, m <= 10 ^ 5`

__思路:__

```text
数学
当 x 为 [1, n] 中的奇数时, y 为 [1, m] 中的偶数
所以这部分为 (1 + n) * m >> 2
反过来 当 y 为 [1, m] 中的奇数时, x 为 [1, n] 中的偶数
这部分为 (1 + m) * n >> 2
所以和为 m * n >> 1
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long flowerGame(int n, int m) 
    {
        return (long long)n * m >> 1LL;
    }
};
```

__Java__:

```Java
class Solution {
    public long flowerGame(int n, int m) {
        return (long)n * m >>> 1L;
    }
}
```

__Python__:

```Python
class Solution:
    def flowerGame(self, n: int, m: int) -> int:
        return m * n >> 1
```
