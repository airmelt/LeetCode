# 2410 Maximum Matching of Players With Trainers 运动员和训练师的最大匹配数

__Description:__

You are given a __0-indexed__ integer array `players`, where `players[i]` represents the __ability__ of the `i ^ th` player. You are also given a __0-indexed__ integer array `trainers`, where `trainers[j]` represents the __training capacity__ of the `j ^ th` trainer.

The `i ^ th` player can __match__ with the `j ^ th` trainer if the player's ability is __less than or equal to__ the trainer's training capacity. Additionally, the `i ^ th` player can be matched with at most one trainer, and the `j ^ th` trainer can be matched with at most one player.

Return _the __maximum__ number of matchings between_ `players` _and_ `trainers` _that satisfy these conditions._

__Example:__

Example 1:

```text
Input: players = [4,7,9], trainers = [8,2,5,8]
Output: 2
Explanation:
One of the ways we can form two matchings is as follows:
```

- players[0] can be matched with trainers[0] since 4 <= 8.
- players[1] can be matched with trainers[3] since 7 <= 8.

It can be proven that 2 is the maximum number of matchings that can be formed.

Example 2:

```text
Input: players = [1,1,1], trainers = [10]
Output: 1
Explanation:
The trainer can be matched with any of the 3 players.
Each player can only be matched with one trainer, so the maximum answer is 1.
```

__Constraints:__

- `1 <= players.length, trainers.length <= 10 ^ 5`
- `1 <= players[i], trainers[j] <= 10 ^ 9`

Note: This question is the same as 445: Assign Cookies.

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `players` ，其中 `players[i]` 表示第 `i` 名运动员的 __能力__ 值，同时给你一个下标从 __0__ 开始的整数数组 `trainers` ，其中 `trainers[j]` 表示第 `j` 名训练师的 __训练能力值__ 。

如果第 `i` 名运动员的能力值 __小于等于__ 第 `j` 名训练师的能力值，那么第 `i` 名运动员可以 __匹配__ 第 `j` 名训练师。除此以外，每名运动员至多可以匹配一位训练师，每位训练师最多可以匹配一位运动员。

请你返回满足上述要求 `players` 和 `trainers` 的 __最大__ 匹配数。

__示例:__

示例 1：

```text
输入：players = [4,7,9], trainers = [8,2,5,8]
输出：2
解释：
得到两个匹配的一种方案是：
```

- players[0] 与 trainers[0] 匹配，因为 4 <= 8 。
- players[1] 与 trainers[3] 匹配，因为 7 <= 8 。

可以证明 2 是可以形成的最大匹配数。

示例 2：

```text
输入：players = [1,1,1], trainers = [10]
输出：1
解释：
训练师可以匹配所有 3 个运动员
每个运动员至多只能匹配一个训练师，所以最大答案是 1 。
```

__提示：__

- `1 <= players.length, trainers.length <= 10 ^ 5`
- `1 <= players[i], trainers[j] <= 10 ^ 9`

__思路:__

```text
排序
将运动员和训练师按照能力值进行排序
遍历训练师
如果当前训练师的能力值大于等于当前运动员的能力值
则匹配成功
运动员指针后移
返回运动员指针的位置即可
时间复杂度为 O(NlogN + MlogM), 空间复杂度为 O(logN + logM)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int matchPlayersAndTrainers(vector<int>& players, vector<int>& trainers) 
    {
        sort(players.begin(), players.end());
        sort(trainers.begin(), trainers.end());
        int i = 0, n = players.size();
        for (const auto& t : trainers) if (i < n and players[i] <= t) ++i;
        return i;
    }
};
```

__Java__:

```Java
class Solution {
    public int matchPlayersAndTrainers(int[] players, int[] trainers) {
        Arrays.sort(players);
        Arrays.sort(trainers);
        int i = 0, n = players.length;
        for (int t : trainers) if (i < n && players[i] <= t) ++i;
        return i;
    }
}
```

__Python__:

```Python
class Solution:
    def matchPlayersAndTrainers(self, players: List[int], trainers: List[int]) -> int:
        players.sort()
        trainers.sort()
        i, n = 0, len(players)
        for t in trainers:
            if i < n and players[i] <= t:
                i += 1
        return i
```
