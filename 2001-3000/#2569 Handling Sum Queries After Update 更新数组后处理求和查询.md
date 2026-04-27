# 2569 Handling Sum Queries After Update 更新数组后处理求和查询

__Description:__

You are given two __0-indexed__ arrays `nums1` and `nums2` and a 2D array `queries` of queries. There are three types of queries:

1. For a query of type 1, `queries[i] = [1, l, r]`. Flip the values from `0` to `1` and from `1` to `0` in `nums1` from index `l` to index `r`. Both `l` and `r` are __0-indexed__.
2. For a query of type 2, `queries[i] = [2, p, 0]`. For every index `0 <= i < n`, set `nums2[i] = nums2[i] + nums1[i] * p`.
3. For a query of type 3, `queries[i] = [3, 0, 0]`. Find the sum of the elements in `nums2`.

Return _an array containing all the answers to the third type queries_.

__Example:__

Example 1:

```text
Input: nums1 = [1,0,1], nums2 = [0,0,0], queries = [[1,1,1],[2,1,0],[3,0,0]]
Output: [3]
Explanation: After the first query nums1 becomes [1,1,1]. After the second query, nums2 becomes [1,1,1], so the answer to the third query is 3. Thus, [3] is returned.
```

Example 2:

```text
Input: nums1 = [1], nums2 = [5], queries = [[2,0,0],[3,0,0]]
Output: [5]
Explanation: After the first query, nums2 remains [5], so the answer to the second query is 5. Thus, [5] is returned.
```

__Constraints:__

- `1 <= nums1.length,nums2.length <= 10 ^ 5`
- `nums1.length = nums2.length`
- `1 <= queries.length <= 10 ^ 5`
- `queries[i].length = 3`
- `0 <= l <= r <= nums1.length - 1`
- `0 <= p <= 10 ^ 6`
- `0 <= nums1[i] <= 1`
- `0 <= nums2[i] <= 10 ^ 9`

__题目描述:__

给你两个下标从 __0__ 开始的数组 `nums1` 和 `nums2` ，和一个二维数组 `queries` 表示一些操作。总共有 3 种类型的操作:

1. 操作类型 1 为 `queries[i] = [1, l, r]` 。你需要将 `nums1` 从下标 `l` 到下标 `r` 的所有 `0` 反转成 `1` 并且所有 `1` 反转成 `0` 。`l` 和 `r` 下标都从 __0__ 开始。
2. 操作类型 2 为 `queries[i] = [2, p, 0]` 。对于 `0 <= i < n` 中的所有下标，令 `nums2[i] = nums2[i] + nums1[i] * p` 。
3. 操作类型 3 为 `queries[i] = [3, 0, 0]` 。求 `nums2` 中所有元素的和。

请你返回一个 _数组_，包含 _所有第三种操作类型_ 的答案。

__示例:__

示例 1：

```text
输入：nums1 = [1,0,1], nums2 = [0,0,0], queries = [[1,1,1],[2,1,0],[3,0,0]]
输出：[3]
解释：第一个操作后 nums1 变为 [1,1,1] 。第二个操作后，nums2 变成 [1,1,1] ，所以第三个操作的答案为 3 。所以返回 [3] 。
```

示例 2：

```text
输入：nums1 = [1], nums2 = [5], queries = [[2,0,0],[3,0,0]]
输出：[5]
解释：第一个操作后，nums2 保持不变为 [5] ，所以第二个操作的答案是 5 。所以返回 [5] 。
```

__提示：__

- `1 <= nums1.length,nums2.length <= 10 ^ 5`
- `nums1.length = nums2.length`
- `1 <= queries.length <= 10 ^ 5`
- `queries[i].length = 3`
- `0 <= l <= r <= nums1.length - 1`
- `0 <= p <= 10 ^ 6`
- `0 <= nums1[i] <= 1`
- `0 <= nums2[i] <= 10 ^ 9`

__思路:__

```text
线段树
用带 lazy 标记线段树记录所有对 nums1 的翻转操作
每次翻转操作时, 将对应区间的节点标记为翻转状态
每次查询的时候将父节点的翻转状态传递到子节点
如果是累加操作, 则将 1 元素的个数乘以 p 加到 nums2 的和上
否则将 nums2 的和添加到结果中
时间复杂度为 O(N + QlogN), 空间复杂度为 O(N), 其中 N 为 nums1 的长度, Q 为查询的数量
```

