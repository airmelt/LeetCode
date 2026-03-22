# 1606 Find Servers That Handled Most Number of Requests 找到处理最多请求的服务器

__Description:__

You have `k` servers numbered from `0` to `k-1` that are being used to handle multiple requests simultaneously. Each server has infinite computational capacity but __cannot handle more than one request at a time__. The requests are assigned to servers according to a specific algorithm:

- The `i ^ th` (0-indexed) request arrives.
- If all servers are busy, the request is dropped (not handled at all).
- If the `(i % k) ^ th` server is available, assign the request to that server.
- Otherwise, assign the request to the next available server (wrapping around the list of servers and starting from 0 if necessary). For example, if the `i ^ th` server is busy, try to assign the request to the `(i+1) ^ th` server, then the `(i+2) ^ th` server, and so on.

You are given a __strictly increasing__ array `arrival` of positive integers, where `arrival[i]` represents the arrival time of the `i ^ th` request, and another array `load`, where `load[i]` represents the load of the `i ^ th` request (the time it takes to complete). Your goal is to find the __busiest server(s)__. A server is considered __busiest__ if it handled the most number of requests successfully among all the servers.

Return a list containing the IDs (0-indexed) of the busiest server(s). You may return the IDs in any order.

__Example:__

Example 1:

![1606-1](https://assets.leetcode.com/uploads/2020/09/08/load-1.png)

```text
Input: k = 3, arrival = [1,2,3,4,5], load = [5,2,3,3,3] 
Output: [1] 
Explanation: 
All of the servers start out available.
The first 3 requests are handled by the first 3 servers in order.
Request 3 comes in. Server 0 is busy, so it's assigned to the next available server, which is 1.
Request 4 comes in. It cannot be handled since all servers are busy, so it is dropped.
Servers 0 and 2 handled one request each, while server 1 handled two requests. Hence server 1 is the busiest server.
```

Example 2:

```text
Input: k = 3, arrival = [1,2,3,4], load = [1,2,1,2]
Output: [0]
Explanation: 
The first 3 requests are handled by first 3 servers.
Request 3 comes in. It is handled by server 0 since the server is available.
Server 0 handled two requests, while servers 1 and 2 handled one request each. Hence server 0 is the busiest server.
```

Example 3:

```text
Input: k = 3, arrival = [1,2,3], load = [10,12,11]
Output: [0,1,2]
Explanation: Each server handles a single request, so they are all considered the busiest.
```

__Constraints:__

- `1 <= k <= 10 ^ 5`
- `1 <= arrival.length, load.length <= 10 ^ 5`
- `arrival.length == load.length`
- `1 <= arrival[i], load[i] <= 10 ^ 9`
- `arrival` is __strictly increasing__.

__题目描述:__

你有 `k` 个服务器，编号为 `0` 到 `k-1` ，它们可以同时处理多个请求组。每个服务器有无穷的计算能力但是 __不能同时处理超过一个请求__ 。请求分配到服务器的规则如下:

- 第 `i` （序号从 0 开始）个请求到达。
- 如果所有服务器都已被占据，那么该请求被舍弃（完全不处理）。
- 如果第 `(i % k)` 个服务器空闲，那么对应服务器会处理该请求。
- 否则，将请求安排给下一个空闲的服务器（服务器构成一个环，必要的话可能从第 0 个服务器开始继续找下一个空闲的服务器）。比方说，如果第 `i` 个服务器在忙，那么会查看第 `(i+1)` 个服务器，第 `(i+2)` 个服务器等等。

给你一个 __严格递增__ 的正整数数组 `arrival` ，表示第 `i` 个任务的到达时间，和另一个数组 `load` ，其中 `load[i]` 表示第 `i` 个请求的工作量（也就是服务器完成它所需要的时间）。你的任务是找到 __最繁忙的服务器__ 。最繁忙定义为一个服务器处理的请求数是所有服务器里最多的。

请你返回包含所有 最繁忙服务器 序号的列表，你可以以任意顺序返回这个列表。

__示例:__

示例 1：

![1606-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/03/load-1.png)

```text
输入：k = 3, arrival = [1,2,3,4,5], load = [5,2,3,3,3] 
输出：[1] 
解释：
所有服务器一开始都是空闲的。
前 3 个请求分别由前 3 台服务器依次处理。
请求 3 进来的时候，服务器 0 被占据，所以它被安排到下一台空闲的服务器，也就是服务器 1 。
请求 4 进来的时候，由于所有服务器都被占据，该请求被舍弃。
服务器 0 和 2 分别都处理了一个请求，服务器 1 处理了两个请求。所以服务器 1 是最忙的服务器。
```

示例 2：

```text
输入：k = 3, arrival = [1,2,3,4], load = [1,2,1,2]
输出：[0]
解释：
前 3 个请求分别被前 3 个服务器处理。
请求 3 进来，由于服务器 0 空闲，它被服务器 0 处理。
服务器 0 处理了两个请求，服务器 1 和 2 分别处理了一个请求。所以服务器 0 是最忙的服务器。
```

示例 3：

```text
输入：k = 3, arrival = [1,2,3], load = [10,12,11]
输出：[0,1,2]
解释：每个服务器分别处理了一个请求，所以它们都是最忙的服务器。
```

示例 4：

```text
输入：k = 3, arrival = [1,2,3,4,8,9,10], load = [5,2,10,3,1,2,2]
输出：[1]
```

示例 5：

```text
输入：k = 1, arrival = [1], load = [1]
输出：[0]
```

__提示：__

- `1 <= k <= 10 ^ 5`
- `1 <= arrival.length, load.length <= 10 ^ 5`
- `arrival.length == load.length`
- `1 <= arrival[i], load[i] <= 10 ^ 9`
- `arrival` 保证 __严格递增__ 。

__思路:__

```text
模拟 ➕ 优先队列 ➕ 有序集合
优先队列里按时间顺序放最早结束的服务器
有序集合中放可用的服务器
遍历请求, 将所有完成的服务器从优先队列中弹出
如果没有空闲服务器, 忽略这个请求
找到第一个不小于 i % k 的服务器
如果找到了可用服务器集合结尾, 跳到集合开头
给服务器计数
并将该服务器从可用服务器集合中移除, 将任务加入优先队列中
最后统计使用最多的服务器加入结果中
时间复杂度为 O((K + N)logK), 空间复杂度为 O(K)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> busiestServers(int k, vector<int>& arrival, vector<int>& load) 
    {
        set<int> available;
        for (int i = 0; i < k; i++) available.insert(i);
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> busy;
        vector<int> result, requests(k);
        for (int i = 0, n = arrival.size(); i < n; i++) 
        {
            while (!busy.empty() and busy.top().first <= arrival[i])
            {
                available.insert(busy.top().second);
                busy.pop();
            }
            if (available.empty()) continue;
            auto pos = available.lower_bound(i % k);
            if (pos == available.end()) pos = available.begin();
            ++requests[*pos];
            busy.push(make_pair(arrival[i] + load[i], *pos));
            available.erase(*pos);
        }
        int max_request = *max_element(requests.begin(), requests.end());
        for (int i = 0; i < k; i++) if (requests[i] == max_request) result.emplace_back(i);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> busiestServers(int k, int[] arrival, int[] load) {
        TreeSet<Integer> available = new TreeSet<Integer>();
        for (int i = 0; i < k; i++) available.add(i);
        PriorityQueue<int[]> busy = new PriorityQueue<int[]>((a, b) -> a[0] - b[0]);
        int[] requests = new int[k];
        for (int i = 0, n = arrival.length; i < n; i++) {
            while (!busy.isEmpty() && busy.peek()[0] <= arrival[i]) available.add(busy.poll()[1]);
            if (available.isEmpty()) continue;
            Integer pos = available.ceiling(i % k);
            if (pos == null) pos = available.first();
            ++requests[pos];
            busy.offer(new int[]{arrival[i] + load[i], pos});
            available.remove(pos);
        }
        int maxRequest = Arrays.stream(requests).max().getAsInt();
        List<Integer> result = new ArrayList<Integer>();
        for (int i = 0; i < k; i++) if (requests[i] == maxRequest) result.add(i);
        return result;
    }
}
```

__Python__:

```Python
from sortedcontainers import SortedList

class Solution:
    def busiestServers(self, k: int, arrival: List[int], load: List[int]) -> List[int]:
        available, busy, requests = SortedList(range(k)), [], [0] * k
        for i, (start, t) in enumerate(zip(arrival, load)):
            while busy and busy[0][0] <= start:
                available.add(busy[0][1])
                heappop(busy)
            if len(available) == 0:
                continue
            pos = available.bisect_left(i % k)
            if (pos := available.bisect_left(i % k)) == len(available):
                pos = 0
            used = available[pos]
            requests[used] += 1
            heappush(busy, (start + t, used))
            available.remove(used)
        max_request = max(requests)
        return [i for i, req in enumerate(requests) if req == max_request]
```
