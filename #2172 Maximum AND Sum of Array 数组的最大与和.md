# 2172 Maximum AND Sum of Array 数组的最大与和

__Description:__

You are given an integer array `nums` of length `n` and an integer `numSlots` such that `2 * numSlots >= n`. There are `numSlots` slots numbered from `1` to `numSlots`.

You have to place all `n` integers into the slots such that each slot contains at __most__ two numbers. The __AND sum__ of a given placement is the sum of the __bitwise__ `AND` of every number with its respective slot number.

- For example, the __AND sum__ of placing the numbers `[1, 3]` into slot `1` and `[4, 6]` into slot `2` is equal to `(1 AND 1) + (3 AND 1) + (4 AND 2) + (6 AND 2) = 1 + 1 + 0 + 2 = 4`.

Return _the maximum possible __AND sum__ of_ `nums` _given_ `numSlots` _slots._

__Example:__

Example 1:

```text
Input: nums = [1,2,3,4,5,6], numSlots = 3
Output: 9
Explanation: One possible placement is [1, 4] into slot 1, [2, 6] into slot 2, and [3, 5] into slot 3. 
This gives the maximum AND sum of (1 AND 1) + (4 AND 1) + (2 AND 2) + (6 AND 2) + (3 AND 3) + (5 AND 3) = 1 + 0 + 2 + 2 + 3 + 1 = 9.
```

Example 2:

```text
Input: nums = [1,3,10,4,7,1], numSlots = 9
Output: 24
Explanation: One possible placement is [1, 1] into slot 1, [3] into slot 3, [4] into slot 4, [7] into slot 7, and [10] into slot 9.
This gives the maximum AND sum of (1 AND 1) + (1 AND 1) + (3 AND 3) + (4 AND 4) + (7 AND 7) + (10 AND 9) = 1 + 1 + 3 + 4 + 7 + 8 = 24.
Note that slots 2, 5, 6, and 8 are empty which is permitted.
```

__Constraints:__

- `n == nums.length`
- `1 <= numSlots <= 9`
- `1 <= n <= 2 * numSlots`
- `1 <= nums[i] <= 15`

__题目描述:__

给你一个长度为 `n` 的整数数组 `nums` 和一个整数 `numSlots` ，满足 `2 * numSlots >= n` 。总共有 `numSlots` 个篮子，编号为 `1` 到 `numSlots` 。

你需要把所有 `n` 个整数分到这些篮子中，且每个篮子 __至多__ 有 2 个整数。一种分配方案的 __与和__ 定义为每个数与它所在篮子编号的 __按位与运算__ 结果之和。

- 比方说，将数字 `[1, 3]` 放入篮子 ___1___ 中， `[4, 6]` 放入篮子 ___2___ 中，这个方案的与和为 (1 AND ___1___) + (3 AND ___1___) + (4 AND ___2___) + (6 AND ___2___) = 1 + 1 + 0 + 2 = 4 。

请你返回将 `nums` 中所有数放入 `numSlots` 个篮子中的最大与和。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,4,5,6], numSlots = 3
输出：9
解释：一个可行的方案是 [1, 4] 放入篮子 1 中，[2, 6] 放入篮子 2 中，[3, 5] 放入篮子 3 中。
最大与和为 (1 AND 1) + (4 AND 1) + (2 AND 2) + (6 AND 2) + (3 AND 3) + (5 AND 3) = 1 + 0 + 2 + 2 + 3 + 1 = 9 。
```

示例 2：

```text
输入：nums = [1,3,10,4,7,1], numSlots = 9
输出：24
解释：一个可行的方案是 [1, 1] 放入篮子 1 中，[3] 放入篮子 3 中，[4] 放入篮子 4 中，[7] 放入篮子 7 中，[10] 放入篮子 9 中。
最大与和为 (1 AND 1) + (1 AND 1) + (3 AND 3) + (4 AND 4) + (7 AND 7) + (10 AND 9) = 1 + 1 + 3 + 4 + 7 + 8 = 24 。
注意，篮子 2 ，5 ，6 和 8 是空的，这是允许的。
```

__提示：__

- `n == nums.length`
- `1 <= numSlots <= 9`
- `1 <= n <= 2 * numSlots`
- `1 <= nums[i] <= 15`

__思路:__

```text
动态压缩 ➕ 贪心
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumANDSum(vector<int>& nums, int numSlots) 
    {
        int n = nums.size(), m = 1 << (numSlots << 1);
        vector<int> dp(m);
        for (int i = 0, c = 0; i < m; i++) if ((c = __builtin_popcount(i)) < n) for (int j = 0; j < (numSlots << 1); j++) if (!(i & (1 << j))) dp[i | (1 << j)] = max(dp[i | (1 << j)], dp[i] + (((j >> 1) + 1) & nums[c]));
        return *max_element(dp.begin(), dp.end());
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumANDSum(int[] nums, int numSlots) {
        int n = nums.length, m = 1 << (numSlots << 1), dp[] = new int[m];
        for (int i = 0, c = 0; i < m; i++) if ((c = Integer.bitCount(i)) < n) for (int j = 0; j < (numSlots << 1); j++) if ((i & (1 << j)) == 0) dp[i | (1 << j)] = Math.max(dp[i | (1 << j)], dp[i] + (((j >> 1) + 1) & nums[c]));
        return Arrays.stream(dp).max().getAsInt();
    }
}
```

__Python__:

```Python
class Solution:
    def maximumANDSum(self, nums: List[int], numSlots: int) -> int:
        dp = [0] * (1 << (numSlots << 1))
        for i, v in enumerate(dp):
            if (c := i.bit_count()) < len(nums):
                for j in range(numSlots << 1):
                    if not (i & (1 << j)):
                        dp[i | (1 << j)] = max(dp[i | (1 << j)], v + (((j >> 1) + 1) & nums[c]))
        return max(dp)
```
