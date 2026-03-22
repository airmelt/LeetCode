# 1921 Eliminate Maximum Number of Monsters 消灭怪物的最大数量

__Description:__

You are playing a video game where you are defending your city from a group of `n` monsters. You are given a __0-indexed__ integer array `dist` of size `n`, where `dist[i]` is the __initial distance__ in kilometers of the `i ^ th` monster from the city.

The monsters walk toward the city at a __constant__ speed. The speed of each monster is given to you in an integer array `speed` of size `n`, where `speed[i]` is the speed of the `i ^ th` monster in kilometers per minute.

You have a weapon that, once fully charged, can eliminate a single monster. However, the weapon takes one minute to charge. The weapon is fully charged at the very start.

You lose when any monster reaches your city. If a monster reaches the city at the exact moment the weapon is fully charged, it counts as a loss, and the game ends before you can use your weapon.

Return _the __maximum__ number of monsters that you can eliminate before you lose, or_ `n` _if you can eliminate all the monsters before they reach the city._

__Example:__

Example 1:

```text
Input: dist = [1,3,4], speed = [1,1,1]
Output: 3
Explanation:
In the beginning, the distances of the monsters are [1,3,4]. You eliminate the first monster.
After a minute, the distances of the monsters are [X,2,3]. You eliminate the second monster.
After a minute, the distances of the monsters are [X,X,2]. You eliminate the third monster.
All 3 monsters can be eliminated.
```

Example 2:

```text
Input: dist = [1,1,2,3], speed = [1,1,1,1]
Output: 1
Explanation:
In the beginning, the distances of the monsters are [1,1,2,3]. You eliminate the first monster.
After a minute, the distances of the monsters are [X,0,1,2], so you lose.
You can only eliminate 1 monster.
```

Example 3:

```text
Input: dist = [3,2,4], speed = [5,3,2]
Output: 1
Explanation:
In the beginning, the distances of the monsters are [3,2,4]. You eliminate the first monster.
After a minute, the distances of the monsters are [X,0,2], so you lose.
You can only eliminate 1 monster.
```

__Constraints:__

- `n == dist.length == speed.length`
- `1 <= n <= 10 ^ 5`
- `1 <= dist[i], speed[i] <= 10 ^ 5`

__题目描述:__

你正在玩一款电子游戏，在游戏中你需要保护城市免受怪物侵袭。给定一个 __下标从 0 开始__ 且大小为 `n` 的整数数组 `dist` ，其中 `dist[i]` 是第 `i` 个怪物与城市的 __初始距离__（单位:米）。

怪物以 __恒定__ 的速度走向城市。每个怪物的速度都以一个长度为 `n` 的整数数组 `speed` 表示，其中 `speed[i]` 是第 `i` 个怪物的速度（单位:千米/分）。

你有一种武器，一旦充满电，就可以消灭 一个 怪物。但是，武器需要 一分钟 才能充电。武器在游戏开始时是充满电的状态，怪物从 第 0 分钟 时开始移动。

一旦任一怪物到达城市，你就输掉了这场游戏。如果某个怪物 恰好 在某一分钟开始时到达城市（距离表示为0），这也会被视为 输掉 游戏，在你可以使用武器之前，游戏就会结束。

返回在你输掉游戏前可以消灭的怪物的 __最大__ 数量。如果你可以在所有怪物到达城市前将它们全部消灭，返回  `n` 。

__示例:__

示例 1：

```text
输入：dist = [1,3,4], speed = [1,1,1]
输出：3
解释：
第 0 分钟开始时，怪物的距离是 [1,3,4]，你消灭了第一个怪物。
第 1 分钟开始时，怪物的距离是 [X,2,3]，你消灭了第二个怪物。
第 3 分钟开始时，怪物的距离是 [X,X,2]，你消灭了第三个怪物。
所有 3 个怪物都可以被消灭。
```

示例 2：

```text
输入：dist = [1,1,2,3], speed = [1,1,1,1]
输出：1
解释：
第 0 分钟开始时，怪物的距离是 [1,1,2,3]，你消灭了第一个怪物。
第 1 分钟开始时，怪物的距离是 [X,0,1,2]，所以你输掉了游戏。
你只能消灭 1 个怪物。
```

示例 3：

```text
输入：dist = [3,2,4], speed = [5,3,2]
输出：1
解释：
第 0 分钟开始时，怪物的距离是 [3,2,4]，你消灭了第一个怪物。
第 1 分钟开始时，怪物的距离是 [X,0,2]，你输掉了游戏。 
你只能消灭 1 个怪物。
```

__提示：__

- `n == dist.length == speed.length`
- `1 <= n <= 10 ^ 5`
- `1 <= dist[i], speed[i] <= 10 ^ 5`

__思路:__

```text
贪心
题意为 0, 1, 2, ..., n 秒分别且只能进行 1 次攻击, 求出最多能防御住的怪物的数量, 类似塔防游戏攻击频率一定时防御的波数
先求出每个怪物到达城市的时间
将时间排序
找到第一个下标比时间小的即可
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int eliminateMaximum(vector<int>& dist, vector<int>& speed) 
    {
        int n = dist.size();
        vector<int> array(n);
        for (int i = 0; i < n; i++) array[i] = (dist[i] - 1) / speed[i];
        sort(array.begin(), array.end());
        for (int i = 0; i < n; i++) if (array[i] < i) return i;
        return n;
    }
};
```

__Java__:

```Java
class Solution {
    public int eliminateMaximum(int[] dist, int[] speed) {
        int n = dist.length, array[] = new int[n];
        for (int i = 0; i < n; i++) array[i] = (dist[i] - 1) / speed[i];
        Arrays.sort(array);
        for (int i = 0; i < n; i++) if (array[i] < i) return i;
        return n;
    }
}
```

__Python__:

```Python
class Solution:
    def eliminateMaximum(self, dist: List[int], speed: List[int]) -> int:
        return len(dist) if all(k < 0 for k in [j - t for j, t in enumerate(sorted([dist[i] / speed[i] for i in range(len(dist))]))]) else [k for k, v in enumerate([j - t for j, t in enumerate(sorted([dist[i] / speed[i] for i in range(len(dist))]))]) if v >= 0][0]
```
