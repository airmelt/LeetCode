# 1627 Graph Connectivity With Threshold 带阈值的图连通性

__Description:__

We have `n` cities labeled from `1` to `n`. Two different cities with labels `x` and `y` are directly connected by a bidirectional road if and only if `x` and `y` share a common divisor __strictly greater__ than some `threshold`. More formally, cities with labels `x` and `y` have a road between them if there exists an integer `z` such that all of the following are true:

- `x % z == 0`,
- `y % z == 0`, and
- `z > threshold`.

Given the two integers, `n` and `threshold`, and an array of `queries`, you must determine for each `queries[i] = [ai, bi]` if cities `ai` and `bi` are connected directly or indirectly. (i.e. there is some path between them).

Return _an array_ `answer`_, where_ `answer.length == queries.length` _and_ `answer[i]` _is_ `true` _if for the_ `i ^ th` _query, there is a path between_ `ai` _and_ `bi`_, or_ `answer[i]` _is_ `false` _if there is no path._

__Example:__

Example 1:

![1627-1](https://assets.leetcode.com/uploads/2020/10/09/ex1.jpg)

```text
Input: n = 6, threshold = 2, queries = [[1,4],[2,5],[3,6]]
Output: [false,false,true]
Explanation: The divisors for each number:
1:   1
2:   1, 2
3:   1, 3
4:   1, 2, 4
5:   1, 5
6:   1, 2, 3, 6
Using the underlined divisors above the threshold, only cities 3 and 6 share a common divisor, so they are the
only ones directly connected. The result of each query:
[1,4]   1 is not connected to 4
[2,5]   2 is not connected to 5
[3,6]   3 is connected to 6 through path 3--6
```

Example 2:

![1627-2](https://assets.leetcode.com/uploads/2020/10/10/tmp.jpg)

```text
Input: n = 6, threshold = 0, queries = [[4,5],[3,4],[3,2],[2,6],[1,3]]
Output: [true,true,true,true,true]
Explanation: The divisors for each number are the same as the previous example. However, since the threshold is 0,
all divisors can be used. Since all numbers share 1 as a divisor, all cities are connected.
```

Example 3:

![1627-3](https://assets.leetcode.com/uploads/2020/10/17/ex3.jpg)

```text
Input: n = 5, threshold = 1, queries = [[4,5],[4,5],[3,2],[2,3],[3,4]]
Output: [false,false,false,false,false]
Explanation: Only cities 2 and 4 share a common divisor 2 which is strictly greater than the threshold 1, so they are the only ones directly connected.
Please notice that there can be multiple queries for the same pair of nodes [x, y], and that the query [x, y] is equivalent to the query [y, x].
```

__Constraints:__

- `2 <= n <= 10 ^ 4`
- `0 <= threshold <= n`
- `1 <= queries.length <= 10 ^ 5`
- `queries[i].length == 2`
- `1 <= ai, bi <= cities`
- `ai != bi`

__题目描述:__

有 `n` 座城市，编号从 `1` 到 `n` 。编号为 `x` 和 `y` 的两座城市直接连通的前提是: `x` 和 `y` 的公因数中，至少有一个 __严格大于__ 某个阈值 `threshold` 。更正式地说，如果存在整数 `z` ，且满足以下所有条件，则编号 `x` 和 `y` 的城市之间有一条道路:

- `x % z == 0`
- `y % z == 0`
- `z > threshold`

给你两个整数 `n` 和 `threshold` ，以及一个待查询数组，请你判断每个查询 `queries[i] = [ai, bi]` 指向的城市 `ai` 和 `bi` 是否连通（即，它们之间是否存在一条路径）。

返回数组 `answer` ，其中 `answer.length == queries.length` 。如果第 `i` 个查询中指向的城市 `ai` 和 `bi` 连通，则 `answer[i]` 为 `true` ；如果不连通，则 `answer[i]` 为 `false` 。

__示例:__

示例 1：

![1627-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/18/ex1.jpg)

```text
输入：n = 6, threshold = 2, queries = [[1,4],[2,5],[3,6]]
输出：[false,false,true]
解释：每个数的因数如下：
1:   1
2:   1, 2
3:   1, 3
4:   1, 2, 4
5:   1, 5
6:   1, 2, 3, 6
所有大于阈值的的因数已经加粗标识，只有城市 3 和 6 共享公约数 3 ，因此结果是： 
[1,4]   1 与 4 不连通
[2,5]   2 与 5 不连通
[3,6]   3 与 6 连通，存在路径 3--6
```

示例 2：

![1627-5](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/18/tmp.jpg)

```text
输入：n = 6, threshold = 0, queries = [[4,5],[3,4],[3,2],[2,6],[1,3]]
输出：[true,true,true,true,true]
解释：每个数的因数与上一个例子相同。但是，由于阈值为 0 ，所有的因数都大于阈值。因为所有的数字共享公因数 1 ，所以所有的城市都互相连通。
```

示例 3：

![1627-6](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/16/ex3.jpg)

```text
输入：n = 5, threshold = 1, queries = [[4,5],[4,5],[3,2],[2,3],[3,4]]
输出：[false,false,false,false,false]
解释：只有城市 2 和 4 共享的公约数 2 严格大于阈值 1 ，所以只有这两座城市是连通的。
注意，同一对节点 [x, y] 可以有多个查询，并且查询 [x，y] 等同于查询 [y，x] 。
```

__提示：__

- `2 <= n <= 10 ^ 4`
- `0 <= threshold <= n`
- `1 <= queries.length <= 10 ^ 5`
- `queries[i].length == 2`
- `1 <= ai, bi <= cities`
- `ai != bi`

__思路:__

```text
并查集
用筛法把所有大于 threshold 的数通过并查集连接
检查 queries 中的是否在同一个并查集中
时间复杂度为 O(NlglgN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    vector<int> parent;
    
    int find(int p) 
    {
        return parent[p] == p ? p : (parent[p] = find(parent[p]));
    }
public:
    vector<bool> areConnected(int n, int threshold, vector<vector<int>>& queries) 
    {
        parent.resize(n + 1);
        iota(parent.begin(), parent.end(), 0);
        for (int i = threshold + 1; i <= n; i++) for (int j = (i << 1); j <= n; j += i) if (find(i) != find(j)) parent[find(i)] = parent[find(j)];
        vector<bool> result;
        for (const auto& query : queries) result.emplace_back(find(query.front()) == find(query.back()));
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private int[] parent;
    
    private int find(int p) {
        return parent[p] == p ? p : (parent[p] = find(parent[p]));
    }
    
    public List<Boolean> areConnected(int n, int threshold, int[][] queries) {
        parent = new int[n + 1];
        for (int i = 1; i <= n; i++) parent[i] = i;
        for (int i = threshold + 1; i <= n; i++) for (int j = (i << 1); j <= n; j += i) if (find(i) != find(j)) parent[find(i)] = parent[find(j)];
        List<Boolean> result = new ArrayList<>();
        for (int[] query : queries) result.add(find(query[0]) == find(query[1]));
        return result;
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
    def areConnected(self, n: int, threshold: int, queries: List[List[int]]) -> List[bool]:
        uf = UF(n + 1)
        for i in range(threshold + 1, n + 1):
            for j in range(2, n // i + 1):
                uf.union(i, i * j)
        return [uf.connected(i, j) for i, j in queries]
```
