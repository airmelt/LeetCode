# 2070 Most Beautiful Item for Each Query 每一个查询的最大美丽值

__Description:__

You are given a 2D integer array `items` where `items[i] = [pricei, beautyi]` denotes the __price__ and __beauty__ of an item respectively.

You are also given a __0-indexed__ integer array `queries`. For each `queries[j]`, you want to determine the __maximum beauty__ of an item whose __price__ is __less than or equal__ to `queries[j]`. If no such item exists, then the answer to this query is `0`.

Return _an array_ `answer` _of the same length as_ `queries` _where_ `answer[j]` _is the answer to the_ `j ^ th` _query_.

__Example:__

Example 1:

```text
Input: items = [[1,2],[3,2],[2,4],[5,6],[3,5]], queries = [1,2,3,4,5,6]
Output: [2,4,5,5,6,6]
Explanation:
```

- For queries[0]=1, [1,2] is the only item which has price <= 1. Hence, the answer for this query is 2.
- For queries[1]=2, the items which can be considered are [1,2] and [2,4].
  The maximum beauty among them is 4.
- For queries[2]=3 and queries[3]=4, the items which can be considered are [1,2], [3,2], [2,4], and [3,5].
  The maximum beauty among them is 5.
- For queries[4]=5 and queries[5]=6, all items can be considered.
  Hence, the answer for them is the maximum beauty of all items, i.e., 6.

Example 2:

```text
Input: items = [[1,2],[1,2],[1,3],[1,4]], queries = [1]
Output: [4]
Explanation: 
The price of every item is equal to 1, so we choose the item with the maximum beauty 4. 
Note that multiple items can have the same price and/or beauty.
```

Example 3:

```text
Input: items = [[10,1000]], queries = [5]
Output: [0]
Explanation:
No item has a price less than or equal to 5, so no item can be chosen.
Hence, the answer to the query is 0.
```

__Constraints:__

- `1 <= items.length, queries.length <= 10 ^ 5`
- `items[i].length == 2`
- `1 <= pricei, beautyi, queries[j] <= 10 ^ 9`

__题目描述:__

给你一个二维整数数组 `items` ，其中 `items[i] = [pricei, beautyi]` 分别表示每一个物品的 __价格__ 和 __美丽值__ 。

同时给你一个下标从 __0__ 开始的整数数组 `queries` 。对于每个查询 `queries[j]` ，你想求出价格小于等于 `queries[j]` 的物品中，__最大的美丽值__ 是多少。如果不存在符合条件的物品，那么查询的结果为 `0` 。

请你返回一个长度与 `queries` 相同的数组 `answer`，其中 `answer[j]`是第 `j` 个查询的答案。

__示例:__

示例 1：

```text
输入：items = [[1,2],[3,2],[2,4],[5,6],[3,5]], queries = [1,2,3,4,5,6]
输出：[2,4,5,5,6,6]
解释：
```

- queries[0]=1 ，[1,2] 是唯一价格 <= 1 的物品。所以这个查询的答案为 2 。
- queries[1]=2 ，符合条件的物品有 [1,2] 和 [2,4] 。
  它们中的最大美丽值为 4 。
- queries[2]=3 和 queries[3]=4 ，符合条件的物品都为 [1,2] ，[3,2] ，[2,4] 和 [3,5] 。
  它们中的最大美丽值为 5 。
- queries[4]=5 和 queries[5]=6 ，所有物品都符合条件。
  所以，答案为所有物品中的最大美丽值，为 6 。

示例 2：

```text
输入：items = [[1,2],[1,2],[1,3],[1,4]], queries = [1]
输出：[4]
解释：
每个物品的价格均为 1 ，所以我们选择最大美丽值 4 。
注意，多个物品可能有相同的价格和美丽值。
```

示例 3：

```text
输入：items = [[10,1000]], queries = [5]
输出：[0]
解释：
没有物品的价格小于等于 5 ，所以没有物品可以选择。
因此，查询的结果为 0 。
```

__提示：__

- `1 <= items.length, queries.length <= 10 ^ 5`
- `items[i].length == 2`
- `1 <= pricei, beautyi, queries[j] <= 10 ^ 9`

__思路:__

```text
排序
由于 items 与答案无关
将 items 按照价格进行排序
然后将 queries 也按照价格进行排序, 并记录下标
遍历 items, 用一个变量记录当前的最大美丽值
将当前最大美丽值赋予查询的下标即可
或者使用二分查找
将 items 的美丽值修改为不大于当前价格的最大美丽值
类似前缀和的方式 items[i][1] = max(items[i][1], items[i - 1][1])
然后对于每一个查询, 使用二分查找找到价格小于等于查询的最大价格的物品
更新美丽值
时间复杂度为 O((N + M)logN), 空间复杂度为 O(logN), 其中 N 为 items 的长度, M 为 queries 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> maximumBeauty(vector<vector<int>>& items, vector<int>& queries) 
    {
        map<int, int> mp;
        for (const auto& item : items) mp[item.front()] = max(mp[item.front()], item.back());
        for (auto it = ++mp.begin(), last = mp.begin(); it != mp.end(); last = it, ++it) it -> second = max(it -> second, last -> second);
        vector<int> result;
        for (const auto& query : queries) 
        {
            auto idx = mp.upper_bound(query);
            result.emplace_back(idx != mp.begin() ? (--idx) -> second : 0);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] maximumBeauty(int[][] items, int[] queries) {
        Arrays.sort(items, (a, b) -> a[0] - b[0]);
        int n = queries.length, m = items.length, result[] = new int[n];
        for (int i = 1; i < m; i++) items[i][1] = Math.max(items[i][1], items[i - 1][1]);
        for (int i = 0, idx = -1; i < n; i++) result[i] = (idx = helper(items, queries[i])) < 0 ? 0 : items[idx][1];  
        return result;
    }

    public int helper(int[][] items, int target) {
        int left = 0, right = items.length;
        while (left < right) {
            int mid = left + ((right - left) >> 1);
            if (items[mid][0] > target) right = mid;
            else left = mid + 1;
        }
        return left - 1;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumBeauty(self, items: List[List[int]], queries: List[int]) -> List[int]:
        items.sort()
        result, a, cur, j, m = [0] * (n := len(queries)), sorted(zip(queries, range(n))), 0, 0, len(items)
        for q, i in a:
            while j < m and items[j][0] <= q:
                cur = max(cur, items[j][1])
                j += 1
            result[i] = cur
        return result
```