__代码:__

__C++__:

```C++
class Solution 
{
    vector<int> cnt1, flip;

    void maintain(int o) 
    {
        cnt1[o] = cnt1[o << 1] + cnt1[(o << 1) + 1];
    }

    void do_(int o, int l, int r) 
    {
        cnt1[o] = r - l + 1 - cnt1[o];
        flip[o] = !flip[o];
    }

    void build(vector<int>& a, int o, int l, int r) 
    {
        if (l == r) 
        {
            cnt1[o] = a[l];
            return;
        }
        int m = (l + r) >> 1;
        build(a, o << 1, l, m);
        build(a, (o << 1) + 1, m + 1, r);
        maintain(o);
    }

    void update(int o, int l, int r, int L, int R) 
    {
        if (L <= l && r <= R) 
        {
            do_(o, l, r);
            return;
        }
        int m = (l + r) >> 1;
        if (flip[o]) 
        {
            do_(o << 1, l, m);
            do_((o << 1) + 1, m + 1, r);
            flip[o] = false;
        }
        if (m >= L) update(o << 1, l, m, L, R);
        if (m < R) update((o << 1) + 1, m + 1, r, L, R);
        maintain(o);
    }
public:
    vector<long long> handleQuery(vector<int>& nums1, vector<int>& nums2, vector<vector<int>>& queries) 
    {
        int n = nums1.size(), m = 2 << (32 - __builtin_clz(n));
        cnt1.resize(m);
        flip.resize(m);
        build(nums1, 1, 0, n - 1);
        vector<long long> result;
        long long s = accumulate(nums2.begin(), nums2.end(), 0LL);
        for (const auto& q : queries) 
        {
            if (q.front() == 1) update(1, 0, n - 1, q[1], q[2]);
            else if (q.front() == 2) s += (long long)q[1] * cnt1[1];
            else if (q.front() == 3) result.emplace_back(s);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long[] handleQuery(int[] nums1, int[] nums2, int[][] queries) {
        int n = nums1.length;
        long s = Arrays.stream(nums2).mapToLong(a -> a).sum();
        var result = new ArrayList<Long>();
        BitSet bitSet = new BitSet(n);
        for (int i = 0; i < n; i++) if (nums1[i] == 1) bitSet.set(i);
        for (int[] q : queries) {
            if (q[0] == 1) bitSet.flip(q[1], q[2] + 1);
            else if (q[0] == 2) s += (long)bitSet.cardinality() * q[1];
            else result.add(s);
        }
        return result.stream().mapToLong(a -> a).toArray();
    }
}
```

__Python__:

```Python
class Solution:
    def handleQuery(self, nums1: List[int], nums2: List[int], queries: List[List[int]]) -> List[int]:
        result, s, cnt1, flip = [], sum(nums2), [0] * (2 << (n := len(nums1)).bit_length()), [False] * (2 << n.bit_length())

        def maintain(o: int) -> None:
            cnt1[o] = cnt1[o << 1] + cnt1[(o << 1) + 1]

        def do(o: int, l: int, r: int) -> None:
            cnt1[o] = r - l + 1 - cnt1[o]
            flip[o] = not flip[o]

        def build(o: int, l: int, r: int) -> None:
            if l == r:
                cnt1[o] = nums1[l]
                return
            m = (l + r) >> 1
            build(o << 1, l, m)
            build((o << 1) + 1, m + 1, r)
            maintain(o)

        def update(o: int, l: int, r: int, L: int, R: int) -> None:
            if L <= l and r <= R:
                do(o, l, r)
                return
            m = (l + r) >> 1
            if flip[o]:
                do(o << 1, l, m)
                do((o << 1) + 1, m + 1, r)
                flip[o] = False
            if m >= L:
                update(o << 1, l, m, L, R)
            if m < R:
                update((o << 1) + 1, m + 1, r, L, R)
            maintain(o)

        build(1, 0, n - 1)
        for op, l, r in queries:
            if op == 1:
                update(1, 0, n - 1, l, r)
            elif op == 2:
                s += l * cnt1[1]
            elif op == 3:
                result.append(s)
        return result
```
