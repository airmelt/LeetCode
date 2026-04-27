# 2940 Find Building Where Alice and Bob Can Meet 找到 Alice 和 Bob 可以相遇的建筑

__Description:__

You are given a __0-indexed__ array `heights` of positive integers, where `heights[i]` represents the height of the `i ^ th` building.

If a person is in building `i`, they can move to any other building `j` if and only if `i < j` and `heights[i] < heights[j]`.

You are also given another array `queries` where `queries[i] = [ai, bi]`. On the `i ^ th` query, Alice is in building `ai` while Bob is in building `bi`.

Return _an array_ `ans` _where_ `ans[i]` _is __the index of the leftmost building__ where Alice and Bob can meet on the_ `i ^ th` _query_. _If Alice and Bob cannot move to a common building on query_ `i`, _set_ `ans[i]` _to_ `-1`.

__Example:__

Example 1:

```text
Input: heights = [6,4,8,5,2,7], queries = [[0,1],[0,3],[2,4],[3,4],[2,2]]
Output: [2,5,-1,5,2]
Explanation: In the first query, Alice and Bob can move to building 2 since heights[0] < heights[2] and heights[1] < heights[2]. 
In the second query, Alice and Bob can move to building 5 since heights[0] < heights[5] and heights[3] < heights[5]. 
In the third query, Alice cannot meet Bob since Alice cannot move to any other building.
In the fourth query, Alice and Bob can move to building 5 since heights[3] < heights[5] and heights[4] < heights[5].
In the fifth query, Alice and Bob are already in the same building.  
For ans[i] != -1, It can be shown that ans[i] is the leftmost building where Alice and Bob can meet.
For ans[i] == -1, It can be shown that there is no building where Alice and Bob can meet.
```

Example 2:

```text
Input: heights = [5,3,8,2,6,1,4,6], queries = [[0,7],[3,5],[5,2],[3,0],[1,6]]
Output: [7,6,-1,4,6]
Explanation: In the first query, Alice can directly move to Bob's building since heights[0] < heights[7].
In the second query, Alice and Bob can move to building 6 since heights[3] < heights[6] and heights[5] < heights[6].
In the third query, Alice cannot meet Bob since Bob cannot move to any other building.
In the fourth query, Alice and Bob can move to building 4 since heights[3] < heights[4] and heights[0] < heights[4].
In the fifth query, Alice can directly move to Bob's building since heights[1] < heights[6].
For ans[i] != -1, It can be shown that ans[i] is the leftmost building where Alice and Bob can meet.
For ans[i] == -1, It can be shown that there is no building where Alice and Bob can meet.
```

__Constraints:__

- `1 <= heights.length <= 5 * 10 ^ 4`
- `1 <= heights[i] <= 10 ^ 9`
- `1 <= queries.length <= 5 * 10 ^ 4`
- `queries[i] = [ai, bi]`
- `0 <= ai, bi <= heights.length - 1`

__题目描述:__

给你一个下标从 __0__ 开始的正整数数组 `heights` ，其中 `heights[i]` 表示第 `i` 栋建筑的高度。

如果一个人在建筑 `i` ，且存在 `i < j` 的建筑 `j` 满足 `heights[i] < heights[j]` ，那么这个人可以移动到建筑 `j` 。

给你另外一个数组 `queries` ，其中 `queries[i] = [ai, bi]` 。第 `i` 个查询中，Alice 在建筑 `ai` ，Bob 在建筑 `bi` 。

请你能返回一个数组 `ans` ，其中 `ans[i]` 是第 `i` 个查询中，Alice 和 Bob 可以相遇的 __最左边的建筑__ 。如果对于查询 `i` ，Alice 和 Bob 不能相遇，令 `ans[i]` 为 `-1` 。

__示例:__

示例 1：

```text
输入：heights = [6,4,8,5,2,7], queries = [[0,1],[0,3],[2,4],[3,4],[2,2]]
输出：[2,5,-1,5,2]
解释：第一个查询中，Alice 和 Bob 可以移动到建筑 2 ，因为 heights[0] < heights[2] 且 heights[1] < heights[2] 。
第二个查询中，Alice 和 Bob 可以移动到建筑 5 ，因为 heights[0] < heights[5] 且 heights[3] < heights[5] 。
第三个查询中，Alice 无法与 Bob 相遇，因为 Alice 不能移动到任何其他建筑。
第四个查询中，Alice 和 Bob 可以移动到建筑 5 ，因为 heights[3] < heights[5] 且 heights[4] < heights[5] 。
第五个查询中，Alice 和 Bob 已经在同一栋建筑中。
对于 ans[i] != -1 ，ans[i] 是 Alice 和 Bob 可以相遇的建筑中最左边建筑的下标。
对于 ans[i] == -1 ，不存在 Alice 和 Bob 可以相遇的建筑。
```

示例 2：

