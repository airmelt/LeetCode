# 3187 Peaks in Array 数组中的峰值

__Description:__

A __peak__ in an array `arr` is an element that is __greater__ than its previous and next element in `arr`.

You are given an integer array `nums` and a 2D integer array `queries`.

You have to process queries of two types:

- `queries[i] = [1, li, ri]`, determine the count of __peak__ elements in the __subarray__ `nums[l_i..r_i]`.
- `queries[i] = [2, index_i, val_i]`, change `nums[index_i]` to `val_i`.

Return an array `answer` containing the results of the queries of the first type in order.

__Notes:__

- The __first__ and the __last__ element of an array or a subarray __cannot__ be a peak.

__Example:__

Example 1:

```text
Input: nums = [3,1,4,2,5], queries = [[2,3,4],[1,0,4]]

Output: [0]

Explanation:

First query: We change nums[3] to 4 and nums becomes [3,1,4,4,5].

Second query: The number of peaks in the [3,1,4,4,5] is 0.
```

Example 2:

```text
Input: nums = [4,1,4,2,1,5], queries = [[2,2,4],[1,0,2],[1,0,4]]

Output: [0,1]

Explanation:

First query: nums[2] should become 4, but it is already set to 4.

Second query: The number of peaks in the [4,1,4] is 0.

Third query: The second 4 is a peak in the [4,1,4,2,1].
```

__Constraints:__

- `3 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`
- `1 <= queries.length <= 10 ^ 5`
- `queries[i][0] == 1` or `queries[i][0] == 2`
- For all `i` that:
  - `queries[i][0] == 1`: `0 <= queries[i][1] <= queries[i][2] <= nums.length - 1`
  - `queries[i][0] == 2`: `0 <= queries[i][1] <= nums.length - 1, 1 <= queries[i][2] <= 10 ^ 5`

__题目描述:__

数组 `arr` 中 __大于__ 前面和后面相邻元素的元素被称为 __峰值__ 元素。

给你一个整数数组 `nums` 和一个二维整数数组 `queries` 。

你需要处理以下两种类型的操作：

- `queries[i] = [1, li, ri]` ，求出子数组 `nums[l_i..r_i]` 中 __峰值__ 元素的数目。
- `queries[i] = [2, index_i, val_i]` ，将 `nums[index_i]` 变为 `val_i` 。

请你返回一个数组 `answer` ，它依次包含每一个第一种操作的答案。

__注意：__

子数组中 __第一个__ 和 __最后一个__ 元素都 __不是__ 峰值元素。

__示例:__

示例 1：

```text
输入：nums = [3,1,4,2,5], queries = [[2,3,4],[1,0,4]]

输出：[0]

解释：

第一个操作：我们将 nums[3] 变为 4 ，nums 变为 [3,1,4,4,5] 。

第二个操作：[3,1,4,4,5] 中峰值元素的数目为 0 。
```

示例 2：

```text
输入：nums = [4,1,4,2,1,5], queries = [[2,2,4],[1,0,2],[1,0,4]]

输出：[0,1]

解释：

第一个操作：nums[2] 变为 4 ，它已经是 4 了，所以保持不变。

第二个操作：[4,1,4] 中峰值元素的数目为 0 。

第三个操作：第二个 4 是 [4,1,4,2,1] 中的峰值元素。
```

__提示：__

- `3 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`
- `1 <= queries.length <= 10 ^ 5`
- `queries[i][0] == 1` 或者 `queries[i][0] == 2`
- 对于所有的 `i` ，都有：
  - `queries[i][0] == 1`: `0 <= queries[i][1] <= queries[i][2] <= nums.length - 1`
  - `queries[i][0] == 2`: `0 <= queries[i][1] <= nums.length - 1, 1 <= queries[i][2] <= 10 ^ 5`

__思路:__

```text
树状数组
如果 nums[i - 1] < nums[i] and nums[i] > nums[i + 1], 则记录 i 为 1, 否则记录 i 为 0
那么求峰值的个数就转化为求 nums 对应的前缀和
由于子数组的第一个和最后一个一定不是峰值
所以只需要记录 n - 1 个数即可
查询的时候按照前缀和返回 pre(r) - pre(l - 1) 即可
更新元素的时候
先撤销 [i - 1, i + 1] 中的树状数组的值, 区间实际上为 [max(i - 1, 1), min(i + 1, n - 1)]
更新 nums 对应的元素
之后再更新 [i - 1, i + 1] 的树状数组的值
时间复杂度为 O((N + Q)logN), 空间复杂度为 O(N), 其中 N 为数组 nums 的长度, Q 为数组 queries 的长度
```

