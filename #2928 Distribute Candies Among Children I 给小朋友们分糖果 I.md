# 2928 Distribute Candies Among Children I 给小朋友们分糖果 I

__Description:__

You are given two positive integers `n` and `limit`.

Return _the __total number__ of ways to distribute_ `n` _candies among_ `3` _children such that no child gets more than_ `limit` _candies._

__Example:__

Example 1:

```text
Input: n = 5, limit = 2
Output: 3
Explanation: There are 3 ways to distribute 5 candies such that no child gets more than 2 candies: (1, 2, 2), (2, 1, 2) and (2, 2, 1).
```

Example 2:

```text
Input: n = 3, limit = 3
Output: 10
Explanation: There are 10 ways to distribute 3 candies such that no child gets more than 3 candies: (0, 0, 3), (0, 1, 2), (0, 2, 1), (0, 3, 0), (1, 0, 2), (1, 1, 1), (1, 2, 0), (2, 0, 1), (2, 1, 0) and (3, 0, 0).
```

__Constraints:__

- `1 <= n <= 50`
- `1 <= limit <= 50`

__题目描述:__

给你两个正整数 `n` 和 `limit` 。

请你将 `n` 颗糖果分给 `3` 位小朋友，确保没有任何小朋友得到超过 `limit` 颗糖果，请你返回满足此条件下的 __总方案数__ 。

__示例:__

示例 1：

```text
输入：n = 5, limit = 2
输出：3
解释：总共有 3 种方法分配 5 颗糖果，且每位小朋友的糖果数不超过 2 ：(1, 2, 2) ，(2, 1, 2) 和 (2, 2, 1) 。
```

示例 2：

```text
输入：n = 3, limit = 3
输出：10
解释：总共有 10 种方法分配 3 颗糖果，且每位小朋友的糖果数不超过 3 ：(0, 0, 3) ，(0, 1, 2) ，(0, 2, 1) ，(0, 3, 0) ，(1, 0, 2) ，(1, 1, 1) ，(1, 2, 0) ，(2, 0, 1) ，(2, 1, 0) 和 (3, 0, 0) 。
```

__提示：__

- `1 <= n <= 50`
- `1 <= limit <= 50`

__思路:__

```text
1. 暴力法
由于 n 和 limit 都比较小
可以写三重嵌套循环找到每一组 (i, j, k)
时间复杂度为 O(N ^ 3), 空间复杂度为 O(1)
2. 数学
使用隔板法可以得到 n 个球放入 3 个盒子的方式数
然后计算至少有一个人分的数目超过 limit
可以直接分 limit + 1 个糖果给他, 剩下的转化为 n - limit - 1 个糖果分给 3 个人, 也适用隔板法, 3 个人独立的需要重复计算 3 次
这里减的就多了, 要加回来至少两个人分的糖果超过 limit
减去的也多了至少 3 个人分得的数量超过 limit 的情况
可以使用 venn 图来简化计算
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int distributeCandies(int n, int limit) 
    {
        return ((n + 2) * (n + 1) >> 1) - 3 * (n > limit ? (n - limit + 1) * (n - limit) >> 1 : 0) + 3 * (n - (limit << 1) > 1 ? (n - (limit << 1)) * (n - (limit << 1) - 1) >> 1 : 0) - (n - 3 * limit > 2 ? (n - 3 * limit - 1) * (n - 3 * limit - 2) >> 1 : 0);
    }
};
```

__Java__:

```Java
class Solution {
    public int distributeCandies(int n, int limit) {
        return ((n + 2) * (n + 1) >>> 1) - 3 * (n > limit ? (n - limit + 1) * (n - limit) >>> 1 : 0) + 3 * (n - (limit << 1) > 1 ? (n - (limit << 1)) * (n - (limit << 1) - 1) >>> 1 : 0) - (n - 3 * limit > 2 ? (n - 3 * limit - 1) * (n - 3 * limit - 2) >>> 1 : 0);
    }
}
```

__Python__:

```Python
class Solution:
    def distributeCandies(self, n: int, limit: int) -> int:
        return f(n + 2) - 3 * f(n - limit + 1) + 3 * f(n - 2 * limit) - f(n - 3 * limit - 1) if (f := lambda n: n * (n - 1) // 2 if n > 1 else 0) else 0
```
