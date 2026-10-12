# 3202 Find the Maximum Length of Valid Subsequence II 找出有效子序列的最大长度 II

__Description:__

You are given an integer array `nums` and a __positive__ integer `k`.
A __subsequence__ `sub` of `nums` with length `x` is called __valid__ if it satisfies:

- `(sub[0] + sub[1]) % k == (sub[1] + sub[2]) % k == ... == (sub[x - 2] + sub[x - 1]) % k.`

Return the length of the __longest valid__ subsequence of nums.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,4,5], k = 2

Output: 5

Explanation:

The longest valid subsequence is [1, 2, 3, 4, 5].
```

Example 2:

```text
Input: nums = [1,4,2,3,1,4], k = 3

Output: 4

Explanation:

The longest valid subsequence is [1, 4, 1, 4].
```

__Constraints:__

- `2 <= nums.length <= 10 ^ 3`
- `1 <= nums[i] <= 10 ^ 7`
- `1 <= k <= 10 ^ 3`

__题目描述:__

给你一个整数数组 `nums` 和一个 __正__ 整数 `k` 。
`nums` 的一个 __子序列__ `sub` 的长度为 `x` ，如果其满足以下条件，则称其为 __有效子序列__ ：

- `(sub[0] + sub[1]) % k == (sub[1] + sub[2]) % k == ... == (sub[x - 2] + sub[x - 1]) % k`

返回 `nums` 的 __最长有效子序列__ 的长度。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,4,5], k = 2

输出：5

解释：

最长有效子序列是 [1, 2, 3, 4, 5] 。
```

示例 2：

```text
输入：nums = [1,4,2,3,1,4], k = 3

输出：4

解释：

最长有效子序列是 [1, 4, 1, 4] 。
```

__提示：__

- `2 <= nums.length <= 10 ^ 3`
- `1 <= nums[i] <= 10 ^ 7`
- `1 <= k <= 10 ^ 3`

__思路:__

```text
动态规划
设 dp[j] 表示是否从 nums 中能选出对 k 的余数为 j 的个数
那么有效子序列长度为 max(dp)
对于以 m = num % k, 它可以从 dp[i - m] + 1 转移而来
注意这里下标不能负数, i - m 需要改为 (i - m + k) % k
时间复杂度为 O(NK), 空间复杂度为 O(K)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumLength(vector<int>& nums, int k) 
    {
        int result = 0;
        for (int i = 0, m = 0; i < k; i++) 
        {
            vector<int> dp(k);
            for (const auto& num : nums) dp[m] = dp[(i - (m = num % k) + k) % k] + 1;
            result = max(result, *max_element(dp.begin(), dp.end()));
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumLength(int[] nums, int k) {
        int result = 0;
        for (int i = 0, m = 0, dp[] = new int[k]; i < k; i++, dp = new int[k]) {
            for (int num : nums) dp[m = num % k] = dp[(i - m + k) % k] + 1;
            result = Math.max(result, Arrays.stream(dp).max().getAsInt());
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumLength(self, nums: List[int], k: int) -> int:
        result = 0
        for i in range(k):
            dp = [0] * k
            for num in nums:
                dp[m] = dp[i - (m := num % k)] + 1
            result = max(result, max(dp))
        return result
```
