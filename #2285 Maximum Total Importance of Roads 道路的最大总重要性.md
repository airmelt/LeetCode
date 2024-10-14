# 2285 Maximum Total Importance of Roads 道路的最大总重要性

__Description:__

You are given an integer `n` denoting the number of cities in a country. The cities are numbered from `0` to `n - 1`.

You are also given a 2D integer array `roads` where `roads[i] = [ai, bi]` denotes that there exists a __bidirectional__ road connecting cities `ai` and `bi`.

You need to assign each city with an integer value from `1` to `n`, where each value can only be used __once__. The __importance__ of a road is then defined as the __sum__ of the values of the two cities it connects.

Return the maximum total importance of all roads possible after assigning the values optimally.

__Example:__

Example 1:

![2285-1](https://assets.leetcode.com/uploads/2022/04/07/ex1drawio.png)

```text
Input: n = 5, roads = [[0,1],[1,2],[2,3],[0,2],[1,3],[2,4]]
Output: 43
Explanation: The figure above shows the country and the assigned values of [2,4,5,3,1].
```

- The road (0,1) has an importance of 2 + 4 = 6.
- The road (1,2) has an importance of 4 + 5 = 9.
- The road (2,3) has an importance of 5 + 3 = 8.
- The road (0,2) has an importance of 2 + 5 = 7.
- The road (1,3) has an importance of 4 + 3 = 7.
- The road (2,4) has an importance of 5 + 1 = 6.

The total importance of all roads is 6 + 9 + 8 + 7 + 7 + 6 = 43.
It can be shown that we cannot obtain a greater total importance than 43.

Example 2:

![2285-2](https://assets.leetcode.com/uploads/2022/04/07/ex2drawio.png)

```text
Input: n = 5, roads = [[0,3],[2,4],[1,3]]
Output: 20
Explanation: The figure above shows the country and the assigned values of [4,3,2,5,1].
```

- The road (0,3) has an importance of 4 + 5 = 9.
- The road (2,4) has an importance of 2 + 1 = 3.
- The road (1,3) has an importance of 3 + 5 = 8.

The total importance of all roads is 9 + 3 + 8 = 20.
It can be shown that we cannot obtain a greater total importance than 20.

__Constraints:__

- `2 <= n <= 5 * 10 ^ 4`
- `1 <= roads.length <= 5 * 10 ^ 4`
- `roads[i].length == 2`
- `0 <= ai, bi <= n - 1`
- `ai != bi`
- There are no duplicate roads.

__题目描述:__

给你一个整数 `n` ，表示一个国家里的城市数目。城市编号为 `0` 到 `n - 1` 。

给你一个二维整数数组 `roads` ，其中 `roads[i] = [ai, bi]` 表示城市 `ai` 和 `bi` 之间有一条 __双向__ 道路。

你需要给每个城市安排一个从 `1` 到 `n` 之间的整数值，且每个值只能被使用 __一次__ 。道路的 __重要性__ 定义为这条道路连接的两座城市数值 __之和__ 。

请你返回在最优安排下，所有道路重要性 之和 最大 为多少。

__示例:__

示例 1：

![2285-3](https://assets.leetcode.com/uploads/2022/04/07/ex1drawio.png)

```text
输入：n = 5, roads = [[0,1],[1,2],[2,3],[0,2],[1,3],[2,4]]
输出：43
解释：上图展示了国家图和每个城市被安排的值 [2,4,5,3,1] 。
```

- 道路 (0,1) 重要性为 2 + 4 = 6 。
- 道路 (1,2) 重要性为 4 + 5 = 9 。
- 道路 (2,3) 重要性为 5 + 3 = 8 。
- 道路 (0,2) 重要性为 2 + 5 = 7 。
- 道路 (1,3) 重要性为 4 + 3 = 7 。
- 道路 (2,4) 重要性为 5 + 1 = 6 。

所有道路重要性之和为 6 + 9 + 8 + 7 + 7 + 6 = 43 。
可以证明，重要性之和不可能超过 43 。

示例 2：

![2285-4](https://assets.leetcode.com/uploads/2022/04/07/ex2drawio.png)

```text
输入：n = 5, roads = [[0,3],[2,4],[1,3]]
输出：20
解释：上图展示了国家图和每个城市被安排的值 [4,3,2,5,1] 。
```

- 道路 (0,3) 重要性为 4 + 5 = 9 。
- 道路 (2,4) 重要性为 2 + 1 = 3 。
- 道路 (1,3) 重要性为 3 + 5 = 8 。

所有道路重要性之和为 9 + 3 + 8 = 20 。
可以证明，重要性之和不可能超过 20 。

__提示：__

- `2 <= n <= 5 * 10 ^ 4`
- `1 <= roads.length <= 5 * 10 ^ 4`
- `roads[i].length == 2`
- `0 <= ai, bi <= n - 1`
- `ai != bi`
- 没有重复道路。

__思路:__

```text
贪心
计算每个城市的度数, 并排序
然后计算总重要性
时间复杂度为 O(NlogN), 空间复杂度为 O(N), 主要是排序的时间复杂度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maximumImportance(int n, vector<vector<int>>& roads) 
    {
        vector<int> degree(n);
        for (const auto& road : roads) 
        {
            ++degree[road.front()];
            ++degree[road.back()];
        }
        sort(degree.begin(), degree.end());
        long long result = 0LL;
        for (int i = 0; i < n; i++) result += (long long)degree[i] * (i + 1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long maximumImportance(int n, int[][] roads) {
        int[] degree = new int[n];
        for (int[] road : roads) {
            ++degree[road[0]];
            ++degree[road[1]];
        }
        Arrays.sort(degree);
        long result = 0L;
        for (int i = 0; i < n; i++) result += (long)degree[i] * (i + 1);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumImportance(self, n: int, roads: List[List[int]]) -> int:
        degree = [0] * n
        for x, y in roads:
            degree[x] += 1
            degree[y] += 1
        return sum(d * i for i, d in enumerate(sorted(degree), 1))
```
