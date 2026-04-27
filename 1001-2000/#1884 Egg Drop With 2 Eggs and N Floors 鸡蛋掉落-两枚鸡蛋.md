# 1884 Egg Drop With 2 Eggs and N Floors 鸡蛋掉落-两枚鸡蛋

__Description:__

You are given __two identical__ eggs and you have access to a building with `n` floors labeled from `1` to `n`.

You know that there exists a floor `f` where `0 <= f <= n` such that any egg dropped at a floor __higher__ than `f` will __break__, and any egg dropped __at or below__ floor `f` will __not break__.

In each move, you may take an __unbroken__ egg and drop it from any floor `x` (where `1 <= x <= n`). If the egg breaks, you can no longer use it. However, if the egg does not break, you may __reuse__ it in future moves.

Return _the __minimum number of moves__ that you need to determine __with certainty__ what the value of_ `f` is.

__Example:__

Example 1:

```text
Input: n = 2
Output: 2
Explanation: We can drop the first egg from floor 1 and the second egg from floor 2.
If the first egg breaks, we know that f = 0.
If the second egg breaks but the first egg didn't, we know that f = 1.
Otherwise, if both eggs survive, we know that f = 2.
```

Example 2:

```text
Input: n = 100
Output: 14
Explanation: One optimal strategy is:
- Drop the 1st egg at floor 9. If it breaks, we know f is between 0 and 8. Drop the 2nd egg starting from floor 1 and going up one at a time to find f within 8 more drops. Total drops is 1 + 8 = 9.
- If the 1st egg does not break, drop the 1st egg again at floor 22. If it breaks, we know f is between 9 and 21. Drop the 2nd egg starting from floor 10 and going up one at a time to find f within 12 more drops. Total drops is 2 + 12 = 14.
- If the 1st egg does not break again, follow a similar process dropping the 1st egg from floors 34, 45, 55, 64, 72, 79, 85, 90, 94, 97, 99, and 100.
Regardless of the outcome, it takes at most 14 drops to determine f.
```

__Constraints:__

- `1 <= n <= 1000`

__题目描述:__

给你 __2 枚相同__ 的鸡蛋，和一栋从第 `1` 层到第 `n` 层共有 `n` 层楼的建筑。

已知存在楼层 `f` ，满足 `0 <= f <= n` ，任何从 __高于__ `f` 的楼层落下的鸡蛋都 __会碎__ ，从 __`f` 楼层或比它低__ 的楼层落下的鸡蛋都 __不会碎__ 。

每次操作，你可以取一枚 __没有碎__ 的鸡蛋并把它从任一楼层 `x` 扔下（满足 `1 <= x <= n`）。如果鸡蛋碎了，你就不能再次使用它。如果某枚鸡蛋扔下后没有摔碎，则可以在之后的操作中 __重复使用__ 这枚鸡蛋。

请你计算并返回要确定 `f` __确切的值__ 的 __最小操作次数__ 是多少？

__示例:__

示例 1：

```text
输入：n = 2
输出：2
解释：我们可以将第一枚鸡蛋从 1 楼扔下，然后将第二枚从 2 楼扔下。
如果第一枚鸡蛋碎了，可知 f = 0；
如果第二枚鸡蛋碎了，但第一枚没碎，可知 f = 1；
否则，当两个鸡蛋都没碎时，可知 f = 2。
```

示例 2：

```text
输入：n = 100
输出：14
解释：
一种最优的策略是：
- 将第一枚鸡蛋从 9 楼扔下。如果碎了，那么 f 在 0 和 8 之间。将第二枚从 1 楼扔下，然后每扔一次上一层楼，在 8 次内找到 f 。总操作次数 = 1 + 8 = 9 。
- 如果第一枚鸡蛋没有碎，那么再把第一枚鸡蛋从 22 层扔下。如果碎了，那么 f 在 9 和 21 之间。将第二枚鸡蛋从 10 楼扔下，然后每扔一次上一层楼，在 12 次内找到 f 。总操作次数 = 2 + 12 = 14 。
- 如果第一枚鸡蛋没有再次碎掉，则按照类似的方法从 34, 45, 55, 64, 72, 79, 85, 90, 94, 97, 99 和 100 楼分别扔下第一枚鸡蛋。
不管结果如何，最多需要扔 14 次来确定 f 。
```

__提示：__

- `1 <= n <= 1000`

__思路:__

```text
1. 数学推导
设总操作次数为 N
第一次选择为 N, 因为至少还需要 N - 1 次来验证第二个鸡蛋
同理所以每一次选择为 N + (N - 1) + ... + 1 = N * (N + 1) / 2 <= n
解出这个一元二次方程即可
时间复杂度为 O(1), 空间复杂度为 O(1)
2. 动态规划
设 dp[i][j] 表示 j 层楼 i + 1 个鸡蛋的最小操作次数
当只有一个鸡蛋时 dp[0][j] = j, 需要逐层验证
当有两个鸡蛋时, 取任意 k 位于 [1, j] 区间内, 有两种选择:
    1. 鸡蛋碎了, dp[i][j] = dp[i - 1][k - 1] + 1
    2. 鸡蛋没碎, dp[i][j] = dp[i][j - k] + 1
两者取最大值, 即
dp[i][j] = min(max(dp[i - 1][k - 1] + 1, dp[i][j - k] + 1)), 1 <= k <= j
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int twoEggDrop(int n) 
    {
        return ceil((-1.0 + sqrt(n * 8 + 1)) / 2);
    }
};
```

__Java__:

```Java
class Solution {
    public int twoEggDrop(int n) {
        return (int)Math.ceil((-1.0 + Math.sqrt(n * 8 + 1)) / 2);
    }
}
```

__Python__:

```Python
class Solution:
    def twoEggDrop(self, n: int) -> int:
        return ceil((-1.0 + sqrt(n * 8 + 1)) / 2)
```
