# 1856 Maximum Subarray Min-Product 子数组最小乘积的最大值

__Description:__

The min-product of an array is equal to the minimum value in the array multiplied by the array's sum.

- For example, the array `[3,2,5]` (minimum value is `2`) has a min-product of `2 * (3+2+5) = 2 * 10 = 20`.

Given an array of integers `nums`, return _the __maximum min-product__ of any __non-empty subarray__ of_ `nums`. Since the answer may be large, return it __modulo__ `10 ^ 9 + 7`.

Note that the min-product should be maximized before performing the modulo operation. Testcases are generated such that the maximum min-product without modulo will fit in a 64-bit signed integer.

A subarray is a contiguous part of an array.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,2]
Output: 14
Explanation: The maximum min-product is achieved with the subarray [2,3,2] (minimum value is 2).
2 * (2+3+2) = 2 * 7 = 14.
```

Example 2:

```text
Input: nums = [2,3,3,1,2]
Output: 18
Explanation: The maximum min-product is achieved with the subarray [3,3] (minimum value is 3).
3 * (3+3) = 3 * 6 = 18.
```

Example 3:

```text
Input: nums = [3,1,5,6,4,2]
Output: 60
Explanation: The maximum min-product is achieved with the subarray [5,6,4] (minimum value is 4).
4 * (5+6+4) = 4 * 15 = 60.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 7`

__题目描述:__

一个数组的 最小乘积 定义为这个数组中 最小值 乘以 数组的 和 。

- 比方说，数组 `[3,2,5]` （最小值是 `2`）的最小乘积为 `2 * (3+2+5) = 2 * 10 = 20` 。

给你一个正整数数组 `nums` ，请你返回 `nums` 任意 __非空子数组__ 的__最小乘积__ 的 __最大值__ 。由于答案可能很大，请你返回答案对  `10 ^ 9 + 7` __取余__ 的结果。

请注意，最小乘积的最大值考虑的是取余操作 之前 的结果。题目保证最小乘积的最大值在 不取余 的情况下可以用 64 位有符号整数 保存。

子数组 定义为一个数组的 连续 部分。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,2]
输出：14
解释：最小乘积的最大值由子数组 [2,3,2] （最小值是 2）得到。
2 * (2+3+2) = 2 * 7 = 14 。
```

示例 2：

```text
输入：nums = [2,3,3,1,2]
输出：18
解释：最小乘积的最大值由子数组 [3,3] （最小值是 3）得到。
3 * (3+3) = 3 * 6 = 18 。
```

示例 3：

```text
输入：nums = [3,1,5,6,4,2]
输出：60
解释：最小乘积的最大值由子数组 [5,6,4] （最小值是 4）得到。
4 * (5+6+4) = 4 * 15 = 60 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 7`

__思路:__

```text
单调栈 ➕ 前缀和
子数组的和可以通过前缀和来计算
假定遍历的每一个数都是当前子数组的最小元素
需要找到每个元素前面和后面第一个比它小的元素的位置 [left[i], right[i]], right[i] 可以取等于也能保证数组中的最小值为 nums[i]
则这个子数组最小值为 nums[i], 前缀和可以通过 pre[right[i] + 1] - pre[left[i]] 来计算
初始化 left[i] = 0, right[i] = n - 1 与前缀和保持一致可以方便计算
用单调栈找到 left[i] 与 right[i]
然后遍历数组并更新结果
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxSumMinProduct(vector<int>& nums) 
    {
        int n = nums.size();
        vector<int> left(n), right(n, n - 1);
        vector<long> pre(n + 1);
        long MOD = 1e9 + 7, result = 0;
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + nums[i];
        stack<int> s;
        for (int i = 0; i < n; i++) 
        {
            while (!s.empty() and nums[s.top()] >= nums[i]) 
            {
                right[s.top()] = i - 1;
                s.pop();
            }
            if (!s.empty()) left[i] = s.top() + 1;
            s.push(i);
        }
        for (int i = 0; i < n; i++) result = max(result, (pre[right[i] + 1] - pre[left[i]]) * nums[i]);
        return result % MOD;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxSumMinProduct(int[] nums) {
        int n = nums.length, left[] = new int[n], right[] = new int[n];
        long MOD = 1_000_000_007, result = 0, pre[] = new long[n + 1];
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + nums[i];
        Arrays.fill(right, n - 1);
        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() && nums[stack.peek()] >= nums[i]) {
                right[stack.peek()] = i - 1;
                stack.pop();
            }
            if (!stack.isEmpty()) left[i] = stack.peek() + 1;
            stack.push(i);
        }
        for (int i = 0; i < n; i++) result = Math.max(result, (pre[right[i] + 1] - pre[left[i]]) * nums[i]);
        return (int)(result % MOD);
    }
}
```

__Python__:

```Python
class Solution:
    def maxSumMinProduct(self, nums: List[int]) -> int:
        MOD, left, right, s, pre = 10 ** 9 + 7, [0] * (n := len(nums)), [n - 1] * n, [], list(accumulate([0] + nums))
        for i, num in enumerate(nums):
            while s and nums[s[-1]] >= num:
                right[s[-1]] = i - 1
                s.pop()
            if s:
                left[i] = s[-1] + 1
            s.append(i)
        return max((pre[right[i] + 1] - pre[left[i]]) * num for i, num in enumerate(nums)) % MOD
```
