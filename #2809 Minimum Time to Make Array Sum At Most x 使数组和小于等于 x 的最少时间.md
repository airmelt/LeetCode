# 2809 Minimum Time to Make Array Sum At Most x 使数组和小于等于 x 的最少时间

__Description:__

You are given two __0-indexed__ integer arrays `nums1` and `nums2` of equal length. Every second, for all indices `0 <= i < nums1.length`, value of `nums1[i]` is incremented by `nums2[i]`. __After__ this is done, you can do the following operation:

- Choose an index `0 <= i < nums1.length` and make `nums1[i] = 0`.

You are also given an integer `x`.

Return _the __minimum__ time in which you can make the sum of all elements of_ `nums1` _to be __less than or equal__ to_ `x`, _or_ `-1` _if this is not possible._

__Example:__

Example 1:

```text
Input: nums1 = [1,2,3], nums2 = [1,2,3], x = 4
Output: 3
Explanation: 
For the 1st second, we apply the operation on i = 0. Therefore nums1 = [0,2+2,3+3] = [0,4,6]. 
For the 2nd second, we apply the operation on i = 1. Therefore nums1 = [0+1,0,6+3] = [1,0,9]. 
For the 3rd second, we apply the operation on i = 2. Therefore nums1 = [1+1,0+2,0] = [2,2,0]. 
Now sum of nums1 = 4. It can be shown that these operations are optimal, so we return 3.
```

Example 2:

```text
Input: nums1 = [1,2,3], nums2 = [3,3,3], x = 4
Output: -1
Explanation: It can be shown that the sum of nums1 will always be greater than x, no matter which operations are performed.
```

__Constraints:__

- `1 <= nums1.length <= 10 ^ 3`
- `1 <= nums1[i] <= 10 ^ 3`
- `0 <= nums2[i] <= 10 ^ 3`
- `nums1.length == nums2.length`
- `0 <= x <= 10 ^ 6`

__题目描述:__

给你两个长度相等下标从 __0__ 开始的整数数组 `nums1` 和 `nums2` 。每一秒，对于所有下标 `0 <= i < nums1.length` ， `nums1[i]` 的值都增加 `nums2[i]` 。操作 __完成后__ ，你可以进行如下操作:

- 选择任一满足 `0 <= i < nums1.length` 的下标 `i` ，并使 `nums1[i] = 0` 。

同时给你一个整数 `x` 。

请你返回使 `nums1` 中所有元素之和 __小于等于__ `x` 所需要的 __最少__ 时间，如果无法实现，那么返回 `-1` 。

__示例:__

示例 1：

```text
输入：nums1 = [1,2,3], nums2 = [1,2,3], x = 4
输出：3
解释：
第 1 秒，我们对 i = 0 进行操作，得到 nums1 = [0,2+2,3+3] = [0,4,6] 。
第 2 秒，我们对 i = 1 进行操作，得到 nums1 = [0+1,0,6+3] = [1,0,9] 。
第 3 秒，我们对 i = 2 进行操作，得到 nums1 = [1+1,0+2,0] = [2,2,0] 。
现在 nums1 的和为 4 。不存在更少次数的操作，所以我们返回 3 。
```

示例 2：

```text
输入：nums1 = [1,2,3], nums2 = [3,3,3], x = 4
输出：-1
解释：不管如何操作，nums1 的和总是会超过 x 。
```

__提示：__

- `1 <= nums1.length <= 10 ^ 3`
- `1 <= nums1[i] <= 10 ^ 3`
- `0 <= nums2[i] <= 10 ^ 3`
- `nums1.length == nums2.length`
- `0 <= x <= 10 ^ 6`

__思路:__

```text
动态规划
如果不操作, 那么 t 秒后 nums1 的和将变为 s1 + t * s2
如果多次操作某个数, 只有最后一次操作有意义, 还会导致其他数增加, 所以每个数最多操作 1 次
如果在第 k 秒操作 nums1[i], 那么它将使得结果减少 nums1[i] + nums2[i] * k
目标就是让 nums1[i] + nums2[i] * k 尽可能大
也就是说需要在前 i 个数中选出一个子序列 j
使得 sum(nums1[i] + nums2[i] * k) 尽可能大
根据排序不等式 nums2[i] 尽可能大的时候 nums2[i] * k 会取得更大值
所以将 nums1[i] 和 nums2[i] 组合并按照 nums2 的大小排序
设 dp[i + 1][j] 表示从前 i 个元素中选择子序列 j 使得 sum(nums1[i] + nums2[i] * j) 最大
对于 i, 如果不选择 j, 也就是从 i - 1 中选, 即 dp[i + 1][j] = dp[i][j]
否则选择 j, dp[i + 1][j] = dp[i][j - 1] + nums1[i] + nums2[i] * j
初始化 dp[0][0] = 0, 选择 0 个元素时减少量为 0
最后找到第一个 i 使得 s1 + s2 * i - dp[n][i] <= x
否则返回 -1
由于 dp[i + 1] 只和 dp[i] 有关
倒序遍历并使用滚动数组, 类似 0-1 背包可以将空间复杂度优化为 O(N)
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumTime(vector<int>& nums1, vector<int>& nums2, int x) 
    {
        int n = nums1.size(), s1 = accumulate(nums1.begin(), nums1.end(), 0), s2 = accumulate(nums2.begin(), nums2.end(), 0);
        vector<int> dp(n + 1), idx(n);
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&](const auto& a, const auto& b) { return nums2[a] < nums2[b]; });
        for (int i = 0; i < n; i++) for (int j = i + 1; j > 0; j--) dp[j] = max(dp[j], dp[j - 1] + nums1[idx[i]] + j * nums2[idx[i]]);
        for (int i = 0; i <= n; i++) if (s1 + s2 * i - dp[i] <= x) return i;
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumTime(List<Integer> nums1, List<Integer> nums2, int x) {
        int n = nums1.size(), s1 = nums1.stream().mapToInt(Integer::intValue).sum(), s2 = nums2.stream().mapToInt(Integer::intValue).sum(), dp[] = new int[n + 1];
        Integer[] idx = new Integer[n];
        for (int i = 0; i < n; i++) idx[i] = i;
        Arrays.sort(idx, (a, b) -> nums2.get(a) - nums2.get(b));
        for (int i = 0; i < n; i++) for (int j = i + 1; j > 0; j--) dp[j] = Math.max(dp[j], dp[j - 1] + nums1.get(idx[i]) + j * nums2.get(idx[i]));
        for (int i = 0; i <= n; i++) if (s1 + s2 * i - dp[i] <= x) return i;
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumTime(self, nums1: List[int], nums2: List[int], x: int) -> int:
        s1, s2, pairs, dp = sum(nums1), sum(nums2), sorted(zip(nums1, nums2), key=lambda p: p[1]), [0] * ((n := len(nums1)) + 1)
        for i, (a, b) in enumerate(pairs):
            for j in range(i + 1, 0, -1):
                dp[j] = max(dp[j], dp[j - 1] + a + b * j)
        return result[0][0] if (result := list(filter(lambda s: s1 + s2 * s[0] - s[1] <= x, zip(range(n + 1), dp)))) else -1
```
