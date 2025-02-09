# 2439 Minimize Maximum of Array 最小化数组中的最大值

__Description:__

You are given a __0-indexed__ array `nums` comprising of `n` non-negative integers.

In one operation, you must:

- Choose an integer `i` such that `1 <= i < n` and `nums[i] > 0`.
- Decrease `nums[i]` by 1.
- Increase `nums[i - 1]` by 1.

Return _the __minimum__ possible value of the __maximum__ integer of_ `nums` _after performing __any__ number of operations_.

__Example:__

Example 1:

```text
Input: nums = [3,7,1,6]
Output: 5
Explanation:
One set of optimal operations is as follows:
```

1. Choose i = 1, and nums becomes [4,6,1,6].
2. Choose i = 3, and nums becomes [4,6,2,5].
3. Choose i = 1, and nums becomes [5,5,2,5].

The maximum integer of nums is 5. It can be shown that the maximum number cannot be less than 5.
Therefore, we return 5.

Example 2:

```text
Input: nums = [10,1]
Output: 10
Explanation:
It is optimal to leave nums as is, and since 10 is the maximum value, we return 10.
```

__Constraints:__

- `n == nums.length`
- `2 <= n <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的数组 `nums` ，它含有 `n` 个非负整数。

每一步操作中，你需要：

- 选择一个满足 `1 <= i < n` 的整数 `i` ，且 `nums[i] > 0` 。
- 将 `nums[i]` 减 1 。
- 将 `nums[i - 1]` 加 1 。

你可以对数组执行 __任意__ 次上述操作，请你返回可以得到的 `nums` 数组中 _最大值_ __最小__ 为多少。

__示例:__

示例 1：

```text
输入：nums = [3,7,1,6]
输出：5
解释：
一串最优操作是：
```

1. 选择 i = 1 ，nums 变为 [4,6,1,6] 。
2. 选择 i = 3 ，nums 变为 [4,6,2,5] 。
3. 选择 i = 1 ，nums 变为 [5,5,2,5] 。

nums 中最大值为 5 。无法得到比 5 更小的最大值。
所以我们返回 5 。

示例 2：

```text
输入：nums = [10,1]
输出：10
解释：
最优解是不改动 nums ，10 是最大值，所以返回 10 。
```

__提示：__

- `n == nums.length`
- `2 <= n <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 9`

__思路:__

```text
递推
若 nums 的长度为 1, 则最大值即为 nums[0]
若 nums 的长度为 2, 如果 nums[1] <= nums[0], 则最大值即为 nums[0], 否则最大值即为 (nums[1] + nums[0] + 1) / 2
依次类推, 并引入前缀和数组 pre, pre[i] 表示 nums[0] + nums[1] + ... + nums[i]
则 nums[i] 的最大值即为 (pre[i] + i) / (i + 1)
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimizeArrayValue(vector<int>& nums) 
    {
        long long result = 0;
        vector<long long> pre(nums.size() + 1);
        for (int i = 0, n = nums.size(); i < n; i++) pre[i + 1] = pre[i] + nums[i];
        for (int i = 0, n = nums.size(); i < n; i++) result = max(result, (pre[i + 1] + i) / (i + 1));
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimizeArrayValue(int[] nums) {
        long pre[] = new long[nums.length + 1], result = 0L;
        for (int i = 0, n = nums.length; i < n; i++) pre[i + 1] = pre[i] + nums[i];
        for (int i = 0, n = nums.length; i < n; i++) result = Math.max(result, (pre[i + 1] + i) / (i + 1));
        return (int)result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimizeArrayValue(self, nums: List[int]) -> int:
        return max((s + i) // (i + 1) for i, s in enumerate(accumulate(nums)))
```
