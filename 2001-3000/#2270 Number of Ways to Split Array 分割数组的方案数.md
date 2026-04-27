# 2270 Number of Ways to Split Array 分割数组的方案数

__Description:__

You are given a __0-indexed__ integer array `nums` of length `n`.

`nums` contains a __valid split__ at index `i` if the following are true:

- The sum of the first `i + 1` elements is __greater than or equal to__ the sum of the last `n - i - 1` elements.
- There is __at least one__ element to the right of `i`. That is, `0 <= i < n - 1`.

Return _the number of __valid splits__ in_ `nums`.

__Example:__

Example 1:

```text
Input: nums = [10,4,-8,7]
Output: 2
Explanation: 
There are three ways of splitting nums into two non-empty parts:
```

- Split nums at index 0. Then, the first part is [10], and its sum is 10. The second part is [4,-8,7], and its sum is 3. Since 10 >= 3, i = 0 is a valid split.
- Split nums at index 1. Then, the first part is [10,4], and its sum is 14. The second part is [-8,7], and its sum is -1. Since 14 >= -1, i = 1 is a valid split.
- Split nums at index 2. Then, the first part is [10,4,-8], and its sum is 6. The second part is [7], and its sum is 7. Since 6 < 7, i = 2 is not a valid split.

Thus, the number of valid splits in nums is 2.

Example 2:

```text
Input: nums = [2,3,1,0]
Output: 2
Explanation: 
There are two valid splits in nums:
```

- Split nums at index 1. Then, the first part is [2,3], and its sum is 5. The second part is [1,0], and its sum is 1. Since 5 >= 1, i = 1 is a valid split.
- Split nums at index 2. Then, the first part is [2,3,1], and its sum is 6. The second part is [0], and its sum is 0. Since 6 >= 0, i = 2 is a valid split.

__Constraints:__

- `2 <= nums.length <= 10 ^ 5`
- `-10 ^ 5 <= nums[i] <= 10 ^ 5`

__题目描述:__

给你一个下标从 __0__ 开始长度为 `n` 的整数数组 `nums` 。

如果以下描述为真，那么 `nums` 在下标 `i` 处有一个 __合法的分割__ :

- 前 `i + 1` 个元素的和 __大于等于__ 剩下的 `n - i - 1` 个元素的和。
- 下标 `i` 的右边 __至少有一个__ 元素，也就是说下标 `i` 满足 `0 <= i < n - 1` 。

请你返回 `nums` 中的 __合法分割__ 方案数。

__示例:__

示例 1：

```text
输入：nums = [10,4,-8,7]
输出：2
解释：
总共有 3 种不同的方案可以将 nums 分割成两个非空的部分：
```

- 在下标 0 处分割 nums 。那么第一部分为 [10] ，和为 10 。第二部分为 [4,-8,7] ，和为 3 。因为 10 >= 3 ，所以 i = 0 是一个合法的分割。
- 在下标 1 处分割 nums 。那么第一部分为 [10,4] ，和为 14 。第二部分为 [-8,7] ，和为 -1 。因为 14 >= -1 ，所以 i = 1 是一个合法的分割。
- 在下标 2 处分割 nums 。那么第一部分为 [10,4,-8] ，和为 6 。第二部分为 [7] ，和为 7 。因为 6 < 7 ，所以 i = 2 不是一个合法的分割。

所以 nums 中总共合法分割方案受为 2 。

示例 2：

```text
输入：nums = [2,3,1,0]
输出：2
解释：
总共有 2 种 nums 的合法分割：
```

- 在下标 1 处分割 nums 。那么第一部分为 [2,3] ，和为 5 。第二部分为 [1,0] ，和为 1 。因为 5 >= 1 ，所以 i = 1 是一个合法的分割。
- 在下标 2 处分割 nums 。那么第一部分为 [2,3,1] ，和为 6 。第二部分为 [0] ，和为 0 。因为 6 >= 0 ，所以 i = 2 是一个合法的分割。

__提示：__

- `2 <= nums.length <= 10 ^ 5`
- `-10 ^ 5 <= nums[i] <= 10 ^ 5`

__思路:__

```text
前缀和
统计前缀和中有多少个前缀和大于等于后缀和
或者说前缀和中有多少个两倍前缀和大于等于总和
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int waysToSplitArray(vector<int>& nums) 
    {
        int n = nums.size(), result = 0;
        vector<long long> pre(n + 1);
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + nums[i]; 
        for (int i = 0; i < n - 1; i++) result += pre[i + 1] >= pre[n] - pre[i + 1];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int waysToSplitArray(int[] nums) {
        int n = nums.length, result = 0;
        long[] pre = new long[n + 1];
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + nums[i]; 
        for (int i = 0; i < n - 1; i++) if (pre[i + 1] >= pre[n] - pre[i + 1]) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def waysToSplitArray(self, nums: List[int]) -> int:
        return 0 if not (pre := list(accumulate(nums))) else sum(pre[i] >= pre[-1] - pre[i] for i in range(len(nums) - 1))
```
