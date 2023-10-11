# 1837 Sum of Digits in Base K K 进制表示下的各位数字总和

__Description:__

Given an integer `n` (in base `10`) and a base `k`, return _the __sum__ of the digits of_ `n` ___after__ converting_ `n` _from base_ `10` _to base_ `k`.

After converting, each digit should be interpreted as a base `10` number, and the sum should be returned in base `10`.

__Example:__

Example 1:

```text
Input: n = 34, k = 6
Output: 9
Explanation: 34 (base 10) expressed in base 6 is 54. 5 + 4 = 9.
```

Example 2:

```text
Input: n = 10, k = 10
Output: 1
Explanation: n is already in base 10. 1 + 0 = 1.
```

__Constraints:__

- `1 <= n <= 100`
- `2 <= k <= 10`

__题目描述:__

给你一个整数 `n`（ `10` 进制）和一个基数 `k` ，请你将 `n` 从 `10` 进制表示转换为 `k` 进制表示，计算并返回转换后各位数字的 __总和__ 。

转换后，各位数字应当视作是 `10` 进制数字，且它们的总和也应当按 `10` 进制表示返回。

__示例:__

示例 1：

```text
输入：n = 34, k = 6
输出：9
解释：34 (10 进制) 在 6 进制下表示为 54 。5 + 4 = 9 。
```

示例 2：

```text
输入：n = 10, k = 10
输出：1
解释：n 本身就是 10 进制。 1 + 0 = 1 。
```

__提示：__

- `1 <= n <= 100`
- `2 <= k <= 10`

__思路:__

```text
数学
k 进制可以写成 k ^ m * am + k ^ (m - 1) * (am - 1) + ... + k ^ 1 * a1 + k ^ 0 * a0
求 sum(a) 可以转化为 n % k + n / k % k + ... + n / k ^ m % k
即 n % k + sumBase(n / k, k)
递归终点为 n < k
时间复杂度为 O(logN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int sumBase(int n, int k) 
    {
        int result = 0;
        while (n)
        {
            result += n % k;
            n /= k;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int sumBase(int n, int k) {
        return n < k ? n : n % k + sumBase(n / k, k);
    }
}
```

__Python__:

```Python
class Solution:
    def sumBase(self, n: int, k: int) -> int:
        return n if n < k else n % k + self.sumBase(n // k, k)
```
