# 2612 Minimum Reverse Operations 最少翻转操作数

__Description:__

You are given an integer `n` and an integer `p` representing an array `arr` of length `n` where all elements are set to 0's, except position `p` which is set to 1. You are also given an integer array `banned` containing restricted positions. Perform the following operation on `arr`:

- Reverse a __subarray__ with size `k` if the single 1 is not set to a position in `banned`.

Return an integer array `answer` with `n` results where the `i ^ th` result is the __minimum__ number of operations needed to bring the single 1 to position `i` in `arr`, or -1 if it is impossible.

__Example:__

Example 1:

```text
Input: n = 4, p = 0, banned = [1,2], k = 4
```

Output: [0,-1,-1,1]

Explanation:

- Initially 1 is placed at position 0 so the number of operations we need for position 0 is 0.
- We can never place 1 on the banned positions, so the answer for positions 1 and 2 is -1.
- Perform the operation of size 4 to reverse the whole array.
- After a single operation 1 is at position 3 so the answer for position 3 is 1.

Example 2:

```text
Input: n = 5, p = 0, banned = [2,4], k = 3
```

Output: [0,-1,-1,-1,-1]

Explanation:

- Initially 1 is placed at position 0 so the number of operations we need for position 0 is 0.
- We cannot perform the operation on the subarray positions `[0, 2]` because position 2 is in banned.
- Because 1 cannot be set at position 2, it is impossible to set 1 at other positions in more operations.

Example 3:

```text
Input: n = 4, p = 2, banned = [0,1,3], k = 1
```

Output: [-1,-1,0,-1]

Explanation:

Perform operations of size 1 and 1 never changes its position.

__Constraints:__

- `1 <= n <= 10 ^ 5`
- `0 <= p <= n - 1`
- `0 <= banned.length <= n - 1`
- `0 <= banned[i] <= n - 1`
- `1 <= k <= n`
- `banned[i] != p`
- all values in `banned` are __unique__

__题目描述:__

给定一个整数 `n` 和一个整数 `p`，它们表示一个长度为 `n` 且除了下标为 `p` 处是 `1` 以外，其他所有数都是 `0` 的数组 `arr`。同时给定一个整数数组 `banned` ，它包含数组中的一些限制位置。在 `arr` 上进行下列操作:

- 如果单个 1 不在 `banned` 中的位置上，反转大小为 `k` 的 __子数组__。

返回一个包含 `n` 个结果的整数数组 `answer`，其中第 `i` 个结果是将 `1` 放到位置 `i` 处所需的 __最少__ 翻转操作次数，如果无法放到位置 `i` 处，此数为 `-1` 。

__示例:__

示例 1：

```text
输入：n = 4, p = 0, banned = [1,2], k = 4
```

输出：[0,-1,-1,1]

解释：

- 一开始 1 位于位置 0，因此我们需要在位置 0 上的操作数是 0。
- 我们不能将 1 放置在被禁止的位置上，所以位置 1 和 2 的答案是 -1。
- 执行大小为 4 的操作以反转整个数组。
- 在一次操作后，1 位于位置 3，因此位置 3 的答案是 1。

示例 2：

```text
输入：n = 5, p = 0, banned = [2,4], k = 3
```

输出：[0,-1,-1,-1,-1]

解释：

- 一开始 1 位于位置 0，因此我们需要在位置 0 上的操作数是 0。
- 我们不能在 `[0, 2]` 的子数组位置上执行操作，因为位置 2 在 banned 中。
- 由于 1 不能够放置在位置 2 上，使用更多操作将 1 放置在其它位置上是不可能的。

示例 3：

```text
输入：n = 4, p = 2, banned = [0,1,3], k = 1
```

输出：[-1,-1,0,-1]

解释：

执行大小为 1 的操作，且 1 永远不会改变位置。

__提示：__

- `1 <= n <= 10 ^ 5`
- `0 <= p <= n - 1`
- `0 <= banned.length <= n - 1`
- `0 <= banned[i] <= n - 1`
- `1 <= k <= n`
- `banned[i] != p`
- `banned` 中的值 __互不相同__ 。

__思路:__

