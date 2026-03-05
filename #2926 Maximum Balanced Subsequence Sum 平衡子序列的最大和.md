# 2926 Maximum Balanced Subsequence Sum 平衡子序列的最大和

__Description:__

You are given a __0-indexed__ integer array `nums`.

A __subsequence__ of `nums` having length `k` and consisting of __indices__ `i_0 < i_1 < ... < i_k-1` is __balanced__ if the following holds:

- `nums[i_j] - nums[i_j-1] >= i_j - i_j-1`, for every `j` in the range `[1, k - 1]`.

A __subsequence__ of `nums` having length `1` is considered balanced.

Return _an integer denoting the __maximum__ possible __sum of elements__ in a __balanced__ subsequence of_ `nums`.

A subsequence of an array is a new non-empty array that is formed from the original array by deleting some (possibly none) of the elements without disturbing the relative positions of the remaining elements.

__Example:__

Example 1:

```text
Input: nums = [3,3,5,6]
Output: 14
Explanation: In this example, the subsequence [3,5,6] consisting of indices 0, 2, and 3 can be selected.
nums[2] - nums[0] >= 2 - 0.
nums[3] - nums[2] >= 3 - 2.
Hence, it is a balanced subsequence, and its sum is the maximum among the balanced subsequences of nums.
The subsequence consisting of indices 1, 2, and 3 is also valid.
It can be shown that it is not possible to get a balanced subsequence with a sum greater than 14.
```

Example 2:

```text
Input: nums = [5,-1,-3,8]
Output: 13
Explanation: In this example, the subsequence [5,8] consisting of indices 0 and 3 can be selected.
nums[3] - nums[0] >= 3 - 0.
Hence, it is a balanced subsequence, and its sum is the maximum among the balanced subsequences of nums.
It can be shown that it is not possible to get a balanced subsequence with a sum greater than 13.
```

Example 3:

