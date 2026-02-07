# 2894 Divisible and Non-divisible Sums Difference 分类求和并作差

__Description:__

You are given positive integers `n` and `m`.

Define two integers as follows:

- `num1`: The sum of all integers in the range `[1, n]` (both __inclusive__) that are __not divisible__ by `m`.
- `num2`: The sum of all integers in the range `[1, n]` (both __inclusive__) that are __divisible__ by `m`.

Return _the integer_ `num1 - num2`.

__Example:__

Example 1:

```text
Input: n = 10, m = 3
Output: 19
Explanation: In the given example:
```

- Integers in the range [1, 10] that are not divisible by 3 are [1,2,4,5,7,8,10], num1 is the sum of those integers = 37.
- Integers in the range [1, 10] that are divisible by 3 are [3,6,9], num2 is the sum of those integers = 18.

We return 37 - 18 = 19 as the answer.

Example 2:

```text
Input: n = 5, m = 6
Output: 15
Explanation: In the given example:
```

- Integers in the range [1, 5] that are not divisible by 6 are [1,2,3,4,5], num1 is the sum of those integers = 15.
- Integers in the range [1, 5] that are divisible by 6 are [], num2 is the sum of those integers = 0.

We return 15 - 0 = 15 as the answer.

Example 3:

```text
Input: n = 5, m = 1
Output: -15
Explanation: In the given example:
```

- Integers in the range [1, 5] that are not divisible by 1 are [], num1 is the sum of those integers = 0.
- Integers in the range [1, 5] that are divisible by 1 are [1,2,3,4,5], num2 is the sum of those integers = 15.

We return 0 - 15 = -15 as the answer.

__Constraints:__

- `1 <= n, m <= 1000`

__题目描述:__

给你两个正整数 `n` 和 `m` 。

现定义两个整数 `num1` 和 `num2` ，如下所示:

- `num1`:范围 `[1, n]` 内所有 __无法被__ `m` __整除__ 的整数之和。
- `num2`:范围 `[1, n]` 内所有 __能够被__ `m` __整除__ 的整数之和。

返回整数 `num1 - num2` 。

__示例:__

示例 1：

```text
输入：n = 10, m = 3
输出：19
解释：在这个示例中：
```

- 范围 [1, 10] 内无法被 3 整除的整数为 [1,2,4,5,7,8,10] ，num1 = 这些整数之和 = 37 。
- 范围 [1, 10] 内能够被 3 整除的整数为 [3,6,9] ，num2 = 这些整数之和 = 18 。

返回 37 - 18 = 19 作为答案。

示例 2：

```text
输入：n = 5, m = 6
输出：15
解释：在这个示例中：
```

- 范围 [1, 5] 内无法被 6 整除的整数为 [1,2,3,4,5] ，num1 = 这些整数之和 =  15 。
- 范围 [1, 5] 内能够被 6 整除的整数为 [] ，num2 = 这些整数之和 = 0 。

返回 15 - 0 = 15 作为答案。

示例 3：

```text
输入：n = 5, m = 1
输出：-15
解释：在这个示例中：
```

- 范围 [1, 5] 内无法被 1 整除的整数为 [] ，num1 = 这些整数之和 = 0 。
- 范围 [1, 5] 内能够被 1 整除的整数为 [1,2,3,4,5] ，num2 = 这些整数之和 = 15 。

返回 0 - 15 = -15 作为答案。

__提示：__

- `1 <= n, m <= 1000`

__思路:__

```text
数学
n 以内的数字和可用等差数列求和公式得到
而 m 的倍数为 m + 2 * m + ... + k * m = (k + 1) * k / 2 * m
所以 num1 - nums2 = (n + 1) * n / 2 - 2 * num2 = (n + 1) * n / 2 - (k + 1) * k * m
k 可由 n / m 算出
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int differenceOfSums(int n, int m) 
    {
        return (n * (n + 1) >> 1) - n / m * (n / m + 1) * m;
    }
};
```

__Java__:

```Java
class Solution {
    public int differenceOfSums(int n, int m) {
        return (n * (n + 1) >>> 1) - n / m * (n / m + 1) * m;
    }
}
```

__Python__:

```Python
class Solution:
    def differenceOfSums(self, n: int, m: int) -> int:
        return (sum(i for i in range(1, n + 1) if i % m) << 1) - ((1 + n) * n >> 1)
```