```text
并查集
对于已经访问的节点, 使用 uf.union(i, i + 1), 这样下次访问 i 时就会直接跳到 i + 1
本题中由于翻转之后, i 对应的下标会变成 i + 2, 所以需要使用 uf.union(i, i + 2)
对于每个节点 i, 需要找到它的最小下标和最大下标
如果 i - k + 1 < 0, 那么最小下标为 k - i - 1, 所以最小下标为 max(i - k + 1, k - i - 1)
如果 i + k - 1 >= n, 那么最大下标为 (n << 1) - k - i - 1, 所以最大下标为 min(i + k - 1, (n << 1) - k - i - 1)
为了删除翻转的位置, 需要使用 uf.union(i, mx + 2)
这样就可以在每次访问 i 时, 直接跳到 i + 2
初始化时将 p 和 p + 2 合并, 然后将 banned 中的每个元素和 banned[i] + 2 合并
result 数组初始化为 -1, 然后将 result[p] 设置为 0
使用队列 q 存储访问的节点, 初始时将 p 入队
然后开始 BFS, 每次从队列中取出一个节点 i, 计算最小下标 mn 和最大下标 mx
从 mn 开始, 每次访问 i + 2, 直到 mx
将 result[j] 设置为 result[i] + 1,
将 j 入队, 并将 uf.union(j, mx + 2)
最后返回 result 数组
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++

class UF 
{
public:
    UF(int n) : parent(n) 
    {
        iota(parent.begin(), parent.end(), 0);
    }

    int find(int p) 
    {
        return parent[p] == p ? parent[p] : (parent[p] = find(parent[p]));
    }

    void merge(int p, int q) 
    {
        parent[find(p)] = find(q);
    }
private:
    vector<int> parent;
};

class Solution 
{
public:
    vector<int> minReverseOperations(int n, int p, vector<int>& banned, int k) 
    {
        UF uf(n + 2);
        uf.merge(p, p + 2); 
        for (const auto& i : banned) uf.merge(i, i + 2);
        vector<int> result(n, -1);
        result[p] = 0;
        queue<int> q;
        q.push(p);
        while (!q.empty()) 
        {
            for (int i = q.front(), mn = max(i - k + 1, k - i - 1), mx = min(i + k - 1, (n << 1) - k - i - 1), j = uf.find(mn); j <= mx; j = uf.find(j + 2)) 
            {
                result[j] = result[i] + 1;
                q.push(j);
                uf.merge(j, mx + 2);
            }
            q.pop();
        }
        return result;
    }
};
```

__Java__:

```Java
class UF {
    private final int[] parent;

    public UF(int n) {
        parent = new int[n];
        for (int i = 0; i < n; i++) parent[i] = i;
    }

    public int find(int p) {
        return parent[p] == p ? parent[p] : (parent[p] = find(parent[p]));
    }

    public void union(int p, int q) {
        parent[find(p)] = find(q);
    }
}

class Solution {
    public int[] minReverseOperations(int n, int p, int[] banned, int k) {
        UF uf = new UF(n + 2);
        uf.union(p, p + 2); 
        for (int i : banned) uf.union(i, i + 2);
        int[] result = new int[n];
        Arrays.fill(result, -1);
        result[p] = 0;
        Queue<Integer> q = new ArrayDeque<>();
        q.offer(p);
        while (!q.isEmpty()) {
            for (int i = q.poll(), mn = Math.max(i - k + 1, k - i - 1), mx = Math.min(i + k - 1, (n << 1) - k - i - 1), j = uf.find(mn); j <= mx; j = uf.find(j + 2)) {
                result[j] = result[i] + 1;
                q.offer(j);
                uf.union(j, mx + 2);
            }
        }
        return result;
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
        self.parent[self.find(p)] = self.find(q)

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
    def minReverseOperations(self, n: int, p: int, banned: List[int], k: int) -> List[int]:
        uf, result, q = UF(n + 2), [-1] * p + [0] + [-1] * (n - p - 1), deque([p])
        uf.union(p, p + 2)
        for i in banned:
            uf.union(i, i + 2)
        while q:
            j, mx = uf.find(mn := max((i := q.popleft()) - k + 1, k - i - 1)), min(i + k - 1, (n << 1) - k - i - 1)
            while j <= mx:
                result[j] = result[i] + 1
                q.append(j)
                uf.union(j, mx + 2) 
                j = uf.find(j + 2) 
        return result
```
