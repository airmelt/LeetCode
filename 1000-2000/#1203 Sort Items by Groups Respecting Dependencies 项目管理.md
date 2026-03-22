# 1203 Sort Items by Groups Respecting Dependencies 项目管理

__Description__:
There are n items each belonging to zero or one of m groups where group[i] is the group that the i-th item belongs to and it's equal to -1 if the i-th item belongs to no group. The items and the groups are zero indexed. A group can have no item belonging to it.

Return a sorted list of the items such that:

The items that belong to the same group are next to each other in the sorted list.
There are some relations between these items where beforeItems[i] is a list containing all the items that should come before the i-th item in the sorted array (to the left of the i-th item).
Return any solution if there is more than one solution and return an empty list if there is no solution.

__Example:__

Example 1:

![Groups](https://assets.leetcode.com/uploads/2019/09/11/1359_ex1.png)

Input: n = 8, m = 2, group = [-1,-1,1,0,0,1,0,-1], beforeItems = [[],[6],[5],[6],[3,6],[],[],[]]
Output: [6,3,4,1,5,2,0,7]

Example 2:

Input: n = 8, m = 2, group = [-1,-1,1,0,0,1,0,-1], beforeItems = [[],[6],[5],[6],[3],[],[4],[]]
Output: []
Explanation: This is the same as example 1 except that 4 needs to be before 6 in the sorted list.

__Constraints:__

1 <= m <= n <= 3 * 10^4
group.length == beforeItems.length == n
-1 <= group[i] <= m - 1
0 <= beforeItems[i].length <= n - 1
0 <= beforeItems[i][j] <= n - 1
i != beforeItems[i][j]
beforeItems[i] does not contain duplicates elements.

__题目描述__:
有 n 个项目，每个项目或者不属于任何小组，或者属于 m 个小组之一。group[i] 表示第 i 个项目所属的小组，如果第 i 个项目不属于任何小组，则 group[i] 等于 -1。项目和小组都是从零开始编号的。可能存在小组不负责任何项目，即没有任何项目属于这个小组。

请你帮忙按要求安排这些项目的进度，并返回排序后的项目列表：

同一小组的项目，排序后在列表中彼此相邻。
项目之间存在一定的依赖关系，我们用一个列表 beforeItems 来表示，其中 beforeItems[i] 表示在进行第 i 个项目前（位于第 i 个项目左侧）应该完成的所有项目。
如果存在多个解决方案，只需要返回其中任意一个即可。如果没有合适的解决方案，就请返回一个 空列表 。

__示例 :__

示例 1：

![项目管理](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/09/22/1359_ex1.png)

输入：n = 8, m = 2, group = [-1,-1,1,0,0,1,0,-1], beforeItems = [[],[6],[5],[6],[3,6],[],[],[]]
输出：[6,3,4,1,5,2,0,7]

示例 2：

输入：n = 8, m = 2, group = [-1,-1,1,0,0,1,0,-1], beforeItems = [[],[6],[5],[6],[3],[],[4],[]]
输出：[]
解释：与示例 1 大致相同，但是在排序后的列表中，4 必须放在 6 的前面。

__提示:__

1 <= m <= n <= 3 * 10^4
group.length == beforeItems.length == n
-1 <= group[i] <= m - 1
0 <= beforeItems[i].length <= n - 1
0 <= beforeItems[i][j] <= n - 1
i != beforeItems[i][j]
beforeItems[i] 不含重复元素

__思路__:

拓扑排序
题意是需要将相同组的都邻接放在一起执行, 同时需要满足所有前置任务
-1 组的只需要满足前置任务先完成, 可以放在任意其他位置
需要进行两次拓扑排序
首先将 -1 的组都分别放到虚拟组中, 组号从 m 开始递增, 这样就将所有任务分成了 max_group_id 个组
先对组进行拓扑排序, 再对组内项目进行拓扑排序
时间复杂度为 O(m + n ^ 2), 空间复杂度为 O(m + n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> sortItems(int n, int m, vector<int>& group, vector<vector<int>>& beforeItems) 
    {
        int max_group_id = m;
        unordered_map<int, unordered_set<int>> group_map;
        for (int i = 0; i < n; i++)
        {
            if (group[i] == -1) group[i] = max_group_id++;
            group_map[group[i]].insert(i);
        }
        vector<int> group_in_degree(max_group_id), project_in_degree(n), group_top, result;
        vector<vector<int>> group_neighbors(max_group_id), project_neighbors(n);
        for (int i = 0; i < n; i++)
        {
            for (const auto& item : beforeItems[i])
            {
                ++project_in_degree[i];
                project_neighbors[item].emplace_back(i);
                if (group[i] == group[item]) continue;
                ++group_in_degree[group[i]];
                group_neighbors[group[item]].emplace_back(group[i]);
            }
        }
        queue<int> q_group;
        for (int i = 0; i < max_group_id; i++) if (!group_in_degree[i]) q_group.push(i);
        while (!q_group.empty())
        {
            int cur = q_group.front();
            q_group.pop();
            group_top.emplace_back(cur);
            for (const auto& item : group_neighbors[cur])
            {
                --group_in_degree[item];
                if (!group_in_degree[item]) q_group.push(item);
            }
        }
        if (group_top.size() != max_group_id) return {};
        for (int i = 0; i < max_group_id; i++)
        {
            queue<int> q;
            vector<int> temp;
            if (group_map[group_top[i]].empty()) continue;
            const auto& cur = group_map[group_top[i]];
            for (const auto& item : cur) if (!project_in_degree[item]) q.push(item);
            while (!q.empty())
            {
                int c = q.front();
                q.pop();
                temp.emplace_back(c);
                for (const auto& d : project_neighbors[c])
                {
                    --project_in_degree[d];
                    if (!project_in_degree[d] && cur.count(d)) q.push(d);
                }
            }
            if (temp.size() != cur.size()) return {};
            result.insert(result.end(), temp.begin(), temp.end());
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] sortItems(int n, int m, int[] group, List<List<Integer>> beforeItems) {
        int maxGroupId = m;
        Map<Integer, Set<Integer>> map = new HashMap<>();
        for (int i = 0; i < n; i++) {
            if (group[i] == -1) group[i] = maxGroupId++;
            if (!map.containsKey(group[i])) map.put(group[i], new HashSet<>());
            map.get(group[i]).add(i);
        }
        int[] groupInDegree = new int[maxGroupId], projectInDegree = new int[n];
        List<Integer>[] groupNeighbors = new ArrayList[maxGroupId], projectNeighbors = new ArrayList[n];
        for (int i = 0; i < n; i++) {
            for (int item : beforeItems.get(i)) {
                ++projectInDegree[i];
                if (projectNeighbors[item] == null) projectNeighbors[item] = new ArrayList<>();
                projectNeighbors[item].add(i);
                if (group[i] == group[item]) continue;
                ++groupInDegree[group[i]];
                if (groupNeighbors[group[item]] == null) groupNeighbors[group[item]] = new ArrayList<>();
                groupNeighbors[group[item]].add(group[i]);
            }
        }
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < maxGroupId; i++) if (groupInDegree[i] == 0) queue.offer(i);
        List<Integer> top = new ArrayList<>();
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            top.add(cur);
            if (groupNeighbors[cur] == null) continue;
            for (int item : groupNeighbors[cur]) {
                --groupInDegree[item];
                if (groupInDegree[item] == 0) queue.offer(item);
            }
        }
        if (top.size() != maxGroupId) return new int[0];
        List<Integer> result = new ArrayList<>();
        for (int i = 0; i < top.size(); i++) {
            Queue<Integer> q = new LinkedList<>();
            List<Integer> temp = new ArrayList<>();
            if (!map.containsKey(top.get(i))) continue;
            for (int item : map.get(top.get(i))) if (projectInDegree[item] == 0) q.offer(item);
            while (!q.isEmpty()) {
                int c = q.poll();
                temp.add(c);
                if (projectNeighbors[c] == null) continue;
                for (int d : projectNeighbors[c]) {
                    --projectInDegree[d];
                    if (projectInDegree[d] == 0 && map.get(top.get(i)).contains(d)) q.offer(d);
                }
            }
            if (temp.size() != map.get(top.get(i)).size()) return new int[0];
            result.addAll(temp);
        }
        return result.stream().mapToInt(i -> i).toArray();
    }
}
```

__Python__:

```Python
class Solution:
    def sortItems(self, n: int, m: int, group: List[int], pres: List[List[int]]) -> List[int]:
        max_group_id, project_indegree, group_indegree, project_neighbors, group_neighbors, group_projects, result = m, defaultdict(int), defaultdict(int), defaultdict(list), defaultdict(list), defaultdict(list), []
        for project in range(n):
            if group[project] == -1:
                group[project] = max_group_id
                max_group_id += 1
        for project in range(n):
            group_projects[group[project]].append(project)
            for pre in pres[project]:
                if group[pre] != group[project]:
                    group_indegree[group[project]] += 1
                    group_neighbors[group[pre]].append(group[project])
                else:
                    project_indegree[project] += 1
                    project_neighbors[pre].append(project)
                    
        def top_sort (items: List[int], indegree: Dict[int, int], neighbors: Dict[int, List[int]]) -> List[int]:
            q, result = collections.deque([item for item in items if not indegree[item]]), []
            while q:
                result.append(cur := q.popleft())
                for neighbor in neighbors[cur]:
                    indegree[neighbor] -= 1
                    if not indegree[neighbor]:
                        q.append(neighbor)
            return result
        group_queue = top_sort([i for i in range(max_group_id)], group_indegree, group_neighbors)
        if len(group_queue) != max_group_id:
            return result
        for group_id in group_queue:
            project_queue = top_sort(group_projects[group_id], project_indegree, project_neighbors)
            if len(project_queue) != len(group_projects[group_id]):
                return []
            result += project_queue
        return result
```
