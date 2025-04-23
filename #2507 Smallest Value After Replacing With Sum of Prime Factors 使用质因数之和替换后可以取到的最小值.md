# 2507 Smallest Value After Replacing With Sum of Prime Factors 使用质因数之和替换后可以取到的最小值

__Description:__

You are given a positive integer `n`.

Continuously replace `n` with the sum of its __prime factors__.

- Note that if a prime factor divides `n` multiple times, it should be included in the sum as many times as it divides `n`.

Return _the smallest value_ `n` _will take on._

__Example:__

Example 1:

```text
Input: n = 15
Output: 5
Explanation: Initially, n = 15.
15 = 3 * 5, so replace n with 3 + 5 = 8.
8 = 2 * 2 * 2, so replace n with 2 + 2 + 2 = 6.
6 = 2 * 3, so replace n with 2 + 3 = 5.
5 is the smallest value n will take on.
```

Example 2:

```text
Input: n = 3
Output: 3
Explanation: Initially, n = 3.
3 is the smallest value n will take on.
```

__Constraints:__

- `2 <= n <= 10 ^ 5`

__题目描述:__

给你一个正整数 `n` 。

请你将 `n` 的值替换为 `n` 的 __质因数__ 之和，重复这一过程。

- 注意，如果 `n` 能够被某个质因数多次整除，则在求和时，应当包含这个质因数同样次数。

返回 `n` 可以取到的最小值。

__示例:__

示例 1：

```text
输入：n = 15
输出：5
解释：最开始，n = 15 。
15 = 3 * 5 ，所以 n 替换为 3 + 5 = 8 。
8 = 2 * 2 * 2 ，所以 n 替换为 2 + 2 + 2 = 6 。
6 = 2 * 3 ，所以 n 替换为 2 + 3 = 5 。
5 是 n 可以取到的最小值。
```

示例 2：

```text
输入：n = 3
输出：3
解释：最开始，n = 3 。
3 是 n 可以取到的最小值。
```

__提示：__

- `2 <= n <= 10 ^ 5`

__思路:__

```text
暴力法
从 i = 2 开始遍历到 sqrt(n)
如果 n 能被 i 整除, 就将 i 加入到和中, 然后将 n 除以 i, 直到 n 不能被 i 整除为止
最后留下的数超过 1 也要加到和中
如果和等于 n, 就返回 n
否则就将 n 替换为和, 继续循环
最坏情况下每次可以将 n 替换为 2 + n / 2
遍历需要 O(sqrt(n)) 的时间
总时间复杂度为 O(sqrt(n) + sqrt(n / 2) + sqrt(n / 4) + ...) = O(sqrt(n))
时间复杂度为 O(N ^ 1 / 2), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int smallestValue(int n) 
    {
        while (1) 
        {
            int s = 0, c = n;
            for (int i = 2; i * i <= c; i++) 
            {
                while (!(c % i))
                {
                    s += i;
                    c /= i;
                }
            }
            if ((s = c > 1 ? s + c : s) == n) return n;
            n = s;
        }
    }
};
```

__Java__:

```Java
class Solution {
    public int smallestValue(int n) {
        while (true) {
            int s = 0, c = n;
            for (int i = 2; i * i <= c; i++) {
                while (c % i == 0) {
                    s += i;
                    c /= i;
                }
            }
            if ((s = c > 1 ? s + c : s) == n) return n;
            n = s;
        }
    }
}
```

__Python__:

```Python
class Solution:
    def smallestValue(self, n: int) -> int:
        while True:
            s, x, c = 0, 2, n
            for x in range(2, int(sqrt(c)) + 1):
                while not c % x:
                    s += x
                    c //= x
            if (s := s + c if c > 1 else s) == n:
                return n
            n = s
```
