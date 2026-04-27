# 2039 The Time When the Network Becomes Idle 网络空闲的时刻

__Description:__

There is a network of `n` servers, labeled from `0` to `n - 1`. You are given a 2D integer array `edges`, where `edges[i] = [ui, vi]` indicates there is a message channel between servers `ui` and `vi`, and they can pass __any__ number of messages to __each other__ directly in __one__ second. You are also given a __0-indexed__ integer array `patience` of length `n`.

All servers are connected, i.e., a message can be passed from one server to any other server(s) directly or indirectly through the message channels.

The server labeled `0` is the __master__ server. The rest are __data__ servers. Each data server needs to send its message to the master server for processing and wait for a reply. Messages move between servers __optimally__, so every message takes the __least amount of time__ to arrive at the master server. The master server will process all newly arrived messages __instantly__ and send a reply to the originating server via the __reversed path__ the message had gone through.

At the beginning of second `0`, each data server sends its message to be processed. Starting from second `1`, at the __beginning__ of __every__ second, each data server will check if it has received a reply to the message it sent (including any newly arrived replies) from the master server:

- If it has not, it will __resend__ the message periodically. The data server `i` will resend the message every `patience[i]` second(s), i.e., the data server `i` will resend the message if `patience[i]` second(s) have __elapsed__ since the __last__ time the message was sent from this server.
- Otherwise, __no more resending__ will occur from this server.

The network becomes idle when there are no messages passing between servers or arriving at servers.

Return the earliest second starting from which the network becomes idle.

__Example:__

Example 1:

