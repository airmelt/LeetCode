# 2718 Sum of Matrix After Queries 查询后矩阵的和

__Description:__

You are given an integer `n` and a __0-indexed__ __2D array__ `queries` where `queries[i] = [type_i, index_i, val_i]`.

Initially, there is a __0-indexed__ `n x n` matrix filled with `0`'s. For each query, you must apply one of the following changes:

- if `type_i == 0`, set the values in the row with `index_i` to `val_i`, overwriting any previous values.
- if `type_i == 1`, set the values in the column with `index_i` to `val_i`, overwriting any previous values.

Return the sum of integers in the matrix after all queries are applied.

__Example:__

Example 1:

![2718-1](https://assets.leetcode.com/uploads/2023/05/11/exm1.png)

```text
Input: n = 3, queries = [[0,0,1],[1,2,2],[0,2,3],[1,0,4]]
Output: 23
Explanation: The image above describes the matrix after each query. The sum of the matrix after all queries are applied is 23.
```

Example 2:

![2718-2](https://assets.leetcode.com/uploads/2023/05/11/exm2.png)

```text
Input: n = 3, queries = [[0,0,4],[0,1,2],[1,0,1],[0,2,3],[1,2,1]]
Output: 17
Explanation: The image above describes the matrix after each query. The sum of the matrix after all queries are applied is 17.
```

__Constraints:__

- `1 <= n <= 10 ^ 4`
- `1 <= queries.length <= 5 * 10 ^ 4`
- `queries[i].length == 3`
- `0 <= type_i <= 1`
- `0 <= index_i < n`
- `0 <= val_i <= 10 ^ 5`

__题目描述:__

给你一个整数 `n` 和一个下标从 __0__ 开始的 __二维数组__ `queries` ，其中 `queries[i] = [type_i, index_i, val_i]` 。

一开始，给你一个下标从 __0__ 开始的 `n x n` 矩阵，所有元素均为 `0` 。每一个查询，你需要执行以下操作之一:

- 如果 `type_i == 0` ，将第 `index_i` 行的元素全部修改为 `val_i` ，覆盖任何之前的值。
- 如果 `type_i == 1` ，将第 `index_i` 列的元素全部修改为 `val_i` ，覆盖任何之前的值。

请你执行完所有查询以后，返回矩阵中所有整数的和。

__示例:__

示例 1：

![2718-3](https://assets.leetcode.com/uploads/2023/05/11/exm1.png)

```text
输入：n = 3, queries = [[0,0,1],[1,2,2],[0,2,3],[1,0,4]]
输出：23
解释：上图展示了每个查询以后矩阵的值。所有操作执行完以后，矩阵元素之和为 23 。
```

示例 2：

![2718-4](https://assets.leetcode.com/uploads/2023/05/11/exm2.png)

```text
输入：n = 3, queries = [[0,0,4],[0,1,2],[1,0,1],[0,2,3],[1,2,1]]
输出：17
解释：上图展示了每一个查询操作之后的矩阵。所有操作执行完以后，矩阵元素之和为 17 。
```

__提示：__

- `1 <= n <= 10 ^ 4`
- `1 <= queries.length <= 5 * 10 ^ 4`
- `queries[i].length == 3`
- `0 <= type_i <= 1`
- `0 <= index_i < n`
- `0 <= val_i <= 10 ^ 5`

__思路:__

```text
逆序遍历 ➕ 哈希表
首先后面的修改会覆盖前面的修改
所以从后往前遍历, 将修改过的行和列分别加入对应的哈希表
出现过的不再处理
对于要处理的行或列, 修改的范围仅限 n - 列或行的哈希表的大小
相当于染色
时间复杂度为 O(M), 空间复杂度为 O(min(M, N)), 其中 M 为查询的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long matrixSumQueries(int n, vector<vector<int>>& queries) 
    {
        long long result = 0LL;
        unordered_set<int> visited[2];
        for (int m = queries.size(), i = m - 1; i > -1; i--) 
        {
            if (!visited[queries[i][0]].count(queries[i][1])) 
            {
                result += (long long)queries[i][2] * (n - visited[queries[i][0] ^ 1].size());
                visited[queries[i][0]].insert(queries[i][1]);
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long matrixSumQueries(int n, int[][] queries) {
        long result = 0L;
        Set<Integer>[] visited = new Set[]{ new HashSet<>(), new HashSet<>() };
        for (int m = queries.length, i = m - 1; i > -1; i--) {
            if (!visited[queries[i][0]].contains(queries[i][1])) {
                result += (long)queries[i][2] * (n - visited[queries[i][0] ^ 1].size());
                visited[queries[i][0]].add(queries[i][1]);
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def matrixSumQueries(self, n: int, queries: List[List[int]]) -> int:
        result, visited = 0, [set(), set()]
        for t, i, v in queries[::-1]:
            if i not in visited[t]:
                result += v * (n - len(visited[t ^ 1]))
                visited[t].add(i)
        return result
```