```text
Input: nums = [-2,-1]
Output: -1
Explanation: In this example, the subsequence [-1] can be selected.
It is a balanced subsequence, and its sum is the maximum among the balanced subsequences of nums.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。

`nums` 一个长度为 `k` 的 __子序列__ 指的是选出 `k` 个 __下标__ `i_0 < i_1 < ... < i_k-1` ，如果这个子序列满足以下条件，我们说它是 __平衡的__ :

- 对于范围 `[1, k - 1]` 内的所有 `j` ， `nums[i_j] - nums[i_j-1] >= i_j - i_j-1` 都成立。

`nums` 长度为 `1` 的 __子序列__ 是平衡的。

请你返回一个整数，表示 `nums` __平衡__ 子序列里面的 __最大元素和__ 。

一个数组的 子序列 指的是从原数组中删除一些元素（也可能一个元素也不删除）后，剩余元素保持相对顺序得到的 非空 新数组。

__示例:__

示例 1：

```text
输入：nums = [3,3,5,6]
输出：14
解释：这个例子中，选择子序列 [3,5,6] ，下标为 0 ，2 和 3 的元素被选中。
nums[2] - nums[0] >= 2 - 0 。
nums[3] - nums[2] >= 3 - 2 。
所以，这是一个平衡子序列，且它的和是所有平衡子序列里最大的。
包含下标 1 ，2 和 3 的子序列也是一个平衡的子序列。
最大平衡子序列和为 14 。
```

示例 2：

```text
输入：nums = [5,-1,-3,8]
输出：13
解释：这个例子中，选择子序列 [5,8] ，下标为 0 和 3 的元素被选中。
nums[3] - nums[0] >= 3 - 0 。
所以，这是一个平衡子序列，且它的和是所有平衡子序列里最大的。
最大平衡子序列和为 13 。
```

示例 3：

```text
输入：nums = [-2,-1]
输出：-1
解释：这个例子中，选择子序列 [-1] 。
这是一个平衡子序列，而且它的和是 nums 所有平衡子序列里最大的。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`

__思路:__

```text
动态规划 ➕ 树状数组
首先将 nums[i] - nums[j] >= i - j
变形为 nums[i] - i >= nums[j] - j
这样就转换成了求 nums[i] - i 的最长上升子序列
设 dp[i] 表示以 i 结尾的最长上升子序列的子序列和
那么 dp[i] = max(dp[j], 0) + nums[i], 其中 j 是满足 nums[i] - i >= nums[j] - j 的最大 dp 的下标, 且 j < i
如果直接更新 dp 时间复杂度为 O(N ^ 2)
考虑用树状数组记录前缀和
BIT 中的 tree 数组用来记录以 i 结尾的最大子序列和
tree[i] = max(low_bit(tree[i]))
最后返回的时候找到 dp 中的最大值即可
由于 nums 的值域非常大
这里可以离散化处理
注意树状数组从 1 开始
所以二分查找之后的值需要加 1
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class BIT
{
public:
    BIT(int _n): n(_n), tree(_n, LLONG_MIN) {}

    void update(int i, long long val)
    {
        while (i < n)
        {
            tree[i] = max(tree[i], val);
            i += i & -i;
        }
    }

    long long pre_max(int i)
    {
        long long result = LLONG_MIN;
        while (i)
        {
            result = max(result, tree[i]);
            i &= (i - 1);
        }
        return result;
    }
private:
    int n;
    vector<long long> tree;
};
class Solution 
{
public:
    long long maxBalancedSubsequenceSum(vector<int>& nums) 
    {
        int n = nums.size(), pos = -1;
        auto b = nums;
        for (int i = 0; i < n; i++) b[i] -= i;
        sort(b.begin(), b.end());
        b.erase(unique(b.begin(), b.end()), b.end());
        BIT tree = BIT(b.size() + 1);
        for (int i = 0; i < n; i++) tree.update(pos = lower_bound(b.begin(), b.end(), nums[i] - i) - b.begin() + 1, max(0LL, tree.pre_max(pos)) + nums[i]);
        return tree.pre_max(b.size());
    }
};
```

__Java__:

```Java
class BIT {
    private long[] tree;
    private int n;

    public BIT(int n) {
        this.n = n;
        tree = new long[n];
        Arrays.fill(tree, Long.MIN_VALUE);
    }

    public void update(int i, long val) {
        while (i < n) {
            tree[i] = Math.max(tree[i], val);
            i += i & -i;
        }
    }

    public long preMax(int i) {
        long result = Long.MIN_VALUE;
        while (i > 0) {
            result = Math.max(result, tree[i]);
            i &= i - 1;
        }
        return result;
    }
}
class Solution {
    public long maxBalancedSubsequenceSum(int[] nums) {
        int n = nums.length, b[] = new int[n], pos = -1;
        for (int i = 0; i < n; i++) b[i] = nums[i] - i;
        Arrays.sort(b);
        BIT tree = new BIT(n + 1);
        for (int i = 0; i < n; i++) tree.update(pos = Arrays.binarySearch(b, nums[i] - i) + 1, Math.max(tree.preMax(pos), 0) + nums[i]);
        return tree.preMax(n);
    }
}
```

__Python__:

```Python
class BIT:
    def __init__(self, n: int):
        self.tree = [-inf] * n
        self.n = n

    def update(self, i: int, val: int) -> None:
        while i < self.n:
            self.tree[i] = max(self.tree[i], val)
            i += i & -i

    def pre_max(self, i: int) -> int:
        result = -inf
        while i:
            result = max(result, self.tree[i])
            i &= i - 1
        return result

class Solution:
    def maxBalancedSubsequenceSum(self, nums: List[int]) -> int:
        tree = BIT(len(b := sorted(set(num - i for i, num in enumerate(nums)))) + 1)
        for i, num in enumerate(nums):
            tree.update(pos := bisect_left(b, num - i) + 1, max(tree.pre_max(pos), 0) + num)
        return tree.pre_max(len(b))
```
