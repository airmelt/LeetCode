# 1425 Constrained Subsequence Sum 带限制的子序列和

__Description:__

Given an integer array `nums` and an integer `k`, return the maximum sum of a __non-empty__ subsequence of that array such that for every two __consecutive__ integers in the subsequence, `nums[i]` and `nums[j]`, where `i < j`, the condition `j - i <= k` is satisfied.

A subsequence of an array is obtained by deleting some number of elements (can be zero) from the array, leaving the remaining elements in their original order.

__Example:__

Example 1:

```text
Input: nums = [10,2,-10,5,20], k = 2
Output: 37
Explanation: The subsequence is [10, 2, 5, 20].
```

Example 2:

```text
Input: nums = [-1,-2,-3], k = 1
Output: -1
Explanation: The subsequence must be non-empty, so we choose the largest number.
```

Example 3:

```text
Input: nums = [10,-2,-10,-5,20], k = 2
Output: 23
Explanation: The subsequence is [10, -2, -5, 20].
```

__Constraints:__

- `1 <= k <= nums.length <= 10 ^ 5`
- `-10 ^ 4 <= nums[i] <= 10 ^ 4`

__题目描述:__

给你一个整数数组 `nums` 和一个整数 `k` ，请你返回 __非空__ 子序列元素和的最大值，子序列需要满足：子序列中每两个 __相邻__ 的整数 `nums[i]` 和 `nums[j]` ，它们在原数组中的下标 `i` 和 `j` 满足 `i < j` 且 `j - i <= k` 。

数组的子序列定义为：将数组中的若干个数字删除（可以删除 0 个数字），剩下的数字按照原本的顺序排布。

__示例:__

示例 1：

```text
输入：nums = [10,2,-10,5,20], k = 2
输出：37
解释：子序列为 [10, 2, 5, 20] 。
```

示例 2：

```text
输入：nums = [-1,-2,-3], k = 1
输出：-1
解释：子序列必须是非空的，所以我们选择最大的数字。
```

示例 3：

```text
输入：nums = [10,-2,-10,-5,20], k = 2
输出：23
解释：子序列为 [10, -2, -5, 20] 。
```

__提示：__

- `1 <= k <= nums.length <= 10 ^ 5`
- `-10 ^ 4 <= nums[i] <= 10 ^ 4`

__思路:__

```text
动态规划 ➕ 单调队列
设 dp[i] 表示选择了 nums[i] 的最大子序和
首先 dp[i] 会从 max(dp[max(0, i - k):i]) 中转移过来
如果 max(dp[max(0, i - k):i]) < 0, 丢弃这个值直接选择 nums[i] 可以得到更大的值
所以转移方程为 dp[i] = max(0, max(dp[max(0, i - k):i])) + nums[i]
最后返回 max(dp)
这个解法的时间复杂度为 O(NK) 会超时
这里可以考虑使用滑动窗口的方式保留区间长度为 k 的最值
如果 j1 < j2 < i, dp[j1] <= dp[j2], 可以抛弃 dp[j1], 因为 dp[j2] 更加有用, 能影响到更多的下标
这样选择双端队列, 在队首弹出超出边界的值, 在队尾弹出不需要重复计算的值
可以将时间复杂度减少为 O(N)
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int constrainedSubsetSum(vector<int>& nums, int k) 
    {
        int result = INT_MIN, n = nums.size();
        deque<int> q;
        vector<int> dp(n);
        for (int i = 0; i < n; i++) 
        {
            if (q.size() and i - q.front() > k) q.pop_front();
            dp[i] = max(0, (q.empty() ? 0 : dp[q.front()])) + nums[i];
            while (q.size() and dp[q.back()] < dp[i]) q.pop_back();
            q.push_back(i);
            result = max(result, dp[i]);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int constrainedSubsetSum(int[] nums, int k) {
        int result = Integer.MIN_VALUE, n = nums.length, dp[] = new int[n];
        Deque<Integer> queue = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            if (!queue.isEmpty() && i - queue.peekFirst() > k) queue.pollFirst();
            dp[i] = Math.max(0, (queue.isEmpty() ? 0 : dp[queue.peekFirst()])) + nums[i];
            while (!queue.isEmpty() && dp[queue.getLast()] < dp[i]) queue.removeLast();
            queue.offerLast(i);
            result = Math.max(result, dp[i]);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def constrainedSubsetSum(self, nums: List[int], k: int) -> int:
        dp, queue = [0] * (n := len(nums)), deque()
        for i in range(n):
            if queue and i - queue[0] > k:
                queue.popleft()
            dp[i] = max(0, 0 if not queue else dp[queue[0]]) + nums[i]
            while queue and dp[queue[-1]] < dp[i]:
                queue.pop()
            queue.append(i)
        return max(dp)
```
