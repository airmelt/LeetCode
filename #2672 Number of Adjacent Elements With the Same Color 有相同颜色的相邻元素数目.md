# 2672 Number of Adjacent Elements With the Same Color 有相同颜色的相邻元素数目

__Description:__

You are given an integer `n` representing an array `colors` of length `n` where all elements are set to 0's meaning __uncolored__. You are also given a 2D integer array `queries` where `queries[i] = [index_i, color_i]`. For the `i ^ th` __query__:

- Set `colors[index_i]` to `color_i`.
- Count the number of adjacent pairs in `colors` which have the same color (regardless of `color_i`).

Return an array `answer` of the same length as `queries` where `answer[i]` is the answer to the `i ^ th` query.

__Example:__

Example 1:

```text
Input: n = 4, queries = [[0,2],[1,2],[3,1],[1,1],[2,1]]

Output: [0,1,1,0,2]

Explanation:
```

- Initially array colors = [0,0,0,0], where 0 denotes uncolored elements of the array.
- After the 1 ^ st query colors = [2,0,0,0]. The count of adjacent pairs with the same color is 0.
- After the 2 ^ nd query colors = [2,2,0,0]. The count of adjacent pairs with the same color is 1.
- After the 3 ^ rd query colors = [2,2,0,1]. The count of adjacent pairs with the same color is 1.
- After the 4 ^ th query colors = [2,1,0,1]. The count of adjacent pairs with the same color is 0.
- After the 5 ^ th query colors = [2,1,1,1]. The count of adjacent pairs with the same color is 2.

Example 2:

```text
Input: n = 1, queries = [[0,100000]]

Output: [0]

Explanation:

After the 1st query colors = [100000]. The count of adjacent pairs with the same color is 0.
```

__Constraints:__

- `1 <= n <= 10 ^ 5`
- `1 <= queries.length <= 10 ^ 5`
- `queries[i].length == 2`
- `0 <= index_i <= n - 1`
- `1 <= color_i <= 10 ^ 5`

__题目描述:__

给定一个整数 `n` 表示一个长度为 `n` 的数组  `colors`，初始所有元素均为 0 ，表示是 __未染色__ 的。同时给定一个二维整数数组 `queries`，其中 `queries[i] = [index_i, color_i]`。对于第 `i` 个 __查询__:

- 将 `colors[index_i]` 染色为 `color_i`。
- 统计 `colors` 中颜色相同的相邻对的数量（无论 `color_i`）。

请你返回一个长度与 `queries` 相等的数组 `answer` ，其中 `answer[i]`是前 `i` 个操作的答案。

__示例:__

示例 1：

```text
输入：n = 4, queries = [[0,2],[1,2],[3,1],[1,1],[2,1]]

输出：[0,1,1,0,2]

解释：
```

- 一开始 colors = [0,0,0,0]，其中 0 表示数组中未染色的元素。
- 在第 1 次查询后 colors = [2,0,0,0]。颜色相同的相邻对的数量是 0。
- 在第 2 次查询后 colors = [2,2,0,0]。颜色相同的相邻对的数量是 1。
- 在第 3 次查询后 colors = [2,2,0,1]。颜色相同的相邻对的数量是 1。
- 在第 4 次查询后 colors = [2,1,0,1]。颜色相同的相邻对的数量是 0。
- 在第 5 次查询后 colors = [2,1,1,1]。颜色相同的相邻对的数量是 2。

示例 2：

```text
输入：n = 1, queries = [[0,100000]]

输出：[0]

解释：

在第一次查询后 colors = [100000]。颜色相同的相邻对的数量是 0。
```

__提示：__

- `1 <= n <= 10 ^ 5`
- `1 <= queries.length <= 10 ^ 5`
- `queries[i].length == 2`
- `0 <= index_i <= n - 1`
- `1 <= color_i <= 10 ^ 5`

__思路:__

```text
模拟
每次更改最多只会影响左右两边的颜色
当 color[queries[i][0]] 不为 0 时, 改变 color[queries[i][0]] 可能会影响到 color[queries[i][0] - 1] 和 color[queries[i][0] + 1], 此时需要更新相邻元素的计数
如果 color[queries[i][0]] 为 0, 不需要更新相邻元素的计数
然后更新 color[queries[i][0]]
更新完 color[queries[i][0]] 之后, 需要更新相邻元素的计数, 比较 color[queries[i][0] - 1] 和 color[queries[i][0]] 以及 color[queries[i][0]] 和 color[queries[i][0] + 1]
最后更新 result[i] 为相邻元素的计数
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> colorTheArray(int n, vector<vector<int>>& queries) 
    {
        int m = queries.size(), cur = 0;
        vector<int> result(m), color(n + 2);
        for (int i = 0; i < m; i++) 
        {

            if (color[queries[i].front() + 1]) cur -= (color[queries[i].front() + 1] == color[queries[i].front()]) + (color[queries[i].front() + 1] == color[queries[i].front() + 2]);
            color[queries[i].front() + 1] = queries[i].back();
            cur += (color[queries[i].front() + 1] == color[queries[i].front()]) + (color[queries[i].front() + 1] == color[queries[i].front() + 2]);
            result[i] = cur;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] colorTheArray(int n, int[][] queries) {
        int m = queries.length, result[] = new int[m], color[] = new int[n + 2], cur = 0;
        for (int i = 0; i < m; i++) {
            if (color[queries[i][0] + 1] > 0) cur -= (color[queries[i][0] + 1] == color[queries[i][0]] ? 1 : 0) + (color[queries[i][0] + 1] == color[queries[i][0] + 2] ? 1 : 0);
            color[queries[i][0] + 1] = queries[i][1];
            cur += (color[queries[i][0] + 1] == color[queries[i][0]] ? 1 : 0) + (color[queries[i][0] + 1] == color[queries[i][0] + 2] ? 1 : 0);
            result[i] = cur;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def colorTheArray(self, n: int, queries: List[List[int]]) -> List[int]:
        result, color, cur = [], [0] * (n + 2), 0
        for i, c in queries:
            cur -= color[i + 1] and (color[i + 1] == color[i]) + (color[i + 1] == color[i + 2])
            color[i + 1] = c
            cur += (color[i + 1] == color[i]) + (color[i + 1] == color[i + 2])
            result.append(cur)
        return result
```
