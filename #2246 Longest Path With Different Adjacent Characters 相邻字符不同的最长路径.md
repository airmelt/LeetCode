# 2246 Longest Path With Different Adjacent Characters 相邻字符不同的最长路径

__Description:__

You are given a __tree__ (i.e. a connected, undirected graph that has no cycles) __rooted__ at node `0` consisting of `n` nodes numbered from `0` to `n - 1`. The tree is represented by a __0-indexed__ array `parent` of size `n`, where `parent[i]` is the parent of node `i`. Since node `0` is the root, `parent[0] == -1`.

You are also given a string `s` of length `n`, where `s[i]` is the character assigned to node `i`.

Return the length of the longest path in the tree such that no pair of adjacent nodes on the path have the same character assigned to them.

__Example:__

Example 1:

![2246-1](https://assets.leetcode.com/uploads/2022/03/25/testingdrawio.png)

```text
Input: parent = [-1,0,0,1,1,2], s = "abacbe"
Output: 3
Explanation: The longest path where each two adjacent nodes have different characters in the tree is the path: 0 -> 1 -> 3. The length of this path is 3, so 3 is returned.
It can be proven that there is no longer path that satisfies the conditions.
```

Example 2:

![2246-2](https://assets.leetcode.com/uploads/2022/03/25/graph2drawio.png)

```text
Input: parent = [-1,0,0,0], s = "aabc"
Output: 3
Explanation: The longest path where each two adjacent nodes have different characters is the path: 2 -> 0 -> 3. The length of this path is 3, so 3 is returned.
```

__Constraints:__

- `n == parent.length == s.length`
- `1 <= n <= 10 ^ 5`
- `0 <= parent[i] <= n - 1` for all `i >= 1`
- `parent[0] == -1`
- `parent` represents a valid tree.
- `s` consists of only lowercase English letters.

__题目描述:__

给你一棵 __树__（即一个连通、无向、无环图），根节点是节点 `0` ，这棵树由编号从 `0` 到 `n - 1` 的 `n` 个节点组成。用下标从 __0__ 开始、长度为 `n` 的数组 `parent` 来表示这棵树，其中 `parent[i]` 是节点 `i` 的父节点，由于节点 `0` 是根节点，所以 `parent[0] == -1` 。

另给你一个字符串 `s` ，长度也是 `n` ，其中 `s[i]` 表示分配给节点 `i` 的字符。

请你找出路径上任意一对相邻节点都没有分配到相同字符的 最长路径 ，并返回该路径的长度。

__示例:__

示例 1：

![2246-3](https://assets.leetcode.com/uploads/2022/03/25/testingdrawio.png)

```text
输入：parent = [-1,0,0,1,1,2], s = "abacbe"
输出：3
解释：任意一对相邻节点字符都不同的最长路径是：0 -> 1 -> 3 。该路径的长度是 3 ，所以返回 3 。
可以证明不存在满足上述条件且比 3 更长的路径。
```

示例 2：

![2246-4](https://assets.leetcode.com/uploads/2022/03/25/graph2drawio.png)

```text
输入：parent = [-1,0,0,0], s = "aabc"
输出：3
解释：任意一对相邻节点字符都不同的最长路径是：2 -> 0 -> 3 。该路径的长度为 3 ，所以返回 3 。
```

__提示：__

- `n == parent.length == s.length`
- `1 <= n <= 10 ^ 5`
- 对所有 `i >= 1` ， `0 <= parent[i] <= n - 1` 均成立
- `parent[0] == -1`
- `parent` 表示一棵有效的树
- `s` 仅由小写英文字母组成

__思路:__

```text
DFS
实质就是求树的直径
使用 DFS 求树的直径
加上一个条件, 必须要和父节点的 s 对应的值不一致即可
最后返回需要加 1, 因为需要返回的是节点数
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int longestPath(vector<int>& parent, string s) 
    {
        int n = parent.size(), result = 0;
        vector<vector<int>> graph(n);
        for (int i = 1; i < n; i++) graph[parent[i]].emplace_back(i);
        auto dfs = [&](auto&& dfs, int x) -> int
        {
            int m = 0;
            for (int y : graph[x])
            {
                int cur = dfs(dfs, y) + 1;
                if (s[x] != s[y])
                {
                    result = max(result, cur + m);
                    m = max(m, cur);
                }
            }
            return m;
        };
        dfs(dfs, 0);
        return ++result;
    }
};
```

__Java__:

```Java
class Solution {
    private int result = 0;

    public int longestPath(int[] parent, String s) {
        Map<Integer, Set<Integer>> graph = new HashMap<>();
        int n = parent.length;
        for (int i = 1; i < n; i++) graph.computeIfAbsent(parent[i], k -> new HashSet<Integer>()).add(i);
        dfs(0, graph, s);
        return ++result;
    }

    private int dfs(int x, Map<Integer, Set<Integer>> graph, String s) {
        int m = 0;
        for (int y : graph.getOrDefault(x, new HashSet<>())) {
            int cur = dfs(y, graph, s) + 1;
            if (s.charAt(y) != s.charAt(x)) {
                result = Math.max(result, cur + m);
                m = Math.max(m, cur);
            }
        }
        return m;
    }
}
```

__Python__:

```Python
class Solution:
    def longestPath(self, parent: List[int], s: str) -> int:
        n, graph, result = len(parent), defaultdict(set), 0
        for i in range(1, n):
            graph[parent[i]].add(i)
        def dfs(x: int) -> int:
            nonlocal result
            m = 0
            for y in graph[x]:
                cur = dfs(y) + 1
                if s[y] != s[x]:
                    result, m = max(result,  m + cur), max(m, cur)
            return m
        dfs(0)
        return result + 1
```
