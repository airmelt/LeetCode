# 3165 Maximum Sum of Subsequence With Non-adjacent Elements 不包含相邻元素的子序列的最大和

__Description:__

You are given an array `nums` consisting of integers. You are also given a 2D array `queries`, where `queries[i] = [pos_i, x_i]`.

For query `i`, we first set `nums[pos_i]` equal to `x_i`, then we calculate the answer to query `i` which is the __maximum__ sum of a __subsequence__ of `nums` where __no two adjacent elements are selected__.

Return the _sum_ of the answers to all queries.

Since the final answer may be very large, return it __modulo__ `10 ^ 9 + 7`.

A __subsequence__ is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

__Example:__

Example 1:

```text
Input: nums = [3,5,9], queries = [[1,-2],[0,-3]]

Output: 21

Explanation:
After the 1st query, nums = [3,-2,9] and the maximum sum of a subsequence with non-adjacent elements is 3 + 9 = 12.
After the 2nd query, nums = [-3,-2,9] and the maximum sum of a subsequence with non-adjacent elements is 9.
```

Example 2:

```text
Input: nums = [0,-1], queries = [[0,-5]]

Output: 0

Explanation:
After the 1st query, nums = [-5,-1] and the maximum sum of a subsequence with non-adjacent elements is 0 (choosing an empty subsequence).
```

__Constraints:__

- `1 <= nums.length <= 5 * 10 ^ 4`
- `-10 ^ 5 <= nums[i] <= 10 ^ 5`
- `1 <= queries.length <= 5 * 10 ^ 4`
- `queries[i] == [pos_i, x_i]`
- `0 <= pos_i <= nums.length - 1`
- `-10 ^ 5 <= x_i <= 10 ^ 5`

__题目描述:__

给你一个整数数组 `nums` 和一个二维数组 `queries`，其中 `queries[i] = [pos_i, x_i]`。

对于每个查询 `i`，首先将 `nums[pos_i]` 设置为 `x_i`，然后计算查询 `i` 的答案，该答案为 `nums` 中 不包含相邻元素 的 __子序列__ 的 __最大__ 和。

返回所有查询的答案之和。

由于最终答案可能非常大，返回其对 `10 ^ 9 + 7` __取余__ 的结果。

__子序列__ 是指从另一个数组中删除一些或不删除元素而不改变剩余元素顺序得到的数组。

__示例:__

示例 1：

```text
输入：nums = [3,5,9], queries = [[1,-2],[0,-3]]

输出：21

解释：
执行第 1 个查询后，nums = [3,-2,9]，不包含相邻元素的子序列的最大和为 3 + 9 = 12。
执行第 2 个查询后，nums = [-3,-2,9]，不包含相邻元素的子序列的最大和为 9 。
```

示例 2：

```text
输入：nums = [0,-1], queries = [[0,-5]]

输出：0

解释：
执行第 1 个查询后，nums = [-5,-1]，不包含相邻元素的子序列的最大和为 0（选择空子序列）。
```

__提示：__

- `1 <= nums.length <= 5 * 10 ^ 4`
- `-10 ^ 5 <= nums[i] <= 10 ^ 5`
- `1 <= queries.length <= 5 * 10 ^ 4`
- `queries[i] == [pos_i, x_i]`
- `0 <= pos_i <= nums.length - 1`
- `-10 ^ 5 <= x_i <= 10 ^ 5`

__思路:__

```text
线段树
考虑打家劫舍的方式
将数组分治
由分治的数组选与不选可以得到
tree[p] = { max(a[0] + b[2], a[1] + b[0]), max(a[0] + b[3], a[1] + b[1]), max(a[2] + b[2], a[3] + b[0]), max(a[2] + b[3], a[3] + b[1]) }
其中数组 a 和 b 分别是分治的两个数组, p 为当前节点
当更新到当前节点时 tree[p][3] = max(nums[l], 0), 其余均初始化为 0
单点修改可以使用线段树的 update 方法
区间查询即为根节点的值
时间复杂度为 O(N + MlogN), 空间复杂度为 O(N), 其中 N 为数组 nums 的大小, M 为数组 queries 的大小
```

__代码:__

__C++__:

