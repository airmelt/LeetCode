# 3040 Maximum Number of Operations With the Same Score II 相同分数的最大操作数目 II

__Description:__

Given an array of integers called `nums`, you can perform __any__ of the following operation while `nums` contains __at least__ `2` elements:

- Choose the first two elements of `nums` and delete them.
- Choose the last two elements of `nums` and delete them.
- Choose the first and the last elements of `nums` and delete them.

The score of the operation is the sum of the deleted elements.

Your task is to find the maximum number of operations that can be performed, such that all operations have the same score.

Return the maximum number of operations possible that satisfy the condition mentioned above.

__Example:__

Example 1:

```text
Input: nums = [3,2,1,2,3,4]
Output: 3
Explanation: We perform the following operations:
```

- Delete the first two elements, with score 3 + 2 = 5, nums = [1,2,3,4].
- Delete the first and the last elements, with score 1 + 4 = 5, nums = [2,3].
- Delete the first and the last elements, with score 2 + 3 = 5, nums = [].

We are unable to perform any more operations as nums is empty.

Example 2:

```text
Input: nums = [3,2,6,1,4]
Output: 2
Explanation: We perform the following operations:
```

- Delete the first two elements, with score 3 + 2 = 5, nums = [6,1,4].
- Delete the last two elements, with score 1 + 4 = 5, nums = [6].

It can be proven that we can perform at most 2 operations.

__Constraints:__

- `2 <= nums.length <= 2000`
- `1 <= nums[i] <= 1000`

__题目描述:__

给你一个整数数组 `nums` ，如果 `nums` __至少__ 包含 `2` 个元素，你可以执行以下操作中的 __任意__ 一个:

- 选择 `nums` 中最前面两个元素并且删除它们。
- 选择 `nums` 中最后两个元素并且删除它们。
- 选择 `nums` 中第一个和最后一个元素并且删除它们。

一次操作的 分数 是被删除元素的和。

在确保 所有操作分数相同 的前提下，请你求出 最多 能进行多少次操作。

请你返回按照上述要求 最多 可以进行的操作次数。

__示例:__

示例 1：

```text
输入：nums = [3,2,1,2,3,4]
输出：3
解释：我们执行以下操作：
```

- 删除前两个元素，分数为 3 + 2 = 5 ，nums = [1,2,3,4] 。
- 删除第一个元素和最后一个元素，分数为 1 + 4 = 5 ，nums = [2,3] 。
- 删除第一个元素和最后一个元素，分数为 2 + 3 = 5 ，nums = [] 。

由于 nums 为空，我们无法继续进行任何操作。

示例 2：

```text
输入：nums = [3,2,6,1,4]
输出：2
解释：我们执行以下操作：
```

- 删除前两个元素，分数为 3 + 2 = 5 ，nums = [6,1,4] 。
- 删除最后两个元素，分数为 1 + 4 = 5 ，nums = [6] 。

至多进行 2 次操作。

__提示：__

- `2 <= nums.length <= 2000`
- `1 <= nums[i] <= 1000`

__思路:__

```text
动态规划
设 dp[i][j + 1] 表示闭区间 [i, j] 的结果, 即 nums[i:j + 1] 能取得的最大分数
分为三种情况
取 nums[0] + nums[1], 那么区间为 [2, n - 1]
取 nums[0] + nums[-1], 那么区间为 [1, n - 2]
取 nums[-1] + nums[-2], 那么区间为 [0, n - 3]
设目标值为 target 转移方程
如果 nums[i] + nums[j] == target, 那么区间 [i, j] 可以缩小为 [i + 1, j - 1], 即 dp[i][j + 1] = max(dp[i][j + 1], dp[i + 1][j] + 1)
其他的以此类推
注意到如果要求得 dp[i][j + 1] 要先求得 dp[i + 1][j], dp[i + 2][j - 1], dp[i][j - 1]
所以需要反向遍历 i, 正向遍历 j
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N ^ 2)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxOperations(vector<int>& nums) 
    {
        int n = nums.size();
        return max({ helper(0, n - 3, nums[n - 1] + nums[n - 2], nums), helper(1, n - 2, nums[0] + nums[n - 1], nums), helper(2, n - 1, nums[0] + nums[1], nums) }) + 1;
    }
private:
    int helper(int start, int end, int target, vector<int>& nums) 
    {
        int n = nums.size();
        vector<vector<int>> dp(n + 1, vector<int>(n + 1));
        for (int i = end - 1; i >= start; i--) 
        {
            for (int j = i + 1, cur = 0; j <= end; j++, cur = 0) 
            {
                if (nums[i] + nums[i + 1] == target) cur = max(cur, dp[i + 2][j + 1] + 1);
                if (nums[j] + nums[j - 1] == target) cur = max(cur, dp[i][j - 1] + 1);
                if (nums[i] + nums[j] == target) cur = max(cur, dp[i + 1][j] + 1);
                dp[i][j + 1] = cur;
            }
        }
        return dp[start][end + 1];
    }
};
```

__Java__:

```Java
class Solution {
    public int maxOperations(int[] nums) {
        int n = nums.length;
        return Math.max(Math.max(helper(0, n - 3, nums[n - 1] + nums[n - 2], nums), helper(1, n - 2, nums[0] + nums[n - 1], nums)), helper(2, n - 1, nums[0] + nums[1], nums)) + 1;
    }

    private int helper(int start, int end, int target, int[] nums) {
        int n = nums.length, dp[][] = new int[n + 1][n + 1];
        for (int i = end - 1; i >= start; i--) {
            for (int j = i + 1, cur = 0; j <= end; j++, cur = 0) {
                if (nums[i] + nums[i + 1] == target) cur = Math.max(cur, dp[i + 2][j + 1] + 1);
                if (nums[j] + nums[j - 1] == target) cur = Math.max(cur, dp[i][j - 1] + 1);
                if (nums[i] + nums[j] == target) cur = Math.max(cur, dp[i + 1][j] + 1);
                dp[i][j + 1] = cur;
            }
        }
        return dp[start][end + 1];
    }
}
```

__Python__:

```Python
class Solution:
    def maxOperations(self, nums: List[int]) -> int:
        @lru_cache(None)
        def dfs(i: int, j: int, target: int) -> int:
            return 0 if i >= j else max(dfs(i + 2, j, target) + 1 if nums[i] + nums[i + 1] == target else 0, dfs(i, j - 2, target) + 1 if nums[j] + nums[j - 1] == target else 0, dfs(i + 1, j - 1, target) + 1 if nums[i] + nums[j] == target else 0)
        return max(dfs(1, (n := len(nums)) - 2, nums[0] + nums[-1]), dfs(0, n - 3, nums[-1] + nums[-2]), dfs(2, n - 1, nums[0] + nums[1])) + 1
```
