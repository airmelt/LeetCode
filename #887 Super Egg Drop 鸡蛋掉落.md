# 887 Super Egg Drop 鸡蛋掉落

__Description__:
You are given k identical eggs and you have access to a building with n floors labeled from 1 to n.

You know that there exists a floor f where 0 <= f <= n such that any egg dropped at a floor higher than f will break, and any egg dropped at or below floor f will not break.

Each move, you may take an unbroken egg and drop it from any floor x (where 1 <= x <= n). If the egg breaks, you can no longer use it. However, if the egg does not break, you may reuse it in future moves.

Return the minimum number of moves that you need to determine with certainty what the value of f is.

__Example:__

Example 1:

Input: k = 1, n = 2
Output: 2
Explanation:
Drop the egg from floor 1. If it breaks, we know that f = 0.
Otherwise, drop the egg from floor 2. If it breaks, we know that f = 1.
If it does not break, then we know f = 2.
Hence, we need at minimum 2 moves to determine with certainty what the value of f is.

Example 2:

Input: k = 2, n = 6
Output: 3

Example 3:

Input: k = 3, n = 14
Output: 4

__Constraints:__

1 <= k <= 100
1 <= n <= 10^4

__题目描述__:
给你 k 枚相同的鸡蛋，并可以使用一栋从第 1 层到第 n 层共有 n 层楼的建筑。

已知存在楼层 f ，满足 0 <= f <= n ，任何从 高于 f 的楼层落下的鸡蛋都会碎，从 f 楼层或比它低的楼层落下的鸡蛋都不会破。

每次操作，你可以取一枚没有碎的鸡蛋并把它从任一楼层 x 扔下（满足 1 <= x <= n）。如果鸡蛋碎了，你就不能再次使用它。如果某枚鸡蛋扔下后没有摔碎，则可以在之后的操作中 重复使用 这枚鸡蛋。

请你计算并返回要确定 f 确切的值 的 最小操作次数 是多少？

__示例 :__

示例 1：

输入：k = 1, n = 2
输出：2
解释：
鸡蛋从 1 楼掉落。如果它碎了，肯定能得出 f = 0 。
否则，鸡蛋从 2 楼掉落。如果它碎了，肯定能得出 f = 1 。
如果它没碎，那么肯定能得出 f = 2 。
因此，在最坏的情况下我们需要移动 2 次以确定 f 是多少。

示例 2：

输入：k = 2, n = 6
输出：3

示例 3：

输入：k = 3, n = 14
输出：4

__提示:__

1 <= k <= 100
1 <= n <= 10^4

__思路__:

1. 动态规划(超时)
设 dp[k][n] 表示用 k 个鸡蛋测试 n 层至少使用的步数
每次选择一个楼层 x
有两种情况
鸡蛋如果碎了那就是 dp[k - 1][x - 1], 少了一个鸡蛋, 保证在 x 层以下
鸡蛋如果没碎那就是 dp[k][n - x], 保证碎的楼层在 x 层以上
初始化 dp[1][i] = i, 表示只有一个鸡蛋的时候只能一层层的尝试
动态转移方程 dp[k][n] = 1 + min(max(dp[k][n - x], dp[k - 1][x - 1])), 其中 0 < x <= n, 取 max 是因为每次都必须保证在最坏情况下都能得到 dp[k][x] 的值, 取 min 是在 x 中选择最优的那一层进行扔鸡蛋的操作
时间复杂度为 O(kn ^ 2), 空间复杂度为 O(kn)
2. 动态规划 ➕ 二分法(Python)
注意到 dp[k][x] 是一个对 x 单调递增的离散函数, 因为当 k, 也就是鸡蛋数固定的时候, 楼层越多肯定尝试次数会增加
那么 dp[k - 1][x - 1] 就是一个对 x 单调递减的离散函数
所以可以尝试使用二分法找到 dp[k][x] = dp[k - 1][x - 1] 对应的 x, 和 x 最接近的整数值就是最小值
需要找到 [left, right], 这个区间长度最多为 2
时间复杂度为 O(knlgn), 空间复杂度为 O(kn)
3. 动态规划 ➕ 决策单调性(Java)
对 dp[k][n] = 1 + min(max(dp[k][n - x], dp[k - 1][x - 1])) 如果固定 k, 随 n 增加 dp[k - 1][x - 1] 不变, dp[k][n - x] 增加, 那么必然有一个点能满足 x(opt) 取到 max(dp[k][n - x], dp[k - 1][x - 1])
找到第一个不满足 max(dp[x - 1], dp2[m - x]) > max(dp[x], dp2[m - x - 1]) 的 x 就是最优的 x
记 dp[i] 为 dp[k][i], 可以用滚动数组的方式更新
时间复杂度为 O(kn), 空间复杂度为 O(n)
4. 数学法(C++)
如果题目改成用 t 次操作, k 个鸡蛋最多能尝试多少层
只要求出最小的 dp[t][k] >= n 即可
t 的范围是 [1, n], 最少也要扔 1 次, 扔 n 次以上也是没有意义的
边界条件 dp[t][1] = t, dp[1][k] = 1
动态转移方程, dp[t][k] = 1 + dp[t - 1][k - 1] + dp[t - 1][k], 扔一次鸡蛋要么它没碎, 要么碎了
同样可以使用滚动数组的方式进行更新
时间复杂度为 O(kn), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int superEggDrop(int k, int n) 
    {
        vector<int> dp(k + 1);
        int result = 0;
        while (dp[k] < n)
        {
            ++result;
            for (int i = k; i > 0; i--) dp[i] = dp[i - 1] + dp[i] + 1;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int superEggDrop(int k, int n) {
        int[] dp = new int[n + 1];
        for (int i = 0; i <= n; i++) dp[i] = i;
        for (int i = 2; i <= k; i++) {
            int dp2[] = new int[n + 1], x = 1;
            for (int j = 1; j <= n; j++) {
                while (x < j && Math.max(dp[x - 1], dp2[j - x]) > Math.max(dp[x], dp2[j - x - 1])) ++x;
                dp2[j] = 1 + Math.max(dp[x - 1], dp2[j - x]);
            }
            dp = dp2;
        }
        return dp[n];
    }
}
```

__Python__:

```Python
class Solution:
    def superEggDrop(self, k: int, n: int) -> int:
        memo = {}
        def dp(k: int, n: int) -> int:
            if (k, n) not in memo:
                if not n:
                    result = 0
                elif k == 1:
                    result = n
                else:
                    left, right = 1, n
                    while left + 1 < right:
                        x = left + ((right - left) >> 1)
                        if (t1 := dp(k - 1, x - 1)) < (t2 := dp(k, n - x)):
                            left = x
                        elif t1 > t2:
                            right = x
                        else:
                            left = right = x
                    result = 1 + min(max(dp(k - 1, x - 1), dp(k, n - x)) for x in (left, right))
                memo[k, n] = result
            return memo[k, n]
        return dp(k, n)
```
