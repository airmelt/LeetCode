# 2945 Find Maximum Non-decreasing Array Length 找到最大非递减数组的长度

__Description:__

You are given a __0-indexed__ integer array `nums`.

You can perform any number of operations, where each operation involves selecting a __subarray__ of the array and replacing it with the __sum__ of its elements. For example, if the given array is `[1,3,5,6]` and you select subarray `[3,5]` the array will convert to `[1,8,6]`.

Return the maximum length of a non-decreasing array that can be made after applying operations.

A subarray is a contiguous non-empty sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums = [5,2,2]
Output: 1
Explanation: This array with length 3 is not non-decreasing.
We have two ways to make the array length two.
First, choosing subarray [2,2] converts the array to [5,4].
Second, choosing subarray [5,2] converts the array to [7,2].
In these two ways the array is not non-decreasing.
And if we choose subarray [5,2,2] and replace it with [9] it becomes non-decreasing. 
So the answer is 1.
```

Example 2:

```text
Input: nums = [1,2,3,4]
Output: 4
Explanation: The array is non-decreasing. So the answer is 4.
```

Example 3:

```text
Input: nums = [4,3,2,6]
Output: 3
Explanation: Replacing [3,2] with [5] converts the given array to [4,5,6] that is non-decreasing.
Because the given array is not non-decreasing, the maximum possible answer is 3.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。

你可以执行任意次操作。每次操作中，你需要选择一个 __子数组__ ，并将这个子数组用它所包含元素的 __和__ 替换。比方说，给定数组是 `[1,3,5,6]` ，你可以选择子数组 `[3,5]` ，用子数组的和 `8` 替换掉子数组，然后数组会变为 `[1,8,6]` 。

请你返回执行任意次操作以后，可以得到的 最长非递减 数组的长度。

子数组 指的是一个数组中一段连续 非空 的元素序列。

__示例:__

示例 1：

```text
输入：nums = [5,2,2]
输出：1
解释：这个长度为 3 的数组不是非递减的。
我们有 2 种方案使数组长度为 2 。
第一种，选择子数组 [2,2] ，对数组执行操作后得到 [5,4] 。
第二种，选择子数组 [5,2] ，对数组执行操作后得到 [7,2] 。
这两种方案中，数组最后都不是 非递减 的，所以不是可行的答案。
如果我们选择子数组 [5,2,2] ，并将它替换为 [9] ，数组变成非递减的。
所以答案为 1 。
```

示例 2：

```text
输入：nums = [1,2,3,4]
输出：4
解释：数组已经是非递减的。所以答案为 4 。
```

示例 3：

```text
输入：nums = [4,3,2,6]
输出：3
解释：将 [3,2] 替换为 [5] ，得到数组 [4,5,6] ，它是非递减的。
最大可能的答案为 3 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`

__思路:__

```text
动态规划 ➕ 单调队列
设 dp[i] 表示前 i 个元素最长的非递减数组的长度
last[i] 表示 dp[i] 取最大值的时候下一个元素的值(因为当前元素至少为 last[i], 由于要保持非递减, 下一个元素就必须不小于 last[i])
则 pre[i] - pre[j] >= last[j]
即 pre[i] >= last[j] + pre[j]
使用单调队列记录 j
如果 pre[j + 1] + last[j + 1] <= pre[i], 说明队首元素已经不在适用, 弹出队首元素
更新 dp[i] 和 last[i]
在更新单调队列之前
检查队尾元素 pre[j] + last[j] >= pre[i] + last[i], 这部分需要都弹出
最后再把 i 加入队列
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findMaximumLength(vector<int>& nums) 
    {
        int n = nums.size(), front = 0, rear = 0;
        vector<int> q(n + 1), dp(n + 1);
        vector<long long> pre(n + 1), last(n + 1);
        for (int i = 1; i <= n; i++) 
        {
            pre[i] = pre[i - 1] + nums[i - 1];
            while (front < rear and pre[q[front + 1]] + last[q[front + 1]] <= pre[i]) ++front;
            dp[i] = dp[q[front]] + 1; 
            last[i] = pre[i] - pre[q[front]];
            while (rear >= front and pre[q[rear]] + last[q[rear]] >= pre[i] + last[i]) --rear;
            q[++rear] = i;
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int findMaximumLength(int[] nums) {
        int n = nums.length, front = 0, rear = 0, q[] = new int[n + 1], dp[] = new int[n + 1];
        long[] pre = new long[n + 1], last = new long[n + 1];
        for (int i = 1; i <= n; i++) {
            pre[i] = pre[i - 1] + nums[i - 1];
            while (front < rear && pre[q[front + 1]] + last[q[front + 1]] <= pre[i]) ++front;
            dp[i] = dp[q[front]] + 1; 
            last[i] = pre[i] - pre[q[front]];
            while (rear >= front && pre[q[rear]] + last[q[rear]] >= pre[i] + last[i]) --rear;
            q[++rear] = i;
        }
        return dp[n];
    }
}
```

__Python__:

```Python
class Solution:
    def findMaximumLength(self, nums: List[int]) -> int:
        q = deque([(0, 0, 0)])
        for num in accumulate(nums):
            while q and q[0][0] <= num:
                t = q.popleft()
            while q and q[-1][0] >= (m := (num << 1) - t[1]):
                q.pop()
            q.append(((num << 1) - t[1], num, t[2] + 1))
        return q.pop()[2]
```
