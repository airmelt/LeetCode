# 1851 Minimum Interval to Include Each Query 包含每个查询的最小区间

__Description:__

You are given a 2D integer array `intervals`, where `intervals[i] = [lefti, righti]` describes the `i ^ th` interval starting at `lefti` and ending at `righti` __(inclusive)__. The __size__ of an interval is defined as the number of integers it contains, or more formally `righti - lefti + 1`.

You are also given an integer array `queries`. The answer to the `j ^ th` query is the __size of the smallest interval__ `i` such that `lefti <= queries[j] <= righti`. If no such interval exists, the answer is `-1`.

Return an array containing the answers to the queries.

__Example:__

Example 1:

```text
Input: intervals = [[1,4],[2,4],[3,6],[4,4]], queries = [2,3,4,5]
Output: [3,3,1,4]
Explanation: The queries are processed as follows:
```

- Query = 2: The interval [2,4] is the smallest interval containing 2. The answer is 4 - 2 + 1 = 3.
- Query = 3: The interval [2,4] is the smallest interval containing 3. The answer is 4 - 2 + 1 = 3.
- Query = 4: The interval [4,4] is the smallest interval containing 4. The answer is 4 - 4 + 1 = 1.
- Query = 5: The interval [3,6] is the smallest interval containing 5. The answer is 6 - 3 + 1 = 4.

Example 2:

```text
Input: intervals = [[2,3],[2,5],[1,8],[20,25]], queries = [2,19,5,22]
Output: [2,-1,4,6]
Explanation: The queries are processed as follows:
```

- Query = 2: The interval [2,3] is the smallest interval containing 2. The answer is 3 - 2 + 1 = 2.
- Query = 19: None of the intervals contain 19. The answer is -1.
- Query = 5: The interval [2,5] is the smallest interval containing 5. The answer is 5 - 2 + 1 = 4.
- Query = 22: The interval [20,25] is the smallest interval containing 22. The answer is 25 - 20 + 1 = 6.

__Constraints:__

- `1 <= intervals.length <= 10 ^ 5`
- `1 <= queries.length <= 10 ^ 5`
- `intervals[i].length == 2`
- `1 <= lefti <= righti <= 10 ^ 7`
- `1 <= queries[j] <= 10 ^ 7`

__题目描述:__

给你一个二维整数数组 `intervals` ，其中 `intervals[i] = [lefti, righti]` 表示第 `i` 个区间开始于 `lefti` 、结束于 `righti`（包含两侧取值，__闭区间__）。区间的 __长度__ 定义为区间中包含的整数数目，更正式地表达是 `righti - lefti + 1` 。

再给你一个整数数组 `queries` 。第 `j` 个查询的答案是满足 `lefti <= queries[j] <= righti` 的 __长度最小区间 `i` 的长度__ 。如果不存在这样的区间，那么答案是 `-1` 。

以数组形式返回对应查询的所有答案。

__示例:__

示例 1：

```text
输入：intervals = [[1,4],[2,4],[3,6],[4,4]], queries = [2,3,4,5]
输出：[3,3,1,4]
解释：查询处理如下：
```

- Query = 2 ：区间 [2,4] 是包含 2 的最小区间，答案为 4 - 2 + 1 = 3 。
- Query = 3 ：区间 [2,4] 是包含 3 的最小区间，答案为 4 - 2 + 1 = 3 。
- Query = 4 ：区间 [4,4] 是包含 4 的最小区间，答案为 4 - 4 + 1 = 1 。
- Query = 5 ：区间 [3,6] 是包含 5 的最小区间，答案为 6 - 3 + 1 = 4 。

示例 2：

```text
输入：intervals = [[2,3],[2,5],[1,8],[20,25]], queries = [2,19,5,22]
输出：[2,-1,4,6]
解释：查询处理如下：
```

- Query = 2 ：区间 [2,3] 是包含 2 的最小区间，答案为 3 - 2 + 1 = 2 。
- Query = 19：不存在包含 19 的区间，答案为 -1 。
- Query = 5 ：区间 [2,5] 是包含 5 的最小区间，答案为 5 - 2 + 1 = 4 。
- Query = 22：区间 [20,25] 是包含 22 的最小区间，答案为 25 - 20 + 1 = 6 。

