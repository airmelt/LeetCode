# 2786 Visit Array Positions to Maximize Score 访问数组中的位置使分数最大

__Description:__

You are given a __0-indexed__ integer array `nums` and a positive integer `x`.

You are __initially__ at position `0` in the array and you can visit other positions according to the following rules:

- If you are currently in position `i`, then you can move to __any__ position `j` such that `i < j`.
- For each position `i` that you visit, you get a score of `nums[i]`.
- If you move from a position `i` to a position `j` and the __parities__ of `nums[i]` and `nums[j]` differ, then you lose a score of `x`.

Return the maximum total score you can get.

__Note__ that initially you have `nums[0]` points.

__Example:__

Example 1:

```text
Input: nums = [2,3,6,1,9,2], x = 5
Output: 13
Explanation: We can visit the following positions in the array: 0 -> 2 -> 3 -> 4.
The corresponding values are 2, 6, 1 and 9. Since the integers 6 and 1 have different parities, the move 2 -> 3 will make you lose a score of x = 5.
The total score will be: 2 + 6 + 1 + 9 - 5 = 13.
```

Example 2:

```text
Input: nums = [2,4,6,8], x = 3
Output: 20
Explanation: All the integers in the array have the same parities, so we can visit all of them without losing any score.
The total score is: 2 + 4 + 6 + 8 = 20.
```

__Constraints:__

- `2 <= nums.length <= 10 ^ 5`
- `1 <= nums[i], x <= 10 ^ 6`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 和一个正整数 `x` 。

你 __一开始__ 在数组的位置 `0` 处，你可以按照下述规则访问数组中的其他位置:

- 如果你当前在位置 `i` ，那么你可以移动到满足 `i < j` 的 __任意__ 位置 `j` 。
- 对于你访问的位置 `i` ，你可以获得分数 `nums[i]` 。
- 如果你从位置 `i` 移动到位置 `j` 且 `nums[i]` 和 `nums[j]` 的 __奇偶性__ 不同，那么你将失去分数 `x` 。

请你返回你能得到的 最大 得分之和。

__注意__ ，你一开始的分数为 `nums[0]` 。

__示例:__

示例 1：

```text
输入：nums = [2,3,6,1,9,2], x = 5
输出：13
解释：我们可以按顺序访问数组中的位置：0 -> 2 -> 3 -> 4 。
对应位置的值为 2 ，6 ，1 和 9 。因为 6 和 1 的奇偶性不同，所以下标从 2 -> 3 让你失去 x = 5 分。
总得分为：2 + 6 + 1 + 9 - 5 = 13 。
```

示例 2：

```text
输入：nums = [2,4,6,8], x = 3
输出：20
解释：数组中的所有元素奇偶性都一样，所以我们可以将每个元素都访问一次，而且不会失去任何分数。
总得分为：2 + 4 + 6 + 8 = 20 。
```

__提示：__

- `2 <= nums.length <= 10 ^ 5`
- `1 <= nums[i], x <= 10 ^ 6`

__思路:__

```text
动态规划
设 dp[i][0] 表示到第 i 个位置, 上一个从偶数转移过来的分数, dp[i][0] 则为奇数
初始化 dp[i][nums[i] & 1] = nums[0], 最小都能选 nums[0], dp[i][nums[i] & 1 ^ 1] = -inf, 当前值的异或初始化为 -inf
那么当 nums[i] 为奇数时, dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] + nums[i] - x), 可以从 i - 1 的偶数直接转移过来, 不选当前值, 从 i - 1 的奇数转移过来, 由于这里是偶数转移, 所以要加上惩罚
dp[i][1] 都是以奇数结尾, 每个都能加上 nums[i], 偶数转移来的需要接受惩罚
其余情况类似, 最后返回 max(dp[-1]) 即可
也可以从另一个角度来思考
注意到数组中都是正数
所以和前面奇偶相同的一定要选, 可以增加结果
从新定义 dp[i][j] 为在 [i, n - 1] 中选择一个子序列使得前一个数的奇偶性为 j, 即模 2 的结果为 j, 的最大得分
如果 nums[i] & 1 != j, 那么不能选, dp[i][j] = dp[i + 1][j]
否则可以选, dp[i][j] = max(dp[i + 1][j], dp[i + 1][(nums[i] & 1) ^ 1] - x) + nums[i], 这里 (nums[i] & 1) ^ 1 表示与 nums[i] 奇偶性不相同的值
那么可以从后往前遍历
最后取 nums[0] 的奇偶作为起点, 因为 nums[0] 必选
再用滚动数组降低空间复杂度到 O(1)
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maxScore(vector<int>& nums, int x) 
    {
        long long dp[2]{};
        for (auto it = nums.rbegin(); it != nums.rend(); it++) dp[(*it) & 1] = max(dp[(*it) & 1], dp[(*it) & 1 ^ 1] - x) + (*it);
        return dp[nums.front() & 1];
    }
};
```

__Java__:

```Java
class Solution {
    public long maxScore(int[] nums, int x) {
        long[] dp = new long[2];
        for (int n = nums.length, i = n - 1; i > -1; i--) dp[nums[i] & 1] = Math.max(dp[nums[i] & 1], dp[nums[i] & 1 ^ 1] - x) + nums[i];
        return dp[nums[0] & 1];
    }
}
```

__Python__:

```Python
class Solution:
    def maxScore(self, nums: List[int], x: int) -> int:
        dp = [0, 0]
        for num in nums[::-1]:
            dp[num & 1] = max(dp[num & 1], dp[(num & 1) ^ 1] - x) + num
        return dp[nums[0] & 1]
```
