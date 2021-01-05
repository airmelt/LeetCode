# 172 Factorial Trailing Zeroes 阶乘后的零

__Description__:
Given an integer n, return the number of trailing zeroes in n!.

__Example__:

Example 1:
Input: 3
Output: 0
Explanation: 3! = 6, no trailing zero.

Example 2:
Input: 5
Output: 1
Explanation: 5! = 120, one trailing zero.

__Note__: Your solution should be in logarithmic time complexity.

__题目描述__:
给定一个整数 n，返回 n! 结果尾数中零的数量。

__示例__:

示例 1:
输入: 3
输出: 0
解释: 3! = 6, 尾数中没有零。

示例 2:
输入: 5
输出: 1
解释: 5! = 120, 尾数中有 1 个零.

__说明__: 你算法的时间复杂度应为 O(log n) 。

__思路__:

10 = 2 \* 5, 由于 5 > 2, 所以 0的个数决定于质因子中有 5的个数
比如 n = 25 -> 5, 10, 15, 20, 25中含有 5, 且 25 = 5 * 5, 所以 0的个数为 6

1. 递归求解,  f(n) = n / 5 + f(n / 5)
2. 注意到 int类型中最大的 5的阶乘为 1220703125, 可以转换为求 5的 n次的个数

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int trailingZeroes(int n) 
    {
        return n < 5 ? 0 : n / 5 + trailingZeroes(n / 5);
    }
};
```

__Java__:

```Java
class Solution {
    public int trailingZeroes(int n) {
        return n / 5 + n / 25 + n / 125 + n / 625 + n / 3125 + n / 15625 + n / 78125 + n / 390625 + n / 1953125 + n / 9765625 + n / 48828125 + n /244140625 + n / 1220703125;
    }
}
```

__Python__:

```Python
class Solution:
    def trailingZeroes(self, n: int) -> int:
        return n // 5 + n // 25 + n // 125 + n // 625 + n // 3125 + n // 15625 + n // 78125 + n // 390625 + n // 1953125 + n // 9765625 + n // 48828125 + n //244140625 + n // 1220703125
```