__代码:__

__C++__:

```C++
class Fenwick
{
public:
    Fenwick(int _n) : tree(n), n(_n) {}

    void update(int i, int val)
    {
        for (; i < n; i += i & -i) tree[i] += val;
    }

    int query(int l, int r)
    {
        return r < l ? 0 : pre(r) - pre(l - 1);
    }
private:
    int n;
    vector<int> tree;

    int pre(int i)
    {
        int result = 0;
        for (; i > 0; i &= i - 1) result += tree[i];
        return result;
    }
};
class Solution 
{
public:
    vector<int> countOfPeaks(vector<int>& nums, vector<vector<int>>& queries)
    {
        int n = nums.size();
        Fenwick tree(n - 1);
        auto update = [&](int i, int val) -> void
        {
            if (nums[i - 1] < nums[i] and nums[i] > nums[i + 1]) tree.update(i, val);
        };
        for (int i = 1; i < n - 1; i++) update(i, 1);  
        vector<int> result;
        for (const auto& query : queries)
        {
            if (query.front() == 1) result.emplace_back(tree.query(query[1] + 1, query[2] - 1));
            else
            {
                for (int j = max(query[1] - 1, 1); j < min(query[1] + 2, n - 1); j++) update(j, -1);
                nums[query[1]] = query[2];
                for (int j = max(query[1] - 1, 1); j < min(query[1] + 2, n - 1); j++) update(j, 1);
            }
        } 
        return result;
    }
};
```

__Java__:

```Java
class Fenwick {
    private final int[] tree;
    private int n;

    Fenwick(int n) {
        this.n = n;
        tree = new int[n];
    }

    public void update(int i, int val) {
        for (; i < n; i += i & -i) tree[i] += val;
    }

    private int pre(int i) {
        int result = 0;
        for (; i > 0; i &= i - 1) result += tree[i];
        return result;
    }

    public int query(int l, int r) {
        return r < l ? 0 : pre(r) - pre(l - 1);
    }
}
class Solution {
    public List<Integer> countOfPeaks(int[] nums, int[][] queries) {
        int n = nums.length;
        Fenwick tree = new Fenwick(n - 1);
        for (int i = 1; i < n - 1; i++) update(tree, nums, i, 1);
        List<Integer> result = new ArrayList<>();
        for (var query : queries) {
            if (query[0] == 1) result.add(tree.query(query[1] + 1, query[2] - 1));
            else {
                for (int j = Math.max(query[1] - 1, 1); j < Math.min(query[1] + 2, n - 1); j++) update(tree, nums, j, -1);
                nums[query[1]] = query[2];
                for (int j = Math.max(query[1] - 1, 1); j < Math.min(query[1] + 2, n - 1); j++) update(tree, nums, j, 1);
            }
        }
        return result;
    }

    private void update(Fenwick tree, int[] nums, int i, int val) {
        if (nums[i - 1] < nums[i] && nums[i] > nums[i + 1]) tree.update(i, val);
    }
}
```

__Python__:

```Python
class Fenwick:
    __slots__ = 'tree', 'n'

    def __init__(self, n: int):
        self.tree = [0] * n
        self.n = n

    def update(self, i: int, val: int) -> None:
        while i < self.n:
            self.tree[i] += val
            i += i & -i

    def pre(self, i: int) -> int:
        result = 0
        while i > 0:
            result += self.tree[i]
            i &= i - 1
        return result

    def query(self, l: int, r: int) -> int:
        return 0 if r < l else self.pre(r) - self.pre(l - 1)

class Solution:
    def countOfPeaks(self, nums, queries):
        tree, result = Fenwick((n := len(nums)) - 1), []

        def update(i: int, val: int) -> None:
            if nums[i - 1] < nums[i] and nums[i] > nums[i + 1]:
                tree.update(i, val)
        for i in range(1, n - 1):
            update(i, 1)
        for op, i, val in queries:
            if op == 1:
                result.append(tree.query(i + 1, val - 1))
                continue
            for j in range(max(i - 1, 1), min(i + 2, n - 1)):
                update(j, -1)
            nums[i] = val
            for j in range(max(i - 1, 1), min(i + 2, n - 1)):
                update(j, 1)
        return result
```
