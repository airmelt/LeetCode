# 2427 Number of Common Factors 公因子的数目

__Description:__

Given two positive integers `a` and `b`, return _the number of __common__ factors of_ `a` _and_ `b`.

An integer `x` is a __common factor__ of `a` and `b` if `x` divides both `a` and `b`.

__Example:__

Example 1:

```text
Input: a = 12, b = 6
Output: 4
Explanation: The common factors of 12 and 6 are 1, 2, 3, 6.
```

Example 2:

```text
Input: a = 25, b = 30
Output: 2
Explanation: The common factors of 25 and 30 are 1, 5.
```

__Constraints:__

- `1 <= a, b <= 1000`

__题目描述:__

给你两个正整数 `a` 和 `b` ，返回 `a` 和 `b` 的 __公__ 因子的数目。

如果 `x` 可以同时整除 `a` 和 `b` ，则认为 `x` 是 `a` 和 `b` 的一个 __公因子__ 。

__示例:__

示例 1：

```text
输入：a = 12, b = 6
输出：4
解释：12 和 6 的公因子是 1、2、3、6 。
```

示例 2：

```text
输入：a = 25, b = 30
输出：2
解释：25 和 30 的公因子是 1、5 。
```

__提示：__

- `1 <= a, b <= 1000`

__思路:__

```text
1. 暴力法
遍历 1 到 min(a, b) 的所有数
判断是否同时整除 a 和 b
若是则计数
时间复杂度为 O(N), 空间复杂度为 O(1)
2. 公约数优化
计算 a 和 b 的最大公约数 g
遍历 1 到 sqrt(g) 的所有数
若 g 能被 i 整除，则 i 和 g / i 都是 g 的因子
注意特判 i * i == g 的情况
时间复杂度为 O(sqrt(g)), 空间复杂度为 O(1), 其中计算 g 的时间复杂度为 O(min(a, b))
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int commonFactors(int a, int b) 
    {
        int result = 0, g = gcd(a, b);
        for (int i = 1; i * i <= g; i++) result += !(g % i) + (!(g % i) and i * i < g);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int commonFactors(int a, int b) {
        int result = 1;
        for (int i = 2, n = Math.min(a, b) + 1; i < n; i++) if (a % i == 0 && b % i == 0) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def commonFactors(self, a: int, b: int) -> int:
        return sum(not gcd(a, b) % i for i in range(1, gcd(a, b) + 1))
```
