# 70 Climbing Stairs 爬楼梯

__Description__:
You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

__Note__: Given n will be a positive integer.

__Example__:

Example 1:
Input: 2
Output: 2
Explanation: There are two ways to climb to the top.

1. 1 step + 1 step
2. 2 steps

Example 2:
Input: 3
Output: 3
Explanation: There are three ways to climb to the top.

1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step

__题目描述__:
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

__注意__：给定 n 是一个正整数。

__示例__:

示例 1：
输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。

1. 1 阶 + 1 阶
2. 2 阶

示例 2：
输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。

1. 1 阶 + 1 阶 + 1 阶
2. 1 阶 + 2 阶
3. 2 阶 + 1 阶

__思路__:

跳到第 n阶的时候, 要么从 n - 1阶跳上, 要么从 n - 2阶跳上
f(n) = f(n - 1) + f(n - 2), 即斐波拉契数列
用动态规划优化
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int climbStairs(int n) 
    {
        int a = 0, b = 1, result = 0;
        while (n--) 
        {
            result = a + b;
            a = b;
            b = result;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int climbStairs(int n) {
        if (n <= 3) return n;
        int[] result = new int[n + 1];
        result[0] = 0;
        result[1] = 1;
        result[2] = 2;
        result[3] = 3;
        for (int i = 4; i < n + 1; i++) result[i] = result[i - 1] + result[i - 2];
        return result[n];
    }
}
```

__Python__:

```Python
from functools import reduce
class Solution:
    def climbStairs(self, n: int) -> int:
        return reduce(lambda r, _: (r[1], sum(r)), range(n), (1, 1))[0]
```
