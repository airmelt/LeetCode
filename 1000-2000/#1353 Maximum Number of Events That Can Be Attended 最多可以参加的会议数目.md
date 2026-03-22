# 1353 Maximum Number of Events That Can Be Attended 最多可以参加的会议数目

__Description:__

You are given an array of events where events[i] = [startDayi, endDayi]. Every event i starts at startDayi and ends at endDayi.

You can attend an event i at any day d where startTimei <= d <= endTimei. You can only attend one event at any time d.

Return the maximum number of events you can attend.

__Example:__

Example 1:

![Events](https://assets.leetcode.com/uploads/2020/02/05/e1.png)

Input: events = [[1,2],[2,3],[3,4]]
Output: 3
Explanation: You can attend all the three events.
One way to attend them all is as shown.
Attend the first event on day 1.
Attend the second event on day 2.
Attend the third event on day 3.

Example 2:

Input: events= [[1,2],[2,3],[3,4],[1,2]]
Output: 4

__Constraints:__

1 <= events.length <= 10^5
events[i].length == 2
1 <= startDayi <= endDayi <= 10^5

__题目描述：__

给你一个数组 events，其中 events[i] = [startDayi, endDayi] ，表示会议 i 开始于 startDayi ，结束于 endDayi 。

你可以在满足 startDayi <= d <= endDayi 中的任意一天 d 参加会议 i 。注意，一天只能参加一个会议。

请你返回你可以参加的 最大 会议数目。

__示例：__

示例 1：

![会议](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/16/e1.png)

输入：events = [[1,2],[2,3],[3,4]]
输出：3
解释：你可以参加所有的三个会议。
安排会议的一种方案如上图。
第 1 天参加第一个会议。
第 2 天参加第二个会议。
第 3 天参加第三个会议。

示例 2：

输入：events= [[1,2],[2,3],[3,4],[1,2]]
输出：4

__提示：__

1 <= events.length <= 10^5
events[i].length == 2
1 <= startDayi <= endDayi <= 10^5

__思路：__

扫描线 ➕ 优先队列
按照开始时间将对应下标加入
对于每个开始时间将对应结束时间加入小根堆
将所有不合法的结束时间都弹出, 然后如果小根堆不为空, 弹出一个时间, 该会议可以参加
时间复杂度为 O(slgn), 空间复杂度为 O(s), 其中 s 表示最大的结束时间

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int maxEvents(vector<vector<int>>& events) 
    {
        vector<vector<int>> starts(100010);
        int result = 0, n = events.size();
        for (int i = 0; i < n; i++) starts[events[i].front()].emplace_back(i);
        priority_queue<int> pq;
        for (int i = 1; i < 100010; i++) 
        {
            for (int j : starts[i]) pq.push(-events[j].back());
            while (!pq.empty() and -pq.top() < i) pq.pop();
            if (!pq.empty()) 
            {
                pq.pop();
                ++result;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxEvents(int[][] events) {
        List<List<Integer>> starts = new ArrayList<>(100010);
        int m = 0, result = 0, n = events.length;
        for (int i = 0; i < n; i++) m = Math.max(m, events[i][1]);
        for (int i = 0; i <= m; i++) starts.add(new ArrayList<>());
        for (int i = 0; i < n; i++) starts.get(events[i][0]).add(i);
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for (int i = 1; i <= m; i++) {
            for (int j : starts.get(i)) pq.offer(events[j][1]);
            while (!pq.isEmpty() && pq.peek() < i) pq.poll();
            if (!pq.isEmpty()) {
                pq.poll();
                ++result;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxEvents(self, events: List[List[int]]) -> int:
        n, starts, heap, result, m = len(events), defaultdict(list), [], 0, max(e for s, e in events)
        for i in range(n):
            starts[events[i][0]].append(i)
        for i in range(1, m + 1):
            for j in starts[i]:
                heappush(heap, events[j][1])
            while heap and heap[0] < i:
                heappop(heap)
            if heap:
                heappop(heap)
                result += 1
        return result
```
