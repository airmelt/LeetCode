# 3161 Block Placement Queries 物块放置查询

__Description:__

There exists an infinite number line, with its origin at 0 and extending towards the __positive__ x-axis.

You are given a 2D array `queries`, which contains two types of queries:

1. For a query of type 1, `queries[i] = [1, x]`. Build an obstacle at distance `x` from the origin. It is guaranteed that there is __no__ obstacle at distance `x` when the query is asked.
2. For a query of type 2, `queries[i] = [2, x, sz]`. Check if it is possible to place a block of size `sz` _anywhere_ in the range `[0, x]` on the line, such that the block __entirely__ lies in the range `[0, x]`. A block __cannot__ be placed if it intersects with any obstacle, but it may touch it. Note that you do __not__ actually place the block. Queries are separate.

Return a boolean array `results`, where `results[i]` is `true` if you can place the block specified in the `i ^ th` query of type 2, and `false` otherwise.

__Example:__

Example 1:

![3161-1](https://assets.leetcode.com/uploads/2024/04/22/example0block.png)

```text
Input: queries = [[1,2],[2,3,3],[2,3,1],[2,2,2]]

Output: [false,true,true]

Explanation:

For query 0, place an obstacle at x = 2. A block of size at most 2 can be placed before x = 3.
```

Example 2:

![3161-2](https://assets.leetcode.com/uploads/2024/04/22/example1block.png)

```text
Input: queries = [[1,7],[2,7,6],[1,2],[2,7,5],[2,7,6]]

Output: [true,true,false]

Explanation:

Place an obstacle at x = 7 for query 0. A block of size at most 7 can be placed before x = 7.
Place an obstacle at x = 2 for query 2. Now, a block of size at most 5 can be placed before x = 7, and a block of size at most 2 before x = 2.
```

__Constraints:__

- `1 <= queries.length <= 15 * 10 ^ 4`
- `2 <= queries[i].length <= 3`
- `1 <= queries[i][0] <= 2`
- `1 <= x, sz <= min(5 * 10 ^ 4, 3 * queries.length)`
- The input is generated such that for queries of type 1, no obstacle exists at distance `x` when the query is asked.
- The input is generated such that there is at least one query of type 2.

__题目描述:__

有一条无限长的数轴，原点在 0 处，沿着 x 轴 __正__ 方向无限延伸。

给你一个二维数组 `queries` ，它包含两种操作：

1. 操作类型 1 ：`queries[i] = [1, x]` 。在距离原点 `x` 处建一个障碍物。数据保证当操作执行的时候，位置 `x` 处 __没有__ 任何障碍物。
2. 操作类型 2 ：`queries[i] = [2, x, sz]` 。判断在数轴范围 `[0, x]` 内是否可以放置一个长度为 `sz` 的物块，这个物块需要 __完全__ 放置在范围 `[0, x]` 内。如果物块与任何障碍物有重合，那么这个物块 __不能__ 被放置，但物块可以与障碍物刚好接触。注意，你只是进行查询，并 __不是__ 真的放置这个物块。每个查询都是相互独立的。

请你返回一个 boolean 数组 `results` ，如果第 `i` 个操作类型 2 的操作你可以放置物块，那么 `results[i]` 为 `true` ，否则为 `false` 。

__示例:__

示例 1：

![3161-3](https://assets.leetcode.com/uploads/2024/04/22/example0block.png)

```text
输入：queries = [[1,2],[2,3,3],[2,3,1],[2,2,2]]

输出：[false,true,true]

解释：

查询 0 ，在 x = 2 处放置一个障碍物。在 x = 3 之前任何大小不超过 2 的物块都可以被放置。
```

示例 2：

![3161-4](https://assets.leetcode.com/uploads/2024/04/22/example1block.png)

```text
输入：queries = [[1,7],[2,7,6],[1,2],[2,7,5],[2,7,6]]

输出：[true,true,false]

解释：

查询 0 在 x = 7 处放置一个障碍物。在 x = 7 之前任何大小不超过 7 的物块都可以被放置。
查询 2 在 x = 2 处放置一个障碍物。现在，在 x = 7 之前任何大小不超过 5 的物块可以被放置，x = 2 之前任何大小不超过 2 的物块可以被放置。
```

__提示：__

- `1 <= queries.length <= 15 * 10 ^ 4`
- `2 <= queries[i].length <= 3`
- `1 <= queries[i][0] <= 2`
- `1 <= x, sz <= min(5 * 10 ^ 4, 3 * queries.length)`
- 输入保证操作 1 中，`x` 处不会有障碍物。
- 输入保证至少有一个操作类型 2 。

__思路:__

```text
数状数组
由于需要计算数组的最大前缀和
增加障碍反而会使得最大前缀和减少
可以反向遍历数组, 那么增加障碍就变成移除障碍
障碍可以记录在有序数组中
设 d[x] 表示以 x 结尾的数组到左侧距离最近的障碍的距离
可以在最后增加一个哨兵为 x 的最大值加 1
设 x 左右的障碍分别为 pre, nxt
添加和删除的时候每次需要 update(nxt, nxt - pre) 
每次查询最大前缀和即可得到是否能够插入
时间复杂度为 O(M + NlogM), 空间复杂度为 O(M), 其中 N 为数组 queries 的长度, M 为 x 的最大值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<bool> getResults(vector<vector<int>>& queries) 
    {
        int m = 0;
        vector<int> pos = {0};
        for (const auto& q : queries) 
        {
            m = max(m, q[1] + 1);
            if (q.front() == 1) pos.push_back(q[1]);
        }
        sort(pos.begin(), pos.end());
        set<int> st(pos.begin(), pos.end());
        st.insert(m);
        tree.resize(m);
        for (int i = 1; i < pos.size(); i++) update(pos[i], pos[i] - pos[i - 1]);
        vector<bool> result;
        for (int i = queries.size() - 1; i >= 0; i--) 
        {
            auto& q = queries[i];
            int x = q[1];
            auto it = st.lower_bound(x);
            int pre = *prev(it);
            if (q.front() == 1) 
            {
                int nxt = *next(it);
                st.erase(it);
                update(nxt, nxt - pre);
            } 
            else result.push_back(max(pre_max(pre), x - pre) >= q.back());
        }
        reverse(result.begin(), result.end());
        return result;
    }
private:
    vector<int> tree;

    void update(int i, int val) 
    {
        for (int n = tree.size(); i < n; i += i & -i) tree[i] = max(tree[i], val);
    }

    int pre_max(int i) 
    {
        int result = 0;
        for (; i; i &= i - 1) result = max(result, tree[i]);
        return result;
    }
};
```

__Java__:

```Java
class Fenwick {
    private final int[] tree;
    private int size;

    public Fenwick(int size) {
        tree = new int[size];
        this.size = size;
    }

    public void update(int i, int val) {
        for (; i < size; i += i & -i) tree[i] = Math.max(tree[i], val);
    }

    public int preMax(int i) {
        int result = 0;
        for (; i > 0; i &= i - 1) result = Math.max(result, tree[i]);
        return result;
    }
}

class Solution {
    public static List<Boolean> getResults(int[][] queries) {
        int m = 0;
        List<Integer> pos = new ArrayList<>();
        pos.add(0);
        for (int[] q : queries) {
            m = Math.max(m, q[1] + 1);
            if (q[0] == 1) pos.add(q[1]);
        }
        Collections.sort(pos);
        TreeSet<Integer> set = new TreeSet<>(pos);
        set.add(m);
        Fenwick t = new Fenwick(m);
        for (int i = 1; i < pos.size(); i++) t.update(pos.get(i), pos.get(i) - pos.get(i - 1));
        List<Boolean> result = new ArrayList<>();
        for (int i = queries.length - 1; i >= 0; i--) {
            int q[] = queries[i], x = q[1], pre = set.floor(x - 1), nxt = 0; 
            if (q[0] == 1) {
                set.remove(x);
                t.update(nxt = set.ceiling(x), nxt - pre);
            } else result.add(Math.max(t.preMax(pre), x - pre) >= q[2]);
        }
        Collections.reverse(result);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def getResults(self, queries: List[List[int]]) -> List[bool]:
        tree, pos = [0] * (m := max(q[1] for q in queries) + 1), [0] + sorted([q[1] for q in queries if q[0] == 1])

        def update(i: int, val: int) -> NoReturn:
            while i < m:
                tree[i] = max(tree[i], val)
                i += i & -i

        def pre_max(i: int) -> int:
            result = 0
            while i:
                result = max(result, tree[i])
                i &= i - 1
            return result
        
        for p, q in pairwise(pos):
            update(q, q - p)
        sl, result = SortedList(pos + [m]), []
        for q in queries[::-1]:
            pre = sl[(i := sl.bisect_left(x := q[1])) - 1]
            if q[0] == 1:
                sl.discard(x)
                update(sl[i], sl[i] - pre)
            else:
                result.append(max(pre_max(pre), x - pre) >= q[2])
        return result[::-1]
```
