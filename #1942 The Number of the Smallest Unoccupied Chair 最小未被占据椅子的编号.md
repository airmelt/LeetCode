# 1942 The Number of the Smallest Unoccupied Chair 最小未被占据椅子的编号

__Description:__

There is a party where `n` friends numbered from `0` to `n - 1` are attending. There is an __infinite__ number of chairs in this party that are numbered from `0` to `infinity`. When a friend arrives at the party, they sit on the unoccupied chair with the __smallest number__.

- For example, if chairs `0`, `1`, and `5` are occupied when a friend comes, they will sit on chair number `2`.

When a friend leaves the party, their chair becomes unoccupied at the moment they leave. If another friend arrives at that same moment, they can sit in that chair.

You are given a __0-indexed__ 2D integer array `times` where `times[i] = [arrivali, leavingi]`, indicating the arrival and leaving times of the `i ^ th` friend respectively, and an integer `targetFriend`. All arrival times are __distinct__.

Return _the __chair number__ that the friend numbered_ `targetFriend` _will sit on_.

__Example:__

Example 1:

```text
Input: times = [[1,4],[2,3],[4,6]], targetFriend = 1
Output: 1
Explanation: 
```

- Friend 0 arrives at time 1 and sits on chair 0.
- Friend 1 arrives at time 2 and sits on chair 1.
- Friend 1 leaves at time 3 and chair 1 becomes empty.
- Friend 0 leaves at time 4 and chair 0 becomes empty.
- Friend 2 arrives at time 4 and sits on chair 0.

Since friend 1 sat on chair 1, we return 1.

Example 2:

```text
Input: times = [[3,10],[1,5],[2,6]], targetFriend = 0
Output: 2
Explanation: 
```

- Friend 1 arrives at time 1 and sits on chair 0.
- Friend 2 arrives at time 2 and sits on chair 1.
- Friend 0 arrives at time 3 and sits on chair 2.
- Friend 1 leaves at time 5 and chair 0 becomes empty.
- Friend 2 leaves at time 6 and chair 1 becomes empty.
- Friend 0 leaves at time 10 and chair 2 becomes empty.

Since friend 0 sat on chair 2, we return 2.

__Constraints:__

- `n == times.length`
- `2 <= n <= 10 ^ 4`
- `times[i].length == 2`
- `1 <= arrivali < leavingi <= 10 ^ 5`
- `0 <= targetFriend <= n - 1`
- Each `arrivali` time is __distinct__.

__题目描述:__

有 `n` 个朋友在举办一个派对，这些朋友从 `0` 到 `n - 1` 编号。派对里有 __无数__ 张椅子，编号为 `0` 到 `infinity` 。当一个朋友到达派对时，他会占据 __编号最小__ 且未被占据的椅子。

- 比方说，当一个朋友到达时，如果椅子 `0` ， `1` 和 `5` 被占据了，那么他会占据 `2` 号椅子。

当一个朋友离开派对时，他的椅子会立刻变成未占据状态。如果同一时刻有另一个朋友到达，可以立即占据这张椅子。

给你一个下标从 __0__ 开始的二维整数数组 `times` ，其中 `times[i] = [arrivali, leavingi]` 表示第 `i` 个朋友到达和离开的时刻，同时给你一个整数 `targetFriend` 。所有到达时间 __互不相同__ 。

请你返回编号为 `targetFriend` 的朋友占据的 __椅子编号__ 。

__示例:__

示例 1：

```text
输入：times = [[1,4],[2,3],[4,6]], targetFriend = 1
输出：1
解释：
```

- 朋友 0 时刻 1 到达，占据椅子 0 。
- 朋友 1 时刻 2 到达，占据椅子 1 。
- 朋友 1 时刻 3 离开，椅子 1 变成未占据。
- 朋友 0 时刻 4 离开，椅子 0 变成未占据。
- 朋友 2 时刻 4 到达，占据椅子 0 。

朋友 1 占据椅子 1 ，所以返回 1 。

示例 2：

```text
输入：times = [[3,10],[1,5],[2,6]], targetFriend = 0
输出：2
解释：
```

- 朋友 1 时刻 1 到达，占据椅子 0 。
- 朋友 2 时刻 2 到达，占据椅子 1 。
- 朋友 0 时刻 3 到达，占据椅子 2 。
- 朋友 1 时刻 5 离开，椅子 0 变成未占据。
- 朋友 2 时刻 6 离开，椅子 1 变成未占据。
- 朋友 0 时刻 10 离开，椅子 2 变成未占据。

朋友 0 占据椅子 2 ，所以返回 2 。

__提示：__

- `n == times.length`
- `2 <= n <= 10 ^ 4`
- `times[i].length == 2`
- `1 <= arrivali < leavingi <= 10 ^ 5`
- `0 <= targetFriend <= n - 1`
- 每个 `arrivali` 时刻 __互不相同__ 。

__思路:__

```text
优先队列
用一个哈希表记录人对应的椅子
将可用的椅子放入优先队列中, 由于最多只有 n 个人, 所以最多只需要 n 个椅子
将到达时间和离开时间和人分别放入一个数组中并升序排序
遍历到达时间数组, 将不早于到达时间的椅子放回优先队列中
给新到达的人安排最优先的椅子
如果来的人就是目标直接返回椅子
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution {
public:
    int smallestChair(vector<vector<int>>& times, int targetFriend) {
        int n = times.size(), i = 0, seat = 0;
        vector<pair<int, int>> arrival, leaving;
        for (int i = 0; i < n; i++)
        {
            arrival.emplace_back(times[i][0], i);
            leaving.emplace_back(times[i][1], i);
        }
        sort(arrival.begin(), arrival.end());
        sort(leaving.begin(), leaving.end());
        priority_queue<int, vector<int>, greater<int>> available;
        for (int i = 0; i < n; i++) available.push(i);
        unordered_map<int, int> seats;
        for (auto&& [time, person] : arrival)
        {
            while (i < n and leaving[i].first <= time) available.push(seats[leaving[i++].second]);
            seats[person] = seat = available.top();
            available.pop();
            if (person == targetFriend) return seat;
        }
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int smallestChair(int[][] times, int targetFriend) {
        int target[] = times[targetFriend], n = times.length, seats[] = new int[n];
        Arrays.sort(times, (o1, o2) -> o1[0] - o2[0]);
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (seats[j] <= times[i][0]) {
                    seats[j] = times[i][1];
                    if (times[i][0] == target[0] && times[i][1] == target[1]) return j;
                    break;
                }
            }
        }
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def smallestChair(self, times: List[List[int]], targetFriend: int) -> int:
        available, cur, arrival, leaving, seats, i = list(range(n := len(times))), 0, sorted([[s, i] for i, (s, _) in enumerate(times)]), sorted([[e, i] for i, (_, e) in enumerate(times)]), defaultdict(int), 0
        for cur, person in arrival:
            while i < n and leaving[i][0] <= cur:
                heappush(available, seats[leaving[i][1]])
                i += 1
            seats[person] = (seat := heappop(available))
            if person == targetFriend:
                return seat
        return -1
```
