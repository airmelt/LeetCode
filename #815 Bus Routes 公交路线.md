# 815 Bus Routes 公交路线

__Description__:
You are given an array routes representing bus routes where routes[i] is a bus route that the ith bus repeats forever.

For example, if routes[0] = [1, 5, 7], this means that the 0th bus travels in the sequence 1 -> 5 -> 7 -> 1 -> 5 -> 7 -> 1 -> ... forever.
You will start at the bus stop source (You are not on any bus initially), and you want to go to the bus stop target. You can travel between bus stops by buses only.

Return the least number of buses you must take to travel from source to target. Return -1 if it is not possible.

__Example:__

Example 1:

Input: routes = [[1,2,7],[3,6,7]], source = 1, target = 6
Output: 2
Explanation: The best strategy is take the first bus to the bus stop 7, then take the second bus to the bus stop 6.

Example 2:

Input: routes = [[7,12],[4,5,15],[6],[15,19],[9,12,13]], source = 15, target = 12
Output: -1

__Constraints:__

1 <= routes.length <= 500.
1 <= routes[i].length <= 10^5
All the values of routes[i] are unique.
sum(routes[i].length) <= 10^5
0 <= routes[i][j] < 10^6
0 <= source, target < 10^6

__题目描述__:
给你一个数组 routes ，表示一系列公交线路，其中每个 routes[i] 表示一条公交线路，第 i 辆公交车将会在上面循环行驶。

例如，路线 routes[0] = [1, 5, 7] 表示第 0 辆公交车会一直按序列 1 -> 5 -> 7 -> 1 -> 5 -> 7 -> 1 -> ... 这样的车站路线行驶。
现在从 source 车站出发（初始时不在公交车上），要前往 target 车站。 期间仅可乘坐公交车。

求出 最少乘坐的公交车数量 。如果不可能到达终点车站，返回 -1 。

__示例 :__

示例 1：

输入：routes = [[1,2,7],[3,6,7]], source = 1, target = 6
输出：2
解释：最优策略是先乘坐第一辆公交车到达车站 7 , 然后换乘第二辆公交车到车站 6 。

示例 2：

输入：routes = [[7,12],[4,5,15],[6],[15,19],[9,12,13]], source = 15, target = 12
输出：-1

__提示:__

1 <= routes.length <= 500.
1 <= routes[i].length <= 10^5
routes[i] 中的所有值 互不相同
sum(routes[i].length) <= 10^5
0 <= routes[i][j] < 10^6
0 <= source, target < 10^6

__思路__:

BFS
用一个哈希表记录每个站经过的公交车
经过的站不需要考虑
每次记录下经过的站和步数
时间复杂度为 O(mn + n ^ 2), 空间复杂度为 O(n ^ 2 + m), 其中 n 为公交车数量, m 为车站数量

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numBusesToDestination(vector<vector<int>>& routes, int source, int target) 
    {
        if (source == target) return 0;
        int n = routes.size();
        unordered_map<int, unordered_set<int>> m;
        for (int i = 0; i < n; i++) for (auto r : routes[i]) m[r].emplace(i);
        queue<pair<int, int>> q;
        q.push({ source, 1 });
        vector<bool> visited(n);
        while (!q.empty())
        {
            auto p = q.front();
            q.pop();
            auto cur = p.first, result = p.second;
            for (auto i : m[cur])
            {
                if (visited[i]) continue;
                for (int s : routes[i])
                {
                    if (s == target) return result;
                    q.push({ s, 1 + result });
                }
                visited[i] = true;
            }
        }
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int numBusesToDestination(int[][] routes, int source, int target) {
        if (source == target) return 0;
        int n = routes.length;
        Map<Integer, Set<Integer>> map = new HashMap<>();
        for (int i = 0; i < n; i++) {
            for (int r : routes[i]) {
                if (!map.containsKey(r)) map.put(r, new HashSet<Integer>());
                map.get(r).add(i);
            }
        }
        Queue<Integer[]> queue = new LinkedList<>();
        queue.offer(new Integer[]{source, 1});
        boolean visited[] = new boolean[n];
        while (!queue.isEmpty()) {
            Integer p[] = queue.poll();
            int cur = p[0], result = p[1];
            for (int i : map.get(cur)) {
                if (visited[i]) continue;
                for (int s : routes[i]) {
                    if (s == target) return result;
                    queue.offer(new Integer[]{s, result + 1});
                }
                visited[i] = true;
            }
        }
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def numBusesToDestination(self, routes: List[List[int]], source: int, target: int) -> int:
        if source == target:
            return 0
        visited, route_dict, queue = [False] * (n := len(routes)), defaultdict(set), [(source, 1)]
        for i, r in enumerate(routes):
            for s in r:
                route_dict[s].add(i)
        while queue:
            cur, result = queue.pop(0)
            for i in route_dict[cur]:
                if visited[i]:
                    continue
                for s in routes[i]:
                    if s == target:
                        return result
                    queue.append((s, result + 1))
                visited[i] = True
        return -1
```
