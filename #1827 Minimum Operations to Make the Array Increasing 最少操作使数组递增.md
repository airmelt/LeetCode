# 1827 Minimum Operations to Make the Array Increasing 最少操作使数组递增

__Description:__

You are given an integer array `nums` (__0-indexed__). In one operation, you can choose an element of the array and increment it by `1`.

- For example, if `nums = [1,2,3]`, you can choose to increment `nums[1]` to make `nums = [1, _3_ ,3]`.

Return _the __minimum__ number of operations needed to make_ `nums` ___strictly__ __increasing__._

An array `nums` is __strictly increasing__ if `nums[i] < nums[i+1]` for all `0 <= i < nums.length - 1`. An array of length `1` is trivially strictly increasing.

__Example:__

Example 1:

```text
Input: nums = [1,1,1]
Output: 3
Explanation: You can do the following operations:
1) Increment nums[2], so nums becomes [1,1,2].
2) Increment nums[1], so nums becomes [1,2,2].
3) Increment nums[2], so nums becomes [1,2,3].
```

Example 2:

```text
Input: nums = [1,5,2,4,1]
Output: 14
```

Example 3:

```text
Input: nums = [8]
Output: 0
```

__Constraints:__

- `1 <= nums.length <= 5000`
- `1 <= nums[i] <= 10 ^ 4`

__题目描述:__

给你一个整数数组 `nums` （__下标从 0 开始__）。每一次操作中，你可以选择数组中一个元素，并将它增加 `1` 。

- 比方说，如果 `nums = [1,2,3]` ，你可以选择增加 `nums[1]` 得到 `nums = [1, _3_ ,3]` 。

请你返回使 `nums` __严格递增__ 的 __最少__ 操作次数。

我们称数组 `nums` 是 __严格递增的__ ，当它满足对于所有的 `0 <= i < nums.length - 1` 都有 `nums[i] < nums[i+1]` 。一个长度为 `1` 的数组是严格递增的一种特殊情况。

__示例:__

示例 1：

```text
输入：nums = [1,1,1]
输出：3
解释：你可以进行如下操作：
1) 增加 nums[2] ，数组变为 [1,1,2] 。
2) 增加 nums[1] ，数组变为 [1,2,2] 。
3) 增加 nums[2] ，数组变为 [1,2,3] 。
```

示例 2：

```text
输入：nums = [1,5,2,4,1]
输出：14
```

示例 3：

```text
输入：nums = [8]
输出：0
```

__提示：__

- `1 <= nums.length <= 5000`
- `1 <= nums[i] <= 10 ^ 4`

__思路:__

```text
贪心
初始化 pre 为 nums[0], pre 取 nums[i] 和 pre + 1 中的较大值
累加 pre 和 nums[i] 的差值即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minOperations(vector<int>& nums) 
    {
        int n = nums.size(), result = 0, pre = nums.front();
        for (int i = 1; i < n; i++) result += (pre = max(pre + 1, nums[i])) - nums[i];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minOperations(int[] nums) {
        int n = nums.length, result = 0, pre = nums[0];
        for (int i = 1; i < n; i++) result += (pre = Math.max(pre + 1, nums[i])) - nums[i];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        n, pre, result = len(nums), nums[0], 0
        for i in range(1, n):
            result += (pre := max(nums[i], pre + 1)) - nums[i]
        return result
```
