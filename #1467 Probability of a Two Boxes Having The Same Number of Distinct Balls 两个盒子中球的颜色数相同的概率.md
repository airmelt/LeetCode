# 1467 Probability of a Two Boxes Having The Same Number of Distinct Balls 两个盒子中球的颜色数相同的概率

__Description:__

Given  `2n` balls of  `k` distinct colors. You will be given an integer array  `balls` of size  `k` where  `balls[i]` is the number of balls of color  `i`.

All the balls will be __shuffled uniformly at random__, then we will distribute the first  `n` balls to the first box and the remaining  `n` balls to the other box (Please read the explanation of the second example carefully).

Please note that the two boxes are considered different. For example, if we have two balls of colors  `a` and  `b`, and two boxes  `[]` and  `()`, then the distribution  `[a] (b)` is considered different than the distribution  `[b] (a)` (Please read the explanation of the first example carefully).

Return _the probability_ that the two boxes have the same number of distinct balls. Answers within  `10 ^ -5` of the actual value will be accepted as correct.

__Example:__

Example 1:

```text
Input: balls = [1,1]
Output: 1.00000
Explanation: Only 2 ways to divide the balls equally:
- A ball of color 1 to box 1 and a ball of color 2 to box 2
- A ball of color 2 to box 1 and a ball of color 1 to box 2
In both ways, the number of distinct colors in each box is equal. The probability is 2/2 = 1
```

Example 2:

```text
Input: balls = [2,1,1]
Output: 0.66667
Explanation: We have the set of balls [1, 1, 2, 3]
This set of balls will be shuffled randomly and we may have one of the 12 distinct shuffles with equal probability (i.e. 1/12):
[1,1 / 2,3], [1,1 / 3,2], [1,2 / 1,3], [1,2 / 3,1], [1,3 / 1,2], [1,3 / 2,1], [2,1 / 1,3], [2,1 / 3,1], [2,3 / 1,1], [3,1 / 1,2], [3,1 / 2,1], [3,2 / 1,1]
After that, we add the first two balls to the first box and the second two balls to the second box.
We can see that 8 of these 12 possible random distributions have the same number of distinct colors of balls in each box.
Probability is 8/12 = 0.66667
```

Example 3:

```text
Input: balls = [1,2,1,2]
Output: 0.60000
Explanation: The set of balls is [1, 2, 2, 3, 4, 4]. It is hard to display all the 180 possible random shuffles of this set but it is easy to check that 108 of them will have the same number of distinct colors in each box.
Probability = 108 / 180 = 0.6
```

__Constraints:__

- `1 <= balls.length <= 8`
- `1 <= balls[i] <= 6`
- `sum(balls)` is even.

__题目描述:__

桌面上有  `2n` 个颜色不完全相同的球，球上的颜色共有  `k` 种。给你一个大小为  `k` 的整数数组  `balls` ，其中  `balls[i]` 是颜色为  `i` 的球的数量。

__示例:__

所有的球都已经 __随机打乱顺序__ ，前  `n` 个球放入第一个盒子，后  `n` 个球放入另一个盒子（请认真阅读示例 2 的解释部分）。

__注意：__这两个盒子是不同的。例如，两个球颜色分别为  `a` 和  `b`，盒子分别为  `[]` 和  `()`，那么  `[a] (b)` 和  `[b] (a)` 这两种分配方式是不同的（请认真阅读示例的解释部分）。

```text
请返回「两个盒子中球的颜色数相同」的情况的概率。答案与真实值误差在  `10 ^ -5` 以内，则被视为正确答案
```

示例 1：

```text
输入：balls = [1,1]
输出：1.00000
解释：球平均分配的方式只有两种：
- 颜色为 1 的球放入第一个盒子，颜色为 2 的球放入第二个盒子
- 颜色为 2 的球放入第一个盒子，颜色为 1 的球放入第二个盒子
这两种分配，两个盒子中球的颜色数都相同。所以概率为 2/2 = 1 。
```

