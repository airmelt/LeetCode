# 2097 Valid Arrangement of Pairs 合法重新排列数对

__Description:__

You are given a __0-indexed__ 2D integer array `pairs` where `pairs[i] = [starti, endi]`. An arrangement of `pairs` is __valid__ if for every index `i` where `1 <= i < pairs.length`, we have `endi-1 == starti`.

Return ___any__ valid arrangement of_ `pairs`.

__Note:__ The inputs will be generated such that there exists a valid arrangement of `pairs`.

__Example:__

Example 1:

```text
Input: pairs = [[5,1],[4,5],[11,9],[9,4]]
Output: [[11,9],[9,4],[4,5],[5,1]]
Explanation:
This is a valid arrangement since endi-1 always equals starti.
end0 = 9 == 9 = start1 
end1 = 4 == 4 = start2
end2 = 5 == 5 = start3
```

Example 2:

```text
Input: pairs = [[1,3],[3,2],[2,1]]
Output: [[1,3],[3,2],[2,1]]
Explanation:
This is a valid arrangement since endi-1 always equals starti.
end0 = 3 == 3 = start1
end1 = 2 == 2 = start2
The arrangements [[2,1],[1,3],[3,2]] and [[3,2],[2,1],[1,3]] are also valid.
```

Example 3:

```text
Input: pairs = [[1,2],[1,3],[2,1]]
Output: [[1,2],[2,1],[1,3]]
Explanation:
This is a valid arrangement since endi-1 always equals starti.
end0 = 2 == 2 = start1
end1 = 1 == 1 = start2
```

__Constraints:__

- `1 <= pairs.length <= 10 ^ 5`
- `pairs[i].length == 2`
- `0 <= starti, endi <= 10 ^ 9`
- `starti != endi`
- No two pairs are exactly the same.
- There __exists__ a valid arrangement of `pairs`.

__题目描述:__

给你一个下标从 __0__ 开始的二维整数数组 `pairs` ，其中 `pairs[i] = [starti, endi]` 。如果 `pairs` 的一个重新排列，满足对每一个下标 `i` （ `1 <= i < pairs.length` ）都有 `endi-1 == starti` ，那么我们就认为这个重新排列是 `pairs` 的一个 __合法重新排列__ 。

请你返回 __任意一个__ `pairs` 的合法重新排列。

_注意:_ 数据保证至少存在一个 `pairs` 的合法重新排列。

__示例:__

示例 1：

```text
输入：pairs = [[5,1],[4,5],[11,9],[9,4]]
输出：[[11,9],[9,4],[4,5],[5,1]]
解释：
输出的是一个合法重新排列，因为每一个 endi-1 都等于 starti 。
end0 = 9 == 9 = start1 
end1 = 4 == 4 = start2
end2 = 5 == 5 = start3
```

示例 2：

```text
输入：pairs = [[1,3],[3,2],[2,1]]
输出：[[1,3],[3,2],[2,1]]
解释：
输出的是一个合法重新排列，因为每一个 endi-1 都等于 starti 。
end0 = 3 == 3 = start1
end1 = 2 == 2 = start2
重新排列后的数组 [[2,1],[1,3],[3,2]] 和 [[3,2],[2,1],[1,3]] 都是合法的。
```

示例 3：

```text
输入：pairs = [[1,2],[1,3],[2,1]]
输出：[[1,2],[2,1],[1,3]]
解释：
输出的是一个合法重新排列，因为每一个 endi-1 都等于 starti 。
end0 = 2 == 2 = start1
end1 = 1 == 1 = start2
```

__提示：__

- `1 <= pairs.length <= 10 ^ 5`
- `pairs[i].length == 2`
- `0 <= starti, endi <= 10 ^ 9`
- `starti != endi`
- `pairs` 中不存在一模一样的数对。
- 至少 __存在__ 一个合法的 `pairs` 重新排列。

__思路:__

```text
欧拉回路
计算节点的出度和入度
入度比出度少 1 的节点就是起点, 否则可以任意选择起点
选择起点之后, 进行 DFS, 将遇到的节点从图中删除, 并将节点加入结果数组
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> validArrangement(vector<vector<int>>& pairs) 
    {
        unordered_map<int, vector<int>> graph;
        unordered_map<int, int> in_degree, out_degree;
        for (const auto& pair : pairs) 
        {
            int u = pair.front(), v = pair.back();
            graph[u].emplace_back(v);
            ++in_degree[v];
            ++out_degree[u];
        }
        int start = pairs.front().front();
        for (const auto& [u, v]: out_degree) 
        {
            if (v == in_degree[u] + 1) 
            {
                start = u;
                break;
            }
        }
        vector<vector<int>> result;
        function<void(int)> dfs = [&](int u) 
        {
            while (!graph[u].empty()) 
            {
                int v = graph[u].back();
                graph[u].pop_back();
                dfs(v);
                result.emplace_back(vector<int>{u, v});
            }
        };
        dfs(start);
        reverse(result.begin(), result.end());
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private Map<Integer, List<Integer>> graph = new HashMap<>();
    private List<Integer> path = new ArrayList<>();

    public int[][] validArrangement(int[][] pairs) {
        
        Map<Integer, Integer> inDegree = new HashMap<>(), outDegree = new HashMap<>();
        int n = pairs.length, start = pairs[0][0], result[][] = new int[n][2];
        for (int[] pair : pairs) {
            int u = pair[0], v = pair[1];
            inDegree.merge(v, 1, Integer::sum);
            outDegree.merge(u, 1, Integer::sum);
            graph.putIfAbsent(u, new ArrayList<>());
            graph.get(u).add(v);
        }
        for (int u : outDegree.keySet()) {
            if (outDegree.get(u) == inDegree.getOrDefault(u, 0) + 1) {
                start = u;
                break;
            }
        }
        dfs(start);
        for (int i = n; i > 0; i--) {
            result[n - i][0] = path.get(i);
            result[n - i][1] = path.get(i - 1);
        }
        return result;
    }

    private void dfs(int u) {
        List<Integer> list = graph.getOrDefault(u, new ArrayList());
        while (!list.isEmpty()) dfs(list.remove(list.size() - 1));
        path.add(u);
    }
}
```

__Python__:

```Python
class Solution:
    def validArrangement(self, pairs: List[List[int]]) -> List[List[int]]:
        def dfs(u: int) -> NoReturn:
            while graph[u]: 
                dfs(graph[u].pop())
            stack.append(u)
        graph, c, stack = defaultdict(list), Counter(), []
        for u, v in pairs:
            c[u] += 1
            c[v] -= 1
            graph[u].append(v)
        dfs(c.most_common(1)[0][0])
        return [[stack[i], stack[i - 1]] for i in range(len(stack) - 1, 0, -1)]
```
