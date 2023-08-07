# 1743 Restore the Array From Adjacent Pairs 从相邻元素对还原数组

__Description:__

There is an integer array `nums` that consists of `n` __unique__ elements, but you have forgotten it. However, you do remember every pair of adjacent elements in `nums`.

You are given a 2D integer array `adjacentPairs` of size `n - 1` where each `adjacentPairs[i] = [ui, vi]` indicates that the elements `ui` and `vi` are adjacent in `nums`.

It is guaranteed that every adjacent pair of elements `nums[i]` and `nums[i+1]` will exist in `adjacentPairs`, either as `[nums[i], nums[i+1]]` or `[nums[i+1], nums[i]]`. The pairs can appear __in any order__.

Return _the original array_ `nums`_. If there are multiple solutions, return __any of them___.

__Example:__

Example 1:

```text
Input: adjacentPairs = [[2,1],[3,4],[3,2]]
Output: [1,2,3,4]
Explanation: This array has all its adjacent pairs in adjacentPairs.
Notice that adjacentPairs[i] may not be in left-to-right order.
```

Example 2:

```text
Input: adjacentPairs = [[4,-2],[1,4],[-3,1]]
Output: [-2,4,1,-3]
Explanation: There can be negative numbers.
Another solution is [-3,1,4,-2], which would also be accepted.
```

Example 3:

```text
Input: adjacentPairs = [[100000,-100000]]
Output: [100000,-100000]
```

__Constraints:__

- `nums.length == n`
- `adjacentPairs.length == n - 1`
- `adjacentPairs[i].length == 2`
- `2 <= n <= 10 ^ 5`
- `-10 ^ 5 <= nums[i], ui, vi <= 10 ^ 5`
- There exists some `nums` that has `adjacentPairs` as its pairs.

__题目描述:__

存在一个由 `n` 个不同元素组成的整数数组 `nums` ，但你已经记不清具体内容。好在你还记得 `nums` 中的每一对相邻元素。

给你一个二维整数数组 `adjacentPairs` ，大小为 `n - 1` ，其中每个 `adjacentPairs[i] = [ui, vi]` 表示元素 `ui` 和 `vi` 在 `nums` 中相邻。

题目数据保证所有由元素 `nums[i]` 和 `nums[i+1]` 组成的相邻元素对都存在于 `adjacentPairs` 中，存在形式可能是 `[nums[i], nums[i+1]]` ，也可能是 `[nums[i+1], nums[i]]` 。这些相邻元素对可以 __按任意顺序__ 出现。

返回 __原始数组__ `nums` 。如果存在多种解答，返回 __其中任意一个__ 即可。

__示例:__

示例 1：

```text
输入：adjacentPairs = [[2,1],[3,4],[3,2]]
输出：[1,2,3,4]
解释：数组的所有相邻元素对都在 adjacentPairs 中。
特别要注意的是，adjacentPairs[i] 只表示两个元素相邻，并不保证其 左-右 顺序。
```

示例 2：

```text
输入：adjacentPairs = [[4,-2],[1,4],[-3,1]]
输出：[-2,4,1,-3]
解释：数组中可能存在负数。
另一种解答是 [-3,1,4,-2] ，也会被视作正确答案。
```

示例 3：

```text
输入：adjacentPairs = [[100000,-100000]]
输出：[100000,-100000]
```

__提示：__

- `nums.length == n`
- `adjacentPairs.length == n - 1`
- `adjacentPairs[i].length == 2`
- `2 <= n <= 10 ^ 5`
- `-10 ^ 5 <= nums[i], ui, vi <= 10 ^ 5`
- 题目数据保证存在一些以 `adjacentPairs` 作为元素对的数组 `nums`

__思路:__

```text
图
由于已经给出所有的相邻元素对，因此可以将每个元素看作图中的一个节点，相邻元素对之间存在一条边，那么整个图中包含 n + 1 个节点和 n 条边，且图中只有 2 个节点的度为 1，其余节点的度都为 2
这两个度为 1 的结点都可以当作图的起点
先将所有结点放入图中, 并记录下每个结点都邻居
接着找到一个度为 1 的结点当作列表的起点, 并将其邻居作为列表的第二个结点
然后从图中依次弹出之前没有遍历过的邻居结点加入列表即可
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> restoreArray(vector<vector<int>>& adjacentPairs) 
    {
        unordered_map<int, vector<int>> m;
        for (const auto& a : adjacentPairs) 
        {
            m[a.front()].push_back(a.back());
            m[a.back()].push_back(a.front());
        }
        int n = adjacentPairs.size() + 1;
        vector<int> result(n);
        for (const auto& [u, adj] : m) 
        {
            if (adj.size() == 1) {
                result.front() = u;
                result[1] = adj.front();
                break;
            }
        }
        for (int i = 2; i < n; i++) result[i] = result[i - 2] == m[result[i - 1]].front() ? m[result[i - 1]].back() : m[result[i - 1]].front();
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] restoreArray(int[][] adjacentPairs) {
        Map<Integer, List<Integer>> map = new HashMap<>();
        for (int[] adjacentPair : adjacentPairs) {
            map.putIfAbsent(adjacentPair[0], new ArrayList<Integer>());
            map.putIfAbsent(adjacentPair[1], new ArrayList<Integer>());
            map.get(adjacentPair[0]).add(adjacentPair[1]);
            map.get(adjacentPair[1]).add(adjacentPair[0]);
        }
        int n = adjacentPairs.length + 1, result[] = new int[n];
        for (Map.Entry<Integer, List<Integer>> entry : map.entrySet()) {
            if (entry.getValue().size() == 1) {
                result[0] = entry.getKey();
                result[1] = entry.getValue().get(0);
                break;
            }
        }
        for (int i = 2; i < n; i++) result[i] = result[i - 2] == map.get(result[i - 1]).get(0) ? map.get(result[i - 1]).get(1) : map.get(result[i - 1]).get(0);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def restoreArray(self, adjacentPairs: List[List[int]]) -> List[int]:
        n, d, l = len(adjacentPairs) + 1, defaultdict(list), []
        for u, v in adjacentPairs:
            d[u].append(v)
            d[v].append(u)
        for u in d:
            if len(d[u]) == 1:
                l.append(u)
                break
        result, visited = [l[0]], {l[0]}
        while len(result) < n:
            next = d[result[-1]]
            if len(next) == 1:
                result.append(next[0])
            else:
                for v in next:
                    if v not in visited:
                        result.append(v)
            visited.add(result[-1])
        return result
```