示例 2：

```text
输入：balls = [2,1,1]
输出：0.66667
解释：球的列表为 [1, 1, 2, 3]
随机打乱，得到 12 种等概率的不同打乱方案，每种方案概率为 1/12 ：
[1,1 / 2,3], [1,1 / 3,2], [1,2 / 1,3], [1,2 / 3,1], [1,3 / 1,2], [1,3 / 2,1], [2,1 / 1,3], [2,1 / 3,1], [2,3 / 1,1], [3,1 / 1,2], [3,1 / 2,1], [3,2 / 1,1]
然后，我们将前两个球放入第一个盒子，后两个球放入第二个盒子。
这 12 种可能的随机打乱方式中的 8 种满足「两个盒子中球的颜色数相同」。
概率 = 8/12 = 0.66667
```

示例 3：

```text
输入：balls = [1,2,1,2]
输出：0.60000
解释：球的列表为 [1, 2, 2, 3, 4, 4]。要想显示所有 180 种随机打乱方案是很难的，但只检查「两个盒子中球的颜色数相同」的 108 种情况是比较容易的。
概率 = 108 / 180 = 0.6 。
```

__提示：__

- `1 <= balls.length <= 8`
- `1 <= balls[i] <= 6`
- `sum(balls)` 是偶数

__思路:__

```text
题意是说有 k 个颜色不同的球, balls 数组每个数对应一个不同颜色的球
将这些球平分放入两个盒子中, 求盒子中球的颜色的种类的数量相同的概率

组合数公式参考 C(m, n) = C(m - 1, n - 1) + C(m - 1, n)
(从 m 个选 n 个, 可以是从 m - 1 个中选 n 个, 也可以是 m - 1 个中选 n - 1 个加上第 m 个)

从 2n 个球中选择 n 个球有 total = C(2n, n) 种方式

将相同颜色的球合并, balls = [x_1, x_2, x_3, ...], total_ball = sum(balls), box_size = total_ball / 2, 即每个盒子的容量为 box_size

1. 回溯
对 k 个颜色不同的球选择第 i 种颜色 x_i 个的方式有 C(x_i, balls[i]) 种方式, 对所有颜色的球使用乘法定理为 C(x_1, balls[1]) * ... * C(x_k, balls[k]) 种方式
每次回溯的时候检查球的数量和颜色的数量是否相同
如果盒子中的球超过了一半的球可以提前剪枝
时间复杂度为 O(N ^ 2 + K * (M + 1) ^ K), 空间复杂度为 O(N ^ 2 + K), N 为每个盒子中的球数, 即 sum(balls)(box_size), K 为球的颜色的种类数, M 为每个颜色的最大球数
2. 动态规划
设 dp[c][t][d] 表示从前 i 种颜色的球中选择 t 个球放入第一个盒子且两个盒子的颜色种类数相差为 d 的方案数
为了保证 d 为正, 可以将 d 的取值范围 [-k, k] 向右平移 k 个单位为 [0, 2k], 也就是说两个盒子的颜色种类数相差为 k 时, 实际上两个盒子的种类数相等
初始化 dp[0][0][k] = 1, 不放入球的方案数为 1
最后满足题意的方案数为 dp[k][n][k], 返回 dp[k][n][k] / total 即可
对第 c 种颜色, 球的取值范围为 [0, balls[c - 1]], 记 ball = balls[c - 1], 从中取 b 个球放入第一个盒子的方案数为 C(ball, b)
如果 b == 0, 那么就是将球全部放入第二个盒子, 两个盒子的颜色差 diff = -1, 如果 b == ball, 那么就是将球全部放入第一个盒子, 两个盒子的颜色差 diff = 1, 否则这次不会改变颜色差 diff = 0
对于盒子中已经有的球 t, b <= t <= box_size, 这一次放入了 b 个球, 球的总数不能大于盒子的容量
所以转移方程为 dp[c][t][d] = dp[c - 1][t - b][d - diff] * C(ball, b), 注意到 d 的取值范围为 [0, 2k], 需要保证 d 和 d - diff 都在该区间内 max(0, diff) <= d <= min(2k, 2k + diff)
由于 dp[c] 只取决于 dp[c - 1], 可以用滚动数组优化空间复杂度
时间复杂度为 O(N ^ 2K ^ 2), 空间复杂度为 O(N ^ 2 + NK), N 为每个盒子中的球数, 即 sum(balls)(box_size), K 为球的颜色的种类数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    double getProbability(vector<int>& balls) 
    {
        int k = balls.size(), total_balls = accumulate(balls.begin(), balls.end(), 0), box_size = total_balls >> 1;
        vector<vector<long long>> combinations(total_balls + 1, vector<long long>(total_balls + 1));
        combinations.front().front() = 1;
        for (int i = 1; i <= total_balls; i++)
        {
            combinations[i][0] = 1;
            combinations[i][i] = 1;
            for (int j = 1; j < i; j++) combinations[i][j] = combinations[i - 1][j - 1] + combinations[i - 1][j];
        }
        vector<vector<double>> dp(box_size + 1, vector<double>((k << 1) + 1));
        dp.front()[k] = 1;
        for (int c = 1; c <= k; c++) 
        {
            vector<vector<double>> cur(box_size + 1, vector<double>((k << 1) + 1));
            int ball = balls[c - 1];
            for (int b = 0; b <= ball; b++) 
            {
                int diff = b == 0 ? -1 : (b == ball);
                for (int t = b; t <= box_size; t++) for (int d = max(0, diff); d <= min(k << 1, (k << 1) + diff); d++) cur[t][d] += dp[t - b][d - diff] * combinations[ball][b];
            }
            dp = cur;
        }
        return dp[box_size][k] / combinations[total_balls][box_size];
    }
};
```

