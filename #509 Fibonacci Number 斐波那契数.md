__Description__:
The Fibonacci numbers, commonly denoted F(n) form a sequence, called the Fibonacci sequence, such that each number is the sum of the two preceding ones, starting from 0 and 1. That is,

F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), for N > 1.
Given N, calculate F(N).

__Example:__
Example 1:
Input: 2
Output: 1
Explanation: F(2) = F(1) + F(0) = 1 + 0 = 1.

Example 2:
Input: 3
Output: 2
Explanation: F(3) = F(2) + F(1) = 1 + 1 = 2.

Example 3:
Input: 4
Output: 3
Explanation: F(4) = F(3) + F(2) = 2 + 1 = 3.
 
__Note:__
0 ≤ N ≤ 30.

__题目描述__:
斐波那契数，通常用 F(n) 表示，形成的序列称为斐波那契数列。该数列由 0 和 1 开始，后面的每一项数字都是前面两项数字的和。也就是：

F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
给定 N，计算 F(N)。

__示例：__
示例 1：
输入：2
输出：1
解释：F(2) = F(1) + F(0) = 1 + 0 = 1.

示例 2：
输入：3
输出：2
解释：F(3) = F(2) + F(1) = 1 + 1 = 2.

示例 3：
输入：4
输出：3
解释：F(4) = F(3) + F(2) = 2 + 1 = 3.
 
__提示：__
0 ≤ N ≤ 30

__思路__:
参考[LeetCode #70 Climbing Stairs 爬楼梯](https://www.jianshu.com/p/8d7ceb7b7cf6)
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    int fib(int N) {
        int a = 0, b = 1;
        while (N--) {
            int temp = a;
            a = b;
            b = temp + b;
        }
        return a;
    }
};
```

__Java__:
```
class Solution {
    public int fib(int N) {
        switch(N) {
            case 0:
                return 0;
            case 1:
            case 2:
                return 1;
            case 3:
                return 2;
            case 4:
                return 3;
            case 5:
                return 5;
            case 6:
                return 8;
            case 7:
                return 13;
            case 8:
                return 21;
            case 9:
                return 34;
            case 10:
                return 55;
            case 11:
                return 89;
            case 12:
                return 144;
            case 13:
                return 233;
            case 14:
                return 377;
            case 15:
                return 610;
            case 16:
                return 987;
            case 17:
                return 1597;
            case 18:
                return 2584;
            case 19:
                return 4181;
            case 20:
                return 6765;
            case 21:
                return 10946;
            case 22:
                return 17711;
            case 23:
                return 28657;
            case 24:
                return 46368;
            case 25:
                return 75025;
            case 26:
                return 121393;
            case 27:
                return 196418;
            case 28:
                return 317811;
            case 29:
                return 514229;
            case 30:
                return 832040;
            default:
                return 0;
        }
    }
}
```

__Python__:
```
class Solution:
    def fib(self, N: int) -> int:
        a = 0
        b = 1
        while N:
            a, b = b, a + b
            N -= 1
        return a
```
