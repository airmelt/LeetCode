# 1751 Maximum Number of Events That Can Be Attended II 最多可以参加的会议数目 II

__Description:__

You are given an array of `events` where `events[i] = [startDayi, endDayi, valuei]`. The `i ^ th` event starts at `startDayi` and ends at `endDayi`, and if you attend this event, you will receive a value of `valuei`. You are also given an integer `k` which represents the maximum number of events you can attend.

You can only attend one event at a time. If you choose to attend an event, you must attend the entire event. Note that the end day is inclusive: that is, you cannot attend two events where one of them starts and the other ends on the same day.

Return the maximum sum of values that you can receive by attending events.

__Example:__

Example 1:

![1751-1](https://assets.leetcode.com/uploads/2021/01/10/screenshot-2021-01-11-at-60048-pm.png)

```text
Input: events = [[1,2,4],[3,4,3],[2,3,1]], k = 2
Output: 7
Explanation: Choose the green events, 0 and 1 (0-indexed) for a total value of 4 + 3 = 7.
```

Example 2:

![1751-2](https://assets.leetcode.com/uploads/2021/01/10/screenshot-2021-01-11-at-60150-pm.png)

```text
Input: events = [[1,2,4],[3,4,3],[2,3,10]], k = 2
Output: 10
Explanation: Choose event 2 for a total value of 10.
Notice that you cannot attend any other event as they overlap, and that you do not have to attend k events.
```

Example 3:

![1751-3](https://assets.leetcode.com/uploads/2021/01/10/screenshot-2021-01-11-at-60703-pm.png)

```text
Input: events = [[1,1,1],[2,2,2],[3,3,3],[4,4,4]], k = 3
Output: 9
Explanation: Although the events do not overlap, you can only attend 3 events. Pick the highest valued three.
```

__Constraints:__

- `1 <= k <= events.length`
- `1 <= k * events.length <= 10 ^ 6`
- `1 <= startDayi <= endDayi <= 10 ^ 9`
- `1 <= valuei <= 10 ^ 6`

__题目描述:__

给你一个 `events` 数组，其中 `events[i] = [startDayi, endDayi, valuei]` ，表示第 `i` 个会议在 `startDayi` 天开始，第 `endDayi` 天结束，如果你参加这个会议，你能得到价值 `valuei` 。同时给你一个整数 `k` 表示你能参加的最多会议数目。

你同一时间只能参加一个会议。如果你选择参加某个会议，那么你必须 完整 地参加完这个会议。会议结束日期是包含在会议内的，也就是说你不能同时参加一个开始日期与另一个结束日期相同的两个会议。

请你返回能得到的会议价值 最大和 。

__示例:__

示例 1：

![1751-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/02/06/screenshot-2021-01-11-at-60048-pm.png)

```text
输入：events = [[1,2,4],[3,4,3],[2,3,1]], k = 2
输出：7
解释：选择绿色的活动会议 0 和 1，得到总价值和为 4 + 3 = 7 。
```

示例 2：

![1751-5](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/02/06/screenshot-2021-01-11-at-60150-pm.png)

```text
输入：events = [[1,2,4],[3,4,3],[2,3,10]], k = 2
输出：10
解释：参加会议 2 ，得到价值和为 10 。
你没法再参加别的会议了，因为跟会议 2 有重叠。你 不 需要参加满 k 个会议。
```

示例 3：

![1751-6](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/02/06/screenshot-2021-01-11-at-60703-pm.png)

```text
输入：events = [[1,1,1],[2,2,2],[3,3,3],[4,4,4]], k = 3
输出：9
解释：尽管会议互不重叠，你只能参加 3 个会议，所以选择价值最大的 3 个会议。
```

__提示：__

- `1 <= k <= events.length`
- `1 <= k * events.length <= 10 ^ 6`
- `1 <= startDayi <= endDayi <= 10 ^ 9`
- `1 <= valuei <= 10 ^ 6`

__思路:__

```text
动态规划 ➕ 二分查找
设 dp[i][j] 表示前 i 个会议中参加 j 个会议的最大价值
不参加第 i 个会议: dp[i][j] = dp[i - 1][j]
参加第 i 个会议: dp[i][j] = dp[p][j - 1] + events[i][2], 其中 p 为第 i 个会议开始时间之前的某个会议, 满足 events[p][1] < events[i][0], 否则为 -1
dp[i][j] = max(dp[i - 1][j], dp[p][j - 1] + events[i][2])
为了避免处理 i = 0 的情况, 可以将 dp[i][j] 定义为前 i + 1 个会议中参加 j 个会议的最大价值
dp[i + 1][j] = max(dp[i][j], dp[p + 1][j - 1] + events[i][2])
可以先将 events 按照结束时间排序, 然后使用二分查找找到 p
时间复杂度为 O(NlogN + NK), 空间复杂度为 O(NK)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxValue(vector<vector<int>>& events, int k) 
    {
        sort(events.begin(), events.end(), [](auto &a, auto &b) { return a[1] < b[1]; });
        int n = events.size(), dp[n + 1][k + 1];
        memset(dp, 0, sizeof(dp));
        for (int i = 0; i < n; i++) for (int j = 1, p = lower_bound(events.begin(), events.begin() + i, events[i].front(), [](auto &e, int lower) { return e[1] < lower; }) - events.begin(); j <= k; ++j) dp[i + 1][j] = max(dp[i][j], dp[p][j - 1] + events[i][2]);
        return dp[n][k];
    }
};
```

__Java__:

```Java
class Solution {
    public int maxValue(int[][] events, int k) {
        Arrays.sort(events, Comparator.comparingInt(x -> x[1]));
        int n = events.length, dp[][] = new int[n + 1][k + 1];
        for (int i = 0; i < n; i++) {
            int p = helper(events, i, events[i][0]);
            for (int j = 1; j <= k; j++) dp[i + 1][j] = Math.max(dp[i][j], dp[p + 1][j - 1] + events[i][2]);
        }
        return dp[n][k];
    }

    private int helper(int[][] events, int right, int val) {
        int left = -1;
        while (left + 1 < right) {
            int mid = ((left + right) >> 1);
            if (events[mid][1] < val) left = mid;
            else right = mid;
        }
        return left;
    }
}
```

__Python__:

```Python
class Solution:
    def maxValue(self, events: List[List[int]], k: int) -> int:
        events.sort(key=lambda e: e[1])
        n = len(events)
        dp = [[0] * (k + 1) for _ in range(n + 1)]
        for i, (start, end, val) in enumerate(events):
            p = bisect_left(events, start, hi=i, key=lambda e: e[1])
            for j in range(1, k + 1):
                dp[i + 1][j] = max(dp[i][j], dp[p][j - 1] + val)
        return dp[-1][-1]
```
