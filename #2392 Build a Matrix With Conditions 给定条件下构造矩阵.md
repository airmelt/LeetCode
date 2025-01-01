# 2392 Build a Matrix With Conditions 给定条件下构造矩阵

__Description:__

You are given a __positive__ integer `k`. You are also given:

- a 2D integer array `rowConditions` of size `n` where `rowConditions[i] = [above_i, below_i]`, and
- a 2D integer array `colConditions` of size `m` where `colConditions[i] = [left_i, right_i]`.

The two arrays contain integers from `1` to `k`.

You have to build a `k x k` matrix that contains each of the numbers from `1` to `k` __exactly once__. The remaining cells should have the value `0`.

The matrix should also satisfy the following conditions:

- The number `above_i` should appear in a __row__ that is strictly __above__ the row at which the number `below_i` appears for all `i` from `0` to `n - 1`.
- The number `left_i` should appear in a __column__ that is strictly __left__ of the column at which the number `right_i` appears for all `i` from `0` to `m - 1`.

Return any matrix that satisfies the conditions. If no answer exists, return an empty matrix.

__Example:__

Example 1:

![2392-1](https://assets.leetcode.com/uploads/2022/07/06/gridosdrawio.png)

```text
Input: k = 3, rowConditions = [[1,2],[3,2]], colConditions = [[2,1],[3,2]]
Output: [[3,0,0],[0,0,1],[0,2,0]]
Explanation: The diagram above shows a valid example of a matrix that satisfies all the conditions.
The row conditions are the following:
```

- Number 1 is in row 1, and number 2 is in row 2, so 1 is above 2 in the matrix.
- Number 3 is in row 0, and number 2 is in row 2, so 3 is above 2 in the matrix.
The column conditions are the following:
- Number 2 is in column 1, and number 1 is in column 2, so 2 is left of 1 in the matrix.
- Number 3 is in column 0, and number 2 is in column 1, so 3 is left of 2 in the matrix.

Note that there may be multiple correct answers.

Example 2:

```text
Input: k = 3, rowConditions = [[1,2],[2,3],[3,1],[2,3]], colConditions = [[2,1]]
Output: []
Explanation: From the first two conditions, 3 has to be below 1 but the third conditions needs 3 to be above 1 to be satisfied.
No matrix can satisfy all the conditions, so we return the empty matrix.
```

__Constraints:__

- `2 <= k <= 400`
- `1 <= rowConditions.length, colConditions.length <= 10 ^ 4`
- `rowConditions[i].length == colConditions[i].length == 2`
- `1 <= above_i, below_i, left_i, right_i <= k`
- `above_i != below_i`
- `left_i != right_i`

__题目描述:__

给你一个 __正__ 整数 `k` ，同时给你:

- 一个大小为 `n` 的二维整数数组 `rowConditions` ，其中 `rowConditions[i] = [above_i, below_i]` 和
- 一个大小为 `m` 的二维整数数组 `colConditions` ，其中 `colConditions[i] = [left_i, right_i]` 。

两个数组里的整数都是 `1` 到 `k` 之间的数字。

你需要构造一个 `k x k` 的矩阵， `1` 到 `k` 每个数字需要 __恰好出现一次__ 。剩余的数字都是 `0` 。

矩阵还需要满足以下条件：

- 对于所有 `0` 到 `n - 1` 之间的下标 `i` ，数字 `above_i` 所在的 __行__ 必须在数字 `below_i` 所在行的上面。
- 对于所有 `0` 到 `m - 1` 之间的下标 `i` ，数字 `left_i` 所在的 _列_ 必须在数字 `right_i` 所在列的左边。

返回满足上述要求的 任意 矩阵。如果不存在答案，返回一个空的矩阵。

__示例:__

示例 1：

![2392-2](https://assets.leetcode.com/uploads/2022/07/06/gridosdrawio.png)

```text
输入：k = 3, rowConditions = [[1,2],[3,2]], colConditions = [[2,1],[3,2]]
输出：[[3,0,0],[0,0,1],[0,2,0]]
解释：上图为一个符合所有条件的矩阵。
行要求如下：
```

- 数字 1 在第 1 行，数字 2 在第 2 行，1 在 2 的上面。
- 数字 3 在第 0 行，数字 2 在第 2 行，3 在 2 的上面。
列要求如下：
- 数字 2 在第 1 列，数字 1 在第 2 列，2 在 1 的左边。
- 数字 3 在第 0 列，数字 2 在第 1 列，3 在 2 的左边。

注意，可能有多种正确的答案。

示例 2：

```text
输入：k = 3, rowConditions = [[1,2],[2,3],[3,1],[2,3]], colConditions = [[2,1]]
输出：[]
解释：由前两个条件可以得到 3 在 1 的下面，但第三个条件是 3 在 1 的上面。
没有符合条件的矩阵存在，所以我们返回空矩阵。
```

__提示：__

- `2 <= k <= 400`
- `1 <= rowConditions.length, colConditions.length <= 10 ^ 4`
- `rowConditions[i].length == colConditions[i].length == 2`
- `1 <= above_i, below_i, left_i, right_i <= k`
- `above_i != below_i`
- `left_i != right_i`

__思路:__

```text
拓扑排序
分别对行和列进行拓扑排序
从 above 到 below, 从 left 到 right 连接一条有向边
如果行或列的拓扑排序结果长度小于 k, 则返回空矩阵, 此时有环
最后按照拓扑排序的结果构造矩阵
时间复杂度为 O(K ^ 2 + M + N), 空间复杂度为 O(M + N), 其中 M 和 N 分别为行和列的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> buildMatrix(int k, vector<vector<int>> &rowConditions, vector<vector<int>> &colConditions) 
    {
        vector<int> row = topo_sort(k, rowConditions), col = topo_sort(k, colConditions), pos(k);
        if (row.size() < k or col.size() < k) return {};
        for (int i = 0; i < k; i++) pos[col[i]] = i;
        vector<vector<int>> result(k, vector<int>(k));
        for (int i = 0; i < k; i++) result[i][pos[row[i]]] = row[i] + 1;
        return result;
    }
private:
    vector<int> topo_sort(int k, vector<vector<int>> &edges) 
    {
        vector<vector<int>> graph(k);
        vector<int> in_degree(k), order;
        for (const auto &edge : edges) 
        {
            graph[edge.front() - 1].push_back(edge.back() - 1);
            ++in_degree[edge.back() - 1];
        }
        queue<int> q;
        for (int i = 0; i < k; i++) if (!in_degree[i]) q.push(i);
        while (!q.empty()) 
        {
            int u = q.front();
            q.pop();
            order.push_back(u);
            for (int v : graph[u]) if (!(--in_degree[v])) q.push(v);
        }
        return order;
    }
};
```

__Java__:

```Java
class Solution {
    public int[][] buildMatrix(int k, int[][] rowConditions, int[][] colConditions) {
        int row[] = topoSort(k, rowConditions), col[] = topoSort(k, colConditions), result[][] = new int[k][k], pos[] = new int[k];
        if (row.length < k || col.length < k) return new int[][]{};
        for (int i = 0; i < k; i++) pos[col[i]] = i;
        for (int i = 0; i < k; i++) result[i][pos[row[i]]] = row[i] + 1;
        return result;
    }

    private int[] topoSort(int k, int[][] edges) {
        List<Integer>[] graph = new ArrayList[k];
        Arrays.setAll(graph, g -> new ArrayList<>());
        int inDegree[] = new int[k], u = 0;
        for (var edge : edges) {
            graph[edge[0] - 1].add(edge[1] - 1);
            ++inDegree[edge[1] - 1];
        }
        var order = new ArrayList<Integer>();
        var q = new ArrayDeque<Integer>();
        for (int i = 0; i < k; i++) if (inDegree[i] == 0) q.push(i);
        while (!q.isEmpty()) {
            order.add(u = q.pop());
            for (var v : graph[u]) if (--inDegree[v] == 0) q.push(v);
        }
        return order.stream().mapToInt(x -> x).toArray();
    }
}
```

__Python__:

```Python
class Solution:
    def buildMatrix(self, k: int, rowConditions: List[List[int]], colConditions: List[List[int]]) -> List[List[int]]:
        def topo_sort(edges: List[List[int]]) -> List[int]:
            graph, in_degree, order = [[] for _ in range(k)], [0] * k, []
            for u, v in edges:
                graph[u - 1].append(v - 1)
                in_degree[v - 1] += 1
            q = deque(i for i, d in enumerate(in_degree) if not d)
            while q:
                order.append(u := q.popleft())
                for v in graph[u]:
                    in_degree[v] -= 1
                    if not in_degree[v]:
                        q.append(v)
            return order if len(order) == k else None

        if (row := topo_sort(rowConditions)) is None or (col := topo_sort(colConditions)) is None:
            return []
        pos, result = {x: i for i, x in enumerate(col)}, [[0] * k for _ in range(k)]
        for i, x in enumerate(row):
            result[i][pos[x]] = x + 1
        return result
```