```text
输入：heights = [5,3,8,2,6,1,4,6], queries = [[0,7],[3,5],[5,2],[3,0],[1,6]]
输出：[7,6,-1,4,6]
解释：第一个查询中，Alice 可以直接移动到 Bob 的建筑，因为 heights[0] < heights[7] 。
第二个查询中，Alice 和 Bob 可以移动到建筑 6 ，因为 heights[3] < heights[6] 且 heights[5] < heights[6] 。
第三个查询中，Alice 无法与 Bob 相遇，因为 Bob 不能移动到任何其他建筑。
第四个查询中，Alice 和 Bob 可以移动到建筑 4 ，因为 heights[3] < heights[4] 且 heights[0] < heights[4] 。
第五个查询中，Alice 可以直接移动到 Bob 的建筑，因为 heights[1] < heights[6] 。
对于 ans[i] != -1 ，ans[i] 是 Alice 和 Bob 可以相遇的建筑中最左边建筑的下标。
对于 ans[i] == -1 ，不存在 Alice 和 Bob 可以相遇的建筑。
```

__提示：__

- `1 <= heights.length <= 5 * 10 ^ 4`
- `1 <= heights[i] <= 10 ^ 9`
- `1 <= queries.length <= 5 * 10 ^ 4`
- `queries[i] = [ai, bi]`
- `0 <= ai, bi <= heights.length - 1`

__思路:__

```text
首先可以先将简单的排除
对于 queries 中的元素 [x, y]
不妨设 x < y
如果 x == y
或者 heights[x] < heights[y]
我们直接选择 y 即可
剩下的按照 i 和 heights[i] 记录下来
1. 单调栈 ➕ 二分
从右往左遍历 heights 
遇见不小于栈顶的就直接弹出, 因为它无论如何也不可能作为结果
对于栈中元素, 其已经按照对应的 heights[i] 从大到小排列
那么对于元素 height, idx
需要找到对应的比 height 大的元素
可以使用二分查找
找到最后一个比 height 大的元素
如果超出边界就返回 -1
否则返回对应的位置
时间复杂度为 O(N + MlogN), 空间复杂度为 O(N + M)
2. 最小堆
如果堆顶元素比 heights[i] 要小
则 idx 对应的元素就为 i
没有处理的元素全部直接加入堆
时间复杂度为 O(N + MlogM), 空间复杂度为 O(N + M)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> leftmostBuildingQueries(vector<int>& heights, vector<vector<int>>& queries) 
    {
        int m = queries.size(), n = heights.size(), x = -1, y = -1;
        vector<int> result(m, -1);
        vector<vector<pair<int, int>>> need_queries(n);
        for (int i = 0; i < m; i++) 
        {
            if ((x = min(queries[i].front(), queries[i].back())) == (y = max(queries[i].front(), queries[i].back())) or heights[x] < heights[y]) result[i] = y;
            else need_queries[y].emplace_back(heights[x], i);
        }
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> pq;
        for (int i = 0; i < n; i++) 
        {
            while (!pq.empty() && pq.top().first < heights[i]) 
            {
                result[pq.top().second] = i;
                pq.pop();
            }
            for (const auto& q : need_queries[i]) pq.push(q);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] leftmostBuildingQueries(int[] heights, int[][] queries) {
        int m = queries.length, n = heights.length, result[] = new int[m], x = -1, y = -1;
        Arrays.fill(result, -1);
        List<int[]>[] needQueries = new ArrayList[n];
        Arrays.setAll(needQueries, i -> new ArrayList<>());
        for (int i = 0; i < m; i++) {
            if ((x = Math.min(queries[i][0], queries[i][1])) == (y = Math.max(queries[i][0], queries[i][1])) || heights[x] < heights[y]) result[i] = y;
            else needQueries[y].add(new int[]{ heights[x], i });
        }
        var pq = new PriorityQueue<int[]>((a, b) -> a[0] - b[0]);
        for (int i = 0; i < n; i++) {
            while (!pq.isEmpty() && pq.peek()[0] < heights[i]) result[pq.poll()[1]] = i;
            for (var q : needQueries[i]) pq.offer(q);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def leftmostBuildingQueries(self, heights: List[int], queries: List[List[int]]) -> List[int]:
        result, need_queries, stack, n = [-1] * (m := len(queries)), [[] for _ in heights], [], len(heights)
        for i, (x, y) in enumerate(queries):
            if x > y:
                x, y = y, x
            if x == y or heights[x] < heights[y]:
                result[i] = y
            else:
                need_queries[y].append((heights[x], i))
        for i in range(n - 1, -1, -1):
            for height, idx in need_queries[i]:
                if (pos := bisect_left(stack, -height, key=lambda j: -heights[j]) - 1) > -1:
                    result[idx] = stack[pos]
            while stack and heights[i] >= heights[stack[-1]]:
                stack.pop()
            stack.append(i)
        return result
```
