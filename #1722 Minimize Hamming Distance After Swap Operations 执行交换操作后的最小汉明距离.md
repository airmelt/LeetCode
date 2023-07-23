# 1722 Minimize Hamming Distance After Swap Operations 执行交换操作后的最小汉明距离

__Description:__

You are given two integer arrays, `source` and `target`, both of length `n`. You are also given an array `allowedSwaps` where each `allowedSwaps[i] = [ai, bi]` indicates that you are allowed to swap the elements at index `ai` and index `bi` __(0-indexed)__ of array `source`. Note that you can swap elements at a specific pair of indices __multiple__ times and in __any__ order.

The __Hamming distance__ of two arrays of the same length, `source` and `target`, is the number of positions where the elements are different. Formally, it is the number of indices `i` for `0 <= i <= n-1` where `source[i] != target[i]` __(0-indexed)__.

Return _the __minimum Hamming distance__ of_ `source` _and_ `target` _after performing __any__ amount of swap operations on array_ `source`_._

__Example:__

Example 1:

```text
Input: source = [1,2,3,4], target = [2,1,4,5], allowedSwaps = [[0,1],[2,3]]
Output: 1
Explanation: source can be transformed the following way:
- Swap indices 0 and 1: source = [2,1,3,4]
- Swap indices 2 and 3: source = [2,1,4,3]
The Hamming distance of source and target is 1 as they differ in 1 position: index 3.
```

Example 2:

```text
Input: source = [1,2,3,4], target = [1,3,2,4], allowedSwaps = []
Output: 2
Explanation: There are no allowed swaps.
The Hamming distance of source and target is 2 as they differ in 2 positions: index 1 and index 2.
```

Example 3:

```text
Input: source = [5,1,2,4,3], target = [1,5,4,2,3], allowedSwaps = [[0,4],[4,2],[1,3],[1,4]]
Output: 0
```

__Constraints:__

- `n == source.length == target.length`
- `1 <= n <= 10 ^ 5`
- `1 <= source[i], target[i] <= 10 ^ 5`
- `0 <= allowedSwaps.length <= 10 ^ 5`
- `allowedSwaps[i].length == 2`
- `0 <= ai, bi <= n - 1`
- `ai != bi`

__题目描述:__

给你两个整数数组 `source` 和 `target` ，长度都是 `n` 。还有一个数组 `allowedSwaps` ，其中每个 `allowedSwaps[i] = [ai, bi]` 表示你可以交换数组 `source` 中下标为 `ai` 和 `bi`（__下标从 0 开始__）的两个元素。注意，你可以按 __任意__ 顺序 __多次__ 交换一对特定下标指向的元素。

相同长度的两个数组 `source` 和 `target` 间的 __汉明距离__ 是元素不同的下标数量。形式上，其值等于满足 `source[i] != target[i]` （__下标从 0 开始__）的下标 `i`（ `0 <= i <= n-1`）的数量。

在对数组 `source` 执行 __任意__ 数量的交换操作后，返回 `source` 和 `target` 间的 __最小汉明距离__ 。

__示例:__

示例 1：

```text
输入：source = [1,2,3,4], target = [2,1,4,5], allowedSwaps = [[0,1],[2,3]]
输出：1
解释：source 可以按下述方式转换：
- 交换下标 0 和 1 指向的元素：source = [2,1,3,4]
- 交换下标 2 和 3 指向的元素：source = [2,1,4,3]
source 和 target 间的汉明距离是 1 ，二者有 1 处元素不同，在下标 3 。
```

示例 2：

```text
输入：source = [1,2,3,4], target = [1,3,2,4], allowedSwaps = []
输出：2
解释：不能对 source 执行交换操作。
source 和 target 间的汉明距离是 2 ，二者有 2 处元素不同，在下标 1 和下标 2 。
```

示例 3：

```text
输入：source = [5,1,2,4,3], target = [1,5,4,2,3], allowedSwaps = [[0,4],[4,2],[1,3],[1,4]]
输出：0
```

__提示：__

- `n == source.length == target.length`
- `1 <= n <= 10 ^ 5`
- `1 <= source[i], target[i] <= 10 ^ 5`
- `0 <= allowedSwaps.length <= 10 ^ 5`
- `allowedSwaps[i].length == 2`
- `0 <= ai, bi <= n - 1`
- `ai != bi`

__思路:__

```text
并查集
将可以交换的两个元素归入一个并查集
分别统计 source 和 target 数组对应并查集的数目
统计相同的元素的总数
最后返回数组长度减去相同元素的数目
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumHammingDistance(vector<int>& source, vector<int>& target, vector<vector<int>>& allowedSwaps) 
    {
        int n = source.size(), result = 0;
        vector<int> parent(n);
        iota(parent.begin(), parent.end(), 0);
        for (const auto &swap : allowedSwaps) parent[find(parent, swap.front())] = find(parent, swap.back());
        unordered_map<int, unordered_map<int, int>> s, t;
        for (int i = 0; i < n; i++) 
        {
            ++s[find(parent, i)][source[i]];
            ++t[find(parent, i)][target[i]];
        }
        for (auto & [k1, v1] : s) for (auto &[k2, v2] : t[k1]) if (v1.count(k2)) result += min(v1[k2], v2); 
        return n - result;
    }
private:
    int find(vector<int>& parent, int x) 
    {
        return (parent[x] == x) ? x : (parent[parent[x]] = find(parent, parent[x]));
    }
};
```

__Java__:

```Java
class Solution {
    private int find(int[] parent, int x) {
        return (parent[x] == x) ? x : (parent[parent[x]] = find(parent, parent[x]));
    }
    
    public int minimumHammingDistance(int[] source, int[] target, int[][] allowedSwaps) {
        int n = source.length, result = 0, parent[] = new int[n];
        for (int i = 0; i < n; i++) parent[i] = i;
        for (int[] swap : allowedSwaps) parent[find(parent, swap[0])] = find(parent, swap[1]);
        Map<Integer, Map<Integer, Integer>> s = new HashMap<>(), t = new HashMap<>();
        for (int i = 0; i < n; i++) {
            int cur = find(parent, i);
            if (!s.containsKey(cur)) s.put(cur, new HashMap<>());
            if (!t.containsKey(cur)) t.put(cur, new HashMap<>());
            Map<Integer, Integer> s1 = s.get(cur), t1 = t.get(cur);
            s1.put(source[i], s1.getOrDefault(source[i], 0) + 1);
            t1.put(target[i], t1.getOrDefault(target[i], 0) + 1);
        }
        for (Map.Entry<Integer, Map<Integer, Integer>> entry1 : s.entrySet()) for (Map.Entry<Integer, Integer> entry2 : t.get(entry1.getKey()).entrySet()) result += Math.min(entry2.getValue(), entry1.getValue().getOrDefault(entry2.getKey(), 0));
        return n - result;
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
    def minimumHammingDistance(self, source: List[int], target: List[int], allowedSwaps: List[List[int]]) -> int:
        uf, d, result = UF(n := len(source)), defaultdict(list), 0
        for u, v in allowedSwaps:
            uf.union(u, v)
        for i in range(n):
            d[uf.find(i)].append(i)
        return sum(len(v) - sum((Counter(source[i] for i in v) & Counter(target[i] for i in v)).values()) for v in d.values())
```