![2039-1](https://assets.leetcode.com/uploads/2021/09/22/quiet-place-example1.png)

```text
Input: edges = [[0,1],[1,2]], patience = [0,2,1]
Output: 8
Explanation:
```

At (the beginning of) second 0,

- Data server 1 sends its message (denoted 1A) to the master server.
- Data server 2 sends its message (denoted 2A) to the master server.

At second 1,

- Message 1A arrives at the master server. Master server processes message 1A instantly and sends a reply 1A back.
- Server 1 has not received any reply. 1 second (1 < patience[1] = 2) elapsed since this server has sent the message, therefore it does not resend the message.
- Server 2 has not received any reply. 1 second (1 == patience[2] = 1) elapsed since this server has sent the message, therefore it resends the message (denoted 2B).

At second 2,

- The reply 1A arrives at server 1. No more resending will occur from server 1.
- Message 2A arrives at the master server. Master server processes message 2A instantly and sends a reply 2A back.
- Server 2 resends the message (denoted 2C).

...
At second 4,

- The reply 2A arrives at server 2. No more resending will occur from server 2.
...

At second 7, reply 2D arrives at server 2.

Starting from the beginning of the second 8, there are no messages passing between servers or arriving at servers.
This is the time when the network becomes idle.

Example 2:

![2039-2](https://assets.leetcode.com/uploads/2021/09/04/network_a_quiet_place_2.png)

```text
Input: edges = [[0,1],[0,2],[1,2]], patience = [0,10,10]
Output: 3
Explanation: Data servers 1 and 2 receive a reply back at the beginning of second 2.
From the beginning of the second 3, the network becomes idle.
```

__Constraints:__

- `n == patience.length`
- `2 <= n <= 10 ^ 5`
- `patience[0] == 0`
- `1 <= patience[i] <= 10 ^ 5` for `1 <= i < n`
- `1 <= edges.length <= min(10 ^ 5, n * (n - 1) / 2)`
- `edges[i].length == 2`
- `0 <= ui, vi < n`
- `ui != vi`
- There are no duplicate edges.
- Each server can directly or indirectly reach another server.

__题目描述:__

给你一个有 `n` 个服务器的计算机网络，服务器编号为 `0` 到 `n - 1` 。同时给你一个二维整数数组 `edges` ，其中 `edges[i] = [ui, vi]` 表示服务器 `ui` 和 `vi` 之间有一条信息线路，在 __一秒__ 内它们之间可以传输 __任意__ 数目的信息。再给你一个长度为 `n` 且下标从 __0__ 开始的整数数组 `patience` 。

题目保证所有服务器都是 相通 的，也就是说一个信息从任意服务器出发，都可以通过这些信息线路直接或间接地到达任何其他服务器。

编号为 `0` 的服务器是 __主__ 服务器，其他服务器为 __数据__ 服务器。每个数据服务器都要向主服务器发送信息，并等待回复。信息在服务器之间按 __最优__ 线路传输，也就是说每个信息都会以 __最少时间__ 到达主服务器。主服务器会处理 __所有__ 新到达的信息并 __立即__ 按照每条信息来时的路线 __反方向__ 发送回复信息。

在 `0` 秒的开始，所有数据服务器都会发送各自需要处理的信息。从第 `1` 秒开始，__每__ 一秒最 __开始__ 时，每个数据服务器都会检查它是否收到了主服务器的回复信息（包括新发出信息的回复信息）:

- 如果还没收到任何回复信息，那么该服务器会周期性 __重发__ 信息。数据服务器 `i` 每 `patience[i]` 秒都会重发一条信息，也就是说，数据服务器 `i` 在上一次发送信息给主服务器后的 `patience[i]` 秒 __后__ 会重发一条信息给主服务器。
- 否则，该数据服务器 __不会重发__ 信息。

当没有任何信息在线路上传输或者到达某服务器时，该计算机网络变为 空闲 状态。

请返回计算机网络变为 空闲 状态的 最早秒数 。

__示例:__

示例 1：

![2039-3](https://assets.leetcode.com/uploads/2021/09/22/quiet-place-example1.png)

```text
输入：edges = [[0,1],[1,2]], patience = [0,2,1]
输出：8
解释：
```

0 秒最开始时，

- 数据服务器 1 给主服务器发出信息（用 1A 表示）。
- 数据服务器 2 给主服务器发出信息（用 2A 表示）。

1 秒时，

- 信息 1A 到达主服务器，主服务器立刻处理信息 1A 并发出 1A 的回复信息。
- 数据服务器 1 还没收到任何回复。距离上次发出信息过去了 1 秒（1 < patience[1] = 2），所以不会重发信息。
- 数据服务器 2 还没收到任何回复。距离上次发出信息过去了 1 秒（1 == patience[2] = 1），所以它重发一条信息（用 2B 表示）。

2 秒时，

- 回复信息 1A 到达服务器 1 ，服务器 1 不会再重发信息。
- 信息 2A 到达主服务器，主服务器立刻处理信息 2A 并发出 2A 的回复信息。
- 服务器 2 重发一条信息（用 2C 表示）。

...

4 秒时，

- 回复信息 2A 到达服务器 2 ，服务器 2 不会再重发信息。

...
7 秒时，回复信息 2D 到达服务器 2 。

从第 8 秒开始，不再有任何信息在服务器之间传输，也不再有信息到达服务器。
所以第 8 秒是网络变空闲的最早时刻。

示例 2：

![2039-4](https://assets.leetcode.com/uploads/2021/09/04/network_a_quiet_place_2.png)

```text
输入：edges = [[0,1],[0,2],[1,2]], patience = [0,10,10]
输出：3
解释：数据服务器 1 和 2 第 2 秒初收到回复信息。
从第 3 秒开始，网络变空闲。
```

__提示：__

- `n == patience.length`
- `2 <= n <= 10 ^ 5`
- `patience[0] == 0`
- 对于 `1 <= i < n` ，满足 `1 <= patience[i] <= 10 ^ 5`
- `1 <= edges.length <= min(10 ^ 5, n * (n - 1) / 2)`
- `edges[i].length == 2`
- `0 <= ui, vi < n`
- `ui != vi`
- 不会有重边。
- 每个服务器都直接或间接与别的服务器相连。

__思路:__

```text
BFS
用 BFS 求出各点到 0 点到距离
由于各个信息之间互不干涉, 各点往返时间为 2 * dis[i]
第一次收到回复的时间为 (2 * dis[i] - 1) / patience[i] * patience[i] + 1
最终答案为各个信息第一次收到回复的时间的最大值
时间复杂度为 O(N + M), 空间复杂度为 O(N + M), 其中 N 为服务器数量, M 为边的数量
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int networkBecomesIdle(vector<vector<int>>& edges, vector<int>& patience) 
    {
        int n = patience.size(), result = 0;
        vector<vector<int>> graph(n);
        vector<int> dis(n, -1);
        dis.front() = 0;
        for (const auto& e : edges) 
        {
            graph[e.front()].emplace_back(e.back());
            graph[e.back()].emplace_back(e.front());
        }
        queue<int> q;
        q.push(0);
        while (!q.empty()) 
        {
            int cur = q.front();
            q.pop();
            for (const auto& nei : graph[cur]) 
            {
                if (dis[nei] == -1) 
                {
                    dis[nei] = dis[cur] + 1;
                    q.push(nei);
                }
            }
        }
        for (int i = 1; i < n; i++) result = max(result, (dis[i] << 1) + ((dis[i] << 1) - 1) / patience[i] * patience[i] + 1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int networkBecomesIdle(int[][] edges, int[] patience) {
        List<List<Integer>> graph = new ArrayList<>();
        int n = patience.length, result = 0, dis[] = new int[n];
        Arrays.fill(dis, -1);
        dis[0] = 0;
        for (int i = 0; i < n; i++) graph.add(new ArrayList<>());
        for (int[] e : edges) {
            graph.get(e[0]).add(e[1]);
            graph.get(e[1]).add(e[0]);
        }
        LinkedList<Integer> queue = new LinkedList<>();
        queue.offer(0);
        while (!queue.isEmpty()) {
            int cur = queue.pollFirst();
            for (int nei : graph.get(cur)) {
                if (dis[nei] == -1) {
                    dis[nei] = dis[cur] + 1;
                    queue.offer(nei);
                }
            }
        }
        for (int i = 1; i < n; i++) result = Math.max(result, (dis[i] << 1) + ((dis[i] << 1) - 1) / patience[i] * patience[i] + 1);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def networkBecomesIdle(self, edges: List[List[int]], patience: List[int]) -> int:
        graph, dis, q, result = defaultdict(set), [0] + [-1] * ((n := len(patience)) - 1), deque([0]), 0
        for u, v in edges:
            graph[u].add(v)
            graph[v].add(u)
        while q:
            cur = q.popleft()
            for nei in graph[cur]:
                if dis[nei] == -1:
                    dis[nei] = dis[cur] + 1
                    q.append(nei)
        for i in range(1, n):
            result = max(result, (l := dis[i] << 1) + ((l - 1) // patience[i] * patience[i]) + 1)
        return result
```
