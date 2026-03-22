# 935 Knight Dialer 骑士拨号器

__Description__:
The chess knight has a unique movement, it may move two squares vertically and one square horizontally, or two squares horizontally and one square vertically (with both forming the shape of an L). The possible movements of chess knight are shown in this diagaram:

A chess knight can move as indicated in the chess diagram below:

![chess](https://assets.leetcode.com/uploads/2020/08/18/chess.jpg)

We have a chess knight and a phone pad as shown below, the knight can only stand on a numeric cell (i.e. blue cell).

![phone](https://assets.leetcode.com/uploads/2020/08/18/phone.jpg)

Given an integer n, return how many distinct phone numbers of length n we can dial.

You are allowed to place the knight on any numeric cell initially and then you should perform n - 1 jumps to dial a number of length n. All jumps should be valid knight jumps.

As the answer may be very large, return the answer modulo 10^9 + 7.

__Example:__

Example 1:

Input: n = 1
Output: 10
Explanation: We need to dial a number of length 1, so placing the knight over any numeric cell of the 10 cells is sufficient.

Example 2:

Input: n = 2
Output: 20
Explanation: All the valid number we can dial are [04, 06, 16, 18, 27, 29, 34, 38, 40, 43, 49, 60, 61, 67, 72, 76, 81, 83, 92, 94]

Example 3:

Input: n = 3
Output: 46

Example 4:

Input: n = 4
Output: 104

Example 5:

Input: n = 3131
Output: 136006598
Explanation: Please take care of the mod.

__Constraints:__

1 <= n <= 5000

__题目描述__:
国际象棋中的骑士可以按下图所示进行移动：

![骑士](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/11/03/knight.png)
 .

![键盘](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/11/03/keypad.png)

这一次，我们将 “骑士” 放在电话拨号盘的任意数字键（如上图所示）上，接下来，骑士将会跳 N-1 步。每一步必须是从一个数字键跳到另一个数字键。

每当它落在一个键上（包括骑士的初始位置），都会拨出键所对应的数字，总共按下 N 位数字。

你能用这种方式拨出多少个不同的号码？

因为答案可能很大，所以输出答案模 10^9 + 7。

__示例 :__

示例 1：

输入：1
输出：10

示例 2：

输入：2
输出：20

示例 3：

输入：3
输出：46

__提示:__

1 <= N <= 5000

__思路__:

动态规划
dp[i][j] 表示到达 i 使用 j 步的方案数
先找到每个按键可以到达的位置, 比如 4 出发可以到达 0, 3, 9
所以 dp[4][j] = dp[0][j - 1] + dp[3][j - 1] + dp[9][j - 1]
其他按键以此类推
注意 5 只有 n = 1 时能访问
注意到 dp 每一行只与上一行有关, 可以用滚动数组优化空间复杂度
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int knightDialer(int n) 
    {
        long x0 = 1, x1 = 1, x2 = 1, x3 = 1, x4 = 1, x6 = 1, x7 = 1, x8 = 1, x9 = 1, pre0 = 1, pre1 = 1, pre2 = 1, pre3 = 1, pre4 = 1, pre6 = 1, pre7 = 1, pre8 = 1, pre9 = 1, mod = 1e9 + 7;
        for (int i = 1; i < n; i++)
        {
            x0 = pre4 + pre6;
            x1 = pre6 + pre8;
            x2 = pre7 + pre9;
            x3 = pre4 + pre8;
            x4 = pre0 + pre3 + pre9;
            x6 = pre0 + pre1 + pre7;
            x7 = pre2 + pre6;
            x8 = pre1 + pre3;
            x9 = pre2 + pre4;
            pre0 = x0 % mod;
            pre1 = x1 % mod;
            pre2 = x2 % mod;
            pre3 = x3 % mod;
            pre4 = x4 % mod;
            pre6 = x6 % mod;
            pre7 = x7 % mod;
            pre8 = x8 % mod;
            pre9 = x9 % mod;
        }
        return (x0 + x1 + x2 + x3 + x4 + (n == 1) + x6 + x7 + x8 + x9) % mod;
    }
};
```

__Java__:

```Java
class Solution {
    public int knightDialer(int n) {
        long x0 = 1, x1 = 1, x2 = 1, x3 = 1, x4 = 1, x6 = 1, x7 = 1, x8 = 1, x9 = 1, pre0 = 1, pre1 = 1, pre2 = 1, pre3 = 1, pre4 = 1, pre6 = 1, pre7 = 1, pre8 = 1, pre9 = 1, mod = 1_000_000_007;
        for (int i = 1; i < n; i++) {
            x0 = pre4 + pre6;
            x1 = pre6 + pre8;
            x2 = pre7 + pre9;
            x3 = pre4 + pre8;
            x4 = pre0 + pre3 + pre9;
            x6 = pre0 + pre1 + pre7;
            x7 = pre2 + pre6;
            x8 = pre1 + pre3;
            x9 = pre2 + pre4;
            pre0 = x0 % mod;
            pre1 = x1 % mod;
            pre2 = x2 % mod;
            pre3 = x3 % mod;
            pre4 = x4 % mod;
            pre6 = x6 % mod;
            pre7 = x7 % mod;
            pre8 = x8 % mod;
            pre9 = x9 % mod;
        }
        return (int)((x0 + x1 + x2 + x3 + x4 + (n == 1 ? 1 : 0) + x6 + x7 + x8 + x9) % mod);
    }
}
```

__Python__:

```Python
class Solution:
    def knightDialer(self, n: int) -> int:
        x0 = x1 = x2 = x3 = x4 = x6 = x7 = x8 = x9 = 1
        for i in range(1, n):
            x0, x1, x2, x3, x4, x6, x7, x8, x9 = x4 + x6, x6 + x8, x7 + x9, x4 + x8, x0 + x3 + x9, x0 + x1 + x7, x2 + x6, x1 + x3, x2 + x4
        return (x0 + x1 + x2 + x3 + x4 + (n == 1) + x6 + x7 + x8 + x9) % (10 ** 9 + 7)
```
