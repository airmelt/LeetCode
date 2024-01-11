# 1952 Three Divisors 三除数

__Description:__

Given an integer `n`, return `true` _if_ `n` _has __exactly three positive divisors__. Otherwise, return_ `false`.

An integer `m` is a __divisor__ of `n` if there exists an integer `k` such that `n = k * m`.

__Example:__

Example 1:

```text
Input: n = 2
Output: false
Explantion: 2 has only two divisors: 1 and 2.
```

Example 2:

```text
Input: n = 4
Output: true
Explantion: 4 has three divisors: 1, 2, and 4.
```

__Constraints:__

- `1 <= n <= 10 ^ 4`

__题目描述:__

给你一个整数 `n` 。如果 `n` __恰好有三个正除数__ ，返回 `true` ；否则，返回 `false` 。

如果存在整数 `k` ，满足 `n = k * m` ，那么整数 `m` 就是 `n` 的一个 __除数__ 。

__示例:__

示例 1：

```text
输入：n = 2
输出：false
解释：2 只有两个除数：1 和 2 。
```

示例 2：

```text
输入：n = 4
输出：true
解释：4 有三个除数：1、2 和 4 。
```

__提示：__

- `1 <= n <= 10 ^ 4`

__思路:__

```text
1. 质因数分解
求出 n 的所有质因数
如果为完全平方因子的就加 2
否则加 1
最后判断因子数是否为 3
时间复杂度为 O(N ^ 1 / 2), 空间复杂度为 O(1)
2. 枚举
n 需要为质数的完全平方
枚举 10000 以内的质数的完全平方即可
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool isThree(int n) 
    {
        int factor = 0;
        for (int i = 1; i * i <= n; i++) factor += (n % i == 0) + (n % i == 0 and i != n / i);
        return factor == 3;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isThree(int n) {
        return n == 4 || n == 9 || n == 25 || n == 49 || n == 121 || n == 169 || n == 289 || n == 361 || n == 529 || n == 841 || n == 961 || n == 1369 || n == 1681 || n == 1849 || n == 2209 || n == 2809 || n == 3481 || n == 3721 || n == 4489 || n == 5041 || n == 5329 || n == 6241 || n == 6889 || n == 7921 || n == 9409;
    }
}
```

__Python__:

```Python
class Solution:
    def isThree(self, n: int) -> bool:
        return n in {4, 9, 25, 49, 121, 169, 289, 361, 529, 841, 961, 1369, 1681, 1849, 2209, 2809, 3481, 3721, 4489, 5041, 5329, 6241, 6889, 7921, 9409}
```
