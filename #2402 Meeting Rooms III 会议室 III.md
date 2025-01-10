# 2402 Meeting Rooms III 会议室 III

__Description:__

You are given an integer `n`. There are `n` rooms numbered from `0` to `n - 1`.

You are given a 2D integer array `meetings` where `meetings[i] = [start_i, end_i]` means that a meeting will be held during the __half-closed__ time interval `[start_i, end_i)`. All the values of `start_i` are __unique__.

Meetings are allocated to rooms in the following manner:

Return the number of the room that held the most meetings. If there are multiple rooms, return the room with the lowest number.

A __half-closed interval__ `[a, b)` is the interval between `a` and `b` __including__ `a` and __not including__ `b`.

__Example:__

Example 1:

```text
Input: n = 2, meetings = [[0,10],[1,5],[2,7],[3,4]]
Output: 0
Explanation:
```

- At time 0, both rooms are not being used. The first meeting starts in room 0.
- At time 1, only room 1 is not being used. The second meeting starts in room 1.
- At time 2, both rooms are being used. The third meeting is delayed.
- At time 3, both rooms are being used. The fourth meeting is delayed.
- At time 5, the meeting in room 1 finishes. The third meeting starts in room 1 for the time period [5,10).
- At time 10, the meetings in both rooms finish. The fourth meeting starts in room 0 for the time period [10,11).

Both rooms 0 and 1 held 2 meetings, so we return 0.

Example 2:

```text
Input: n = 3, meetings = [[1,20],[2,10],[3,5],[4,9],[6,8]]
Output: 1
Explanation:
```

- At time 1, all three rooms are not being used. The first meeting starts in room 0.
- At time 2, rooms 1 and 2 are not being used. The second meeting starts in room 1.
- At time 3, only room 2 is not being used. The third meeting starts in room 2.
- At time 4, all three rooms are being used. The fourth meeting is delayed.
- At time 5, the meeting in room 2 finishes. The fourth meeting starts in room 2 for the time period [5,10).
- At time 6, all three rooms are being used. The fifth meeting is delayed.
- At time 10, the meetings in rooms 1 and 2 finish. The fifth meeting starts in room 1 for the time period [10,12).

Room 0 held 1 meeting while rooms 1 and 2 each held 2 meetings, so we return 1.

__Constraints:__

- `1 <= n <= 100`
- `1 <= meetings.length <= 10 ^ 5`
- `meetings[i].length == 2`
- `0 <= start_i < end_i <= 5 * 10 ^ 5`
- All the values of `start_i` are __unique__.

__题目描述:__

给你一个整数 `n` ，共有编号从 `0` 到 `n - 1` 的 `n` 个会议室。

给你一个二维整数数组 `meetings` ，其中 `meetings[i] = [start_i, end_i]` 表示一场会议将会在 __半闭__ 时间区间 `[start_i, end_i)` 举办。所有 `start_i` 的值 __互不相同__ 。

会议将会按以下方式分配给会议室：

返回举办最多次会议的房间 编号 。如果存在多个房间满足此条件，则返回编号 最小 的房间。

__半闭区间__ `[a, b)` 是 `a` 和 `b` 之间的区间，__包括__ `a` 但 __不包括__ `b` 。

__示例:__

示例 1：

```text
输入：n = 2, meetings = [[0,10],[1,5],[2,7],[3,4]]
输出：0
解释：
```

- 在时间 0 ，两个会议室都未占用，第一场会议在会议室 0 举办。
- 在时间 1 ，只有会议室 1 未占用，第二场会议在会议室 1 举办。
- 在时间 2 ，两个会议室都被占用，第三场会议延期举办。
- 在时间 3 ，两个会议室都被占用，第四场会议延期举办。
- 在时间 5 ，会议室 1 的会议结束。第三场会议在会议室 1 举办，时间周期为 [5,10) 。
- 在时间 10 ，两个会议室的会议都结束。第四场会议在会议室 0 举办，时间周期为 [10,11) 。

会议室 0 和会议室 1 都举办了 2 场会议，所以返回 0 。

示例 2：

```text
输入：n = 3, meetings = [[1,20],[2,10],[3,5],[4,9],[6,8]]
输出：1
解释：
```