__提示：__

- `1 <= intervals.length <= 10 ^ 5`
- `1 <= queries.length <= 10 ^ 5`
- `intervals[i].length == 2`
- `1 <= lefti <= righti <= 10 ^ 7`
- `1 <= queries[j] <= 10 ^ 7`

__思路:__

```text
离线查询 ➕ 优先队列
对于某一个查询, 可以遍历 intervals 找到合适的区间再从中选择最小的区间, 这样每一次查询都需要遍历 intervals 数组
查询的时候可以按照查询的值从小到大排序
将 intervals 按照左端点从小到大排序
这样每一次查询就不必遍历 intervals 数组, 而是从上一次遍历的位置开始遍历
设置优先队列, 优先队列中存储的是区间的长度, 左端点, 右端点, 优先队列按照区间的长度从小到大排序, 即小根堆
每一次需要将 left <= queries[x] <= right, 的 intervals[i] 加入优先队列
若 i 不超过 intervals 的长度且 intervals[i][0] <= queries[x], 则将 (区间的长度, 左端点, 右端点) 加入优先队列
若 i 超过 intervals 的长度或者 intervals[i][0] > queries[x], 则停止将区间加入优先队列
若 pq 不为空, 则检查 pq 的堆顶元素的右端点是否小于 queries[x], 若小于则将堆顶元素弹出
若 pq 不为空, 则将 pq 的堆顶元素的区间长度加入 result[x] 中
时间复杂度为 O(NlogN + MlogM), 空间复杂度为 O(N + M), 其中 N 为 queries 的长度, M 为 intervals 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> minInterval(vector<vector<int>>& intervals, vector<int>& queries) 
    {
        int n = queries.size();
        vector<int> result(n, -1);
        set<pair<int, int>> s;
        for (int i = 0; i < n; i++) s.emplace(queries[i], i);
        sort(intervals.begin(), intervals.end(), [&](const auto& a, const auto& b) { return a[1] - a[0] < b[1] - b[0]; });
        for (const auto &v : intervals)
        {
            auto it = s.lower_bound({v[0], -1});
            while (it != s.end() and it -> first <= v[1]) 
            {
                result[it -> second] = v[1] - v[0] + 1;
                s.erase(it++);
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] minInterval(int[][] intervals, int[] queries) {
        int n = queries.length, m = intervals.length, result[] = new int[n], p = 0;
        Integer[] idx = new Integer[queries.length];
        for (int i = 0; i < n; i++) idx[i] = i;
        Arrays.sort(idx, (i, j) -> queries[i] - queries[j]);
        Arrays.sort(intervals, (i, j) -> i[0] - j[0]);
        Arrays.fill(result, -1);
        PriorityQueue<int[]> pq = new PriorityQueue<int[]>((a, b) -> a[0] - b[0]);
        for (int x : idx) {
            while (p < m && intervals[p][0] <= queries[x]) pq.offer(new int[]{intervals[p][1] - intervals[p][0] + 1, intervals[p][0], intervals[p++][1]});
            while (!pq.isEmpty() && pq.peek()[2] < queries[x]) pq.poll();
            if (!pq.isEmpty()) result[x] = pq.peek()[0];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minInterval(self, intervals: List[List[int]], queries: List[int]) -> List[int]:
        result, m, pq, idx, i = [-1] * (n := len(queries)), len(intervals), [], list(range(n)), 0
        idx.sort(key=lambda x: queries[x])
        intervals.sort(key=lambda x: x[0])
        for x in idx:
            while i < m and intervals[i][0] <= queries[x]:
                heappush(pq, (intervals[i][1] - intervals[i][0] + 1, intervals[i][0], intervals[i][1]))
                i += 1
            while pq and pq[0][2] < queries[x]:
                heappop(pq)
            if pq:
                result[x] = pq[0][0]
        return result
```
