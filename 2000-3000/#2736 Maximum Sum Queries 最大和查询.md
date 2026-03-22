# 2736 Maximum Sum Queries 最大和查询

__Description:__

You are given two __0-indexed__ integer arrays `nums1` and `nums2`, each of length `n`, and a __1-indexed 2D array__ `queries` where `queries[i] = [xi, yi]`.

For the `i ^ th` query, find the __maximum value__ of `nums1[j] + nums2[j]` among all indices `j` `(0 <= j < n)`, where `nums1[j] >= xi` and `nums2[j] >= yi`, or __-1__ if there is no `j` satisfying the constraints.

Return _an array_ `answer` _where_ `answer[i]` _is the answer to the_ `i ^ th` _query._

__Example:__

Example 1:

```text
Input:  nums1 = [4,3,1,2], nums2 = [2,4,9,5], queries = [[4,1],[1,3],[2,5]]
Output:  [6,10,7]
Explanation:  
For the 1st query xi = 4 and yi = 1, we can select index j = 0 since nums1[j] >= 4 and nums2[j] >= 1. The sum nums1[j] + nums2[j] is 6, and we can show that 6 is the maximum we can obtain.

For the 2nd query xi = 1 and yi = 3, we can select index j = 2 since nums1[j] >= 1 and nums2[j] >= 3. The sum nums1[j] + nums2[j] is 10, and we can show that 10 is the maximum we can obtain. 

For the 3rd query xi = 2 and yi = 5, we can select index j = 3 since nums1[j] >= 2 and nums2[j] >= 5. The sum nums1[j] + nums2[j] is 7, and we can show that 7 is the maximum we can obtain.

Therefore, we return [6,10,7].
```

Example 2:

```text
Input:  nums1 = [3,2,5], nums2 = [2,3,4], queries = [[4,4],[3,2],[1,1]]
Output:  [9,9,9]
Explanation:  For this example, we can use index j = 2 for all the queries since it satisfies the constraints for each query.
```

Example 3:

```text
Input:  nums1 = [2,1], nums2 = [2,3], queries = [[3,3]]
Output:  [-1]
Explanation:  There is one query in this example with xi = 3 and yi = 3. For every index, j, either nums1[j] < xi or nums2[j] < yi. Hence, there is no solution.
```

__Constraints:__

- `nums1.length == nums2.length`
- `n == nums1.length`
- `1 <= n <= 10 ^ 5`
- `1 <= nums1[i], nums2[i] <= 10 ^ 9`
- `1 <= queries.length <= 10 ^ 5`
- `queries[i].length == 2`
- `xi == queries[i][1]`
- `yi == queries[i][2]`
- `1 <= xi, yi <= 10 ^ 9`

__题目描述:__

给你两个长度为 `n` 、下标从 __0__ 开始的整数数组 `nums1` 和 `nums2` ，另给你一个下标从 __1__ 开始的二维数组 `queries` ，其中 `queries[i] = [xi, yi]` 。

对于第 `i` 个查询，在所有满足 `nums1[j] >= xi` 且 `nums2[j] >= yi` 的下标 `j` `(0 <= j < n)` 中，找出 `nums1[j] + nums2[j]` 的 __最大值__ ，如果不存在满足条件的 `j` 则返回 __-1__ 。

返回数组 `answer` ，其中 `answer[i]` 是第 `i` 个查询的答案。

__示例:__

示例 1：

```text
输入: nums1 = [4,3,1,2], nums2 = [2,4,9,5], queries = [[4,1],[1,3],[2,5]]
输出: [6,10,7]
解释: 
对于第 1 个查询: xi = 4 且 yi = 1 ，可以选择下标 j = 0 ，此时 nums1[j] >= 4 且 nums2[j] >= 1 。 nums1[j] + nums2[j] 等于 6 ，可以证明 6 是可以获得的最大值。
对于第 2 个查询: xi = 1 且 yi = 3 ，可以选择下标 j = 2 ，此时 nums1[j] >= 1 且 nums2[j] >= 3 。 nums1[j] + nums2[j] 等于 10 ，可以证明 10 是可以获得的最大值。
对于第 3 个查询: xi = 2 且 yi = 5 ，可以选择下标 j = 3 ，此时 nums1[j] >= 2 且 nums2[j] >= 5 。 nums1[j] + nums2[j] 等于 7 ，可以证明 7 是可以获得的最大值。
因此，我们返回 [6,10,7] 。
```

示例 2：

```text
输入: nums1 = [3,2,5], nums2 = [2,3,4], queries = [[4,4],[3,2],[1,1]]
输出: [9,9,9]
解释: 对于这个示例，我们可以选择下标 j = 2 ，该下标可以满足每个查询的限制。
```

示例 3：

