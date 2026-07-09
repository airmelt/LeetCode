# 3096 Minimum Levels to Gain More Points 得到更多分数的最少关卡数目

__Description:__

You are given a binary array `possible` of length `n`.

Alice and Bob are playing a game that consists of `n` levels. Some of the levels in the game are __impossible__ to clear while others can __always__ be cleared. In particular, if `possible[i] == 0`, then the `i ^ th` level is __impossible__ to clear for __both__ the players. A player gains `1` point on clearing a level and loses `1` point if the player fails to clear it.

At the start of the game, Alice will play some levels in the __given order__ starting from the `0 ^ th` level, after which Bob will play for the rest of the levels.

Alice wants to know the minimum number of levels she should play to gain more points than Bob, if both players play optimally to maximize their points.

Return _the __minimum__ number of levels Alice should play to gain more points_. _If this is __not__ possible, return_ `-1`.

__Note__ that each player must play at least `1` level.

__Example:__

Example 1:

```text
Input: possible = [1,0,1,0]

Output: 1

Explanation:

Let's look at all the levels that Alice can play up to:
```

- If Alice plays only level 0 and Bob plays the rest of the levels, Alice has 1 point, while Bob has -1 + 1 - 1 = -1 point.
- If Alice plays till level 1 and Bob plays the rest of the levels, Alice has 1 - 1 = 0 points, while Bob has 1 - 1 = 0 points.
- If Alice plays till level 2 and Bob plays the rest of the levels, Alice has 1 - 1 + 1 = 1 point, while Bob has -1 point.

Alice must play a minimum of 1 level to gain more points.

Example 2:

```text
Input: possible = [1,1,1,1,1]

Output: 3

Explanation:

Let's look at all the levels that Alice can play up to:
```

- If Alice plays only level 0 and Bob plays the rest of the levels, Alice has 1 point, while Bob has 4 points.
- If Alice plays till level 1 and Bob plays the rest of the levels, Alice has 2 points, while Bob has 3 points.
- If Alice plays till level 2 and Bob plays the rest of the levels, Alice has 3 points, while Bob has 2 points.
- If Alice plays till level 3 and Bob plays the rest of the levels, Alice has 4 points, while Bob has 1 point.

Alice must play a minimum of 3 levels to gain more points.

Example 3:

```text
Input: possible = [0,0]

Output: -1

Explanation:

The only possible way is for both players to play 1 level each. Alice plays level 0 and loses 1 point. Bob plays level 1 and loses 1 point. As both players have equal points, Alice can't gain more points than Bob.
```

__Constraints:__

- `2 <= n == possible.length <= 10 ^ 5`
- `possible[i]` is either `0` or `1`.

__题目描述:__

给你一个长度为 `n` 的二进制数组 `possible` 。

Alice 和 Bob 正在玩一个有 `n` 个关卡的游戏，游戏中有一些关卡是 __困难__ 模式，其他的关卡是 __简单__ 模式。如果 `possible[i] == 0` ，那么第 `i` 个关卡是 __困难__ 模式，两个玩家 __都不可能__ 通过。一个玩家通过一个简单模式的关卡可以获得 `1` 分，遇到困难模式的关卡将失去 `1` 分。

游戏的一开始，Alice 将从第 `0` 级开始 __按顺序__ 完成一些关卡，然后 Bob 会完成剩下的所有关卡。

假设两名玩家都采取最优策略，目的是 最大化 自己的得分，Alice 想知道自己 最少 需要完成多少个关卡，才能获得比 Bob 更多的分数。

请你返回 Alice 获得比 Bob 更多的分数所需要完成的 __最少__ 关卡数目，如果 __无法__ 达成，那么返回 `-1` 。

__注意__，每个玩家都至少需要完成 `1` 个关卡。

__示例:__

示例 1：

```text
输入：possible = [1,0,1,0]

输出：1

解释：

我们来看一下 Alice 可以完成的关卡数目：
```

- 如果 Alice 只完成关卡 0 ，Bob 完成剩下的所有关卡，那么 Alice 获得 1 分，Bob 获得 -1 + 1 - 1 = -1 分。
- 如果 Alice 完成到关卡 1 ，Bob 完成剩下的所有关卡，那么 Alice 获得 1 - 1 = 0 分，Bob 获得 1 - 1 = 0 分。
- 如果 Alice 完成到关卡 2 ，Bob 完成剩下的所有关卡，那么 Alice 获得 1 - 1 + 1 = 1 分，Bob 获得 -1 分。

Alice 需要完成至少一个关卡获得更多的分数。

示例 2：

```text
输入：possible = [1,1,1,1,1]

输出：3

解释：

我们来看一下 Alice 可以完成的关卡数目：
```

- 如果 Alice 只完成关卡 0 ，Bob 完成剩下的所有关卡，那么 Alice 获得 1 分，Bob 获得 4 分。
- 如果 Alice 完成到关卡 1 ，Bob 完成剩下的所有关卡，那么 Alice 获得 2 分，Bob 获得 3 分。
- 如果 Alice 完成到关卡 2 ，Bob 完成剩下的所有关卡，那么 Alice 获得 3 分，Bob 获得 2 分。
- 如果 Alice 完成到关卡 3 ，Bob 完成剩下的所有关卡，那么 Alice 获得 4 分，Bob 获得 1 分。

Alice 需要完成至少三个关卡获得更多的分数。

示例 3：

```text
输入：possible = [0,0]

输出：-1

解释：

两名玩家只能各完成 1 个关卡，Alice 完成关卡 0 得到 -1 分，Bob 完成关卡 1 得到 -1 分。两名玩家得分相同，所以 Alice 无法得到更多分数。
```

__提示：__

- `2 <= n == possible.length <= 10 ^ 5`
- `possible[i]` 要么是 `0` 要么是 `1` 。

__思路:__

```text
前缀和
找到前缀和比后缀和大的点就返回
没找到的情况下返回 -1
pre > sum - pre
即 pre * 2 > sum
所以可以将 1 视为 2, 0 视为 -2 加到前缀和
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumLevels(vector<int>& possible) 
    {
        int n = possible.size(), pre = 0, s = (accumulate(possible.begin(), possible.end(), 0) << 1) - n;
        for (int i = 0; i < n - 1; i++) if ((pre += possible[i] > 0 ? 2 : -2) > s) return i + 1;
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumLevels(int[] possible) {
        int n = possible.length, pre = 0, s = (Arrays.stream(possible).sum() << 1) - n;
        for (int i = 0; i < n - 1; i++) if ((pre += possible[i] > 0 ? 2 : -2) > s) return i + 1;
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumLevels(self, possible: List[int]) -> int:
        alice, s, n = 1 if possible[0] else -1, sum([1 if i else -1 for i in possible[1:]]), len(possible)
        for i in range(1, n):
            if alice > s:
                return i
            alice += 1 if possible[i] else -1
            s -= 1 if possible[i] else -1
        return -1
```
