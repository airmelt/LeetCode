# 3160 Find the Number of Distinct Colors Among the Balls 所有球里面不同颜色的数目

__Description:__

You are given an integer `limit` and a 2D array `queries` of size `n x 2`.

There are `limit + 1` balls with __distinct__ labels in the range `[0, limit]`. Initially, all balls are uncolored. For every query in `queries` that is of the form `[x, y]`, you mark ball `x` with the color `y`. After each query, you need to find the number of colors among the balls.

Return an array `result` of length `n`, where `result[i]` denotes the number of colors after `i ^ th` query.

__Note__ that when answering a query, lack of a color _will not_ be considered as a color.

__Example:__

Example 1:

![3160-1](https://assets.leetcode.com/uploads/2024/04/17/ezgifcom-crop.gif)

```text
Input: limit = 4, queries = [[1,4],[2,5],[1,3],[3,4]]

Output: [1,2,2,3]

Explanation:

After query 0, ball 1 has color 4.
After query 1, ball 1 has color 4, and ball 2 has color 5.
After query 2, ball 1 has color 3, and ball 2 has color 5.
After query 3, ball 1 has color 3, ball 2 has color 5, and ball 3 has color 4.
```

Example 2:

![3160-2](https://assets.leetcode.com/uploads/2024/04/17/ezgifcom-crop2.gif)

```text
Input: limit = 4, queries = [[0,1],[1,2],[2,2],[3,4],[4,5]]

Output: [1,2,2,3,4]

Explanation:

After query 0, ball 0 has color 1.
After query 1, ball 0 has color 1, and ball 1 has color 2.
After query 2, ball 0 has color 1, and balls 1 and 2 have color 2.
After query 3, ball 0 has color 1, balls 1 and 2 have color 2, and ball 3 has color 4.
After query 4, ball 0 has color 1, balls 1 and 2 have color 2, ball 3 has color 4, and ball 4 has color 5.
```

__Constraints:__

- `1 <= limit <= 10 ^ 9`
- `1 <= n == queries.length <= 10 ^ 5`
- `queries[i].length == 2`
- `0 <= queries[i][0] <= limit`
- `1 <= queries[i][1] <= 10 ^ 9`

__题目描述:__

给你一个整数 `limit` 和一个大小为 `n x 2` 的二维数组 `queries` 。

总共有 `limit + 1` 个球，每个球的编号为 `[0, limit]` 中一个 __互不相同__ 的数字。一开始，所有球都没有颜色。`queries` 中每次操作的格式为 `[x, y]` ，你需要将球 `x` 染上颜色 `y` 。每次操作之后，你需要求出所有球颜色的数目。

请你返回一个长度为 `n` 的数组 `result` ，其中 `result[i]` 是第 `i` 次操作以后颜色的数目。

__注意__ ，没有染色的球不算作一种颜色。

__示例:__

示例 1：

![3160-2](https://assets.leetcode.com/uploads/2024/04/17/ezgifcom-crop2.gif)

```text
输入：limit = 4, queries = [[1,4],[2,5],[1,3],[3,4]]

输出：[1,2,2,3]

解释：

操作 0 后，球 1 颜色为 4 。
操作 1 后，球 1 颜色为 4 ，球 2 颜色为 5 。
操作 2 后，球 1 颜色为 3 ，球 2 颜色为 5 。
操作 3 后，球 1 颜色为 3 ，球 2 颜色为 5 ，球 3 颜色为 4 。
```

示例 2：

![3149-4](https://assets.leetcode.com/uploads/2024/04/04/example1gif.gif)

```text
输入：limit = 4, queries = [[0,1],[1,2],[2,2],[3,4],[4,5]]

输出：[1,2,2,3,4]

解释：

操作 0 后，球 0 颜色为 1 。
操作 1 后，球 0 颜色为 1 ，球 1 颜色为 2 。
操作 2 后，球 0 颜色为 1 ，球 1 和 2 颜色为 2 。
操作 3 后，球 0 颜色为 1 ，球 1 和 2 颜色为 2 ，球 3 颜色为 4 。
操作 4 后，球 0 颜色为 1 ，球 1 和 2 颜色为 2 ，球 3 颜色为 4 ，球 4 颜色为 5 。
```

__提示：__

- `1 <= limit <= 10 ^ 9`
- `1 <= n == queries.length <= 10 ^ 5`
- `queries[i].length == 2`
- `0 <= queries[i][0] <= limit`
- `1 <= queries[i][1] <= 10 ^ 9`

__思路:__

```text
哈希表
记录每一个球的颜色在 color 中, 由于所有颜色都大于 0, 可以初始化颜色为 0
如果颜色之前已经被染色过那么在 color 中就会有记录
相应的在 groups 里减去这个记录
如果 groups 对应的组没有元素则移除该组
然后记录颜色和组
结果更新为组的个数即可
时间复杂度为 O(N), 空间复杂度为 O(N), 其中 N 为数组 queries 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> queryResults(int limit, vector<vector<int>>& queries) 
    {
        vector<int> result;
        unordered_map<int, int> color, groups;
        for (const auto& query : queries) 
        {
            if (color.count(query.front())) if (!(--groups[color[query.front()]])) groups.erase(color[query.front()]);
            color[query.front()] = query.back();
            ++groups[query.back()];
            result.emplace_back(groups.size());
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] queryResults(int limit, int[][] queries) {
        int n = queries.length, result[] = new int[n];
        Map<Integer, Integer> color = new HashMap<>(), groups = new HashMap<>();
        for (int i = 0, c = 0; i < n; i++) {
            if (color.containsKey(queries[i][0])) if (groups.merge(c = color.get(queries[i][0]), -1, Integer::sum) == 0) groups.remove(c);
            color.put(queries[i][0], queries[i][1]);
            groups.merge(queries[i][1], 1, Integer::sum);
            result[i] = groups.size();
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def queryResults(self, limit: int, queries: List[List[int]]) -> List[int]:
        result, groups, color = queries[:], defaultdict(int), defaultdict(int)
        for i, (x, y) in enumerate(queries):
            if (pre := color[x]):
                groups[pre] -= 1
                if not groups[pre]:
                    del groups[pre]
            groups[y] += 1
            color[x], result[i] = y, len(groups)
        return result
```
