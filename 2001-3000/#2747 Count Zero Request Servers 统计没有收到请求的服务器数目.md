# 2747 Count Zero Request Servers 统计没有收到请求的服务器数目

__Description:__

You are given an integer `n` denoting the total number of servers and a __2D__ __0-indexed__ integer array `logs`, where `logs[i] = [server_id, time]` denotes that the server with id `server_id` received a request at time `time`.

You are also given an integer `x` and a __0-indexed__ integer array `queries`.

Return _a __0-indexed__ integer array_ `arr` _of length_ `queries.length` _where_ `arr[i]` _represents the number of servers that __did not receive__ any requests during the time interval_ `[queries[i] - x, queries[i]]`.

Note that the time intervals are inclusive.

__Example:__

Example 1:

```text
Input: n = 3, logs = [[1,3],[2,6],[1,5]], x = 5, queries = [10,11]
Output: [1,2]
Explanation: 
For queries[0]: The servers with ids 1 and 2 get requests in the duration of [5, 10]. Hence, only server 3 gets zero requests.
For queries[1]: Only the server with id 2 gets a request in duration of [6,11]. Hence, the servers with ids 1 and 3 are the only servers that do not receive any requests during that time period.
```

Example 2:

```text
Input: n = 3, logs = [[2,4],[2,1],[1,2],[3,1]], x = 2, queries = [3,4]
Output: [0,1]
Explanation: 
For queries[0]: All servers get at least one request in the duration of [1, 3].
For queries[1]: Only server with id 3 gets no request in the duration [2,4].
```

__Constraints:__

- `1 <= n <= 10 ^ 5`
- `1 <= logs.length <= 10 ^ 5`
- `1 <= queries.length <= 10 ^ 5`
- `logs[i].length == 2`
- `1 <= logs[i][0] <= n`
- `1 <= logs[i][1] <= 10 ^ 6`
- `1 <= x <= 10 ^ 5`
- `x < queries[i] <= 10 ^ 6`

__题目描述:__

给你一个整数 `n` ，表示服务器的总数目，再给你一个下标从 __0__ 开始的 __二维__ 整数数组 `logs` ，其中 `logs[i] = [server_id, time]` 表示 id 为 `server_id` 的服务器在 `time` 时收到了一个请求。

同时给你一个整数 `x` 和一个下标从 __0__ 开始的整数数组 `queries` 。

请你返回一个长度等于 `queries.length` 的数组 `arr` ，其中 `arr[i]` 表示在时间区间 `[queries[i] - x, queries[i]]` 内没有收到请求的服务器数目。

注意时间区间是个闭区间。

__示例:__

示例 1：

```text
输入：n = 3, logs = [[1,3],[2,6],[1,5]], x = 5, queries = [10,11]
输出：[1,2]
解释：
对于 queries[0]：id 为 1 和 2 的服务器在区间 [5, 10] 内收到了请求，所以只有服务器 3 没有收到请求。
对于 queries[1]：id 为 2 的服务器在区间 [6,11] 内收到了请求，所以 id 为 1 和 3 的服务器在这个时间段内没有收到请求。
```

示例 2：

```text
输入：n = 3, logs = [[2,4],[2,1],[1,2],[3,1]], x = 2, queries = [3,4]
输出：[0,1]
解释：
对于 queries[0]：区间 [1, 3] 内所有服务器都收到了请求。
对于 queries[1]：只有 id 为 3 的服务器在区间 [2,4] 内没有收到请求。
```

__提示：__

- `1 <= n <= 10 ^ 5`
- `1 <= logs.length <= 10 ^ 5`
- `1 <= queries.length <= 10 ^ 5`
- `logs[i].length == 2`
- `1 <= logs[i][0] <= n`
- `1 <= logs[i][1] <= 10 ^ 6`
- `1 <= x <= 10 ^ 5`
- `x < queries[i] <= 10 ^ 6`

__思路:__

```text
滑动窗口
将 logs 按照时间顺序排序
将 queries 也按照时间进行排序
遍历 queries, 用一个数记录窗口外的服务器数量
将当前不大于 queries[i] 的 logs 按照序号加入窗口, 如果这个是新加入的序号, 在窗口外的计数减 1
将当前比 queries[i] - x 的 logs 从窗口移除, 如果这个是序号对应的最后一个 log, 在窗口外的计数加 1
每一个查询对应一个窗口外的服务器数量
时间复杂度为 O(MlogM + QlogQ), 空间复杂度为 O(N + Q), 主要瓶颈在排序
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> countServers(int n, vector<vector<int>>& logs, int x, vector<int>& queries) 
    {
        int m = logs.size(), q = queries.size(), out = n, left = 0, right = 0;
        vector<int> result(q), cur(n + 1), idx(q);
        sort(logs.begin(), logs.end(), [&](const auto& a, const auto& b) { return a.back() < b.back(); });
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&](const auto& a, const auto& b) { return queries[a] < queries[b]; });
        for (const auto& i : idx) 
        {
            while (right < m and logs[right][1] <= queries[i]) if (cur[logs[right++][0]]++ == 0) --out;
            while (left < m and logs[left][1] < queries[i] - x) if (--cur[logs[left++][0]] == 0) ++out;
            result[i] = out;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] countServers(int n, int[][] logs, int x, int[] queries) {
        int m = logs.length, q = queries.length, cur[] = new int[n + 1], out = n, result[] = new int[q], left = 0, right = 0;
        Arrays.sort(logs, (a, b) -> a[1] - b[1]);
        var idx = new Integer[q];
        for (int i = 0; i < q; i++) idx[i] = i;
        Arrays.sort(idx, (a, b) -> queries[a] - queries[b]);
        for (int i : idx) {
            while (right < m && logs[right][1] <= queries[i]) if (cur[logs[right++][0]]++ == 0) --out;
            while (left < m && logs[left][1] < queries[i] - x) if (--cur[logs[left++][0]] == 0) ++out;
            result[i] = out;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countServers(self, n: int, logs: List[List[int]], x: int, queries: List[int]) -> List[int]:
        result, cur, left, right, m, out = [0] * len(queries), defaultdict(int), 0, 0, len(logs), n
        logs.sort(key=lambda log: log[1])
        for i, q in sorted(enumerate(queries), key=lambda query: query[1]):
            while right < m and logs[right][1] <= q:
                if not cur[server := logs[right][0]]:
                    out -= 1
                cur[server] += 1
                right += 1
            while left < m and logs[left][1] < q - x:
                if cur[server := logs[left][0]] == 1:
                    out += 1
                cur[server] -= 1
                left += 1
            result[i] = out
        return result
```
