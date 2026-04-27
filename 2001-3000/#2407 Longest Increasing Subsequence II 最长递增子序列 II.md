# 2407 Longest Increasing Subsequence II 最长递增子序列 II

__Description:__

You are given an integer array `nums` and an integer `k`.

Find the longest subsequence of `nums` that meets the following requirements:

- The subsequence is __strictly increasing__ and
- The difference between adjacent elements in the subsequence is __at most__ `k`.

Return the length of the longest subsequence that meets the requirements.

A subsequence is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

__Example:__

Example 1:

```text
Input: nums = [4,2,1,4,3,4,5,8,15], k = 3
Output: 5
Explanation:
The longest subsequence that meets the requirements is [1,3,4,5,8].
The subsequence has a length of 5, so we return 5.
Note that the subsequence [1,3,4,5,8,15] does not meet the requirements because 15 - 8 = 7 is larger than 3.
```

Example 2:

```text
Input: nums = [7,4,5,1,8,12,4,7], k = 5
Output: 4
Explanation:
The longest subsequence that meets the requirements is [4,5,8,12].
The subsequence has a length of 4, so we return 4.
```

Example 3:

```text
Input: nums = [1,5], k = 1
Output: 1
Explanation:
The longest subsequence that meets the requirements is [1].
The subsequence has a length of 1, so we return 1.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i], k <= 10 ^ 5`

__题目描述:__

给你一个整数数组 `nums` 和一个整数 `k` 。

找到 `nums` 中满足以下要求的最长子序列:

- 子序列 __严格递增__
- 子序列中相邻元素的差值 __不超过__ `k` 。

请你返回满足上述要求的 最长子序列 的长度。

子序列 是从一个数组中删除部分元素后，剩余元素不改变顺序得到的数组。

__示例:__

示例 1：

```text
输入：nums = [4,2,1,4,3,4,5,8,15], k = 3
输出：5
解释：
满足要求的最长子序列是 [1,3,4,5,8] 。
子序列长度为 5 ，所以我们返回 5 。
注意子序列 [1,3,4,5,8,15] 不满足要求，因为 15 - 8 = 7 大于 3 。
```

示例 2：

```text
输入：nums = [7,4,5,1,8,12,4,7], k = 5
输出：4
解释：
满足要求的最长子序列是 [4,5,8,12] 。
子序列长度为 4 ，所以我们返回 4 。
```

示例 3：

```text
输入：nums = [1,5], k = 1
输出：1
解释：
满足要求的最长子序列是 [1] 。
子序列长度为 1 ，所以我们返回 1 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i], k <= 10 ^ 5`

__思路:__

```text
动态规划 ➕ 线段树
设 dp[i][j] 表示以 j 结尾的 nums[:i] 的最长递增子序列的长度
dp[i][j] = max(dp[i][j], dp[i - 1][k] + 1) for k in range(max(0, j - k), j)
使用线段树维护区间最大值
时间复杂度为 O(NlogM), 空间复杂度为 O(M), 其中 M 为 nums 中的最大值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int lengthOfLIS(vector<int>& nums, int k) 
    {
        int n = *max_element(nums.begin(), nums.end());
        vector<int> dp(n << 2);
        function<void(int, int, int, int, int)> modify = [&](int o, int l, int r, int i, int val) -> void
        {
            if (l == r) 
            {
                dp[o] = val;
                return;
            }
            int m = (l + r) >> 1;
            if (i <= m) modify(o << 1, l, m, i, val);
            else modify((o << 1) + 1, m + 1, r, i, val);
            dp[o] = max(dp[o << 1], dp[(o << 1) + 1]);
        };
        function<int(int, int, int, int, int)> query = [&](int o, int l, int r, int L, int R) -> int
        {
            if (L <= l and r <= R) return dp[o];
            int result = 0, m = (l + r) >> 1;
            if (L <= m) result = query(o << 1, l, m, L, R);
            if (R > m) result = max(result, query((o << 1) + 1, m + 1, r, L, R));
            return result;
        };
        for (const auto& x : nums) modify(1, 1, n, x, 1 + (x == 1 ? 0 : query(1, 1, n, max(x - k, 1), x - 1)));
        return dp[1];
    }
};
```

