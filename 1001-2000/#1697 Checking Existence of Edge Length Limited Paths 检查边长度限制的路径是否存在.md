# 1697 Checking Existence of Edge Length Limited Paths 检查边长度限制的路径是否存在

__Description:__

An undirected graph of `n` nodes is defined by `edgeList`, where `edgeList[i] = [ui, vi, disi]` denotes an edge between nodes `ui` and `vi` with distance `disi`. Note that there may be __multiple__ edges between two nodes.

Given an array `queries`, where `queries[j] = [pj, qj, limitj]`, your task is to determine for each `queries[j]` whether there is a path between `pj` and `qj` such that each edge on the path has a distance __strictly less than__ `limitj` .

Return _a __boolean array___ `answer`_, where_ `answer.length == queries.length` _and the_ `j ^ th` _value of_ `answer` _is_ `true` _if there is a path for_ `queries[j]` _is_ `true`_, and_ `false` _otherwise_.

__Example:__

Example 1:

![1697-1](https://assets.leetcode.com/uploads/2020/12/08/h.png)

```text
Input: n = 3, edgeList = [[0,1,2],[1,2,4],[2,0,8],[1,0,16]], queries = [[0,1,2],[0,2,5]]
Output: [false,true]
Explanation: The above figure shows the given graph. Note that there are two overlapping edges between 0 and 1 with distances 2 and 16.
For the first query, between 0 and 1 there is no path where each distance is less than 2, thus we return false for this query.
For the second query, there is a path (0 -> 1 -> 2) of two edges with distances less than 5, thus we return true for this query.
```

Example 2:

![1697-2](https://assets.leetcode.com/uploads/2020/12/08/q.png)

```text
Input: n = 5, edgeList = [[0,1,10],[1,2,5],[2,3,9],[3,4,13]], queries = [[0,4,14],[1,4,13]]
Output: [true,false]
Explanation: The above figure shows the given graph.
```

__Constraints:__

- `2 <= n <= 10 ^ 5`
- `1 <= edgeList.length, queries.length <= 10 ^ 5`
- `edgeList[i].length == 3`
- `queries[j].length == 3`
- `0 <= ui, vi, pj, qj <= n - 1`
- `ui != vi`
- `pj != qj`
- `1 <= disi, limitj <= 10 ^ 9`
- There may be __multiple__ edges between two nodes.

__题目描述:__

给你一个 `n` 个点组成的无向图边集 `edgeList` ，其中 `edgeList[i] = [ui, vi, disi]` 表示点 `ui` 和点 `vi` 之间有一条长度为 `disi` 的边。请注意，两个点之间可能有 __超过一条边__ 。

给你一个查询数组 `queries` ，其中 `queries[j] = [pj, qj, limitj]` ，你的任务是对于每个查询 `queries[j]` ，判断是否存在从 `pj` 到 `qj` 的路径，且这条路径上的每一条边都 __严格小于__ `limitj` 。

请你返回一个 _布尔数组_  `answer` ，其中 `answer.length == queries.length` ，当 `queries[j]` 的查询结果为 `true` 时， `answer` 第 `j` 个值为 `true` ，否则为 `false` 。

__示例:__

示例 1：

![1697-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/12/19/h.png)

```text
输入：n = 3, edgeList = [[0,1,2],[1,2,4],[2,0,8],[1,0,16]], queries = [[0,1,2],[0,2,5]]
输出：[false,true]
解释：上图为给定的输入数据。注意到 0 和 1 之间有两条重边，分别为 2 和 16 。
对于第一个查询，0 和 1 之间没有小于 2 的边，所以我们返回 false 。
对于第二个查询，有一条路径（0 -> 1 -> 2）两条边都小于 5 ，所以这个查询我们返回 true 。
```

示例 2：

![1697-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/12/19/q.png)

```text
输入：n = 5, edgeList = [[0,1,10],[1,2,5],[2,3,9],[3,4,13]], queries = [[0,4,14],[1,4,13]]
输出：[true,false]
解释：上图为给定数据。
```

__提示：__

- `2 <= n <= 10 ^ 5`
- `1 <= edgeList.length, queries.length <= 10 ^ 5`
- `edgeList[i].length == 3`
- `queries[j].length == 3`
- `0 <= ui, vi, pj, qj <= n - 1`
- `ui != vi`
- `pj != qj`
- `1 <= disi, limitj <= 10 ^ 9`
- 两个点之间可能有 __多条__ 边。

__思路:__

```text
并查集
将所有连通的点加入并查集
为了保证查询的时候当前的权值足够小, 可以先将边按照权值大小进行排序
然后将查询的边按照权值大小进行排序
如果权值比当前边的权值还大, 没必要将两个点进行合并
否则将两个点进行合并
检查两个点是否连通即可
时间复杂度为 O(N + ElogE + MlogM + (E + M)logN), 空间复杂度为 O(N + M + logE), 其中 E 为 edgeList 的长度, M 为 queries 的长度, 分别需要对 edgeList 和 queries 进行排序, 并查集的空间复杂度为 O(N), 保存下标排序的空间复杂度为 O(M)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<bool> distanceLimitedPathsExist(int n, vector<vector<int>>& edgeList, vector<vector<int>>& queries) 
    {
        sort(edgeList.begin(), edgeList.end(), [](const auto& e1, const auto& e2) { return e1.back() < e2.back(); });
        int k = 0, m = queries.size(), e = edgeList.size();
        vector<int> idx(m), uf(n);
        vector<bool> result(m);
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&](int i1, int i2) { return queries[i1].back() < queries[i2].back(); });
        iota(uf.begin(), uf.end(), 0);
        for (const auto& i : idx) 
        {
            while (k < e and edgeList[k].back() < queries[i].back()) merge(uf, edgeList[k][0], edgeList[k++][1]);
            result[i] = find(uf, queries[i][0]) == find(uf, queries[i][1]);
        }
        return result;
    }
private:
    int find(vector<int> &uf, int x) 
    {
        return uf[x] == x ? x : uf[x] = find(uf, uf[x]);
    }

    void merge(vector<int> &uf, int x, int y) 
    {
        uf[find(uf, x)] = find(uf, y);
    }
};
```

__Java__:

```Java
class Solution {
    public boolean[] distanceLimitedPathsExist(int n, int[][] edgeList, int[][] queries) {
        Arrays.sort(edgeList, (a, b) -> a[2] - b[2]);
        Integer[] index = new Integer[queries.length];
        int k = 0, m = queries.length, e = edgeList.length, uf[] = new int[n];
        for (int i = 0; i < m; i++) index[i] = i;
        for (int i = 0; i < n; i++) uf[i] = i;
        Arrays.sort(index, (a, b) -> queries[a][2] - queries[b][2]);
        boolean[] result = new boolean[m];
        for (int i : index) {
            while (k < e && edgeList[k][2] < queries[i][2]) union(uf, edgeList[k][0], edgeList[k++][1]);
            result[i] = find(uf, queries[i][0]) == find(uf, queries[i][1]);
        }
        return result;
    }

    private int find(int[] uf, int x) {
        return uf[x] == x ? x : (uf[x] = find(uf, uf[x]));
    }

    private void union(int[] uf, int x, int y) {
        uf[find(uf, x)] = find(uf, y);
    }
}
```

__Python__:

```Python
class UF:
    def __init__(self, n: int) -> None:
        self.parent = [i for i in range(n)]
        self.weight = [1] * n
        self.count = n

    def union(self, p: int, q: int) -> None:
        """
        连接两个点
        :param p: 一个节点
        :param q: 另一个节点
        :return:
        """
        p = self.find(p)
        q = self.find(q)
        if p == q:
            return
        if self.weight[p] > self.weight[q]:
            self.parent[q] = p
            self.weight[p] += self.weight[q]
        else:
            self.parent[p] = q
            self.weight[q] += self.weight[p]
        self.count -= 1

    def connected(self, p: int, q: int) -> bool:
        """
        检查两个点是否在同一分量
        :param p: 一个节点
        :param q: 另一个节点
        :return: 返回两个点是否在同一个分量
        """
        return self.find(p) == self.find(q)

    def find(self, p: int) -> int:
        """
        查找根节点, 并进行路径压缩
        :param p: 一个节点
        :return: 根节点
        """
        while self.parent[p] != p:
            self.parent[p] = self.parent[self.parent[p]]
            p = self.parent[p]
        return p

class Solution:
    def distanceLimitedPathsExist(self, n: int, edgeList: List[List[int]], queries: List[List[int]]) -> List[bool]:
        edgeList.sort(key=lambda e: e[2])
        uf, result, k = UF(n), [False] * len(queries), 0
        for i, (p, q, limit) in sorted(enumerate(queries), key=lambda p: p[1][2]):
            while k < len(edgeList) and edgeList[k][2] < limit:
                uf.union(edgeList[k][0], edgeList[k][1])
                k += 1
            result[i] = uf.find(p) == uf.find(q)
        return result
```
