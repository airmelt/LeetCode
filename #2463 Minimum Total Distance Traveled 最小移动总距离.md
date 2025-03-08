# 2463 Minimum Total Distance Traveled 最小移动总距离

__Description:__

There are some robots and factories on the X-axis. You are given an integer array `robot` where `robot[i]` is the position of the `i ^ th` robot. You are also given a 2D integer array `factory` where `factory[j] = [position_j, limit_j]` indicates that `position_j` is the position of the `j ^ th` factory and that the `j ^ th` factory can repair at most `limit_j` robots.

The positions of each robot are unique. The positions of each factory are also unique. Note that a robot can be in the same position as a factory initially.

All the robots are initially broken; they keep moving in one direction. The direction could be the negative or the positive direction of the X-axis. When a robot reaches a factory that did not reach its limit, the factory repairs the robot, and it stops moving.

At any moment, you can set the initial direction of moving for some robot. Your target is to minimize the total distance traveled by all the robots.

Return the minimum total distance traveled by all the robots. The test cases are generated such that all the robots can be repaired.

Note that

- All robots move at the same speed.
- If two robots move in the same direction, they will never collide.
- If two robots move in opposite directions and they meet at some point, they do not collide. They cross each other.
- If a robot passes by a factory that reached its limits, it crosses it as if it does not exist.
- If the robot moved from a position `x` to a position `y`, the distance it moved is `|y - x|`.

__Example:__

Example 1:

![2463-1](https://assets.leetcode.com/uploads/2022/09/15/example1.jpg)

```text
Input: robot = [0,4,6], factory = [[2,2],[6,2]]
Output: 4
Explanation: As shown in the figure:
```

- The first robot at position 0 moves in the positive direction. It will be repaired at the first factory.
- The second robot at position 4 moves in the negative direction. It will be repaired at the first factory.
- The third robot at position 6 will be repaired at the second factory. It does not need to move.

The limit of the first factory is 2, and it fixed 2 robots.
The limit of the second factory is 2, and it fixed 1 robot.
The total distance is |2 - 0| + |2 - 4| + |6 - 6| = 4. It can be shown that we cannot achieve a better total distance than 4.

Example 2:

![2463-2](https://assets.leetcode.com/uploads/2022/09/15/example-2.jpg)

```text
Input: robot = [1,-1], factory = [[-2,1],[2,1]]
Output: 2
Explanation: As shown in the figure:
```

- The first robot at position 1 moves in the positive direction. It will be repaired at the second factory.
- The second robot at position -1 moves in the negative direction. It will be repaired at the first factory.

The limit of the first factory is 1, and it fixed 1 robot.
The limit of the second factory is 1, and it fixed 1 robot.
The total distance is |2 - 1| + |(-2) - (-1)| = 2. It can be shown that we cannot achieve a better total distance than 2.

__Constraints:__

- `1 <= robot.length, factory.length <= 100`
- `factory[j].length == 2`
- `-10 ^ 9 <= robot[i], position_j <= 10 ^ 9`
- `0 <= limit_j <= robot.length`
- The input will be generated such that it is always possible to repair every robot.

__题目描述:__

X 轴上有一些机器人和工厂。给你一个整数数组 `robot` ，其中 `robot[i]` 是第 `i` 个机器人的位置。再给你一个二维整数数组 `factory` ，其中 `factory[j] = [position_j, limit_j]` ，表示第 `j` 个工厂的位置在 `position_j` ，且第 `j` 个工厂最多可以修理 `limit_j` 个机器人。

每个机器人所在的位置 互不相同 。每个工厂所在的位置也 互不相同 。注意一个机器人可能一开始跟一个工厂在 相同的位置 。

所有机器人一开始都是坏的，他们会沿着设定的方向一直移动。设定的方向要么是 X 轴的正方向，要么是 X 轴的负方向。当一个机器人经过一个没达到上限的工厂时，这个工厂会维修这个机器人，且机器人停止移动。

任何时刻，你都可以设置 部分 机器人的移动方向。你的目标是最小化所有机器人总的移动距离。

请你返回所有机器人移动的最小总距离。测试数据保证所有机器人都可以被维修。

注意：

- 所有机器人移动速度相同。
- 如果两个机器人移动方向相同，它们永远不会碰撞。
- 如果两个机器人迎面相遇，它们也不会碰撞，它们彼此之间会擦肩而过。
- 如果一个机器人经过了一个已经达到上限的工厂，机器人会当作工厂不存在，继续移动。
- 机器人从位置 `x` 到位置 `y` 的移动距离为 `|y - x|` 。

__示例:__

示例 1：

![2463-3](https://pic.leetcode-cn.com/1667542978-utuiPv-image.png)

```text
输入：robot = [0,4,6], factory = [[2,2],[6,2]]
输出：4
解释：如上图所示：
```

- 第一个机器人从位置 0 沿着正方向移动，在第一个工厂处维修。
- 第二个机器人从位置 4 沿着负方向移动，在第一个工厂处维修。
- 第三个机器人在位置 6 被第二个工厂维修，它不需要移动。

第一个工厂的维修上限是 2 ，它维修了 2 个机器人。
第二个工厂的维修上限是 2 ，它维修了 1 个机器人。
总移动距离是 |2 - 0| + |2 - 4| + |6 - 6| = 4 。没有办法得到比 4 更少的总移动距离。

示例 2：

![2463-4](https://pic.leetcode-cn.com/1667542984-OAIRFN-image.png)

```text
输入：robot = [1,-1], factory = [[-2,1],[2,1]]
输出：2
解释：如上图所示：
```

- 第一个机器人从位置 1 沿着正方向移动，在第二个工厂处维修。
- 第二个机器人在位置 -1 沿着负方向移动，在第一个工厂处维修。

第一个工厂的维修上限是 1 ，它维修了 1 个机器人。
第二个工厂的维修上限是 1 ，它维修了 1 个机器人。
总移动距离是 |2 - 1| + |(-2) - (-1)| = 2 。没有办法得到比 2 更少的总移动距离。

__提示：__

- `1 <= robot.length, factory.length <= 100`
- `factory[j].length == 2`
- `-10 ^ 9 <= robot[i], position_j <= 10 ^ 9`
- `0 <= limit_j <= robot.length`
- 测试数据保证所有机器人都可以被维修。

__思路:__

```text
动态规划
将 robot 和工厂按照位置排序
设 dp[i][j] 表示前 i 个工厂维修 j 个机器人的最小移动距离
dp[i][j] = min(dp[i][j], dp[i - 1][j - k] + cost)
其中 k 表示第 i 个工厂维修的机器人的数量
k 的范围为 1 到 limit
limit 表示第 i 个工厂的维修上限
cost 表示第 i 个工厂维修 k 个机器人的移动距离
cost = sum(|robot[j - k] - pos|)
其中 pos 表示第 i 个工厂的位置
即 factory[i] = (pos, limit)
由于 dp[i][j] 只与 dp[i - 1][j - k] 有关
所以可以使用滚动数组来优化空间复杂度
时间复杂度为 O(NM ^ 2), 空间复杂度为 O(M), 其中 N 表示工厂的数量, M 表示机器人的数量
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long minimumTotalDistance(vector<int>& robot, vector<vector<int>>& factory) 
    {
        int m = robot.size(), j = 0, k = 0, p = 0, pos = 0, limit = 0;
        long long cost = 0LL;
        sort(robot.begin(), robot.end());
        sort(factory.begin(), factory.end());
        vector<long long> dp(m + 1, LLONG_MAX >> 1);
        dp.front() = 0LL;
        for (const auto& fa : factory) for (j = m, pos = fa.front(), limit = fa.back(); j > 0; j--) for (cost = 0LL, k = 1, p = min(j, limit); k <= p; k++) dp[j] = min(dp[j], dp[j - k] + (cost += abs(robot[j - k] - pos)));
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public long minimumTotalDistance(List<Integer> robot, int[][] factory) {
        int m = robot.size(), pos = 0, limit = 0, j = 0, k = 0, p = 0;
        Collections.sort(robot);
        Arrays.sort(factory, (a, b) -> a[0] - b[0]);
        long dp[] = new long[m + 1], cost = 0L;
        Arrays.fill(dp, Long.MAX_VALUE >> 1);
        dp[0] = 0;
        for (int[] fa : factory) for (pos = fa[0], limit = fa[1], j = m; j > 0; j--) for (cost = 0L, k = 1, p = Math.min(j, limit); k <= p; k++) dp[j] = Math.min(dp[j], dp[j - k] + (cost += Math.abs(robot.get(j - k) - pos)));
        return dp[m];
    }
}
```

__Python__:

```Python
class Solution:
    def minimumTotalDistance(self, robot: List[int], factory: List[List[int]]) -> int:
        factory.sort()
        robot.sort()
        dp = [0] + [inf] * (m := len(robot))
        for pos, limit in factory:
            for j in range(m, 0, -1):
                cost = 0
                for k in range(1, min(j, limit) + 1):
                    dp[j] = min(dp[j], dp[j - k] + (cost := cost + abs(robot[j - k] - pos)))
        return dp[m]
```