__Java__:

```Java
class Solution {
    int[] dp;

    public int lengthOfLIS(int[] nums, int k) {
        int n = Arrays.stream(nums).max().getAsInt();
        dp = new int[n << 2];
        for (var x : nums) modify(1, 1, n, x, 1 + (x == 1 ? 0 : query(1, 1, n, Math.max(x - k, 1), x - 1)));
        return dp[1];
    }

    private void modify(int o, int l, int r, int i, int val) {
        if (l == r) {
            dp[o] = val;
            return;
        }
        int m = (l + r) >> 1;
        if (i <= m) modify(o << 1, l, m, i, val);
        else modify((o << 1) + 1, m + 1, r, i, val);
        dp[o] = Math.max(dp[o << 1], dp[(o << 1) + 1]);
    }

    private int query(int o, int l, int r, int L, int R) { 
        if (L <= l && r <= R) return dp[o];
        int result = 0, m = (l + r) >> 1;
        if (L <= m) result = query(o << 1, l, m, L, R);
        if (R > m) result = Math.max(result, query((o << 1) + 1, m + 1, r, L, R));
        return result;
    }
}
```

__Python__:

```Python
class SegmentTree:
    def __init__(self, data: List[Any], merge: Callable = max) -> None:
        self.data = data
        self.n = len(data)
        self.tree = [None] * (self.n << 2)
        self._merge = merge
        if self.n:
            self._build(0, 0, self.n - 1)

    def query(self, ql: int, qr: int) -> Any:
        return self._query(0, 0, self.n - 1, ql, qr)

    def update(self, index: int, value: Any) -> None:
        self.data[index] = value
        self._update(0, 0, self.n - 1, index)

    def _build(self, tree_index: int, left: int, right: int) -> None:
        if left == right:
            self.tree[tree_index] = self.data[left]
            return
        mid = left + ((right - left) >> 1)
        left_index, right_index = (tree_index << 1) + 1, (tree_index << 1) + 2
        self._build(left_index, left, mid)
        self._build(right_index, mid + 1, right)
        self.tree[tree_index] = self._merge(self.tree[left_index], self.tree[right_index])

    def _query(self, tree_index: int, left: int, right: int, ql: int, qr: int) -> Any:
        if left == ql and right == qr:
            return self.tree[tree_index]
        mid = left + ((right - left) >> 1)
        left_index, right_index = (tree_index << 1) + 1, (tree_index << 1) + 2
        if qr <= mid:
            return self._query(left_index, left, mid, ql, qr)
        elif ql > mid:
            return self._query(right_index, mid + 1, right, ql, qr)
        return self._merge(self._query(left_index, left, mid, ql, mid), 
                           self._query(right_index, mid + 1, right, mid + 1, qr))

    def _update(self, tree_index: int, left: int, right: int, index: int) -> None:
        if left == right == index:
            self.tree[tree_index] = self.data[index]
            return
        mid = left + ((right - left) >> 1)
        left_index, right_index = (tree_index << 1) + 1, (tree_index << 1) + 2
        if index > mid:
            self._update(right_index, mid + 1, right, index)
        else:
            self._update(left_index, left, mid, index)
        self.tree[tree_index] = self._merge(self.tree[left_index], self.tree[right_index])
        
class Solution:
    def lengthOfLIS(self, nums: List[int], k: int) -> int:
        tree = SegmentTree([0] * (10 ** 5 + 1))
        for x in nums:
            note = tree.query(max(0, x - k), x - 1)
            tree.update(x, note + 1)
        return tree.query(0, 10 ** 5)
```