- 在时间 1 ，所有三个会议室都未占用，第一场会议在会议室 0 举办。
- 在时间 2 ，会议室 1 和 2 未占用，第二场会议在会议室 1 举办。
- 在时间 3 ，只有会议室 2 未占用，第三场会议在会议室 2 举办。
- 在时间 4 ，所有三个会议室都被占用，第四场会议延期举办。
- 在时间 5 ，会议室 2 的会议结束。第四场会议在会议室 2 举办，时间周期为 [5,10) 。
- 在时间 6 ，所有三个会议室都被占用，第五场会议延期举办。
- 在时间 10 ，会议室 1 和 2 的会议结束。第五场会议在会议室 1 举办，时间周期为 [10,12) 。

会议室 1 和会议室 2 都举办了 2 场会议，所以返回 1 。

__提示：__

- `1 <= n <= 100`
- `1 <= meetings.length <= 10 ^ 5`
- `meetings[i].length == 2`
- `0 <= start_i < end_i <= 5 * 10 ^ 5`
- `start_i` 的所有值 __互不相同__

__思路:__

```text
贪心 ➕ 优先队列
先将会议按照开始时间进行排序
设置两个堆 (也可以只设置一个) 
一个 using 记录会议结束的时间以及当前正在进行的会议室编号
另一个 idle 记录空闲的会议室
如果可以每次需要开会的时候从 idle 里取出一个会议室
否则这时会议室都被占据, 等待 using 中最快结束的会议结束之后立马原地安排下一场会议
在每次安排会议前将结束的会议从 using 中取出加入到 idle 里
记录每场会议所在的会议室编号
最后遍历会议室取最大的即可
时间复杂度为 O(N + M(logN + logM)), 空间复杂度为 O(N), 其中 M 为 meetings 的长度
```

__代码:__

__C++__:

```C++
class Solution {
    public int mostBooked(int n, int[][] meetings) {
        int result = 0, count[] = new int[n];
        Arrays.sort(meetings, (a, b) -> Integer.compare(a[0], b[0]));
        var idle = new PriorityQueue<Integer>();
        for (int i = 0; i < n; i++) idle.offer(i);
        var using = new PriorityQueue<Pair<Long, Integer>>((a, b) -> !Objects.equals(a.getKey(), b.getKey()) ? Long.compare(a.getKey(), b.getKey()) : Integer.compare(a.getValue(), b.getValue()));
        for (var m : meetings) {
            long s = m[0], e = m[1];
            int i = 0;
            while (!using.isEmpty() && using.peek().getKey() <= s) idle.offer(using.poll().getValue());
            if (idle.isEmpty()) {
                var p = using.poll();
                e += p.getKey() - s;
                i = p.getValue();
            } else i = idle.poll();
            ++count[i];
            using.offer(new Pair<>(e, i));
        }
        for (int i = 0; i < n; i++) if (count[i] > count[result]) result = i;
        return result;
    }
}
```

__Java__:

```Java
class Solution {
    public int mostBooked(int n, int[][] meetings) {
        int result = 0, count[] = new int[n];
        Arrays.sort(meetings, (a, b) -> Integer.compare(a[0], b[0]));
        var idle = new PriorityQueue<Integer>();
        for (int i = 0; i < n; i++) idle.offer(i);
        var using = new PriorityQueue<Pair<Long, Integer>>((a, b) -> !Objects.equals(a.getKey(), b.getKey()) ? Long.compare(a.getKey(), b.getKey()) : Integer.compare(a.getValue(), b.getValue()));
        for (var m : meetings) {
            long s = m[0], e = m[1];
            int i = 0;
            while (!using.isEmpty() && using.peek().getKey() <= s) idle.offer(using.poll().getValue());
            if (idle.isEmpty()) {
                var p = using.poll();
                e += p.getKey() - s;
                i = p.getValue();
            } else i = idle.poll();
            ++count[i];
            using.offer(new Pair<>(e, i));
        }
        for (int i = 0; i < n; i++) if (count[i] > count[result]) result = i;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def mostBooked(self, n: int, meetings: List[List[int]]) -> int:
        count, idle, using = [0] * n, list(range(n)), []
        meetings.sort(key=lambda m: m[0])
        for s, e in meetings:
            while using and using[0][0] <= s:
                heappush(idle, heappop(using)[1])
            if not idle:
                nxt, i = heappop(using)
                e += nxt - s
            else:
                i = heappop(idle)
            count[i] += 1
            heappush(using, (e, i))
        return count.index(max(count))
            
```
