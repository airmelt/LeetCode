# 2532 Time to Cross a Bridge 过桥的时间

__Description:__

There are `k` workers who want to move `n` boxes from the right (old) warehouse to the left (new) warehouse. You are given the two integers `n` and `k`, and a 2D integer array `time` of size `k x 4` where `time[i] = [right_i, pick_i, left_i, put_i]`.

The warehouses are separated by a river and connected by a bridge. Initially, all `k` workers are waiting on the left side of the bridge. To move the boxes, the `i ^ th` worker can do the following:

- Cross the bridge to the right side in `right_i` minutes.
- Pick a box from the right warehouse in `pick_i` minutes.
- Cross the bridge to the left side in `left_i` minutes.
- Put the box into the left warehouse in `put_i` minutes.

The `i ^ th` worker is __less efficient__ than the j `^ th` worker if either condition is met:

- `left_i + right_i > left_j + right_j`
- `left_i + right_i == left_j + right_j` and `i > j`

The following rules regulate the movement of the workers through the bridge:

- Only one worker can use the bridge at a time.
- When the bridge is unused prioritize the __least efficient__ worker (who have picked up the box) on the right side to cross. If not, prioritize the __least efficient__ worker on the left side to cross.
- If enough workers have already been dispatched from the left side to pick up all the remaining boxes, __no more__ workers will be sent from the left side.

Return the elapsed minutes at which the last box reaches the left side of the bridge.

__Example:__

Example 1:

```text
Input: n = 1, k = 3, time = [[1,1,2,1],[1,1,3,1],[1,1,4,1]]
```

Output: 6

Explanation:

From 0 to 1 minutes: worker 2 crosses the bridge to the right.
From 1 to 2 minutes: worker 2 picks up a box from the right warehouse.
From 2 to 6 minutes: worker 2 crosses the bridge to the left.
From 6 to 7 minutes: worker 2 puts a box at the left warehouse.
The whole process ends after 7 minutes. We return 6 because the problem asks for the instance of time at which the last worker reaches the left side of the bridge.

Example 2:

```text
Input: n = 3, k = 2, time = [[1,5,1,8],[10,10,10,10]]
```

Output: 37

Explanation:

![2532-1](https://assets.leetcode.com/uploads/2024/11/21/378539249-c6ce3c73-40e7-4670-a8b5-7ddb9abede11.png)

The last box reaches the left side at 37 seconds. Notice, how we do not put the last boxes down, as that would take more time, and they are already on the left with the workers.

__Constraints:__

- `1 <= n, k <= 10 ^ 4`
- `time.length == k`
- `time[i].length == 4`
- `1 <= left_i, pick_i, right_i, put_i <= 1000`

__题目描述:__

共有 `k` 位工人计划将 `n` 个箱子从右侧的（旧）仓库移动到左侧的（新）仓库。给你两个整数 `n` 和 `k`，以及一个二维整数数组 `time` ，数组的大小为 `k x 4` ，其中 `time[i] = [right_i, pick_i, left_i, put_i]` 。

一条河将两座仓库分隔，只能通过一座桥通行。旧仓库位于河的右岸，新仓库在河的左岸。开始时，所有 `k` 位工人都在桥的左侧等待。为了移动这些箱子，第 `i` 位工人（下标从 __0__ 开始）可以:

- 从左岸（新仓库）跨过桥到右岸（旧仓库），用时 `right_i` 分钟。
- 从旧仓库选择一个箱子，并返回到桥边，用时 `pick_i` 分钟。不同工人可以同时搬起所选的箱子。
- 从右岸（旧仓库）跨过桥到左岸（新仓库），用时 `left_i` 分钟。
- 将箱子放入新仓库，并返回到桥边，用时 `put_i` 分钟。不同工人可以同时放下所选的箱子。

如果满足下面任一条件，则认为工人 `i` 的 __效率低于__ 工人 `j` :

- `left_i + right_i > left_j + right_j`
- `left_i + right_i == left_j + right_j` 且 `i > j`

工人通过桥时需要遵循以下规则：

- 同时只能有一名工人过桥。
- 当桥梁未被使用时，优先让右侧 __效率最低__ 的工人（已经拿起盒子的工人）过桥。如果不是，优先让左侧 __效率最低__ 的工人通过。
- 如果左侧已经派出足够的工人来拾取所有剩余的箱子，则 __不会__ 再从左侧派出工人。

请你返回最后一个箱子 到达桥左侧 的时间。

__示例:__

示例 1：

```text
输入：n = 1, k = 3, time = [[1,1,2,1],[1,1,3,1],[1,1,4,1]]
```

输出：6

解释：

从 0 到 1 分钟：工人 2 通过桥到达右侧。
从 1 到 2 分钟：工人 2 从右侧仓库拿起箱子。
从 2 到 6 分钟：工人 2 通过桥到达左侧。
从 6 到 7 分钟：工人 2 向左侧仓库放下箱子。
整个过程在 7 分钟后结束。我们返回 6 因为该问题要求的是最后一名工人到达桥梁左侧的时间。

示例 2：

```text
输入：n = 3, k = 2, time = [[1,5,1,8],[10,10,10,10]]
```

输出：37

解释：

![2532-2](https://assets.leetcode.com/uploads/2024/11/21/378539249-c6ce3c73-40e7-4670-a8b5-7ddb9abede11.png)

最后一个盒子在37秒时到达左侧。请注意，我们并 没有 放下最后一个箱子，因为那样会花费更多时间，而且它们已经和工人们一起在左边。

__提示：__

- `1 <= n, k <= 10 ^ 4`
- `time.length == k`
- `time[i].length == 4`
- `1 <= left_i, pick_i, right_i, put_i <= 1000`

__思路:__

```text
优先队列
工人一共有 4 种不同的状态, 分别是
在左边等待过桥: left_bridge
在右边等待过桥: right_bridge
在左边工作: left_work
在右边工作: right_work
过桥的优先级判断是先计算 left_i + right_i, 然后再判断 i 的大小, 只需要记录下标即可
工作的优先级判断是计算工作的结束时间, 需要记录结束时间和下标
记录需要 need 和 done, 需要的搬运的箱子数量和已经完成的箱子数量
先将所有工人放入 left_bridge 中, 然后开始循环
循环结束的条件是 need == 0, 即右边的箱子都搬完了
如果 right_bridge 不为空, 说明右边有工人可以过桥, 先将 right_bridge 中的工人过桥, 将当前时间加上他过桥的时间 time[i][2](left_i), 再记录他在左边工作完成的时间 cur_time + time[i][3](put_i), 将他放入 left_work 中, 需要的箱子数量减一
否则检查是否可以从 left_bridge 中取出工人过桥, 如果可以, 将 left_bridge 中的工人过桥, 将当前时间加上他过桥的时间 time[i][0](right_i), 再记录他在右边工作完成的时间 cur_time + time[i][1](pick_i), 将他放入 right_work 中, 已经完成的箱子数量加一, 需要检查箱子是否已经完成, 如果并不需要搬箱子, 不进入
否则当前时间跳到 left_work 和 right_work 中的最小值, 由于最后并不需要 left_work 完成工作, 所以当 done == n 时, left_work 中的工人可以不工作, 直接跳过
更新完当前时间之后
把 left_work 中所有小于等于当前时间的工人放入 right_bridge 中
把 right_work 中所有小于等于当前时间的工人放入 left_bridge 中
时间复杂度为 O(NlogK), 空间复杂度为 O(K)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findCrossingTime(int n, int k, vector<vector<int>>& time) 
    {
        auto wait_priority_cmp = [&](int x, int y) { return time[x][0] + time[x][2] != time[y][0] + time[y][2] ? time[x][0] + time[x][2] < time[y][0] + time[y][2] : x < y; };
        priority_queue<int, vector<int>, decltype(wait_priority_cmp)> left_bridge(wait_priority_cmp), right_bridge(wait_priority_cmp);
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> left_work, right_work;
        int need = n, done = 0, cur_time = 0, i = 0, INF = 0x3f3f3f3f;
        for (i = 0; i < k; i++) left_bridge.push(i);
        while (need) 
        {
            if (!right_bridge.empty()) 
            {
                cur_time += time[i = right_bridge.top()][2];
                right_bridge.pop();
                left_work.push({cur_time + time[i][3], i});
                --need;
            } 
            else if (!left_bridge.empty() and done < n) 
            {
                cur_time += time[i = left_bridge.top()][0];
                left_bridge.pop();
                right_work.push({cur_time + time[i][1], i});
                ++done;
            } 
            else cur_time = min(!left_work.empty() and done < n ? left_work.top().first : INF, !right_work.empty() ? right_work.top().first : INF);
            while (!right_work.empty() and right_work.top().first <= cur_time) 
            {
                right_bridge.push(right_work.top().second);
                right_work.pop();
            }
            while (!left_work.empty() and left_work.top().first <= cur_time) 
            {
                left_bridge.push(left_work.top().second);
                left_work.pop();
            }
        }
        return cur_time;
    }
};
```

__Java__:

```Java
class Solution {
    public int findCrossingTime(int n, int k, int[][] time) {
        PriorityQueue<Integer> leftBridge = new PriorityQueue<Integer>((x, y) -> { return time[x][0] + time[x][2] == time[y][0] + time[y][2] ? y - x : time[y][0] + time[y][2] - time[x][0] - time[x][2]; }), rightBridge = new PriorityQueue<Integer>((x, y) -> { return time[x][0] + time[x][2] == time[y][0] + time[y][2] ? y - x : time[y][0] + time[y][2] - time[x][0] - time[x][2]; });
        PriorityQueue<int[]> leftWork = new PriorityQueue<int[]>((x, y) -> { return x[0] == y[0] ? x[1] - y[1] : x[0] - y[0]; }), rightWork = new PriorityQueue<int[]>((x, y) -> { return x[0] == y[0] ? x[1] - y[1] : x[0] - y[0]; });
        int need = n, done = 0, curTime = 0, i = 0, INF = 0x3f3f3f3f;
        for (i = 0; i < k; i++) leftBridge.offer(i);
        while (need > 0) {
            if (!rightBridge.isEmpty()) {
                curTime += time[i = rightBridge.poll()][2];
                leftWork.offer(new int[]{curTime + time[i][3], i});
                --need;
            } else if (!leftBridge.isEmpty() && done < n) {
                curTime += time[i = leftBridge.poll()][0];
                rightWork.offer(new int[]{curTime + time[i][1], i});
                ++done;
            } else curTime = Math.min(!leftWork.isEmpty() && done < n ? leftWork.peek()[0] : INF, !rightWork.isEmpty() ? rightWork.peek()[0] : INF);
            while (!rightWork.isEmpty() && rightWork.peek()[0] <= curTime) rightBridge.offer(rightWork.poll()[1]);
            while (!leftWork.isEmpty() && leftWork.peek()[0] <= curTime) leftBridge.offer(leftWork.poll()[1]);
        }
        return curTime;
    }
}
```

__Python__:

```Python
class Solution:
    def findCrossingTime(self, n: int, k: int, time: List[List[int]]) -> int:
        time, left_bridge, right_bridge, left_work, right_work, cur_time, done, need = sorted([[i, right, pick, left, put] for i, (right, pick, left, put) in enumerate(time)], key=lambda x: (-x[1] - x[3], -x[0])), list(range(k)), [], [], [], 0, 0, n
        while need:
            if right_bridge:
                cur_time += time[i := heappop(right_bridge)][3]
                heappush(left_work, (cur_time + time[i][4], i))
                need -= 1
            elif left_bridge and done < n:
                cur_time += time[i := heappop(left_bridge)][1]
                heappush(right_work, (cur_time + time[i][2], i))
                done += 1
            else:
                cur_time = min(left_work[0][0] if left_work and done < n else inf, right_work[0][0] if right_work else inf)
            while right_work and right_work[0][0] <= cur_time:
                heappush(right_bridge, heappop(right_work)[-1])
            while left_work and left_work[0][0] <= cur_time:
                heappush(left_bridge, heappop(left_work)[-1])
        return cur_time
```