```text
输入: nums1 = [2,1], nums2 = [2,3], queries = [[3,3]]
输出: [-1]
解释: 示例中的查询 xi = 3 且 yi = 3 。对于每个下标 j ，都只满足 nums1[j] < xi 或者 nums2[j] < yi 。因此，不存在答案。
```

__提示：__

- `nums1.length == nums2.length`
- `n == nums1.length`
- `1 <= n <= 10 ^ 5`
- `1 <= nums1[i], nums2[i] <= 10 ^ 9`
- `1 <= queries.length <= 10 ^ 5`
- `queries[i].length == 2`
- `xi == queries[i][1]`
- `yi == queries[i][2]`
- `1 <= xi, yi <= 10 ^ 9`

__思路:__

```text
单调栈 ➕ 二分
将 nums1 和 nums2 成对放在一起并按 x 从大到小排序
将查询加上原来的下标也按 x 从大到小排序
那么, 对于小于查询中的 x 的都不需要考虑
排序之后 y 较大的会在前面
所以如果 x 相等, y 较小的也不用考虑
在单调栈中从小到大记录 y, 对应的 x + y 就从大到小
利用二分找到第一个比 y 小的, 此时对应的 x + y 就最大
每次将当前 nums[j][0] + nums[j][1] >= x + y 弹出
如果单调栈为空或者 y 更小就把当前值加入进来
时间复杂度为 O(NlogN + QlogQ + NlogQ), 空间复杂度为 O(N + Q)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> maximumSumQueries(vector<int>& nums1, vector<int>& nums2, vector<vector<int>>& queries) 
    {
        int n = nums1.size(), q = queries.size(), j = 0;
        vector<pair<int, int>> nums(n), stack;
        for (int i = 0; i < n; i++) nums[i] = { nums1[i], nums2[i] };
        sort(nums.begin(), nums.end(), [](auto& a, auto& b) { return a.first > b.first; });
        for (int i = 0; i < q; i++) queries[i].emplace_back(i);
        sort(queries.begin(), queries.end(), [](auto& a, auto& b) { return a.front() > b.front(); });
        vector<int> result(q, -1);
        for (const auto& query : queries)
        {
            for (; j < n and nums[j].first >= query.front(); j++)
            {
                while (!stack.empty() and stack.back().second <= nums[j].first + nums[j].second) stack.pop_back();
                if (stack.empty() or stack.back().first < nums[j].second) stack.emplace_back(make_pair(nums[j].second, nums[j].first + nums[j].second));
            }
            auto it = lower_bound(stack.begin(), stack.end(), query[1], [](const auto& s, int target) { return s.first < target; });
            result[query.back()] = it == stack.end() ? -1 : it -> second;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] maximumSumQueries(int[] nums1, int[] nums2, int[][] queries) {
        int n = nums1.length, q = queries.length, nums[][] = new int[n][2], result[] = new int[q], j = 0;
        for (int i = 0; i < n; i++) {
            nums[i][0] = nums1[i];
            nums[i][1] = nums2[i];
        }
        Arrays.sort(nums, (a, b) -> b[0] - a[0]);
        for (int i = 0; i < q; i++) queries[i] = new int[]{ queries[i][0], queries[i][1], i };
        Arrays.sort(queries, (a, b) -> b[0] - a[0]);
        Arrays.fill(result, -1);
        var stack = new ArrayDeque<int[]>();
        var treeMap = new TreeMap<Integer, Integer>();
        for (int[] query : queries) {
            for (; j < n && nums[j][0] >= query[0]; j++) {
                while (!stack.isEmpty() && stack.peek()[1] <= nums[j][0] + nums[j][1]) treeMap.remove(stack.pop()[0]);
                if (stack.isEmpty() || stack.peek()[0] < nums[j][1]) {
                    stack.push(new int[]{ nums[j][1], nums[j][0] + nums[j][1] });
                    treeMap.put(nums[j][1], nums[j][0] + nums[j][1]);
                }
            }
            result[query[2]] = treeMap.ceilingEntry(query[1]) != null ? treeMap.ceilingEntry(query[1]).getValue() : -1;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumSumQueries(self, nums1: List[int], nums2: List[int], queries: List[List[int]]) -> List[int]:
        nums, queries, result, j, stack, n = sorted((x, y) for x, y in zip(nums1, nums2))[::-1], sorted((x, y, i) for i, (x, y) in enumerate(queries))[::-1], [-1] * len(queries), 0, [], len(nums1)
        print(nums)
        for x, y, i in queries:
            while j < n and nums[j][0] >= x:
                while stack and stack[-1][1] <= sum(nums[j]):
                    stack.pop()
                if not stack or stack[-1][0] <= nums[j][1]:
                    stack.append((nums[j][1], sum(nums[j])))
                j += 1
            result[i] = stack[p][1] if (p := bisect_left(stack, (y, ))) < len(stack) else -1
        return result
```
