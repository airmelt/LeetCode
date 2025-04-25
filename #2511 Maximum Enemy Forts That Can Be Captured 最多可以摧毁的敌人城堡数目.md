# 2511 Maximum Enemy Forts That Can Be Captured 最多可以摧毁的敌人城堡数目

__Description:__

You are given a __0-indexed__ integer array `forts` of length `n` representing the positions of several forts. `forts[i]` can be `-1`, `0`, or `1` where:

- `-1` represents there is __no fort__ at the `i ^ th` position.
- `0` indicates there is an __enemy__ fort at the `i ^ th` position.
- `1` indicates the fort at the `i ^ th` the position is under your command.

Now you have decided to move your army from one of your forts at position `i` to an empty position `j` such that:

- `0 <= i, j <= n - 1`
- The army travels over enemy forts __only__. Formally, for all `k` where `min(i,j) < k < max(i,j)`, `forts[k] == 0.`

While moving the army, all the enemy forts that come in the way are captured.

Return _the __maximum__ number of enemy forts that can be captured_. In case it is __impossible__ to move your army, or you do not have any fort under your command, return `0`_._

__Example:__

Example 1:

```text
Input: forts = [1,0,0,-1,0,0,0,0,1]
Output: 4
Explanation:
```

- Moving the army from position 0 to position 3 captures 2 enemy forts, at 1 and 2.
- Moving the army from position 8 to position 3 captures 4 enemy forts.

Since 4 is the maximum number of enemy forts that can be captured, we return 4.

Example 2:

```text
Input: forts = [0,0,1,-1]
Output: 0
Explanation: Since no enemy fort can be captured, 0 is returned.
```

__Constraints:__

- `1 <= forts.length <= 1000`
- `-1 <= forts[i] <= 1`

__题目描述:__

给你一个长度为 `n` ，下标从 __0__ 开始的整数数组 `forts` ，表示一些城堡。 `forts[i]` 可以是 `-1` ， `0` 或者 `1` ，其中:

- `-1` 表示第 `i` 个位置 __没有__ 城堡。
- `0` 表示第 `i` 个位置有一个 __敌人__ 的城堡。
- `1` 表示第 `i` 个位置有一个你控制的城堡。

现在，你需要决定，将你的军队从某个你控制的城堡位置 `i` 移动到一个空的位置 `j` ，满足:

- `0 <= i, j <= n - 1`
- 军队经过的位置 __只有__ 敌人的城堡。正式的，对于所有 `min(i,j) < k < max(i,j)` 的 `k` ，都满足 `forts[k] == 0` 。

当军队移动时，所有途中经过的敌人城堡都会被 摧毁 。

请你返回 __最多__ 可以摧毁的敌人城堡数目。如果 __无法__ 移动你的军队，或者没有你控制的城堡，请返回 `0` 。

__示例:__

示例 1：

```text
输入：forts = [1,0,0,-1,0,0,0,0,1]
输出：4
解释：
```

- 将军队从位置 0 移动到位置 3 ，摧毁 2 个敌人城堡，位置分别在 1 和 2 。
- 将军队从位置 8 移动到位置 3 ，摧毁 4 个敌人城堡。

4 是最多可以摧毁的敌人城堡数目，所以我们返回 4 。

示例 2：

```text
输入：forts = [0,0,1,-1]
输出：0
解释：由于无法摧毁敌人的城堡，所以返回 0 。
```

__提示：__

- `1 <= forts.length <= 1000`
- `-1 <= forts[i] <= 1`

__思路:__

```text
模拟
统计 1 和 -1 之间的 0 的个数
可以用 pre 记录上一个不为 0 的下标
当前值与 pre 不相等时更新 result = max(result, i - pre - 1)
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int captureForts(vector<int>& forts) 
    {
        int result = 0, pre = -1, n = forts.size();
        for (int i = 0; i < n; i++) 
        {
            if (forts[i])
            {
                if (pre > -1 and forts[i] != forts[pre]) result = max(result, i - pre - 1);
                pre = i;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int captureForts(int[] forts) {
        int result = 0, pre = -1, n = forts.length;
        for (int i = 0; i < n; i++) {
            if (forts[i] != 0) {
                if (pre > -1 && forts[i] != forts[pre]) result = Math.max(result, i - pre - 1);
                pre = i;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def captureForts(self, forts: List[int]) -> int:
        return max((t for (a, _), (b, t), (c, _) in zip(G, G[1:], G[2:]) if (a, b, c) in {(1, 0, -1), (-1, 0, 1)}), default=0) if (G := [(c, len([*g])) for c, g in groupby(forts)]) else 0
```
