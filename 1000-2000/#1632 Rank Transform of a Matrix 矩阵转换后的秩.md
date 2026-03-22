# 1632 Rank Transform of a Matrix 矩阵转换后的秩

__Description:__

Given an `m x n` `matrix`, return _a new matrix_ `answer` _where_ `answer[row][col]` _is the_ ___rank__ of_ `matrix[row][col]`.

The rank is an integer that represents how large an element is compared to other elements. It is calculated using the following rules:

- The rank is an integer starting from `1`.
- If two elements `p` and `q` are in the __same row or column__, then:

  - If `p < q` then `rank(p) < rank(q)`
  - If `p == q` then `rank(p) == rank(q)`
  - If `p > q` then `rank(p) > rank(q)`

- The __rank__ should be as __small__ as possible.

  - If `p < q` then `rank(p) < rank(q)`
  - If `p == q` then `rank(p) == rank(q)`
  - If `p > q` then `rank(p) > rank(q)`

The test cases are generated so that `answer` is unique under the given rules.

__Example:__

Example 1:

![1632-1](https://assets.leetcode.com/uploads/2020/10/18/rank1.jpg)

```text
Input: matrix = [[1,2],[3,4]]
Output: [[1,2],[2,3]]
Explanation:
The rank of matrix[0][0] is 1 because it is the smallest integer in its row and column.
The rank of matrix[0][1] is 2 because matrix[0][1] > matrix[0][0] and matrix[0][0] is rank 1.
The rank of matrix[1][0] is 2 because matrix[1][0] > matrix[0][0] and matrix[0][0] is rank 1.
The rank of matrix[1][1] is 3 because matrix[1][1] > matrix[0][1], matrix[1][1] > matrix[1][0], and both matrix[0][1] and matrix[1][0] are rank 2.
```

Example 2:

![1632-2](https://assets.leetcode.com/uploads/2020/10/18/rank2.jpg)

```text
Input: matrix = [[7,7],[7,7]]
Output: [[1,1],[1,1]]
```

Example 3:

![1632-3](https://assets.leetcode.com/uploads/2020/10/18/rank3.jpg)

```text
Input: matrix = [[20,-21,14],[-19,4,19],[22,-47,24],[-19,4,19]]
Output: [[4,2,3],[1,3,4],[5,1,6],[1,3,4]]
```

__Constraints:__

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 500`
- `-10 ^ 9 <= matrix[row][col] <= 10 ^ 9`

__题目描述:__

给你一个 `m x n` 的矩阵 `matrix` ，请你返回一个新的矩阵 `answer` ，其中 `answer[row][col]` 是 `matrix[row][col]` 的秩。

每个元素的 秩 是一个整数，表示这个元素相对于其他元素的大小关系，它按照如下规则计算：

- 秩是从 1 开始的一个整数。
- 如果两个元素 `p` 和 `q` 在 __同一行__ 或者 __同一列__ ，那么:

  - 如果 `p < q` ，那么 `rank(p) < rank(q)`
  - 如果 `p == q` ，那么 `rank(p) == rank(q)`
  - 如果 `p > q` ，那么 `rank(p) > rank(q)`

- _秩_ 需要越 __小__ 越好。

  - 如果 `p < q` ，那么 `rank(p) < rank(q)`
  - 如果 `p == q` ，那么 `rank(p) == rank(q)`
  - 如果 `p > q` ，那么 `rank(p) > rank(q)`

题目保证按照上面规则 `answer` 数组是唯一的。

__示例:__

示例 1：

![1632-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/25/rank1.jpg)

```text
输入：matrix = [[1,2],[3,4]]
输出：[[1,2],[2,3]]
解释：
matrix[0][0] 的秩为 1 ，因为它是所在行和列的最小整数。
matrix[0][1] 的秩为 2 ，因为 matrix[0][1] > matrix[0][0] 且 matrix[0][0] 的秩为 1 。
matrix[1][0] 的秩为 2 ，因为 matrix[1][0] > matrix[0][0] 且 matrix[0][0] 的秩为 1 。
matrix[1][1] 的秩为 3 ，因为 matrix[1][1] > matrix[0][1]， matrix[1][1] > matrix[1][0] 且 matrix[0][1] 和 matrix[1][0] 的秩都为 2 。
```

示例 2：

![1632-5](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/25/rank2.jpg)

```text
输入：matrix = [[7,7],[7,7]]
输出：[[1,1],[1,1]]
```

示例 3：

![1632-6](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/25/rank3.jpg)

```text
输入：matrix = [[20,-21,14],[-19,4,19],[22,-47,24],[-19,4,19]]
输出：[[4,2,3],[1,3,4],[5,1,6],[1,3,4]]
```

示例 4：

![1632-7](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/25/rank4.jpg)

```text
输入：matrix = [[7,3,6],[1,4,5],[9,8,2]]
输出：[[5,1,4],[1,2,3],[6,3,1]]
```

__提示：__

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 500`
- `-10 ^ 9 <= matrix[row][col] <= 10 ^ 9`

__思路:__

```text
并查集
相同元素需要赋相同的秩, 可以用并查集连接同一个根结点
为了获得排序可以用优先队列或者有序哈希表完成
从小到大去重排序后, 得到秩可以记录在行和列中
这样可以得到最大的秩为上次的最大秩加 1, 初始为 0
时间复杂度为 O(MNlogMN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class UF 
{
private:
    vector<int> parent, weight;
public:
    UF(int n) 
    {
        parent = vector<int>(n);
        weight = vector<int>(n, 1);
        iota(parent.begin(), parent.end(), 0);
    }

    void unite(int p, int q) 
    {
        if ((p = find(p)) == (q = find(q))) return;
        if (weight[p] > weight[q])
        {
            parent[q] = p;
            weight[p] += weight[q];
        }
        else
        {
            parent[p] = q;
            weight[q] += weight[p];
        }
    }

    int find(int p) 
    {
        return (parent[p] == p) ? p : (parent[p] = find(parent[p]));
    }

    void reset(int p) 
    {
        parent[p] = p;
        weight[p] = 1;
    }
};

class Solution 
{
public:
    vector<vector<int>> matrixRankTransform(vector<vector<int>>& matrix) 
    {
        int m = matrix.size(), n = matrix.front().size();
        map<int, vector<pair<int, int>>> d;
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) d[matrix[i][j]].push_back({i, j});
        vector<int> row_max(m), col_max(n), rank(m + n);
        vector<vector<int>> result(m, vector<int>(n));
        UF uf(m + n);
        for (const auto& [_, ps] : d) 
        {
            for (const auto& [i, j] : ps) uf.unite(i, j + m);
            for (const auto& [i, j] : ps) rank[uf.find(i)] = max({rank[uf.find(i)], row_max[i], col_max[j]});
            for (const auto& [i, j] : ps) result[i][j] = row_max[i] = col_max[j] = 1 + rank[uf.find(i)];
            for (const auto& [i, j] : ps) 
            {
                uf.reset(i);
                uf.reset(j + m);
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class UF {
    private int[] parent;
    private int[] weight;

    public UF(int n) {
        parent = new int[n];
        for (int i = 0; i < n; i++) parent[i] = i;
        weight = new int[n];
        Arrays.fill(weight, 1);
    }

    public int find(int p) {
        return (parent[p] == p) ? parent[p] : (parent[p] = find(parent[p]));
    }

    public void union(int p, int q) {
        if ((p = find(p)) == (q = find(q))) return;
        if (weight[p] > weight[q]) {
            parent[q] = p;
            weight[p] += weight[q];
        } else {
            parent[p] = q;
            weight[q] += weight[p];
        }
    }

    public void reset(int p) {
        parent[p] = p;
        weight[p] = 1;
    }
}


class Solution {
    public int[][] matrixRankTransform(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length, rowMax[] = new int[m], colMax[] = new int[n], result[][] = new int[m][n], rank[] = new int[m + n];
        TreeMap<Integer, List<int[]>> d = new TreeMap<>();
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) d.computeIfAbsent(matrix[i][j], k -> new ArrayList<>()).add(new int[] {i, j});
        UF uf = new UF(m + n);
        for (var ps : d.values()) {
            for (var p : ps) uf.union(p[0], p[1] + m);
            for (var p : ps) rank[uf.find(p[0])] = Math.max(rank[uf.find(p[0])], Math.max(rowMax[p[0]], colMax[p[1]]));
            for (var p : ps) result[p[0]][p[1]] = rowMax[p[0]] = colMax[p[1]] = 1 + rank[uf.find(p[0])];
            for (var p : ps) {
                uf.reset(p[0]);
                uf.reset(p[1] + m);
            }
        }
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
    def matrixRankTransform(self, matrix: List[List[int]]) -> List[List[int]]:
        row_max, col_max, d, rank, result, uf = [0] * (m := len(matrix)), [0] * (n := len(matrix[0])), defaultdict(list), defaultdict(int), matrix[:], UF(m + n)
        for i, row in enumerate(matrix):
            for j, v in enumerate(row):
                d[v].append((i, j))
        for v in sorted(d):
            for i, j in d[v]:
                uf.union(i, j + m)
            for i, j in d[v]:
                rank[uf.find(i)] = max(rank[uf.find(i)], row_max[i], col_max[j])
            for i, j in d[v]:
                result[i][j] = row_max[i] = col_max[j] = 1 + rank[uf.find(i)]
            for i, j in d[v]:
                uf.parent[i], uf.weight[i], uf.parent[j + m], uf.weight[j + m] = i, 1, j + m, 1
        return result
```
