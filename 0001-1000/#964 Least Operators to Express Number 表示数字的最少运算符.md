# 964 Least Operators to Express Number 表示数字的最少运算符

__Description__:
Given a single positive integer x, we will write an expression of the form x (op1) x (op2) x (op3) x ... where each operator op1, op2, etc. is either addition, subtraction, multiplication, or division (+, -, \*, or /). For example, with x = 3, we might write 3 \* 3 / 3 + 3 - 3 which is a value of 3.

When writing such an expression, we adhere to the following conventions:

The division operator (/) returns rational numbers.
There are no parentheses placed anywhere.
We use the usual order of operations: multiplication and division happen before addition and subtraction.
It is not allowed to use the unary negation operator (-). For example, "x - x" is a valid expression as it only uses subtraction, but "-x + x" is not because it uses negation.
We would like to write an expression with the least number of operators such that the expression equals the given target. Return the least number of operators used.

__Example:__

Example 1:

Input: x = 3, target = 19
Output: 5
Explanation: 3 \* 3 + 3 \* 3 + 3 / 3.
The expression contains 5 operations.
Example 2:

Input: x = 5, target = 501
Output: 8
Explanation: 5 \* 5 \* 5 \* 5 - 5 \* 5 \* 5 + 5 / 5.
The expression contains 8 operations.

Example 3:

Input: x = 100, target = 100000000
Output: 3
Explanation: 100 \* 100 \* 100 * 100.
The expression contains 3 operations.

__Constraints:__

2 <= x <= 100
1 <= target <= 2 * 10^8

__题目描述__:
给定一个正整数 x，我们将会写出一个形如 x (op1) x (op2) x (op3) x ... 的表达式，其中每个运算符 op1，op2，… 可以是加、减、乘、除（+，-，\*，或是 /）之一。例如，对于 x = 3，我们可以写出表达式 3 \* 3 / 3 + 3 - 3，该式的值为 3 。

在写这样的表达式时，我们需要遵守下面的惯例：

除运算符（/）返回有理数。
任何地方都没有括号。
我们使用通常的操作顺序：乘法和除法发生在加法和减法之前。
不允许使用一元否定运算符（-）。例如，“x - x” 是一个有效的表达式，因为它只使用减法，但是 “-x + x” 不是，因为它使用了否定运算符。
我们希望编写一个能使表达式等于给定的目标值 target 且运算符最少的表达式。返回所用运算符的最少数量。

__示例 :__

示例 1：

输入：x = 3, target = 19
输出：5
解释：3 \* 3 + 3 \* 3 + 3 / 3 。表达式包含 5 个运算符。

示例 2：

输入：x = 5, target = 501
输出：8
解释：5 \* 5 \* 5 \* 5 - 5 \* 5 \* 5 + 5 / 5 。表达式包含 8 个运算符。

示例 3：

输入：x = 100, target = 100000000
输出：3
解释：100 \* 100 \* 100 \* 100 。表达式包含 3 个运算符。

__提示:__

2 <= x <= 100
1 <= target <= 2 * 10^8

__思路__:

1. 动态规划
这个正整数一定有解, 至少可以一直使用 x / x = 1 加到目标值
先使用乘法可以快速增长到 target 附近
如果 cur = target 可以直接返回
如果 cur > target, 可以使用 cur - x / x * n = target 来达到 target
如果 cur < target, 如果 x < target, target= 2, x = 3, 那么可以用 3 / 3 + 3 / 3 = 2 或者 3 - 3 / 3 = 2 来达到, 比较两者使用的最少字符
先用对数快速计算需要几个 x 相乘, 再逼近 target
时间复杂度为 O(lgn), 空间复杂度为 O(lgn)
2. 数学
用 x 进制来表示 target
比如 x = 10, target = 99, 可以表示为 100 - 1, 10 ^ 2 - 10 ^ 0
定义两个数量分别为正向和反向, 比如 target = 90, 可以是 9 * 10 也可以是 10 ^ 2 - 10
正向为 nums[i] \* i, 其中 i 表示这一位的余数, 也就是 target % (x ^ n)
反向为 (x - nums[i]) \* i + min(last_reverse - i - 1, i + 1), 因为反向可能会引起高位的变化, 比如说, x = 10, target = 158, 对于 8 来说应该是 10 - 2, 这样高位就变成了 160, 同样, 如果反向表示应该为 200 - 40, 所以需要记录下进位
时间复杂度为 O(lgn), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int leastOpsExpressTarget(int x, int target) 
    {
        vector<int> nums(32);
        int n = 0, last_reverse = INT_MAX, result = -1;
        while (target) 
        {
            nums[n++] = target % x;
            target /= x;
        }
        for (int i = n - 1; i > -1 ; i--) 
        {
            int m = i == 0 ? 2 : i, forward = nums[i] * m, reverse = (x - nums[i]) * m + min(last_reverse - i - 1 , i + 1);
            result += min(forward, reverse);
            last_reverse = max(reverse - forward, 0);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int leastOpsExpressTarget(int x, int target) {
        int nums[] = new int[32], n = 0, lastReverse = Integer.MAX_VALUE, result = -1;
        while (target > 0) {
            nums[n++] = target % x;
            target /= x;
        }
        for (int i = n - 1; i > -1 ; i--) {
            int m = i == 0 ? 2 : i, forward = nums[i] * m, reverse = (x - nums[i]) * m + Math.min(lastReverse - i - 1 , i + 1);
            result += Math.min(forward, reverse);
            lastReverse = Math.max(reverse - forward, 0);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def leastOpsExpressTarget(self, x: int, target: int) -> int:
        @lru_cache(None)
        def dfs(cur: int) -> int:
            if cur < x:
                return min(2 * cur - 1, (x - cur) * 2)
            if cur == 0:
                return 0
            result = dfs(cur - (sums := x ** (p := int(math.log(cur, x))))) + p
            if sums * x - cur < cur:
                result = min(result, p + 1 + dfs(sums * x - cur))
            return result
        return dfs(target)
```
