# 1673 Find the Most Competitive Subsequence 找出最具竞争力的子序列

__Description:__

Given an integer array `nums` and a positive integer `k`, return _the most __competitive__ subsequence of_ `nums` _of size_ `k`.

An array's subsequence is a resulting sequence obtained by erasing some (possibly zero) elements from the array.

We define that a subsequence `a` is more __competitive__ than a subsequence `b` (of the same length) if in the first position where `a` and `b` differ, subsequence `a` has a number __less__ than the corresponding number in `b`. For example, `[1,3,4]` is more competitive than `[1,3,5]` because the first position they differ is at the final number, and `4` is less than `5`.

__Example:__

Example 1:

```text
Input: nums = [3,5,2,6], k = 2
Output: [2,6]
Explanation: Among the set of every possible subsequence: {[3,5], [3,2], [3,6], [5,2], [5,6], [2,6]}, [2,6] is the most competitive.
```

Example 2:

```text
Input: nums = [2,4,3,3,5,4,9,6], k = 4
Output: [2,3,3,4]
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 9`
- `1 <= k <= nums.length`

__题目描述:__

给你一个整数数组 `nums` 和一个正整数 `k` ，返回长度为 `k` 且最具 __竞争力__ 的 `nums` 子序列。

数组的子序列是从数组中删除一些元素（可能不删除元素）得到的序列。

在子序列 `a` 和子序列 `b` 第一个不相同的位置上，如果 `a` 中的数字小于 `b` 中对应的数字，那么我们称子序列 `a` 比子序列 `b`（相同长度下）更具 __竞争力__ 。 例如， `[1,3,4]` 比 `[1,3,5]` 更具竞争力，在第一个不相同的位置，也就是最后一个位置上， `4` 小于 `5` 。

__示例:__

示例 1：

```text
输入：nums = [3,5,2,6], k = 2
输出：[2,6]
解释：在所有可能的子序列集合 {[3,5], [3,2], [3,6], [5,2], [5,6], [2,6]} 中，[2,6] 最具竞争力。
```

示例 2：

```text
输入：nums = [2,4,3,3,5,4,9,6], k = 4
输出：[2,3,3,4]
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 9`
- `1 <= k <= nums.length`

__思路:__

```text
单调栈
从左往右遍历数组, 如果当前元素比栈顶元素小, 且栈中元素个数加上剩余元素个数大于等于 k, 那么栈顶元素出栈, 直到栈顶元素小于当前元素或者栈中元素个数加上剩余元素个数小于 k
当栈不满时, 当前元素入栈
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> mostCompetitive(vector<int>& nums, int k) 
    {
        if (k == nums.size()) return nums;
        int top = -1, n = nums.size();
        vector<int> result(k);
        for (int i = 0; i < n; i++) 
        {
            while (top > -1 and nums[i] < result[top] and top + n - i >= k) result[top--] = 0; 
            if (top < k - 1) result[++top] = nums[i];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] mostCompetitive(int[] nums, int k) {
        if (k == nums.length) return nums;
        int result[] = new int[k], top = -1, n = nums.length;
        for (int i = 0; i < n; i++) {
            while (top > -1 && nums[i] < result[top] && top + n - i >= k) result[top--] = 0; 
            if (top < k - 1) result[++top] = nums[i];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def mostCompetitive(self, nums: List[int], k: int) -> List[int]:
        stack, remain = [], len(nums) - k
        for digit in nums:
            while remain and stack and stack[-1] > digit:
                stack.pop()
                remain -= 1
            stack.append(digit)
        return stack[:k]
```
