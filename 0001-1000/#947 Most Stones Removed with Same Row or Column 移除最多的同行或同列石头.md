# 947 Most Stones Removed with Same Row or Column 移除最多的同行或同列石头

__Description__:
On a 2D plane, we place n stones at some integer coordinate points. Each coordinate point may have at most one stone.

A stone can be removed if it shares either the same row or the same column as another stone that has not been removed.

Given an array stones of length n where stones[i] = [xi, yi] represents the location of the ith stone, return the largest possible number of stones that can be removed.

__Example:__

Example 1:

Input: stones = [[0,0],[0,1],[1,0],[1,2],[2,1],[2,2]]
Output: 5
Explanation: One way to remove 5 stones is as follows:

1. Remove stone [2,2] because it shares the same row as [2,1].
2. Remove stone [2,1] because it shares the same column as [0,1].
3. Remove stone [1,2] because it shares the same row as [1,0].
4. Remove stone [1,0] because it shares the same column as [0,0].
5. Remove stone [0,1] because it shares the same row as [0,0].
Stone [0,0] cannot be removed since it does not share a row/column with another stone still on the plane.

Example 2:

Input: stones = [[0,0],[0,2],[1,1],[2,0],[2,2]]
Output: 3
Explanation: One way to make 3 moves is as follows:

1. Remove stone [2,2] because it shares the same row as [2,0].
2. Remove stone [2,0] because it shares the same column as [0,0].
3. Remove stone [0,2] because it shares the same row as [0,0].
Stones [0,0] and [1,1] cannot be removed since they do not share a row/column with another stone still on the plane.

Example 3:

Input: stones = [[0,0]]
Output: 0
Explanation: [0,0] is the only stone on the plane, so you cannot remove it.

__Constraints:__

1 <= stones.length <= 1000
0 <= xi, yi <= 10^4
No two stones are at the same coordinate point.

__题目描述__:
n 块石头放置在二维平面中的一些整数坐标点上。每个坐标点上最多只能有一块石头。

如果一块石头的 同行或者同列 上有其他石头存在，那么就可以移除这块石头。

给你一个长度为 n 的数组 stones ，其中 stones[i] = [xi, yi] 表示第 i 块石头的位置，返回 可以移除的石子 的最大数量。

__示例 :__

示例 1：

输入：stones = [[0,0],[0,1],[1,0],[1,2],[2,1],[2,2]]
输出：5
解释：一种移除 5 块石头的方法如下所示：

1. 移除石头 [2,2] ，因为它和 [2,1] 同行。
2. 移除石头 [2,1] ，因为它和 [0,1] 同列。
3. 移除石头 [1,2] ，因为它和 [1,0] 同行。
4. 移除石头 [1,0] ，因为它和 [0,0] 同列。
5. 移除石头 [0,1] ，因为它和 [0,0] 同行。
石头 [0,0] 不能移除，因为它没有与另一块石头同行/列。

示例 2：

输入：stones = [[0,0],[0,2],[1,1],[2,0],[2,2]]
输出：3
解释：一种移除 3 块石头的方法如下所示：

1. 移除石头 [2,2] ，因为它和 [2,0] 同行。
2. 移除石头 [2,0] ，因为它和 [0,0] 同列。
3. 移除石头 [0,2] ，因为它和 [0,0] 同行。
石头 [0,0] 和 [1,1] 不能移除，因为它们没有与另一块石头同行/列。

示例 3：

输入：stones = [[0,0]]
输出：0
解释：[0,0] 是平面上唯一一块石头，所以不可以移除它。

__提示:__

1 <= stones.length <= 1000
0 <= xi, yi <= 10^4
不会有两块石头放在同一个坐标点上

__思路__:

并查集
按照横纵坐标将点分到不同的并查集中去
将纵坐标加上一个偏移量与横坐标关联起来
集合中存放的是并查集中只剩下一个元素, 即并查集的数量
返回石头的长度减去并查集的数量就是可以消去的石头数
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int removeStones(vector<vector<int>>& stones) 
    {
        const int N = 20005;
        vector<int> parent(N);
        for (int i = 0; i < N; i++) parent[i] = i;
        int n = stones.size();
        for (const auto& stone : stones) parent[find(stone.front(), parent)] = parent[find(stone.back() + 10000, parent)];
        set<int> s;
        for (const auto& stone : stones) s.insert(find(stone.front(), parent));
        return n - s.size();
    }
private:
    int find(int x, vector<int>& parent) 
    {
        return x == parent[x] ? x : (parent[x] = find(parent[x], parent));
    }
};
```

__Java__:

```Java
class Solution {
    private final int N = 20005;
    private int[] parent;
    
    public int removeStones(int[][] stones) {
        int n = stones.length;
        parent = new int[N];
        for (int i = 0; i < N; i++) parent[i] = i;
        for (int[] stone : stones) parent[find(stone[0])] = parent[find(stone[1] + 10000)];
        Set<Integer> set = new HashSet<>();
        for (int[] stone : stones) set.add(find(stone[0]));
        return n - set.size();
    }
    
    private int find(int x) {
        return x == parent[x] ? x : (parent[x] = find(parent[x]));
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
    def removeStones(self, stones: List[List[int]]) -> int:
        uf = UF(20000)
        for x, y in stones:
            uf.union(x, y + 10000)
        return len(stones) - len({ uf.find(x) for x, y in stones })
```