```C++
class Solution 
{
    void maintain(vector<array<unsigned int, 4>>& tree, int p) 
    {
        auto& a = tree[p << 1];
        auto& b = tree[(p << 1) + 1];
        tree[p] = { max(a[0] + b[2], a[1] + b[0]), max(a[0] + b[3], a[1] + b[1]), max(a[2] + b[2], a[3] + b[0]), max(a[2] + b[3], a[3] + b[1]) };
    }

    void build(vector<array<unsigned int, 4>>& tree, vector<int>& nums, int p, int l, int r) 
    {
        if (l == r) 
        {
            tree[p][3] = max(nums[l], 0);
            return;
        }
        int mid = l + ((r - l) >> 1);
        build(tree, nums, p << 1, l, mid);
        build(tree, nums, (p << 1) + 1, mid + 1, r);
        maintain(tree, p);
    };

    void update(vector<array<unsigned int, 4>>& tree, int p, int l, int r, int i, int val) 
    {
        if (l == r) 
        {
            tree[p][3] = max(val, 0);
            return;
        }
        int mid = l + ((r - l) >> 1);
        if (i <= mid) update(tree, p << 1, l, mid, i, val);
        else update(tree, (p << 1) + 1, mid + 1, r, i, val);
        maintain(tree, p);
    };
public:
    int maximumSumSubsequence(vector<int>& nums, vector<vector<int>>& queries) 
    {
        int n = nums.size();
        vector<array<unsigned int, 4>> tree(2 << (32 - __builtin_clz(n)));
        build(tree, nums, 1, 0, n - 1);
        long long result = 0LL, MOD = 1e9 + 7LL;
        for (const auto& query : queries)
        {
            update(tree, 1, 0, n - 1, query.front(), query.back());
            result += tree[1].back();
        }
        return result % MOD;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumSumSubsequence(int[] nums, int[][] queries) {
        int n = nums.length;
        long tree[][] = new long[2 << (32 - Integer.numberOfLeadingZeros(n))][4], result = 0L, MOD = 1_000_000_007L;
        build(tree, nums, 1, 0, n - 1);
        for (var query : queries) {
            update(tree, 1, 0, n - 1, query[0], query[1]);
            result += tree[1][3];
        }
        return (int) (result % MOD);
    }

    private void maintain(long[][] tree, int p) {
        long a[] = tree[p << 1], b[] = tree[(p << 1) + 1];
        tree[p][0] = Math.max(a[0] + b[2], a[1] + b[0]);
        tree[p][1] = Math.max(a[0] + b[3], a[1] + b[1]);
        tree[p][2] = Math.max(a[2] + b[2], a[3] + b[0]);
        tree[p][3] = Math.max(a[2] + b[3], a[3] + b[1]);
    }

    private void build(long[][] tree, int[] nums, int p, int l, int r) {
        if (l == r) {
            tree[p][3] = Math.max(nums[l], 0);
            return;
        }
        int mid = l + ((r - l) >>> 1);
        build(tree, nums, p << 1, l, mid);
        build(tree, nums, (p << 1) + 1, mid + 1, r);
        maintain(tree, p);
    }

    private void update(long[][] tree, int p, int l, int r, int i, int val) {
        if (l == r) {
            tree[p][3] = Math.max(val, 0);
            return;
        }
        int mid = l + ((r - l) >>> 1);
        if (i <= mid) update(tree, p << 1, l, mid, i, val);
        else update(tree, (p << 1) + 1, mid + 1, r, i, val);
        maintain(tree, p);
    }
}
```

__Python__:

```Python
class Solution:
    def maximumSumSubsequence(self, nums: List[int], queries: List[List[int]]) -> int:
        n = len(nums)
        tree, result = [[0] * 4 for _ in range(2 << n.bit_length())], 0

        def maintain(p: int) -> NoReturn:
            a, b = tree[p << 1], tree[(p << 1) + 1]
            tree[p] = [max(a[0] + b[2], a[1] + b[0]), max(a[0] + b[3], a[1] + b[1]), max(a[2] + b[2], a[3] + b[0]), max(a[2] + b[3], a[3] + b[1])]

        def build(p: int, l: int, r: int) -> NoReturn:
            if l == r:
                tree[p][3] = max(nums[l], 0)
                return
            build(p << 1, l, mid := l + ((r - l) >> 1))
            build((p << 1) + 1, mid + 1, r)
            maintain(p)
        
        def update(p: int, l: int, r: int, i: int, val: int) -> NoReturn:
            if l == r:
                tree[p][3] = max(val, 0)
                return
            if i <= (mid := l + ((r - l) >> 1)):
                update(p << 1, l, mid, i, val)
            else:
                update((p << 1) + 1, mid + 1, r, i, val)
            maintain(p)

        build(1, 0, n - 1)
        for i, val in queries:
            update(1, 0, n - 1, i, val)
            result += tree[1][3]
        return result % (10 ** 9 + 7)
```
