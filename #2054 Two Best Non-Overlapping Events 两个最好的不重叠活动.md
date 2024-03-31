# 2054 Two Best Non-Overlapping Events 两个最好的不重叠活动

__Description:__

You are given a __0-indexed__ 2D integer array of `events` where `events[i] = [startTimei, endTimei, valuei]`. The `i ^ th` event starts at `startTimei` and ends at `endTimei`, and if you attend this event, you will receive a value of `valuei`. You can choose __at most__ __two__ __non-overlapping__ events to attend such that the sum of their values is __maximized__.

Return this maximum sum.

Note that the start time and end time is __inclusive__: that is, you cannot attend two events where one of them starts and the other ends at the same time. More specifically, if you attend an event with end time `t`, the next event must start at or after `t + 1`.

__Example:__

Example 1:

![2054-1](https://assets.leetcode.com/uploads/2021/09/21/picture5.png)

```text
Input: events = [[1,3,2],[4,5,2],[2,4,3]]
Output: 4
Explanation: Choose the green events, 0 and 1 for a sum of 2 + 2 = 4.
```

Example 2:

![2054-2](https://assets.leetcode.com/uploads/2021/09/21/picture1.png)

```text
Input: events = [[1,3,2],[4,5,2],[1,5,5]]
Output: 5
Explanation: Choose event 2 for a sum of 5.
```

Example 3:

![2054-3](https://assets.leetcode.com/uploads/2021/09/21/picture3.png)

```text
Input: events = [[1,5,3],[1,5,1],[6,6,5]]
Output: 8
Explanation: Choose events 0 and 2 for a sum of 3 + 5 = 8.
```

__Constraints:__

- `2 <= events.length <= 10 ^ 5`
- `events[i].length == 3`
- `1 <= startTimei <= endTimei <= 10 ^ 9`
- `1 <= valuei <= 10 ^ 6`

__题目描述:__

给你一个下标从 __0__ 开始的二维整数数组 `events` ，其中 `events[i] = [startTimei, endTimei, valuei]` 。第 `i` 个活动开始于 `startTimei` ，结束于 `endTimei` ，如果你参加这个活动，那么你可以得到价值 `valuei` 。你 __最多__ 可以参加 __两个时间不重叠__ 活动，使得它们的价值之和 __最大__ 。

请你返回价值之和的 最大值 。

注意，活动的开始时间和结束时间是 __包括__ 在活动时间内的，也就是说，你不能参加两个活动且它们之一的开始时间等于另一个活动的结束时间。更具体的，如果你参加一个活动，且结束时间为 `t` ，那么下一个活动必须在 `t + 1` 或之后的时间开始。

__示例:__

示例 1:

![2054-4](https://assets.leetcode.com/uploads/2021/09/21/picture5.png)

```text
输入：events = [[1,3,2],[4,5,2],[2,4,3]]
输出：4
解释：选择绿色的活动 0 和 1 ，价值之和为 2 + 2 = 4 。
```

示例 2：

![2054-5](https://assets.leetcode.com/uploads/2021/09/21/picture1.png)

```text
输入：events = [[1,3,2],[4,5,2],[1,5,5]]
输出：5
解释：选择活动 2 ，价值和为 5 。
```

示例 3：

![2054-6](https://assets.leetcode.com/uploads/2021/09/21/picture3.png)

```text
输入：events = [[1,5,3],[1,5,1],[6,6,5]]
输出：8
解释：选择活动 0 和 2 ，价值之和为 3 + 5 = 8 。
```

__提示：__

- `2 <= events.length <= 10 ^ 5`
- `events[i].length == 3`
- `1 <= startTimei <= endTimei <= 10 ^ 9`
- `1 <= valuei <= 10 ^ 6`

__思路:__

```text
优先队列
将活动按照开始时间排序
使用优先队列存储活动的结束时间和价值
弹出结束时间晚于当前活动开始时间的活动
更新当前最大价值
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxTwoEvents(vector<vector<int>>& events) 
    {
        int result = 0, cur = 0;
        sort(events.begin(), events.end());
        priority_queue<pair<int, int>> pq;
        for (const auto& event : events)
        {
            while (!pq.empty() and -pq.top().first < event.front())
            {
                cur = max(cur, -pq.top().second);
                pq.pop();
            }
            result = max(result, cur + event.back());
            pq.push(make_pair(-event[1], -event[2]));
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxTwoEvents(int[][] events) {
        Arrays.sort(events, (a, b) -> (a[0] - b[0]));
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> (a[1] - b[1]));
        int result = 0, cur = 0;
        for (int[] event : events) {
            while (!pq.isEmpty() && pq.peek()[1] < event[0]) cur = Math.max(cur, pq.poll()[2]);
            result = Math.max(result, cur + event[2]);
            pq.offer(event);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxTwoEvents(self, events: List[List[int]]) -> int:
        cur, result, heap = 0, 0, []
        for start, end, val in sorted(events):
            while heap and heap[0][0] < start:
                cur = max(cur, heappop(heap)[1])
            result = max(result, cur + val)
            heappush(heap, (end, val))
        return result
```
