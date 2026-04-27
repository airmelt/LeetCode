# 1879 Minimum XOR Sum of Two Arrays 两个数组最小的异或值之和

__Description:__

You are given two integer arrays `nums1` and `nums2` of length `n`.

The __XOR sum__ of the two integer arrays is `(nums1[0] XOR nums2[0]) + (nums1[1] XOR nums2[1]) + ... + (nums1[n - 1] XOR nums2[n - 1])` (__0-indexed__).

- For example, the __XOR sum__ of `[1,2,3]` and `[3,2,1]` is equal to `(1 XOR 3) + (2 XOR 2) + (3 XOR 1) = 2 + 0 + 2 = 4`.

Rearrange the elements of `nums2` such that the resulting __XOR sum__ is _minimized_ .

Return the XOR sum after the rearrangement.

__Example:__

Example 1:

```text
Input:  nums1 = [1,2], nums2 = [2,3]
Output:  2
Explanation: Rearrange nums2 so that it becomes [3,2].
The XOR sum is (1 XOR 3) + (2 XOR 2) = 2 + 0 = 2.
```

Example 2:

```text
Input:  nums1 = [1,0,3], nums2 = [5,3,4]
Output:  8
Explanation: Rearrange nums2 so that it becomes [5,4,3]. 
The XOR sum is (1 XOR 5) + (0 XOR 4) + (3 XOR 3) = 4 + 4 + 0 = 8.
```

__Constraints:__

- `n == nums1.length`
- `n == nums2.length`
- `1 <= n <= 14`
- `0 <= nums1[i], nums2[i] <= 10 ^ 7`

__题目描述:__

给你两个整数数组 `nums1` 和 `nums2` ，它们长度都为 `n` 。

两个数组的 __异或值之和__ 为 `(nums1[0] XOR nums2[0]) + (nums1[1] XOR nums2[1]) + ... + (nums1[n - 1] XOR nums2[n - 1])` （__下标从 0 开始__）。

- 比方说， `[1,2,3]` 和 `[3,2,1]` 的 __异或值之和__ 等于 `(1 XOR 3) + (2 XOR 2) + (3 XOR 1) = 2 + 0 + 2 = 4` 。

请你将 `nums2` 中的元素重新排列，使得 __异或值之和__ __最小__ 。

请你返回重新排列之后的 异或值之和 。

__示例:__

示例 1：

```text
输入: nums1 = [1,2], nums2 = [2,3]
输出: 2
解释: 将 nums2 重新排列得到 [3,2] 。
异或值之和为 (1 XOR 3) + (2 XOR 2) = 2 + 0 = 2 。
```

示例 2：

```text
输入: nums1 = [1,0,3], nums2 = [5,3,4]
输出: 8
解释: 将 nums2 重新排列得到 [5,4,3] 。
异或值之和为 (1 XOR 5) + (0 XOR 4) + (3 XOR 3) = 4 + 4 + 0 = 8 。
```

__提示：__

- `n == nums1.length`
- `n == nums2.length`
- `1 <= n <= 14`
- `0 <= nums1[i], nums2[i] <= 10 ^ 7`

__思路:__

```text
动态规划 ➕ 状态压缩
设 dp[mask] 表示从 nums2 中选择 mask 对应位置为 1 的数与 nums1 的前 count(mask) 个数进行异或运算的和的最小值
初始化 dp[0] = 0, 表示选择 0 个数的时候为 0
其余初始化为一个较大值
当 mask & (1 << i) 不为 0 时
dp[mask] = min(dp[mask], dp[mask ^ (1 << i)] + nums2[i] ^ nums1[__builtin_popcount(mask) - 1])
时间复杂度为 O(N * 2 ^ N), 空间复杂度为 O(2 ^ N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumXORSum(vector<int>& nums1, vector<int>& nums2) 
    {
        int n = nums1.size();
        vector<int> dp(1 << n, INT_MAX);
        dp.front() = 0;
        for (int mask = 1; mask < (1 << n); mask++) for (int i = 0; i < n; i++) if (mask & (1 << i)) dp[mask] = min(dp[mask], dp[mask ^ (1 << i)] + (nums1[__builtin_popcount(mask) - 1] ^ nums2[i]));
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumXORSum(int[] nums1, int[] nums2) {
        int n = nums1.length, dp[] = new int[1 << n];
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0;
        for (int mask = 1; mask < (1 << n); mask++) for (int i = 0; i < n; i++) if ((mask & (1 << i)) != 0) dp[mask] = Math.min(dp[mask], dp[mask ^ (1 << i)] + (nums1[count(mask) - 1] ^ nums2[i]));
        return dp[(1 << n) - 1];
    }

    private int count(int mask) {
        int result = 0;
        while (mask > 0) {
            result++;
            mask &= mask - 1;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumXORSum(self, nums1: List[int], nums2: List[int]) -> int:
        dp = [0] + [inf] * ((1 << (n := len(nums1))) - 1)
        for mask in range(1, 1 << n):
            for i in range(n):
                if mask & (1 << i):
                    dp[mask] = min(dp[mask], dp[mask ^ (1 << i)] + (nums1[bin(mask).count('1') - 1] ^ nums2[i]))
        return dp[-1]
```