__Java__:

```Java
class Solution {
    public double getProbability(int[] balls) {
        int k = balls.length, totalBalls = Arrays.stream(balls).sum(), boxSize = totalBalls >> 1;
        long[][] combinations = new long[totalBalls + 1][totalBalls + 1];
        combinations[0][0] = 1;
        for (int i = 1; i <= totalBalls; i++) {
            combinations[i][0] = 1;
            combinations[i][i] = 1;
            for (int j = 1; j < i; j++) combinations[i][j] = combinations[i - 1][j - 1] + combinations[i - 1][j];
        }
        double[][] dp = new double[boxSize + 1][(k << 1) + 1];
        dp[0][k] = 1;
        for (int c = 1; c <= k; c++) {
            double[][] cur = new double[boxSize + 1][(k << 1) + 1];
            int ball = balls[c - 1];
            for (int b = 0; b <= ball; b++) {
                int diff = b == 0 ? -1 : (b == ball ? 1 : 0);
                for (int t = b; t <= boxSize; t++) {
                    for (int d = Math.max(0, diff); d <= Math.min(k * 2, k * 2 + diff); d++) {
                        cur[t][d] += dp[t - b][d - diff] * combinations[ball][b];
                    }
                }
            }
            dp = cur;
        }
        return dp[boxSize][k] / combinations[totalBalls][boxSize];
    }
}
```

__Python__:

```Python
class Solution:
    def getProbability(self, balls: List[int]) -> float:
        result, n, s = 0, len(balls), sum(balls)
        
        def dfs(ball: int, b1: int, c1: int, b2: int, c2: int, cur: int) -> NoReturn:
            nonlocal result
            if b1 > (s >> 1) or b2 > (s >> 1):
                return
            if ball == n:
                result += cur * (b1 == b2 and c1 == c2)
                return
            for b in range(balls[ball] + 1):
                dfs(ball + 1, b1 + b, c1 + (not not b), b2 + balls[ball] - b, c2 + (b != balls[ball]), cur * comb(balls[ball], b))
        dfs(0, 0, 0, 0, 0, 1)
        return result / comb(s, s >> 1)
```
