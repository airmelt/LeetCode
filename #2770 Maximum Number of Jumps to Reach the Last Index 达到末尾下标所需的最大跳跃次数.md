# 2770 Maximum Number of Jumps to Reach the Last Index 达到末尾下标所需的最大跳跃次数

__Description:__

You are given a __0-indexed__ array `nums` of `n` integers and an integer `target`.

You are initially positioned at index `0`. In one step, you can jump from index `i` to any index `j` such that:

- `0 <= i < j < n`
- `-target <= nums[j] - nums[i] <= target`

Return _the __maximum number of jumps__ you can make to reach index_ `n - 1`.

If there is no way to reach index `n - 1`, return `-1`.

__Example:__

Example 1:

```text
Input: nums = [1,3,6,4,1,2], target = 2
Output: 3
Explanation: To go from index 0 to index n - 1 with the maximum number of jumps, you can perform the following jumping sequence:
```

- Jump from index 0 to index 1.
- Jump from index 1 to index 3.
- Jump from index 3 to index 5.

It can be proven that there is no other jumping sequence that goes from 0 to n - 1 with more than 3 jumps. Hence, the answer is 3.

Example 2:

```text
Input: nums = [1,3,6,4,1,2], target = 3
Output: 5
Explanation: To go from index 0 to index n - 1 with the maximum number of jumps, you can perform the following jumping sequence:
```

- Jump from index 0 to index 1.
- Jump from index 1 to index 2.
- Jump from index 2 to index 3.
- Jump from index 3 to index 4.
- Jump from index 4 to index 5.

It can be proven that there is no other jumping sequence that goes from 0 to n - 1 with more than 5 jumps. Hence, the answer is 5.

Example 3:

```text
Input: nums = [1,3,6,4,1,2], target = 0
Output: -1
Explanation: It can be proven that there is no jumping sequence that goes from 0 to n - 1. Hence, the answer is -1.
```

__Constraints:__

- `2 <= nums.length == n <= 1000`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`
- `0 <= target <= 2 * 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始、由 `n` 个整数组成的数组 `nums` 和一个整数 `target` 。

你的初始位置在下标 `0` 。在一步操作中，你可以从下标 `i` 跳跃到任意满足下述条件的下标 `j` :

- `0 <= i < j < n`
- `-target <= nums[j] - nums[i] <= target`

返回到达下标 `n - 1` 处所需的 __最大跳跃次数__ 。

如果无法到达下标 `n - 1` ，返回 `-1` 。

__示例:__

示例 1：

```text
输入：nums = [1,3,6,4,1,2], target = 2
输出：3
解释：要想以最大跳跃次数从下标 0 到下标 n - 1 ，可以按下述跳跃序列执行操作：
```

- 从下标 0 跳跃到下标 1 。
- 从下标 1 跳跃到下标 3 。
- 从下标 3 跳跃到下标 5 。

可以证明，从 0 到 n - 1 的所有方案中，不存在比 3 步更长的跳跃序列。因此，答案是 3 。

示例 2：

```text
输入：nums = [1,3,6,4,1,2], target = 3
输出：5
解释：要想以最大跳跃次数从下标 0 到下标 n - 1 ，可以按下述跳跃序列执行操作：
```

- 从下标 0 跳跃到下标 1 。
- 从下标 1 跳跃到下标 2 。
- 从下标 2 跳跃到下标 3 。
- 从下标 3 跳跃到下标 4 。
- 从下标 4 跳跃到下标 5 。

可以证明，从 0 到 n - 1 的所有方案中，不存在比 5 步更长的跳跃序列。因此，答案是 5 。

示例 3：

```text
输入：nums = [1,3,6,4,1,2], target = 0
输出：-1
解释：可以证明不存在从 0 到 n - 1 的跳跃序列。因此，答案是 -1 。
```

__提示：__

- `2 <= nums.length == n <= 1000`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`
- `0 <= target <= 2 * 10 ^ 9`

__思路:__

```text
动态规划
设 dp[i] 表示跳到 i 可达的最大步数
初始化 dp 为 -1
dp[0] 为 0 表示最开始就在 0 位置处
搜索 i 前面的所有元素, 如果已经能到达 j, dp[j] != -1
且 j 能到达 i, abs(nums[i] - nums[j]) <= target
那么就将 dp[i] 更新为 max(dp[i], dp[j] + 1)
最后返回 dp[-1] 即可
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumJumps(vector<int>& nums, int target) 
    {
        int n = nums.size();
        vector<int> dp(n, -1);
        dp.front() = 0;
        for (int i = 1; i < n; i++) for (int j = 0; j < i; j++) if (abs(nums[i] - nums[j]) <= target and dp[j] != -1) dp[i] = max(dp[i], dp[j] + 1);
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumJumps(int[] nums, int target) {
        int n = nums.length, dp[] = new int[n];
        Arrays.fill(dp, -1);
        dp[0] = 0;
        for (int i = 1; i < n; i++) for (int j = 0; j < i; j++) if (Math.abs(nums[i] - nums[j]) <= target && dp[j] != -1) dp[i] = Math.max(dp[i], dp[j] + 1);
        return dp[n - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def maximumJumps(self, nums: List[int], target: int) -> int:
        dp = [0] + [-1] * ((n := len(nums)) - 1)
        for i in range(1, n):
            for j in range(i):
                if abs(nums[i] - nums[j]) <= target and dp[j] != -1:
                    dp[i] = max(dp[i], dp[j] + 1)
        return dp[-1]
```
