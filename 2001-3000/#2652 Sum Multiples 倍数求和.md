# 2652 Sum Multiples 倍数求和

__Description:__

Given a positive integer `n`, find the sum of all integers in the range `[1, n]` __inclusive__ that are divisible by `3`, `5`, or `7`.

Return an integer denoting the sum of all numbers in the given range satisfying the constraint.

__Example:__

Example 1:

```text
Input:  n = 7
Output:  21
Explanation:  Numbers in the range [1, 7] that are divisible by 3, 5, or 7 are 3, 5, 6, 7. The sum of these numbers is 21.
```

Example 2:

```text
Input:  n = 10
Output:  40
Explanation:  Numbers in the range [1, 10] that are divisible by 3, 5, or 7 are 3, 5, 6, 7, 9, 10. The sum of these numbers is 40.
```

Example 3:

```text
Input:  n = 9
Output:  30
Explanation:  Numbers in the range [1, 9] that are divisible by 3, 5, or 7 are 3, 5, 6, 7, 9. The sum of these numbers is 30.
```

__Constraints:__

- `1 <= n <= 10 ^ 3`

__题目描述:__

给你一个正整数 `n` ，请你计算在 `[1，n]` 范围内能被 `3`、 `5`、 `7` 整除的所有整数之和。

返回一个整数，用于表示给定范围内所有满足约束条件的数字之和。

__示例:__

示例 1：

```text
输入: n = 7
输出: 21
解释: 在 [1, 7] 范围内能被 3、 5、 7 整除的所有整数分别是 3、 5、 6、 7 。数字之和为 21。
```

示例 2：

```text
输入: n = 10
输出: 40
解释: 在 [1, 10] 范围内能被 3、 5、 7 整除的所有整数分别是 3、 5、 6、 7、 9、 10 。数字之和为 40。
```

示例 3：

```text
输入: n = 9
输出: 30
解释: 在 [1, 9] 范围内能被 3、 5、 7 整除的所有整数分别是 3、 5、 6、 7、 9 。数字之和为 30。
```

__提示：__

- 1 <= n <= 10 ^ 3

__思路:__

```text
数学
我们可以利用等差数列求和的公式来计算结果
对于能够整除 m 的和为 (n / m + 1) * n / m * m / 2
首项为 m, 公差为 m, 项数为 n / m, 末项为 (n / m) * m
即 (m + n / m * m) * n / m / 2
然后利用容斥原理, 计算出最终的结果
f(a) + f(b) + f(c) - f(ab) - f(ac) - f(bc) + f(abc)
即 (-1) ** (n - 1) * f(abc...), n 为 abc... 的次数和
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int sumOfMultiples(int n) 
    {
        return (n / 3 * (n / 3 + 1) * 3 + n / 5 * (n / 5 + 1) * 5 + n / 7 * (n / 7 + 1) * 7 - n / 15 * (n / 15 + 1) * 15 - n / 21 * (n / 21 + 1) * 21 - n / 35 * (n / 35 + 1) * 35 + n / 105 * (n / 105 + 1) * 105) >> 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int sumOfMultiples(int n) {
        return (n / 3 * (n / 3 + 1) * 3 + n / 5 * (n / 5 + 1) * 5 + n / 7 * (n / 7 + 1) * 7 - n / 15 * (n / 15 + 1) * 15 - n / 21 * (n / 21 + 1) * 21 - n / 35 * (n / 35 + 1) * 35 + n / 105 * (n / 105 + 1) * 105) >>> 1;
    }
}
```

__Python__:

```Python
class Solution:
    def sumOfMultiples(self, n: int) -> int:
        return (n // 3 * (n // 3 + 1) * 3 + n // 5 * (n // 5 + 1) * 5 + n // 7 * (n // 7 + 1) * 7 - n // 15 * (n // 15 + 1) * 15 - n // 21 * (n // 21 + 1) * 21 - n // 35 * (n // 35 + 1) * 35 + n // 105 * (n // 105 + 1) * 105) >> 1
```
