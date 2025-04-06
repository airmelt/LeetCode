# 351 Android Unlock Patterns 安卓系统手势解锁

__Description:__

Android devices have a special lock screen with a `3 x 3` grid of dots. Users can set an "unlock pattern" by connecting the dots in a specific sequence, forming a series of joined line segments where each segment's endpoints are two consecutive dots in the sequence. A sequence of `k` dots is a __valid__ unlock pattern if both of the following are true:

- All the dots in the sequence are __distinct__.
- If the line segment connecting two consecutive dots in the sequence passes through the __center__ of any other dot, the other dot __must have previously appeared__ in the sequence. No jumps through the center non-selected dots are allowed.
  - For example, connecting dots `2` and `9` without dots `5` or `6` appearing beforehand is valid because the line from dot `2` to dot `9` does not pass through the center of either dot `5` or `6`.
  - However, connecting dots `1` and `3` without dot `2` appearing beforehand is invalid because the line from dot `1` to dot `3` passes through the center of dot `2`.

Here are some example valid and invalid unlock patterns:

![351-1](https://assets.leetcode.com/uploads/2018/10/12/android-unlock.png)

- The 1st pattern `[4,1,3,6]` is invalid because the line connecting dots `1` and `3` pass through dot `2`, but dot `2` did not previously appear in the sequence.
- The 2nd pattern `[4,1,9,2]` is invalid because the line connecting dots `1` and `9` pass through dot `5`, but dot `5` did not previously appear in the sequence.
- The 3rd pattern `[2,4,1,3,6]` is valid because it follows the conditions. The line connecting dots `1` and `3` meets the condition because dot `2` previously appeared in the sequence.
- The 4th pattern `[6,5,4,1,9,2]` is valid because it follows the conditions. The line connecting dots `1` and `9` meets the condition because dot `5` previously appeared in the sequence.

Given two integers `m` and `n`, return _the __number of unique and valid unlock patterns__ of the Android grid lock screen that consist of __at least__ `m` keys and __at most__ `n` keys._

Two unlock patterns are considered __unique__ if there is a dot in one sequence that is not in the other, or the order of the dots is different.

__Example:__

Example 1:

```text
Input: m = 1, n = 1
Output: 9
```

Example 2:

```text
Input: m = 1, n = 2
Output: 65
```

__Constraints:__

- `1 <= m, n <= 9`

__题目描述:__

我们都知道安卓有个手势解锁的界面，是一个 `3 x 3` 的点所绘制出来的网格。用户可以设置一个 “解锁模式” ，通过连接特定序列中的点，形成一系列彼此连接的线段，每个线段的端点都是序列中两个连续的点。如果满足以下两个条件，则 `k` 点序列是有效的解锁模式：

- 解锁模式中的所有点 __互不相同__ 。
- 假如模式中两个连续点的线段需要经过其他点的 __中心__ ，那么要经过的点 __必须提前出现__ 在序列中（已经经过），不能跨过任何还未被经过的点。
  - 例如，点 `5` 或 `6` 没有提前出现的情况下连接点 `2` 和 `9` 是有效的，因为从点 `2` 到点 `9` 的线没有穿过点 `5` 或 `6` 的中心。
  - 然而，点 `2` 没有提前出现的情况下连接点 `1` 和 `3` 是无效的，因为从圆点 `1` 到圆点 `3` 的直线穿过圆点 `2` 的中心。

以下是一些有效和无效解锁模式的示例：

![351-2](https://assets.leetcode.com/uploads/2018/10/12/android-unlock.png)

- 无效手势：`[4,1,3,6]` ，连接点 1 和点 3 时经过了未被连接过的 2 号点。
- 无效手势：`[4,1,9,2]` ，连接点 1 和点 9 时经过了未被连接过的 5 号点。
- 有效手势：`[2,4,1,3,6]` ，连接点 1 和点 3 是有效的，因为虽然它经过了点 2 ，但是点 2 在该手势中之前已经被连过了。
- 有效手势：`[6,5,4,1,9,2]` ，连接点 1 和点 9 是有效的，因为虽然它经过了按键 5 ，但是点 5 在该手势中之前已经被连过了。

给你两个整数，分别为 `​​m` 和 `n` ，那么请返回有多少种 __不同且有效的解锁模式__ ，是 __至少__ 需要经过 `m` 个点，但是 __不超过__ `n` 个点的。

两个解锁模式 __不同__ 需满足：经过的点不同或者经过点的顺序不同。

__示例:__

示例 1：

```text
输入：m = 1, n = 1
输出：9
```

示例 2：

```text
输入：m = 1, n = 2
输出：65
```

__提示：__

- `1 <= m, n <= 9`

__思路:__

```text
1. 动态规划
将 1-9 映射到 0-8
使用 block 记录所有需要经过的键
比如 (0, 2) 表示从 0 到 2 需要经过 1
使用 dp 记录当前状态
dp[(used, unused, last)], 表示当前状态为 used, 未使用的键为 unused, 上一个键为 last, used, unused 都是二进制表示, 位 1 表示已经使用
初始化 dp[(0, 0b111111111, None)] = 1, 0b111111111 表示所有的键都未使用, 即 (1 << 9) - 1
dp[used ^ (1 << j), unused ^ (1 << j), j] += dp[used, unused, last], 其中 0 < j < 9
每次需要选择一个键, 从未使用的键中选择一个, 即 unused & (1 << j) == 1
如果 (last, j) 在 block 中而且 block 需要的还未被使用, 就不能使用, 即 (last, j) not in block and (1 << block[last, j]) & unused
否则可以转移 cur[used ^ (1 << j), unused ^ (1 << j), j] += dp[used, unused, last]
这里使用滚动数组, 优化空间
预处理时间复杂度为 O(9 * 9), 空间复杂度为 O(9 * 9 * 9)
2. 打表
注意到 0 < m, n < 10, 可以打表
预计算所有的 [1, 9] 的数值
返回 get(n) - get(m - 1) 即可
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfPatterns(int m, int n) 
    {
        return vector<int>{0, 9, 65, 385, 2009, 9161, 35177, 108089, 248793, 389497}[n] - vector<int>{0, 9, 65, 385, 2009, 9161, 35177, 108089, 248793, 389497}[m - 1];
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfPatterns(int m, int n) {
        return new int[]{0, 9, 65, 385, 2009, 9161, 35177, 108089, 248793, 389497}[n] - new int[]{0, 9, 65, 385, 2009, 9161, 35177, 108089, 248793, 389497}[m - 1];
    }
}
```

__Python__:

```Python
block = {(0, 2): 1, (2, 0): 1, (2, 8): 5, (8, 2): 5, (6, 8): 7, (8, 6): 7, (6, 0): 3, (0, 6): 3, (3, 5): 4, (5, 3): 4, (1, 7): 4, (7, 1): 4, (8, 0): 4, (0, 8): 4, (2, 6): 4, (6, 2): 4}
dp = {(0, (1 << 9) - 1, None): 1}
table = [0] * 10

for i in range(1, 10):
    cur = defaultdict(int)
    for (used, unused, last), v in dp.items():
        for j in range(9):
            if unused & (1 << j):
                if (last, j) in block and (1 << block[last, j]) & unused: continue
                cur[used | (1 << j), unused ^ (1 << j), j] += v
        table[i] = sum(cur.values())
        dp = cur

class Solution:
    def numberOfPatterns(self, m: int, n: int) -> int:
        return sum(table[m:n + 1])
```
